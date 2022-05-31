---
title: "Lua 语言入门（一）"
date: 2019-09-29T20:20:36+08:00
draft: false
tags: [Lua]
categories: [Lua]
---
<!--more-->

### 说在前面
由于前段时间使用 Redis 相关技术接触到了 Lua 脚本用于提升 Redis 性能，再加上之前对 Nginx
可以使用 Lua 技术进行扩展非常感兴趣，所以这段时间抽空学习了下Lua，现在把自己初学 Lua 的
经历记录下来，供自己与各位同行参考和交流。


- 1.代码块
   进入交互模式
   lua
   执行 lua 脚本后进入交互模式
   Lua -i test.lua
   交互模式中执行 lua 脚本
   dofile("test.lua")

- 2.词法惯例
   标识符：大小写敏感
   注释：单行--，多行--[[]]--，只有第一行和最后一行被注释---[[]]--
   分号：两条语句之间可写可不写

- 3.全局变量
   默认 nil

- 4.变量和类型
   由于 Lua 是动态语言，所以变量没有类型，但是每个值都有它们自己的类型
   八个基本类型：
   ```
   value:
   > type(nil)			  --> nil
   > type(true)           --> boolean
   > type(10.4 * 3)       --> number
   > type("Hello world")  --> string
   > type(io.stdin)       --> userdata
   > type(print)          --> function
   > type(type)           --> function
   > type({})             --> table
   > type(type(X))        --> string
   ```

- 5.基本类型
   Boolean: 在条件判断中 false 和 nil 被当作 false，其他都被当作 ture。lua 的三元操作符和别的语言不一样

- 6.命令行
   lua 					进入交互模式
   lua -i test.lua 	    执行脚本后进入交互模式
   lua -e "..."		    执行 lua 代码并在终端打印出来
   lua -l libname	    加载一个 library

- 7.数字类型
   值相等的两个整数和浮点数之间是相等的，可以用math.type来判断数字的确切类型。
   1 == 1.0 // true
   type(1) // number
   type(1.0) // number
   math.type(1) // integer
   math.type(1.0) // float

