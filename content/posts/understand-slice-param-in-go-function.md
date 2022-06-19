---
title: "理解Go函数中的切片传参"
date: 2022-06-19T17:00:00+08:00
draft: false
tags: [Slice]
categories: [Golang]
---
<!--more-->

## 切片（Slice）
我们知道在 Go 中切片的底层数据结构是一个结构体，结构体中有一个指针 array 指向一个底层数组，len 表示当前切片的长度，cap 表示当前切片的容量。

```
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

## 函数使用切片传参涉及到的拷贝及扩容

### 例一：切片传参且函数内只有读操作
```
package main

import "fmt"

func test(nums []int) {
	fmt.Printf("\ninner before address: %p\n", nums)
	fmt.Println("inner before len: ", len(nums))
	for _, num := range nums {
		_ = num
	}
	fmt.Printf("inner after address: %p\n", nums)
	fmt.Println("inner after len: ", len(nums))
}

func main() {
	nums := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Println("outer before print: ", nums)
	fmt.Printf("outer before address: %p\n", nums)
	fmt.Println("outer before len: ", len(nums))

	test(nums)

	fmt.Println("\nouter after print: ", nums)
	fmt.Printf("outer after address: %p\n", nums)
	fmt.Println("outer after len: ", len(nums))
}
```

输入结果如下：
```
outer before print:  [0 1 2 3 4 5 6 7 8 9]
outer before address: 0xc00012c000
outer before len:  10

inner before address: 0xc00012c000
inner before len:  10
inner after address: 0xc00012c000
inner after len:  10

outer after print:  [0 1 2 3 4 5 6 7 8 9]
outer after address: 0xc00012c000
outer after len:  10
```

输出解释：
1. 将切片按值传递给函数，函数内只读切片，不涉及到对切片的修改操作。
2. 从输出可以看到函数内和函数外切片指向的底层数组的内存地址都是一样的。
3. 并且可以看到主函数调用 test 函数前后打印的原数组的值都是不变的。


### 例二：切片传参且函数有修改操作（不涉及扩容）
```
func test(nums []int) {
	fmt.Printf("inner before address: %p\n", nums)
	fmt.Println("inner before len: ", len(nums))
	for i, num := range nums {
		nums[i] = num + 1
	}
	fmt.Printf("inner after address: %p\n", nums)
	fmt.Println("inner after len: ", len(nums))
}
```

输入结果如下：
```
outer before print:  [0 1 2 3 4 5 6 7 8 9]
outer before address: 0xc00001c050
outer before len:  10

inner before address: 0xc00001c050
inner before len:  10
inner after address: 0xc00001c050
inner after len:  10

outer after print:  [1 2 3 4 5 6 7 8 9 10]
outer after address: 0xc00001c050
outer after len:  10
```

输出解释：
1. 将切片按值传递给函数，函数内对切片的各元素执行加一操作，但不涉及扩容。
2. 从输出可以看到函数内和函数外切片指向的底层数组的内存地址都是一样的，即不涉及扩容。
3. 但可以看到主函数调用 test 函数前后打印的原数组的值都是不一致的，函数会修改原切片的值。


### 例三：切片传参且函数有修改操作（涉及扩容）
```
func test(nums []int) {
	fmt.Printf("inner before address %p\n", nums)
	fmt.Println("inner before len: ", len(nums))
	for i, num := range nums {
		nums[i] = num + 1
	}
	nums = append(nums, 100)
	fmt.Printf("inner after address: %p\n", nums)
	fmt.Println("inner after len: ", len(nums))
}
```

输入结果如下：
```
outer before print:  [0 1 2 3 4 5 6 7 8 9]
outer before address: 0xc00001c050
outer before len:  10

inner before address 0xc00001c050
inner before len:  10
inner after address: 0xc000108000
inner after len:  11

outer after print:  [1 2 3 4 5 6 7 8 9 10]
outer after address: 0xc00001c050
outer after len:  10
```

输出解释：
1. 将切片按值传递给函数，函数内对切片的各元素执行加一操作，并且最后向切片 append 一个数，涉及到扩容。
2. 从输出可以看到函数内切片 appen 操作的时候内存发生了变化，会在内存新分配一块空间出来，即发生扩容。
3. 且函数外部的切片还是指向原来切片的底层数组，没有发生变化；但是原切片的值还是发生了改变，函数内部对切片内元素执行加一操作由于是在扩容之前，所以影响到了原切片。


### 例四：切片指针传参且函数有修改操作（不涉及扩容）
```
func test(nums *[]int) {
	fmt.Printf("\ninner before address: %p\n", nums)
	fmt.Println("inner before len: ", len(*nums))
	for i := 0; i < len(*nums); i++ {
		(*nums)[i] = (*nums)[i] + 1
	}
	fmt.Printf("inner after address: %p\n", nums)
	fmt.Println("inner after len: ", len(*nums))
}

func main() {
	nums := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Println("outer before print: ", nums)
	fmt.Printf("outer before address: %p\n", nums)
	fmt.Println("outer before len: ", len(nums))

	test(&nums)

	fmt.Println("\nouter after print: ", nums)
	fmt.Printf("outer after address: %p\n", nums)
	fmt.Println("outer after len: ", len(nums))
}
```

输入结果如下：
```
outer before print:  [0 1 2 3 4 5 6 7 8 9]
outer before address: 0xc00001c050
outer before len:  10

inner before address: 0xc00000c030
inner before len:  10
inner after address: 0xc00000c030
inner after len:  10

outer after print:  [1 2 3 4 5 6 7 8 9 10]
outer after address: 0xc00001c050
```

输出解释：
1. 将切片指针传递给函数，函数内对切片的各元素执行加一操作，不涉及扩容。
2. 由于传参方式是切片指针，函数内部会复制指针，也就是会同时出现两个指针指向原有的内存空间。
3. 所以主函数调用 test 函数前后的原切片底层数组指向的内存地址不变，但切片里的各元素值发生了加一变化。


### 例五：切片指针传参且函数有修改操作（涉及扩容）
```
func test(nums *[]int) {
	fmt.Printf("\ninner before address: %p\n", nums)
	fmt.Println("inner before len: ", len(*nums))
	for i := 0; i < len(*nums); i++ {
		(*nums)[i] = (*nums)[i] + 1
	}
	*nums = append(*nums, 100)
	fmt.Printf("inner after address: %p\n", nums)
	fmt.Println("inner after len: ", len(*nums))
}

```

输入结果如下：
```
outer before print:  [0 1 2 3 4 5 6 7 8 9]
outer before address: 0xc0000b6000
outer before len:  10

inner before address: 0xc0000a4018
inner before len:  10
inner after address: 0xc0000a4018
inner after len:  11

outer after print:  [1 2 3 4 5 6 7 8 9 10 100]
outer after address: 0xc0000ba000
outer after len:  11
```

输出解释：
1. 将切片指针传递给函数，函数内对切片的各元素执行加一操作，并且最后向切片 append 一个数。
2. 由于传参方式是切片指针，函数内部会复制指针，也就是会同时出现两个指针指向原有的内存空间。
3. 由于函数内部有一个 append 操作，所以需要分配一块新的内存空间用来扩容。
4. 执行 test 函数后，主函数 nums 切片底层数组指向的内存地址发生了改变，指向了新分配的内存地址；且元素内部的值都发生了加一变化（除了最后 append 的那个元素）。


## 参考
https://halfrost.com/go_slice/
