---
title: "Golang json unmarshal 大整数丢失精度问题"
date: 2023-03-26T13:00:00+08:00
draft: false
categories: [Golang]
tags: [json]
---

## 问题介绍
在 Golang 中使用 json Unmarshal 将一个 json 解析为不确定的结构 interface{} 时候，会将数字的接收类型默认设置为 float64。

当原 json 结构中的整数太长，会导致转为 float64 超出精度，导致精度丢失。


## 例子
```
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
)

var jsonstring = `{
	"id": 1130839459432311551,
	"id1": 113083945943231,
	"id2": 12344556, 
	"create_time": "2023-03-22 16:58:38",
	"labels": []
}`

func f1() {
	var data interface{}
	err := json.Unmarshal([]byte(jsonstring), &data)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%+v\n", data)

	dataBytes, err := json.Marshal(data)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%+s\n", string(dataBytes))
}


func main() {
	f1()
}
```

## 解决方法
Golang 中提供了 [json.NewDecode().UseNumber()](https://pkg.go.dev/encoding/json#Decoder.UseNumber) 的方式来解决。

UseNumber causes the Decoder to unmarshal a number into an interface{} as a Number instead of as a float64.

```
func f2() {
	var data interface{}
	decoder := json.NewDecoder(bytes.NewReader([]byte(jsonstring)))
	decoder.UseNumber()
	err := decoder.Decode(&data)
	if err != nil {
		panic(err)
	}

	fmt.Printf("%+v\n", data)

	dataBytes, err := json.Marshal(data)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%+s\n", string(dataBytes))
}
```

## golang unmarshal string into int64, or marshal int64 to string
在和前端交互的时候，在 json 结构中放大整数的 uid，前端使用 javascript json 处理的时候会掉丢失精度，虽然 javascript 有解决方式，但是后端接口将 int64 转为 string 更直接。

在 Golang 中的解决办法，就是在渲染 json 数据之前，将返回结构体中的 int64 类型的字段 json 标签类型设为 string 即可。

```
type Data struct {
   ID   int64  `json:"id,string,omitempty"`
   Name string `json:"name,omitempty"`
}
```


## Ref
- [Decoder.UseNumber](https://pkg.go.dev/encoding/json#Decoder.UseNumber)
- [Some trouble with JSON and large integers displaying in scientific notation](https://www.reddit.com/r/golang/comments/wyl6fl/some_trouble_with_json_and_large_integers/)
- [JSON unmarshaling with long numbers gives floating point number](https://stackoverflow.com/questions/22343083/json-unmarshaling-with-long-numbers-gives-floating-point-number)
- [Decoding JSON using json.Unmarshal vs json.NewDecoder.Decode](https://stackoverflow.com/questions/21197239/decoding-json-using-json-unmarshal-vs-json-newdecoder-decode)
- [深入理解Go Json.Unmarshal精度丢失之谜](https://zhuanlan.zhihu.com/p/467537027)
- [golang json 避免大整数变为浮点数](https://rivers-shall.github.io/2020/06/23/golang-json-%E9%81%BF%E5%85%8D%E5%A4%A7%E6%95%B4%E6%95%B0%E5%8F%98%E4%B8%BA%E6%B5%AE%E7%82%B9%E6%95%B0/)
- https://stackoverflow.com/questions/21151765/cannot-unmarshal-string-into-go-value-of-type-int64
