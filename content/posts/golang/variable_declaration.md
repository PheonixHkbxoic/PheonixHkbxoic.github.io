---
Title: "变量声明与赋值"
Date: 2023-12-20 22:15:37
draft: true
description: "Go 中命名规则是，名称以字母或下划线开头，后面可跟任意数量的字符、数字和下划线，字符区分大小写，名称本身没有长度限制，但是 Go 的编程风格倾向于使用短名称，特别是局部变量"
lastMod: 2023-12-29
cover:
    hidden: false
    image: "post/golang-001.jpeg"
weight: 100

categories: [golang]

tags:
- 变量
- 声明
- 赋值

slug: ""
comments: true
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示当前路径

---

## 变量声明与赋值

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
