---
title: "SSO 单点登录"
date: 2018-04-01T23:24:03+08:00
draft: false
tags: [SSO]
categories: [Architecture]
---
<!--more-->

### 说在前面

由于公司要开发几套系统，并且需要几套系统使用统一的用户管理功能，TM让我研究如何实现多系统的单点登录，这周零零碎碎研究了下，所以写下这篇文章，加深下理解，也方便以后回顾。

### 介绍

Single sign-on(SSO) 是多个相关但彼此独立的软件系统访问控制的一个属性，也就是用户只使用一个账号和密码来访问多个系统。 单点登录在大型的系统中经常用到，例如：刚开始你的百度搜索 www.baidu.com 和百度云盘都是没有登录的，现在当你登录了百度搜索的系统后，再去刷新一下百度云盘的系统时，这时你会发现百度云盘这边也是显示登录的，这种情况就是使用 SSO 技术实现的。 CAS(Central Authentication Service) 旨在为 Web 应用系统提供一种可靠的单点登录解决方法（属于 Web SSO ）。

### HTTP 知识介绍

由于 HTTP 是无状态的协议，所以我们需要使用某些机制来保存web用户浏览的状态。Cookie 和 Session 都是用来保存web状态的两种技术，区别是 Cookie 是在客户端来保存用户状态，Session 是在服务端来保存用户状态。

### SSO 的几种情况

##### 1.同域下的不同服务

例如我们有两个服务 test.com/site1 test.com/site2 这是最简单的一种情况(不过一般大型系统会用域来区分两个服务)，两个服务共享一个主机地址，且在同一个域下。这种情况下两个业务共享一 个 Cookie 和 Session，所以不用做特别的处理，按照常规的验证方式即可，当你在 site1 上登录了，在 site2 上自然而然也是登录的。

#### 2.同父域不同子域的服务

例如 site1.x.com site2.x.com 这种情况两个服务的主机是不相同的(父域相同，子域不同)，这种情况我们一般可以设置两个服务的 Cookie 都是在当前域的顶级域下的，也就是 x.com 域下。 cas.x.com 设置一个 CAS(Central Authentication Service)，所有的与登录和验证token和设置 Cookie 的 操作我们都放在 CAS 上面

#### 3.完全不同域的服务

例如 site1.com site2.com

### SSO 介绍

**单点登录 Single Sign-on(SSO)：** 一种对于许多相互关连，但是又是各自独立的软件系统，提供访问控制的属性。拥有这项属性，当用户登录时，就可以获取所有系统的访问权限，不用对每个单一系统都逐一登录 **单点注销 Single Sign-off：** 只需要单一的注销动作，就可以结束对于多个系统的访问权限 **SSO 的优点** \- 降低访问第三方网站风险（用户密码不存储或外部管理，使用token） - 提高用户体验（不再多次登录不同的服务）

### CAS 介绍

