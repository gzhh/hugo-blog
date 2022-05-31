---
title: "Lua 语言入门（二）"
date: 2019-10-06T09:22:54+08:00
draft: false
tags: [Lua]
categories: [Lua]
---
<!--more-->

### 说在前面
这一篇接着 [Lua 语言入门（一）](http://blog.gzhh.tech/2019/09/29/play-lua-first/) 继续写，主要是 lua 稍微高级一点的编程方式。

- 1.Closure 闭包

  ```lua
  -- 普通函数
  function foo (x) return 2*x end
  print(foo(1))
  
  -- 将函数复制给一个变量
  bar = function (x) return 3*x end
  print(bar(1))
  
  -- 匿名函数
  network = {
      {name = "grauna",  IP = "210.26.30.34"},
      {name = "arraial", IP = "210.26.30.23"},
      {name = "lua",     IP = "210.26.23.12"},
      {name = "derain",  IP = "210.26.23.20"},
  }
  table.sort(network, function (a, b) return (a.name > b.name) end)
  for _, item in ipairs(network) do
      print("name:", item.name, "  ", "IP:", item.IP)
  end
  
  
  -- 非全局函数1
  --[[
  Lib = {
      foo = function (x,y) return x + y end,
      goo = function (x,y) return x - y end
  }
  --]]
  --[[
  Lib = {}
  function Lib.foo (x,y) return x + y end
  function Lib.goo (x,y) return x - y end
  --]]
  Lib = {}
  Lib.foo = function (x,y) return x + y end
  Lib.bar = function (x,y) return x - y end
  print(Lib.foo(2, 3), Lib.bar(2, 3))
  
  -- 非全局函数2
  -- local function fact () ... end 这样会出现递归调用全局函数的错误
  local fact
  fact = function (n)
      if n == 0 then return 1
      else return n*fact(n-1)
      end
  end
  print(fact(5))
  
  
  -- a closure is a function that accesses one or more local variables from its enclosing environment.
  -- 闭包是一个可以从它自身的闭合环境中访问一个或多个局部变量的函数
  -- 闭包(在函数中写另一个闭合函数) 
  -- 词法作用域1
  names = {"Peter", "Paul", "Mary"}
  grades = {Mary = 10, Paul = 7, Peter = 8}
  table.sort(names, function (n1, n2)
      return grades[n1] > grades[n2]
  end)
  for key, value in pairs(grades) do
      print("name:", key, "  ", "grade:", value)
  end
  
  -- 词法作用域2
  names = {"Peter", "Paul", "Mary"}
  grades = {Mary = 10, Paul = 7, Peter = 8}
  function sortbygrade (names, grades)
      table.sort(names, function (n1, n2)
          return grades[n1] > grades[n2]
      end)
  end
  sortbygrade(names, grades)
  for key, value in pairs(grades) do
      print("name:", key, "  ", "grade:", value)
  end
  
  -- 词法作用域3
  function newCounter ()
      local count = 0
      return function () -- anonymous function
          count = count + 1
          return count
      end
  end
  c1 = newCounter()
  print(c1()) --> 1
  print(c1()) --> 2
  c2 = newCounter()
  print(c2())  --> 1
  print(c1())  --> 3
  print(c2())  --> 2
  -- c1 和 c2 是不同的闭包
  ```
  
- 2.模式匹配

  ```lua
  -- string.find
  s = "hello world"
  i, j = string.find(s, "hello")
  print(i, j)                         --> 1 5
  print(string.sub(s, i, j))          --> hello
  print(string.find(s, "world"))      --> 7 11
  i, j = string.find(s, "l")
  print(i, j)                         --> 3 3
  print(string.find(s, "lll"))        --> nil
  
  -- 第三个参数表示从哪个位置开始查找
  -- 第四个参数表示是否是文本查找，表示第二个参数中的字符串没有其他特别的意思
  i, j = string.find("a [word]", "[", 1, true)   --> 3   3
  
  
  -- string.match
  print(string.match("hello world", "hello")) --> hello
  date = "Today is 17/7/1990"
  d = string.match(date, "%d+/%d+/%d")
  print(d) --> 17/7/1990
  
  
  -- string.gsub
  s = string.gsub("Lua is cute", "cute", "great")
  print(s)         --> Lua is great
  s = string.gsub("all lii", "l", "x")
  print(s)         --> axx xii
  s = string.gsub("Lua is great", "Sol", "Sun")
  print(s)         --> Lua is great
  
  s = string.gsub("all lii", "l", "x", 1)
  print(s)          --> axl lii
  s = string.gsub("all lii", "l", "x", 2)
  print(s)          --> axx lii
  
  
  -- string.gmatch
  s = "some string"
  words = {}
  -- {"some", "string"}
  for w in string.gmatch(s, "%a+") do
      words[#words + 1] = w
  end
  
  
  --[[
  -- 正则表达式中预定义字符所表示的意思
  .               all characters
  %a              letters
  %c              control characters
  %d              digits
  %g              printable characters except spaces
  %l              lower-case letters
  %p              punctuation characters
  %s              space characters
  %u              upper-case letters
  %w              alphanumeric characters
  %x              hexadecimal digits
  
  -- 魔法字符
  ( ) . % + - * ? [  ] ^ $
  ]]
  ```

- 3.日期与时间

  ```lua
  -- os.time
  os.time({year=2015, month=8, day=15, hour=12, min=45, sec=20})
  
  -- os.date
  os.date("%d/%m/%Y", 1569752320)
  ```

- 4.Iterator 迭代器 和 Generic 范型

  ```lua
  -- 范型可以简化迭代器的繁琐
  -- iterator
  function values (t)
      local i = 0
      return function () i = i + 1; return t[i] end
  end
  
  t = {10, 20, 30}
  iter = values(t) -- iter is a closure
  while true do
      local element = iter()
      if element == nil then break end
      print(element)
  end
  
  
  -- generic
  t = {10, 20, 30}
  for element in values(t) do
      print(element)
  end
  
  
  -- 使用 iterator 来遍历从终端输入的所有单词
  function allwords ()
      local line = io.read()
      local pos = 1
      return function ()
          while line do
              local w, e = string.match(line, "(%w+)()", pos)
              if w then
                  pos = e
                  return w
              else
                  line = io.read()
                  pos = 1
              end
          end
          return nil
      end
  end
  
  for word in allwords() do
      print(word)
  end
  ```

