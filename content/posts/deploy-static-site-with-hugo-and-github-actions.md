---
title: "使用 Hugo 和 Github Actions 部署静态网站"
date: 2022-09-17T02:43:00+08:00
draft: false
categories: [Blog]
tags: [Blog, Hugo]
---

{{< toc >}}

## 说在前面
年初将自己个人博客的部署方式改为 Github Actions 自动部署，其中碰到的坑一直没有空整理成文章输出出来，今天统一将内容整理到这篇文章。

写之前顺便提下自己写技术博客的历程吧，最开始写博客的时候差不多是在大二的样子，当时主要是在 CSDN 上面写，内容差不多都是记录总结学习各个技术栈碰到的坑；后来大三快出来实习的时候，买了台阿里云的低配 ECS，使用 Wordpress 将自己的个人博客部署在上面；再后来觉得 Wordpress 需要数据库存储内容、且不太好能结合 Markdown 写博客、还有没有太好看的主题等原因，然后就换成在 Jekyll 和 Hexo 对比中选择了后者；Hexo 还是部署在阿里云的，部署方式是通过在 ECS 上搭建 Git 仓库，通过 Git 钩子将 Hexo 生成的博客静态页面文件，推送到 Nginx 服务的托管目录，完成部署。

到了去年年底，用了几年的阿里云 ECS 到期了，由于续费太贵就没续费，于是就买了腾讯云的 Lighthouse 服务器，打算把博客也迁移过来。但是之前使用 Hexo 的那套部署方式比较麻烦，所以搜索了一番就决定使用 Hugo 和 Github Page 来部署静态博客。


## 博客搭建
### 首先跟着 Hugo 官方教程一步步操作
[Hugo quick-start](https://gohugo.io/getting-started/quick-start/)
- 本地安装 Hugo
- 新建本地 Hugo blog 项目
- 添加主题及修改主题配置
- 新建文章
- 本地查看
- 生成静态文件

### 再将静态内容发布到 Github Page
1. 在 Github 新建项目 gzhh.github.io
```
    Github 会将该仓库下的内容部署成静态网站
```

2. cd 到本地项目的 public 目录，执行下列命令，将生成的博客静态文件推送到远程 Github Page 仓库
```
    git init
    git remote add origin [git@github.com](mailto:git@github.com):gzhh/gzhh.github.io.git
    git add .
    git commit -m "first commit"
    git push -f origin master
```

3. 切换分支
```
    将 master 切换到 main 分支
    git branch -m master main
    
    推送到远端
    git push -u origin main
    
    删除 master
    git push origin --delete master
```

4. 至此，完成初步部署，可以通过访问 https://gzhh.github.io 来浏览博客内容


## 使用 Github Action 优化部署
按照上述方式，hugo -D 生成静态内容到 public 目录，再进入到 public 目录将修改提交并推送到远程 Github Page 仓库。

始终觉得应该可以省掉一个步骤，于是偶然了解到可以通过强大的 Github Actions 实现自动化 workflow，类似 CI/CD 的特性，下面继续写使用其优化博客部署流程。

### 将本地博客项目同步到远程 Github 仓库
新建远程博客模版仓库，并将本地博客项目与其同步
- https://github.com/gzhh/hugo-template
- git remote add origin git@github.com:gzhh/hugo-template

### 新建 workflow
- 在博客模版项目下新建 workflow 配置文件 .github/workflows/deploy.yml，可参考 [deploy.yml](https://github.com/gzhh/hugo-template/blob/main/.github/workflows/deploy.yml)
- 大致流程就是监听博客模版项目的 main 分支是否有提交，如果有则执行静态博客构建任务
  - 配置 ubuntu-latest 系统环境
  - 安装 hugo
  - 生成静态文件
  - 创建 CNAME 静态文件到 public 目录
  - 将生成的静态文件发布到 Github Page 仓库（注意这里需要配置 Actions secrets）
- 将修改提交到 Github 远程博客模版仓库
  - 进入 Github 该博客模版仓库的 Actions，可看到名称为 deploy 的 workflow
  - 并且也能看到每次触发执行任务的日志，从日志中可以很清晰的查看任务调度流程及错误输出
  - 进入 Github Page 仓库，发现静态文件内容也随之更新

至此，我们已经完成使用 Hugo + Github Actions + Github Page 来部署我们的博客，后续新写文章只需要将本地博客模版仓库的 md 文件提交到 Github 远程仓库，就能通过 Github Actions 完成静态博客的内容发布。


## 配置域名及域名解析
使用 Github Page 提供的域名 gzhh.github.io 浏览博客有时候会觉得不太个性化，于是在 Godaddy 上买了 gzhh.tech 域名，将 DNS 托管到 Cloudflare，新建一条 blog 指向 gzhh.github.io 的 CNAME 记录，那么生效后就可以通过 blog.gzhh.tech 来访问博客站点。

前文已提到需要在 Github Page 项目的根目录添加一个 CNAME 文件，内容为 blog.gzhh.tech。


## 博客图片
最开始博客图片就直接放在 Github 仓库里，但是 Github 仓库的图片加载比较慢，所以就放弃了这种方案。

然后也考虑了免费图床，例如 https://imgur.com，不过觉得图片放在免费图床可能对隐私及使用权方面有些顾虑，也放弃了这种方案。

再然后也考虑了付费对象存储，例如阿里云的 OSS，虽说花不了多少费用，但还是觉得文件放在别人那里不太自由，以后迁移的话还需要额外处理，也放弃了这方案。

最后如果想各方面都满足自己的要求，那就自己动手自建个图床（还未开源）
- 使用 Golang 和 Gin 搭建图片上传服务
- 简单做了登录认证功能，并配置 https 以增加安全
- 最后使用 Nginx 部署图片服务 https://cdn.gzhh.tech


## 博客评论
博客评论使用的 Disqus
- 注册 disqus 账号，并在 disqus 后台新建站点 `gzhh-tech`
- 然后在博客模版项目的 config.toml 配置文件中添加 `disqusShortname = "gzhh-tech"`，提交修改后就能将 Hugo 与 Disqus 绑定


## 参考
- https://pages.github.com
- https://github.com/features/actions
- https://gohugo.io/documentation
- https://gohugo.io/getting-started/quick-start
- https://github.com/luizdepra/hugo-coder
- https://www.cloudflare.com
- https://disqus.com
- 搭建参考1: https://h1z3y3.me/posts/hugo-auto-deploy-github-with-actions/
- 搭建参考2: https://www.pseudoyu.com/zh/2022/05/29/deploy_your_blog_using_hugo_and_github_action

