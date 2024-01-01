---
Title: "GO:变量声明与赋值"
Date: 2023-12-20 22:15:37
draft: false
description: ""
lastMod: 2024-1-1
cover:
    hidden: false
    image: "post/golang-variable.jpeg"
weight: 1

categories: [golang]

tags: [变量, 声明, 赋值,常量,零值,默认值]

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
# disableShare: true    # 底部不显示分享栏

---

## 一、变量声明与赋值

Go 中命名规则是，名称以字母或下划线开头，后面可跟任意数量的字符、数字和下划线，字符区分大小写，名称本身没有长度限制，但是 Go 的编程风格倾向于使用短名称，特别是局部变量.

Go 中有 25 个关键字，这些关键字不可用来命名。

|          |             |        |           |        |
| -------- | ----------- | ------ | --------- | ------ |
| break    | default     | func   | interface | select |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

### 单变量声明与赋值

```golang
// 声明 默认值为0
var a int

// 声明并赋值
var c int = 3
var b = 1

// 单独赋值
a = 2

// 不能重复声明

// 简短声明：声明并赋值 简化形式
d := 4
fmt.Println(a, b, c, d)
```

### 多变量声明与赋值

```golang
// 多变量 声明
var aa, bb int
fmt.Println(aa, bb)

// 多变量 声明并赋值
var cc, dd int = 1, 2
var cc2, dd2 = 1, 2
fmt.Println(cc, dd, cc2, dd2)

// 多变量 声明并赋值 简化形式
ee, ff := 1, 2
fmt.Println(ee, ff)
// 只要有一个变量没声明过 就不会报错
ee, fff := 1, 2
fmt.Println(ee, fff)
// 全部都声明过 报错
//ee, ff := 1, 2

// 多变量 但不同类型
var (
    aaa int
    bbb string
    aaa2 = 3
    bbb2 = ""
)
fmt.Println(aaa, bbb, aaa2, bbb2)

// 多变量 但不同类型 简化形式
ccc, ddd := 3, ""
fmt.Println(ccc, ddd)
```

### 全局变量与局部变量

```golang
// 全局变量 只声明赋值 可以不使用 不会报错
var x int

func main() {
    // 局部变量 只声明 可以会报错
    //var xx int = 4

}
```

### 赋值类型不同时

```golang
// 赋值类型不同时 可能会强转，不能强转则会报错
var mi int = 5.0
fmt.Println(mi)
// '"5.0"' (type string) cannot be represented by the type int
//var mii int = "5.0"
//fmt.Println(mii)

// 可以自己转
var mx int = int(5.0)
mii, _ := strconv.Atoi("5.0")
fmt.Println(mx, mii)
```


### 常量
```golang
const a = 5
const b float32 = 5.0
const c = "abc"

const (
    x = true
    y = complex(3.0, 2.0)
    z uint = 5
    m = iota
)
```

- 常量类型可以省略，由编译器自动推断，是在编译时预告处理的，而不是在运行时
- 常量只能是基本数据类型，包括整数、浮点数、布尔值和字符串
- 常量不允许被重复定义


## 二、常见类型
布尔bool, 字符串string, 整数(integer)，浮点数(float)，复数(complex)  
接口interface{}，结构体struct，数组(array)，切片(slice)，通道chan，字典map，函数func()  
括号中的类型并不存在，只是一个统称  


整数：byte, int(默认), int8, int16, int32, int64, uint, uint8, uint16, uint32, uint64, rune, uintptr  
浮点数：float32(默认), float64  
复数：complex64, complex128  

注: 
- byte=uint8, rune=int32, 而int、uint、uintptr的大小是不确定的，取决取系统，可能为32或64位
- uintptr只是一个无符号整形，一般是地址值，不是指针，不参与gc，需要通过uintptr(unsafe.Pointer(指针变量))强转得到
- unsafe.Pointer指针对象，参与gc，可通过unsafe.Pointer(指针变量)得到
- any任意类型，即interface{}接口

## 三、零值，默认值
1. bool: false
2. string: ""
3. 整数：0
4. 浮点数：0
5. 复数: (0+0i)
6. 数组：元素的默认值
7. 结构体：字段类型的默认值
8. 接口，数组，切片，通道，字典，函数的零值都是nil，但数组与切片的默认值为[],字典的默认值为[]

注：不同类型的值不能相互比较，nil实际上也有很多类型，不同类型的nil也不相等，nil != nil