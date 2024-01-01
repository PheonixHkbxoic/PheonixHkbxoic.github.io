---
title: "Go: string"
date: 2024-01-01
draft: false

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
# disableShare: true    # 底部不显示分享栏

description: ""
lastMod: 2024-01-01
cover:
    hidden: false
    image: "post/golang-string.jpeg"
weight: 1
categories: [golang]

tags: [字符串,string]
---

## 字符串

### 声明与赋值
```golang
var a string
var a string = "x"
var a = "x"
a := "x\"x"
a := `x"x`
```

### 数据结构
```golang
type StringHeader struct {
	Data uintptr
	Len  int
}
```
* Data是数据的地址，Len是数据的长度
* uintptr不参与gc
* 本质上是字节数组
* 字符串是不可变的
* 字符串的赋值并不是拷贝底层的字符串数组，而是数组指针和长度字段的拷贝
```golang
a := "hello,world"
b := a

fmt.Println(a)
fmt.Println(b)
fmt.Printf("%x, %x\n", &a, &b)

aptr := (*reflect.StringHeader) (unsafe.Pointer(&a))
bptr := (*reflect.StringHeader) (unsafe.Pointer(&b))

fmt.Println("a data ptr:", unsafe.Pointer(aptr.Data))
fmt.Println("b data ptr:", unsafe.Pointer(bptr.Data))
```
```log
// a,b的字符串一样
hello,world
hello,world
// a,b的指针不一样，显然b是对a的StringHeader复制了一份
c0000543d0, c0000543e0

// a,b的Data一样，指向的数据是同一个
a data ptr: 0xfda3cf
b data ptr: 0xfda3cf

// 所以 字符串的赋值是浅拷贝
```
* 重新赋值 不会改变原来变量的地址，即StringHeader还是同一个，但Data已指向了新的数据地址
* 其他操作都是深拷贝，如传参，切分，拼接，转换

## 与字节数组、字符数组的相互转换
字符串转换成字节数组
> bs := []byte(a)

字节数组转换成字符串
> b := string(bs)

需要说明的是 字节数组并不等于字符数组，因为非ASCII字符一般会占多个字节  
所以字节数并不一定等于字符数
字符串转换成字符数组
> cs := []rune(a)

字符数组转换成字符串
> c := string(cs)

## 字符串拼接
* 使用+直接拼接
* 使用Strings.Builder
* 使用strings.Join方法

## 字符串比较
* == 区分大小写，比Compare快
* strings.Compare 区分大小写
* strings.EqualFold 不区分大小写