- 8.算术操作符
   加 减 乘 除 整除 取余 + - * / // %
   9 // 2 --> 4
   -9 // 2 --> -5
   a % b == a - ((a // b) * b)

- 9.关系操作符
   lt gt le ge eq nee < > <= >= == ~=
   使用 == 或 ~= 时，如果两个值是不同的类型，lua 就认为它们是不想等的，否则，
   lua 才会根据类型比较它们的值。

- 10.math 库
   数学函数、三角函数、对数函数等
   随机数：
   math.randomseed(os.time())
   math.random()

- 11.字符串类型
    a = "hello "
    b = 'world'
    字符串长度 #a #"hello world"
    字符串拼接 a .. b
    长字符串：
    html = [[
    ...
    ]]

- 12.number 和 string 之间的转换
    隐式类型转换：
    10..20 --> 1020 类型为 string
    "10" + 1 --> 11.0 类型为 number
    显式类型转换：
    tonumber(str) // 如果str可以解释为number值，返回integers或floats，否则返回 nil

- 13.字符串库
    ```
    > string.rep("abc", 3)				--> abcabcabc
    > string.reverse("A Long Line!")	--> !eniL gnoL A
    > string.lower("A Long Line!")		--> a long line!
    > string.upper("A Long Line!")		--> A LONG LINE!
    > string.sub(s, i, j)				--> from the ith to the jth character
    
    print(string.char(97))						--> a
    i = 99; print(string.char(i, i+1, i+2))		--> cde
    print(string.byte("abc"))					--> 97
    print(string.byte("abc", 2))				--> 98
    print(string.byte("abc", -1))				--> 99
    
    string.format("x = %d  y = %d", 10, 20)		--> x = 10  y = 20
    string.format("x = %f", 200)				--> x = 200.000000
    > tag, title = "h1", "a title"
    > string.format("<%s>%s</%s>", tag, title, tag)
    --> <h1>a title</h1>
    
    > string.find("hello world", "wor")   --> 7   9
    > string.find("hello world", "war")   --> nil
    
    > string.gsub("hello world", "l", ".")		--> he..o wor.d    3
    > string.gsub("hello world", "ll", "..")	--> he..o world    1
    > string.gsub("hello world", "a", ".")		--> hello world    0
    ```

- 14.utf-8 支持

    ```
    utf8.len("résumé") --> 2
    utf8.char(114, 233, 115, 117, 109, 233) --> résumé
    utf8.codepoint("résumé", 6, 7)          --> 109    233
    
    
    > s = "Nähdään"
    > utf8.codepoint(s, utf8.offset(s, 5))  --> 228
    > utf8.char(228)						--> ä
    
    for i, c in utf8.codes("Ação") do
    	print(i, c)
    end
      --> 1    65
      --> 2    231
      --> 4    227
      --> 6    111
    ```

- 15.Tables

    ```
    可以用 tables 表示 arrays, sets, structs, records
    // 数组
    a = {}
    a["x"] = 10
    b = a						-- 'b' refers to the same table as 'a'
    b["x"]
    b["x"] = 20
    a["x"]						-- 20
    a = nil						-- only 'b' still refers to the table
    b = nil						-- no references left to the table
    
    // 结构体
    > a = {}					-- empty table
    > a.x = 10				    -- same as a["x"] = 10
    > a.x						-- same as a["x"]
    > a.y						-- same as a["y"]
    
    // table constructors (可以不同形式，不同类型，可以多层嵌套)
    days = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}
    print(days[4])  --> Wednesday
    
    a = {x = 10, y = 20}
    a = {}; a.x = 10; a.y = 20
    
    
    a = {[1] = "red", [2] = "green", [3] = "blue",}
    
    
    // array or list
    -- read 10 lines, storing them in a table
    a = {}
    for i = 1, 10 do
    	a[i] = io.read()
    end
     -- print the lines, from 1 to #a
    for i = 1, #a do
      print(a[i])
    end
    
    // table 遍历
    // key–value pairs
    t = {10, print, x = 12, k = "hi"}
    for k, v in pairs(t) do
    	print(k, v)
    end
    
    // list
    t = {10, print, 12, "hi"}
    for k, v in ipairs(t) do
    	print(k, v)
    end
    for k = 1, #t do
    	print(k, t[k])
    end
    ```

- 16.table 库

    ```
    t = {}
    for line in io.lines() do
    	table.insert(t, line)
    end
    print(#t)         --> (number of lines read)
    
    table.insert(t, x)
    table.remove(t)
    table.move(a, f, e, t)	--> 将索引 f 至 e 对应的元素移至 t 
    ```

- 17.函数

    ```
    // 函数例子
    -- add the elements of sequence 'a'
    function add (a)
    	local sum = 0
    	for i = 1, #a do
    		sum = sum + a[i]
    	end
    	return sum
    end
    
    function f (a, b) print(a, b) end
    f()				--> nil    nil
    f(3)			--> 3      nil
    f(3, 4)			--> 3      4
    f(3, 4, 5)		--> 3      4      (5 is discarded)
    
    
    // 多个返回值
    function maximum (a)
      local mi = 1					-- index of the maximum value
      local m = a[mi]				-- maximum value
      for i = 1, #a do
        if a[i] > m then
          mi = i; m = a[i]
    	end end
      return m, mi					-- return the maximum and its index
    end
    
    function foo0 () end						-- returns no results
    function foo1 () return "a" end				-- returns 1 result
    function foo2 () return "a", "b" end		-- returns 2 results
    
    
    // 可变函数
    function add (...)
      local s = 0
      for _, v in ipairs{...} do
    s=s+ v end
    return s end
    print(add(3, 4, 10, 25, 12)) --> 54
    
    function nonils (...)
      local arg = table.pack(...)
      for i = 1, arg.n do
      	if arg[i] == nil then return false end
      end
      return true
    end
    print(nonils(2,3,nil))	--> false
    print(nonils(2,3))		--> true
    print(nonils())			--> true
    print(nonils(nil))	    --> false
    
    print(select(1, "a", "b", "c"))		--> a    b    c
    print(select(2, "a", "b", "c"))		--> b    c
    print(select(3, "a", "b", "c"))		--> c
    print(select("#", "a", "b", "c"))	--> 3
    
    print(table.unpack{10,20,30})    --> 10   20   30
    a,b = table.unpack{10,20,30}     -- a=10, b=20, 30 is discarded
    ```
    
- 18.IO

    ```
    一、简单io
    io.write(str)
    io.write(str1, str2, str3)		--> 连续打印几个字符串
    
    io.read()
    --[[
    "a" 读整个文件
    "l" 读下一行(舍掉新行)
    "L" 读下一行(保留新行)
    "n" 读一个数字
    num 读num个字符作为一个字符串(字符不够的就按输入字符计，字符过多的则舍掉多余的字符)
    --]]
    
    --[[
    例子：排序一个文件
    local lines = {}
    
    -- read the lines lin table 'lines'
    for line in io.lines() do
        lines[#lines + 1] = line
    end
    
    -- sort
    table.sort(lines)
    
    -- write all the lines
    for _, l in ipairs(lines) do
        io.write(l, "\n")
    end
    --]]
    
    --[[
    for count = 1, math.huge do
        local line = io.read("L")
        if line == nil then break end
        io.write(string.format("%6d  ", count), line)
    end
    --]]
    
    --[[
    local count = 0
    for line in io.lines() do
        count = count + 1
        io.write(string.format("%6d  ", count), line, "\n")
    end
    --]]
    
    --[[
    while true do
        local block = io.read(2^13)          -- block size is 8K
        if not block then break end
        io.write(block)
    end
    --]]
    
    --[[
    while true do
        local n1, n2, n3 = io.read("n", "n", "n")
        if not n1 then break end
        print(math.max(n1, n2, n3))
    end
    --]]
    
    
    二、完全io
    file = io.open (filename [, mode])
    --[[
    r 只读
    w 只写
    a 附加写
    r+ 读写(文件必须存在)
    w+ 读写(文件不存在则可以自己创建)
    a+ 读写(附加写)
    b 二进制模式
    --]]
    
    local f = assert(io.open(filename, mode))		--> 使用assert检查错误
    
    local f = assert(io.open(filename, "r"))
    local t = f:read("a")
    f:close()
    
    预定义 C streams
    io.stdin, io.stdout, io.stderr
    t = io.stdin:read()
    io.stdout:write(t)
    -- io.stderr:write('this is error stream')
    
    输入输出的几种方式
    io.read()
    io.input()
    io.stdin:read()
    
    io.write('hello world')
    io.output():write('hello world')
    io.stdout:write('hello world')
    
    
    三、其他文件操作
    io.flush() -- 把当前输出流刷新到磁盘
    f:flush() -- 刷流f到磁盘
    f:seek() -- get或set在文件中一个流的位置
    ```

- 19.os 函数

    ```
    os.rename -- 重命名文件
    os.remove -- 删除文件
    os.getenv("HOME") -- 环境变量
    os.execute -- 执行系统命令
    
    function createDir (dirname)
    	os.execute("mkdir " .. dirname)
    end
    ```

- 20.流程控制结构

    ```
    if then else
    
    while
    
    repeat until
    
    for 有两种形式
    1.Numerical for
    for i = 1, #a do
    	something
    end
    2.Generic for
    -- pairs, ipairs, io.lines()
    for k, v in ipairs(t) do
    	something
    end
    
    break, return, goto
    
    ```
