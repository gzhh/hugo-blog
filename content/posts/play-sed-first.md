---
title: "玩转sed操作文本-基础篇"
date: 2017-11-17T18:42:35+08:00
draft: false
tags: [Sed]
categories: [Linux]
---

### 介绍
sed 全名是 stream editor，是文本流处理方面的强大工具，和另一个文本处理工具 awk 工具有的一比，接下来就让我们简单介绍下它的基本使用和适用场景。
我们这篇主要讲的是 Gnu sed，还有一个常用的是 BSD sed（Mac OS就是源自 BSD 的），两者略微有点不同，不过他们俩都是由 Posix sed 演化而来。

### 基本格式
sed 命令的执行有两种基本格式
sed [-参数] command [file ...]
sed [-参数] [-e command] [-f command_file] [-i extension]

### 参数
默认情况下，sed 不会操作原来的文本，加上 -i 参数则会改变原文件的内容
sed -i command [file ...]

### 替换

    1.将文件中每行所有的字符 , 都替换为空字符(不加 g 标记则只替换每行第一个匹配)
    sed 's/,/ /g' file.txt

    2.从第 N 处匹配开始替换 /ng
    sed 's/,/ /2g' file.txt

    3.将字符 , 替换为空字符并保存替换后的内容
    sed -i 's/,/ /g' file.txt

    4.-n 选项和 p 标记一起使用表示只打印那些发生替换的行
    sed -n 's/,/ /gp' file.txt 

### 删除
    
    1.删除空白行
    sed '/^$/d' file

    2.删除文件的第2行：
    sed '2d' file
    
    3.删除文件的第2行到末尾所有行：
    sed '2,$d' file
    删除文件的1~3行
    sed '1,3d' file
    
    4.删除文件首行和最后一行：
    sed '1d' file
    sed '$d' file
    
    5.删除文件中所有开头是test的行：
    sed '/^test/'d file

### 注释和取消注释操作

    1.注释：查找到指定文本 0.0.0.1 的这行，并在 0.0.0.1 前面加上 # 符号
    sed -i '/0.0.0.1/s/^/#/' filename
    
    2.取消注释：把每一行开始的 # 去掉，其余的保持不变
    sed 's/^#//'  filename
    
    3.删除所有注释行
    sed '/#/d' filename
    
    4.将注释和取消注释放入 crontab
    */30 */22 * * 1-5 sed -i '/0.0.0.1/s/^/#/' /etc/hosts
    */30 */7 * * 1-5 sed -i 's/#0.0.0.1/0.0.0.1/' /etc/hosts 
   
### 取消注释匹配行例子

    注释匹配行
    $ cat httpd.conf 
    #Include httpd-ssl.conf
    #Include httpd-abc.conf
    #Include httpd-def.conf
    
    $ sed  's/^#\(.*\)\(httpd-ssl\)\(.*\)/\1\2\3/' httpd.conf 
    Include httpd-ssl.conf
    #Include httpd-abc.conf
    #Include httpd-def.conf
    
### 参考
- [BSD/macOS Sed vs. GNU Sed vs. the POSIX Sed specification](https://riptutorial.com/sed/topic/9436/bsd-macos-sed-vs--gnu-sed-vs--the-posix-sed-specification)
- [https://www.zhukun.net/archives/6975](https://www.zhukun.net/archives/6975)