**集中式认证服务 Central Authentication Service(CAS)：** 是一种针对万维网的单点登录协议。它的目的是允许一个用户访问多个应用程序，而只需提供一次凭证（如用户名和密码）。它还允许web应用程序在没有获得用户的安全凭据（如密码）的情况下对用户进行身份验证 **描述：** CAS 协议涉及到至少三个方面：客户端Web浏览器、Web应用请求的身份验证和CAS服务器。它还可能涉及一个后台服务（例如数据库服务器），它并没有自己的HTTP接口，但与一个Web应用程序进行通信。当客户端访问访问应用程序，请求身份验证时，应用程序重定向到CAS。CAS验证客户端是否被授权，通常通过在数据库对用户名和密码进行检查。如果身份验证成功，CAS令客户端返回到应用程序，并传递身份验证票（Security ticket）。然后，应用程序通过安全连接连接CAS，并提供自己的服务标识和验证票。之后CAS给出了关于特定用户是否已成功通过身份验证的应用程序授信信息 **cas flow diagram** ![cas_flow_diagram](https://apereo.github.io/cas/4.2.x/images/cas_flow_diagram.png)

### SSO 实现一(同父域不同子域)

该例子是一个简单的不完全跨域的例子，认证流程比上面讲的流程稍微简单点，没有考虑到很全面的安全问题，只是简单的用php实现的，关于跨域问题可以参考我的另一篇文章 跨域 **场景描述：** 现在有两个业务站 `site1.x.com` 和 `site2.x.com`，一个 CAS 站 `myauth.x.com` 业务站 site1 第一次访问： 1.进入业务站 site1，被发现未登录，被重定向到 cas 的 login 页面 2.在 login 页面填写账号和密码，post 提交后认证用户是否存在 3.然后根据信息生成一个 token 传递给业务站 site1，并重定向到 site1 4.site1 携带 token 重定向到 cas 的 authentication 进行验证 5.验证成功后，将 token 和其他用户信息写入 cookie，并重定向回业务站 site1 6.业务站 site1 检验 cookie 中的 token，确认登录 业务站 site2 第一次访问： 直接判断父域的 cookie 中是否存在 token 和用户信息，并判断 cookie 中是登录为状态，若是则 site2 直接登录；若不是则重定向到 cas 的 login 页面重新进行登录 单点登出： 在 site1 站点击 logout，可以看到 site1 站已经退出登录状态，并重定向到 cas 的 login 页面， 再在 site2 站刷新页面，也可以看到是退出登录，并重定向到 cas 的 login 页面，反之亦然 **实现代码** site1.x.com/index.php

    <?php
    /**
     * Created by PhpStorm.
     * User: forrest
     * Date: 18-3-20
     * Time: 上午10:58
     */
    
    $self = urlencode('site1.x.com/index.php');
    
    if (isset($_COOKIE['token']) && isset($_COOKIE['username']) && isset($_COOKIE['time'])) {
        if (!isset($_COOKIE['isLogin']) || $_COOKIE['isLogin'] != 1) {
            // goto authentication
            $location = 'Location:http://myauth.x.com/authentication.php?from='
                .$self. '&token=' .$_COOKIE['token']. '&time=' .$_COOKIE['time']. '&username=' .$_COOKIE['username'];
            header($location);
        }
    
        // logout AND goto
        echo 'you are login site1 <a href="http://myauth.x.com/logout.php?from=' .$self. '">Logout</a><br>';
        $href = 'http://myauth.x.com/authentication.php?from=site2.x.com&token='
            .$_COOKIE['token']. '&time=' .$_COOKIE['time']. '&username=' .$_COOKIE['username'];
        echo '<a href="' .$href. '">Go to site2.x.com</a>';
    
    } else if (isset($_GET['token']) && isset($_GET['time']) && isset($_GET['username'])) {
        // check token
        $location = 'Location:http://myauth.x.com/authentication.php?from='
            .$self. '&token=' .$_GET['token']. '&time=' .$_GET['time']. '&username=' .$_GET['username'];
        header($location);
    
    } else {
        $location = 'Location:http://myauth.x.com/login.php?from=' . $self;
        header($location);
    }
    

myauth.x.com/login.php

    <?php
    /**
     * Created by PhpStorm.
     * User: forrest
     * Date: 18-3-20
     * Time: 上午11:02
     */
    
    $from = isset($_GET['from']) ? $_GET['from'] : '';
    
    if (isset($_POST['dosubmit'])) {
        $username = addslashes($_POST['username']);
        $password = sha1($_POST['password']);
        $time = time();
        // create token
        if (!empty($username) && !empty($password)) {
            // select
            include_once 'db.php';
            $db = DB::getInstance();
            $sql = 'SELECT * FROM `user` WHERE `username`=:name AND `password`=:pass LIMIT 1';
            $stmt = $db->prepare($sql);
            $stmt->bindParam(':name', $username);
            $stmt->bindParam(':pass', $password);
            $stmt->execute();
    
            if ($stmt->fetchColumn() > 0) {
                // 1.create token
                $token = create_token($username, $time);
                // 2.redirect old service application
                $location = "Location:http://{$from}?token={$token}&time={$time}&username={$username}";
                header($location);
    
            } else {
                echo 'Login failed';
            }
    
        }
    }
    
    function create_token($username, $time) {
        $request_ip = $_SERVER['REMOTE_ADDR'];
        return sha1(md5($username).base64_encode($request_ip).md5($time));
    }
    
    ?>
    
    
    <html>
    <head>
        <meta charset="utf-8">
        <title>Login</title>
    </head>
    <body>
    <form method="post" action="login.php?from=<?=urlencode($from)?>">
        <h1>Login</h1>
        Username: <input type="text" name="username"><br>
        Password: <input type="password" name="password"><br>
        <input type="submit" name="dosubmit" value="submit">
        &nbsp;&nbsp;&nbsp;<a href="register.php?from=<?=$from?>">GoToRegister</a>
    </form>
    </body>
    </html>
    
    

myauth.x.com/authentication.php

    <?php
    
    $from = isset($_GET['from']) ? $_GET['from'] : '';
    $token = isset($_GET['token']) ? $_GET['token'] : '';
    $time = isset($_GET['time']) ? $_GET['time'] : '';
    $username = isset($_GET['username']) ? $_GET['username'] : '';
    
    // 1.check token
    if (!empty($from) && !empty($token) && !empty($time) && !empty($username)) {
        // (1).check expire, 10 minutes
        if (time() - $time > 10 * 60) {
            echo 'Sorry, token is expired';
            exit;
        }
        // (2).check valid
        $tmp_token = create_token($username, $time);
        if (is_valid($token, $tmp_token)) {
            // 1).write token in cookie
            setcookie('token', $token, time() + 10 * 60, '/', 'x.com');
            setcookie('time', $time, time() + 10 * 60, '/', 'x.com');
            setcookie('username', $username, time() + 10 * 60, '/', 'x.com');
            setcookie('isLogin', 1, time() + 10 * 60, '/', 'x.com');
    
            // 2).location from
            $location = "Location:http://{$from}";
            header($location);
        } else {
            echo 'Sorry, token is invalid';
            exit;
        }
    }
    
    function is_valid($token, $tmp_token)
    {
        if ($token === $tmp_token) {
            return true;
        }
        return false;
    }
    
    function create_token($username, $time) {
        $request_ip = $_SERVER['REMOTE_ADDR'];
        return sha1(md5($username).base64_encode($request_ip).md5($time));
    }
    
    ?>
    

site2.x.com/index.php

    <?php
    /**
     * Created by PhpStorm.
     * User: forrest
     * Date: 18-3-20
     * Time: 上午10:58
     */
    
    $self = urlencode('site2.x.com/index.php');
    
    if (isset($_COOKIE['token']) && isset($_COOKIE['username']) && isset($_COOKIE['time'])) {
        if (!isset($_COOKIE['isLogin']) || $_COOKIE['isLogin'] != 1) {
            // goto authentication
            $location = 'Location:http://myauth.x.com/authentication.php?from='
                .$self. '&token=' .$_COOKIE['token']. '&time=' .$_COOKIE['time']. '&username=' .$_COOKIE['username'];
            header($location);
        }
    
        // logout AND goto
        echo 'you are login site2 <a href="http://myauth.x.com/logout.php?from=' .$self. '">Logout</a><br>';
        $href = 'http://myauth.x.com/authentication.php?from=site1.x.com&token='
            .$_COOKIE['token']. '&time=' .$_COOKIE['time']. '&username=' .$_COOKIE['username'];
        echo '<a href="' .$href. '">Go to site1.x.com</a>';
    
    } else if (isset($_GET['token']) && isset($_GET['time']) && isset($_GET['username'])) {
        // check token
        $location = 'Location:http://myauth.x.com/authentication.php?from='
            .$self. '&token=' .$_GET['token']. '&time=' .$_GET['time']. '&username=' .$_GET['username'];
        header($location);
    
    } else {
        $location = 'Location:http://myauth.x.com/login.php?from=' . $self;
        header($location);
    }
    

myauth.x.com/logout.php

    <?php
    /**
     * Created by PhpStorm.
     * User: forrest
     * Date: 18-3-20
     * Time: 下午1:22
     */
    
    setcookie('username', '', 0, '/', 'x.com');
    setcookie('token', '', 0, '/', 'x.com');
    setcookie('time', '', 0, '/', 'x.com');
    
    $from = isset($_GET['from']) ? $_GET['from'] : '';
    $location = 'Location:http://' . $from;
    header($location);
    

### 参考

[Single sign-on](https://en.wikipedia.org/wiki/Single_sign-on)
[Central Authentication Service](https://en.wikipedia.org/wiki/Central_Authentication_Service)
[单点登录](https://zh.wikipedia.org/wiki/%E5%96%AE%E4%B8%80%E7%99%BB%E5%85%A5)
[集中式认证服务](https://zh.wikipedia.org/wiki/%E9%9B%86%E4%B8%AD%E5%BC%8F%E8%AE%A4%E8%AF%81%E6%9C%8D%E5%8A%A1)
[CAS protocol](https://apereo.github.io/cas/4.2.x/protocol/CAS-Protocol.html)
