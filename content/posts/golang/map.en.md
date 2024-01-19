---
title: "Go: Map"
date: 2024-01-03
draft: true

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
summary: ""
lastMod: 2024-01-04
cover:
    hidden: false
    image: "post/golang-map.jpeg"
weight: 30
categories: [golang]

tags: [golang,map,kv]
---



## Map


### 一、声明与赋值
```go
// 声明 指明key，value的类型
var a map[string]string
// 赋值
a = map[string]string{}

// 声明并赋值
var b map[string]string = map[string]string{}
var c = map[string]string{
    "a" : "a",
    "b" : "b",
}
d := map[string]string{}

// 使用make赋值, 没有容量参数
var e = make(map[string]string, 5)
```

### 二、注意事项
* 只声明的map未初始化, 为nil，添加元素会报错， 查询、删除不会报错相当于空操作
* 不支持并发，否则报错panic
* key的类型不同，hash的计算方式也不同
* range遍历是随机的
* ok可判断myKey是否存在：v,ok = m["myKey"]；即使不存在v也是零值
* 查询键值对时，最好检查键是否存在，避免操作零值
* delete删除键值对
* map为引用类型，不能比较
* 数组和切片允许对元素进行取址操作，但不允许对map的元素进行取址操作
* value可以是Go支持的任意数据类型，而key则所有限制
> key的数据类型必须是可以使用==和!=进行比较  
> 所以，key不能是函数、切片、map，因此这些数据类型不能进行比较  
> 另外，而数组和结构体则可以作为map的key  
> 不过，如果数组的元素包含函数、切片、map，则数组不能作为map的key，结构体的字段如果有以上三者，也同样不能作为map的key。


### 三、实现原理

#### 3.1 插入


#### 3.2 更新


#### 3.3 扩容
