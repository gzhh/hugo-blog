---
title: "【VIM】vim常用操作"
date: 2018-03-23T09:22:30+08:00
draft: false
tags: [Vim]
categories: [Linux]
---
<!--more-->

### 1.一般模式(默认模式)：
Ctrl+f 	向下移动一页
Ctrl+b  向上移动一页
0或Home	移到这一行的最前面字符处
$或End	移到这一行的最后面字符处
G		移到这个文件的最后一行
nG		移到这个文件的第n行
gg		移到这个文件的第一行
n+Enter 向下移动n行

查找
/word   向下寻找一个名称为word的字符串
?word	向上寻找一个名称为word的字符串
n 		向下重复前一个查找
N 		向上重复前一个查找

替换
:n1,n2s/word1/word2/g 		在第n1行与n2行直接寻找word1这个字符串，并将该字符串替换为word2
:n1,n2s/word1/word2/gc		逐个询问是否替换
:1,$s/word1/word2/g 		第一行到最后一行
;1,$s/word1/word2/gc 		逐个询问

第一行到最后一行
:1,$s/search/replace/g
:%s/search/replace/g

指定行m,n
:m,ns/search/replace/g

当前行到x行
:.,.+xs/search/replace/g

当前行到最后一行
:.,$s/search/replace/g

删除
x,X 	向后向前删除字符，(Del, Backspace)
nx
dd		删除光标所在的那一行
ndd		往下删除n行

复制
yy		复制光标所在的那一行
nyy		向下复制n行

粘贴
p,p     向上向下粘贴

复原前一个操作
u

重复上一个操作
Ctrl+r或.


### 2.编辑模式
一般模式按i,I,o,O,a,A,r,R其中一个可进入编辑模式，编辑模式按Esc可退回一般模式
i,I 	光标所在行插入
a,A 	光标下一个字符插入，光标行之后一个字符插入
o,O 	下一行插入，上一行插入
r,R 	替换光标所在字符


### 3.命令行模式
一般模式按:,/,?其中一个可进入命令行模式,命令行模式按Esc可退回一般模式
!		强制
:w 		将数据写入硬盘
:q 		离开vi
:wq 	保存后离开
:w [filename] 		类似于另存文件
:n1,n2 w [filename] n1到n2行数据存入filename

:set nu 	显示行号
:set nonu 	取消行号


### 4.vim的使用
vim filename 	进入vim
Ctrl+z 		 	将vim丢到后台执行，返回命令行

1).选择块
v 			字符选择，选到反白
V 			行选择
Ctrl+v 		块选择
y 			将反白复制
p 			粘贴
d 			将反白删除

2).多文件编辑
:n 			编辑下一个
:N 			    上
:files 		列出目前这个vim的打开的所有文件

3).多窗口
:sp [filename] 		打开一个新窗口
Ctrl+w+j或Ctrl+w+↓  窗口向下切换
Ctrl+w+k或Ctrl+w+↑        上
Ctrl+w+q 	        离开

4).vim环境设置与记录： ~/.vimrc, ~/.viminfo
