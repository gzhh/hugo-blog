---
title: "LaTeX学习记录"
date: 2019-10-20T17:23:05+08:00
draft: false
tags: [LaTeX]
categories: [Write]
---
<!--more-->

### 说在前面
最早接触的文字排版是 markdown，当时在 sublime 上面装了个插件就能把 .md 后缀文件生成 pdf，后来陆续读了
一些 LaTeX 排版的电子书籍(当时并不知道是 LaTeX 排版的)，觉得这样的排版很简洁，一直想使用这样的排版工具
写一些文档之类的东西，后来了解到 LaTeX 基于 Knuth 发明的 TeX 演变出来的排版工具，现在笔者就把 LaTeX 的简单使用记录下来，方便
读者和自己查阅。

### LaTeX 文本介绍

- 1.空格

  多个空格和 tab 都被当作一个空格，多个空行被当作一个空行，一个空行表示两个段落之间的分隔

- 2.特殊字符

  特殊字符的显示可以在前面加反斜线 \

- 3.LaTeX 命令

  LaTeX 命令只由反斜线 \ 和大小写字母组成，命令名通过空格、数字或其他非字母字符结束

  LaTeX 忽略命令后面的空格，可以使用命令加上 {} 再加上空格来保留空格

- 4.注释

  单行注释：一行中 % 后面的字符都会被注释

  多行注释：\begin{comment} 和 \end{comment} 直接的文本会被注释

### LaTeX 文本结构

- 1.开始命令

  \documentclass{...}

- 2.加载包命令

  \usepackage{...}

- 3.开始文本主体命令

  \begin{document}

- 4.结束文本主体命令

  \end{document}

例子一：

```
\documentclass{article}
\begin{document}
This is a text.
\end{document}
```

例子二：

```
\documentclass[a4paper,11pt]{article}
\usepackage{latexsym}
\author{H.~Partl}
\title{Minimalism}
\frenchspacing
\begin{document}
\maketitle
\tableofcontents
\section{Start}
Well, and here begins my lovely article.
\section{End}
\ldots{} and here it ends.
\end{document}
```

### LaTeX 布局

- 1.文档布局

  结构：

  \documentclass[options]{class}

  例子：

  \documentclass[11pt,twoside,a4paper]{article}

- 2.包（引入更强大的功能）

  例子：

  \usepackage[options]{package}

- 3.页面角标风格

  LaTeX 预定义了三种页头和页脚结合的页面风格

  \pagestyle{style} 的 style 参数如下

  - 1).plain: prints the page numbers on the bottom of the page, in the middle of the footer. This is the default page style.
  - 2).headings: prints the current chapter heading and the page number in the header on each page, while the footer remains empty. (This is the style used in this document)
  - 3).empty: sets both the header and the footer to be empty.

- 4.大文档项目

  当 LaTeX 文档太大的时候，有两种办法可以把文档分成多个部分

  - 1).\include{filename}

    这个命令可以引入一个新的 filename.tex 文件，会另起一个新页

  - 2).\includeonly{filename,filename,. . . }

    这个命令参数之间不能有空格，可以用在在文档序言中用来只输入一些 \included files



### 结构

- 1.公式

  \begin{equation} ... \end{equation}

- 2.换行和换页

  换行 `\\`和 \newline（但是还在原来的语段）

  避免换行后的换页 `\\*` 

  换页：\newpage

  换行换页扩展：\linebreak[n], \nolinebreak[n], \pagebreak[n] and \nopagebreak[n]

  连字符：\hyphenation{word list} `su\-per`

  将几个单词留在一行展示：`\mbox{text}`

### 符号

- 1.单引号、双引号（Quotation Marks）：可以 `"xxx'x'xxx"`
- 2.连接符号（Dashes and Hyphens）：`-、--、---`
- 3.波浪号（Tilde）：`\~、$\sim$` 居上和局中
- 4.省略（Ellipsis）：`\ldots`
- 5.联结（Ligatures）：`\\`、`\mbox{}`
- 6.音调和特殊符号（Accents）：参考 [lshort 2.4.6](http://www.ctex.org/documents/shredder/src/lshort.pdf )

### 标题、章和节

- 下列section命令可用在article class

```
\section{...}
\paragraph{...}
\subsection{...}
\subparagraph{...}
\subsubsection{...}
\appendix
```

- 这两个section命令可用在book class

  `\part{...} 和 \chapter{...}`

- 目录命令：

  ```
  \tableofcontents
  \maketitle
  \title{...}, \author{...} and optionally \date{...}
  \frontmatter, \mainmatter and \backmatter
  ```

- 参考：

  `\label{marker}, \ref{marker} and \pageref{marker}`

- 脚注：

  `\footnote{footnote text}`

- 强调：

  `\emph{text}`

- 单词边界符号：

  `\begin{environment} text \end{environment}`

  1).列项、枚举、描述 Itemize, Enumerate, and Description

  2).左对齐、右对齐、居中 Flushleft, Flushright, and Center

  3).引用、引证、篇 Quote, Quotation, and Verse

  4).逐字打印 Printing Verbatim

  5).列表 Tabular

  参考 [lshort 2.11](http://www.ctex.org/documents/shredder/src/lshort.pdf )

### 数学公式

下次使用到了再补上

### 特殊用法

后续使用到特殊用法会逐渐补上

### 自定义用法

### 参考
- 1.[latex](https://www.latex-project.org)
- 2.[lshort](http://www.ctex.org/documents/shredder/src/lshort.pdf)
