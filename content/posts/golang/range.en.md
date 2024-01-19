---
title: "Go: range"
date: 2024-01-02
draft: false

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
summary: "迭代数组、切片、映射、字符串和通道,i,v是值传递，修改v数组元素不变，
数组与切片循环前次数已确定，遍历通道直到关闭, 没有数据则会阻塞"
lastMod: 2024-01-02
cover:
    hidden: false
    image: "post/golang-range.jpeg"
weight: 30
categories: [golang]

tags: [golang,range]
---

## range

用于迭代**数组、切片、映射、字符串和通道**等数据结构。

通过`range`，可以逐个访问集合中的元素。

### 特点

- 对于数组、切片、字符串，range返回**索引和对应的值**。
- 字符串返回的是rune而不是字节
- 对于映射，range返回键和对应的值。
- 对于通道，range会遍历通道直到关闭, 没有数据则会阻塞。

### 示例

```go
// 1.数组或arr := []int{1, 2, 3}
// 可以只使用索引
for index := range arr{

}
// 可以只使用value
for _, value := range arr{

}

// 全部使用
for index, value := range arr{

}

// 也可以只循环次数
for range arr{

}

// 遍历nil也不报错， 循环一次也不执行
for range nil{

}

// 2.字典
m := map[string]int{
    "a": 1,
  "b": 2,
  "c": 3
}
for k, v := range m {

}

// 3.通道
ch := make(chan int)
go func(){
    ch <- 1
  ch <- 2
  close(ch)
}
// 打印1和2
for data := range ch{
    fmt.Println(data)
}
```

### 注意事项

* 对于channel，没有数据时，会被阻塞

* 尽量避免遍历过程中修改原数据

* index、value接收range返回值会发生一次值深拷贝，但变量指针地址是不变的
  
  ```go
  arr := []int{1, 2, 3}
  for i, v := range arr {
      fmt.Println(&v, &arr[i])
      // 每次v的值在变，但是v的地址(&v)是不变的
      // 最后&v会指向数组最后一个元素
  }
  ```

* **Go中所有赋值都是值传递**，所以直接修改v的值，数组元素不会发生变化  
  值类型复制(赋值)都是深拷贝，引用类型一般都是浅拷贝。  
  深浅拷贝的本质就是看拷贝内容是数据还是数据的地址。
  如果结构中不含指针，则直接赋值就是深度拷贝；
  如果结构中含有指针（包括自定义指针，以及切片，map等使用了指针的内置类型）是浅拷贝  
  这意味着v.Xxx(是指针的话)改的话，数组元素的Xxx也会变  
  特别注意：如果元素是指针，它也是值传递，拷贝的是指针，是浅拷贝，所以操作指针会使对应的值都改变

* 数组与切片循环前次数已确定，所以新添加的元素无法遍历

* map的遍历是随机的，所以新添加的数据可能立即遍历到，也可能不会遍历到
