---
title: Go 传递切片入参的隐藏风险
subtitle: The Hidden Crisis in Passing Slice as Function Argument
catalog: true
date: 2020-11-26
tags: ["Go"]
---

# Go 切片数据结构

Go 中常用的切片（Slice）数据结构是一枚动态数组，提供方便的局部索引功能，切片长度并不固定，并且会在容量不足时自动扩容。

切片实质上是对一个底层数组的**抽象视图**，由 Go 运行时维护。在运行时，切片由如下的`SliceHeader`结构体表示，其中`Data`字段是指向底层数组的指针，`Len`表示当前切片的长度，而`Cap`表示当前切片的容量，也就是`Data`数组的大小。

```go
type SliceHeader struct {
	Data uintptr
	Len  int
	Cap  int
}
```
> 代码示例：Go 切片 Slice 数据结构

# 切片作为函数入参

许多 Go 开发者对传递切片作为函数入参的习惯性认知是：**传递切片，等同于传递指针，函数内部对切片的修改，将会影响到函数外部的切片。**这一习惯性认知在大部分情况下都是正确的。如以下代码所示：在`modify`函数中修改切片，外部`main`函数中的切片受到了影响。

```go
package main

import (
	"fmt"
)

func modify(s []string) {
	for i := 0; i < len(s); i++ {
		s[i] = "b"
	}
	fmt.Println("Inner:", s)
}

func main() {
	s := []string{"a1", "a2"}
	fmt.Println("Before:", s)
	modify(s)
	fmt.Println("After:", s)
}
```
> 代码示例：Go 切片作为函数入参

```plain
Before: [a1 a2]
Inner: [b b]
After: [b b]
```
> 代码示例：Go 切片作为函数入参 - 程序输出

我们对示例代码做一些修改，在内部函数中**触发切片的扩容机制**，事情看起来就非常有趣了：函数内部对切片的修改并没有影响到函数外部的切片。

```diff
func modify(s []string) {
+   s = append(s, "b")
	for i := 0; i < len(s); i++ {
		s[i] = "b"
	}
	fmt.Println("Inner:", s)
}
```
> 代码示例：Go 切片作为函数入参（切片扩容）

```plain
Before: [a1 a2]
Inner: [b b b]
After: [a1 a2]
```
> 代码示例：Go 切片作为函数入参（切片扩容）- 程序输出

# 解释

在 Go 中，函数参数传递机制为**值拷贝**。

将切片作为函数参数传递，实际上是拷贝了`SliceHeader`结构体传入函数，结构体包含一枚指向底层数组的指针，因此在函数内修改切片，操作的底层数组是相同的。

但是如果函数内的切片操作触发了切片扩容（如：使用`append`追加元素），Go 运行时会为切片分配一块新的内存空间并将原切片的所有元素拷贝过去，函数内部切片的底层数组指针指向了新分配的内存空间，而函数外部切片底层数组指针仍指向分配前的地址空间，由此出现了内外切片不一致的有趣情形。

我们可以通过一个简单的方式验证扩容前后切片的变化，即：打印出扩容前后切片数组的地址。而我们在函数外部打印切片的数组地址，会发现其与扩容前地址相同。

```diff
func modify(s []string) {
+   fmt.Printf("Before grow slice, &s[0]: %p\n", &s[0])
    s = append(s, "b")
	for i := 0; i < len(s); i++ {
		s[i] = "b"
	}
    fmt.Println("Inner:", s)
+   fmt.Printf("After grow slice, &s[0]: %p\n", &s[0])
}
```
> 代码示例：打印扩容前后切片数组地址（首元素地址）

# 建议

理解了 Go 中切片作为函数参数传递的内部原理后，如何在代码中正确运用切片传参也就比较明晰了。

- **操作不涉及切片容量变化，直接传递切片。**

- **操作涉及切片容量变化，且需要反馈给调用方，传递切片指针。**

# 参考资料

- Arrays, slices (and strings): The mechanics of 'append'：https://blog.golang.org/slices

- Go 语言设计与实现：https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-array-and-slice/

<!-- EOF -->
