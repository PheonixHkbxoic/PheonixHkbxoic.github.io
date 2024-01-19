---
title: "Go: 数组"
date: 2024-01-03
draft: false

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
summary: "数组是内存中一片连续的区域，固定长度，元素类型相同，是值类型，大数组值参等值传递效率低
编译时进行类型检查 ，索引越界检查"
lastMod: 2024-01-03
cover:
    hidden: false
    image: "post/golang-array.jpeg"
weight: 30
categories: [golang]

tags: [golang,数组,Array]
---

## 数组

数组是存放在连续内存空间上的相同类型数据的集合。**查询简单，增加和删除困难**。

数组可以通过下标快速访问数组元素。但是因为数组在内存空间的地址是连续的，所以我们在删除或者增添元素的时候，就难免要移动其他元素的地址。数组的元素是不能删的，只能覆盖。

二维数组在内存的空间地址也是连续的

### 一、声明与赋值

```go
var a [3]int
var b = [3]int{1,2,3}
c := [3]int{1,2,3}
d := [...]int{1,2,3}
```

* 数组是**内存中一片连续的区域**，跟Slice，Map等结构体完全不一样

* 需要指定长度，是个常量，不能动态指定，[...]方式在编译时自动推断

* 元素类型相同

* 是值类型，类型与长度都相同时才被认为两个数组是同类型

### 二、编译时检测、内存分配

数组在编译阶段最终被解析为`types.Array`类型，包含元素类型`Elem`和数组长度`Bound`

```go
// Array contains Type fields specific to array types.
type Array struct {
    Elem  *Type // element type
    Bound int64 // number of elements; <0 if unknown yet
}
```

并会进行类型检查 ，索引越界检查

### 三、运行时

1. 当编译时无法判断是否越界时，SSA生成 ***IsInBounds*** 越界检查指令，若越界，触发 ***PanicBounds*** 指令执行 `runtime.panicIndex`

2. 运行时通过`newarray()`函数对数组内存进行分配，如果数组大小不超过`32kb`则会直接分配到栈上， 否则分配到堆区内存

### 四、注意

`Go`中的传值方式是按值传递，这意味着给变量赋值、给函数传参时，都是直接拷贝一个副本然后将副本赋值给对方的。这样的拷贝方式意味着：

- 如果数据结构体积庞大，则要完整拷贝一个数据结构副本时效率会很低
- 函数内部修改数据结构时，只能在函数内部生效，函数一退出就失效了，因为它修改的是副本对象

### 五、参考

[golang数组内存分配原理_Golang_脚本之家](https://www.jb51.net/article/253391.htm)
