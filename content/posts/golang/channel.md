---
title: "Go: Channel"
date: 2024-01-01
draft: false

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
summary: ""
lastMod: 2024-01-01
cover:
    hidden: false
    image: "post/golang-channel.jpeg"
weight: 30
categories: [golang]

tags: [通道,channel,chan]
---

## 通道

在Go语言中，channel是一种特殊的类型，用于在并发编程中实现不同的goroutine之间的通信和同步。本文将深入探讨golang的channel是如何工作的，并介绍如何使用channel来提高程序的性能和可靠性。

### 一、什么是Channel
在Go语言中，使用goroutine单纯地将函数并发执行是没有意义的。函数与函数间需要交换数据才能体现并发执行函数的意义。

虽然可以使用共享内存进行数据交换，但是共享内存在不同的goroutine中容易发生竞态问题。为了保证数据交换的正确性，必须使用互斥量对内存进行加锁，这种做法势必造成性能问题。

Go语言的并发模型是CSP（Communicating Sequential Processes），提倡通过通信共享内存而不是通过共享内存而实现通信。

如果说goroutine是Go程序并发的执行体，channel就是它们之间的连接。channel是可以让一个goroutine发送特定值到另一个goroutine的通信机制。

Channel是Go中的一个核心类型，你可以把它看成一个管道，通过它并发核心单元就可以发送或者接收数据进行通讯(communication)。

Channel提供了一种同步的机制，确保在数据发送和接收之间的正确顺序和时机。通过使用channel，我们可以避免在多个goroutine之间共享数据时出现的竞争条件和其他并发问题。

Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序。每一个通道都是一个具体类型的导管，也就是声明channel的时候需要为其指定元素类型。

Channel的操作符是箭头 <- (箭头的指向就是数据的流向)。

### 二、声明与赋值
```go
// var 变量 chan 元素类型
var ch chan string
// make(chan 元素类型, [容量])
c := make(chan string, 10)
```

* 只声明的chan零值为nil，无论发送与接收都会阻塞
* 需要使用make创建并赋值，容量可不写或为0 表示无缓冲
* 无缓冲通道必须至少有一个接收方才能发送成功，同理至少有一个发送放才能接收成功
* chan有三种操作
  1. ch <- "abc" 表示发送
  2. <- ch 表示接收
  3. close(ch) 表示关闭
* chan有方向，一般用在函数参数声明时，对通道操作进行限制
  1. `c chan <- string`表示通道c只能发送数据
  2. `c <- chan string`表示通道c只能接收数据
  3. `c chan string`表示通道c可以发送和接收数据
* panic出现的情况
  1. 关闭值为nil的通道，会panic
  2. close后再发送数据，会panic
  3. 重复close，会panic
* 阻塞出现的情况
  1. 通道为nil，无论发送与接收都会阻塞
  2. 无缓冲通道必须至少有一个接收方才能发送成功，同理至少有一个发送放才能接收成功
  3. 有缓冲时：缓冲为空时接收方阻塞， 缓冲满时发送方阻塞
* 关闭通道后接收方能接收数据直到数据为空，但是还会返回数据 `v, ok := <- ch`此时ok为false，v为对应数据的零值；可用于检测通道是否关闭

### 三、接收数据
channel 有一个特性：close关闭之后，在发送的时候会 panic，但是在接收的时候，是可以正常接收的。

#### 1. for-range
```go
for v := range ch {

}
```
range ch会一起迭代直到ch关闭，如果没有数据则会阻塞

#### 2. for{}死循环
```go
for {
    v, ok := <- ch
    if !ok {
        break
    }
    fmt.Println(v)
}
```
for{}列表循环的关键是要判断ch是否已关闭，如果已关闭则可退出死循环  
通道中有数据则会接收到值，否则会阻塞

#### 3. select
上面两种方式都是从单通道中接收数据，而select可处理多个通道  

* select类似switch, 一次只能处理一个case，它不是循环for
* select只能用于channel
* 若想一直处理数据，要在外面加for{}死循环
    ```go
    for {
        select {
        case v := <- ch:
            fmt.Println(v)
        case v2 := <- ch2:
            fmt.Println(v2)
        default:
            break
        }
    }
    ```
* 多个case分支满足时，则会`伪随机`选择一个处理
* 所有case都不满足时，没有default则阻塞，有default则处理default
* select能接收数据，也能用于发送数据
* case和default都没有的select会永远阻塞
* select中没有fallthough
* break
  1. case中可以没有break，即case执行完就退出select了
  2. 你也可以认为每个case最后会默认存在break
  3. 你加了break就代表提前退出select，break后面的代码不会执行
  4. break不会退出for，除非使用标签进行break或使用goto
  5. return跟select,for,switch没关系；退出的是函数
  6. switch有fallthough, 并且fallthough后面的条件不会进行判断


### 四、原理
hchan结构如下：
```go
type hchan struct {
    qcount   uint           // 当前队列中剩余元素个数
    dataqsiz uint           // 环形队列长度，即可以存放的元素个数
    buf      unsafe.Pointer // 环形队列指针
    elemsize uint16         // 每个元素的大小
    closed   uint32         // 标识关闭状态
    elemtype *_type         // 元素类型
    sendx    uint           // 队列下标，指示元素写入时存放到队列中的位置
    recvx    uint           // 队列下标，指示元素从队列的该位置读出
    recvq    waitq          // 等待读消息的goroutine队列，即等待接收队列
    sendq    waitq          // 等待写消息的goroutine队列，即等待发送队列
    lock     mutex          // 互斥锁，chan不允许并发读写
}
```


### 五、参考链接
* [go channel 万字详解](https://zhuanlan.zhihu.com/p/643013131)
* [Go Channel的基本使用及底层原理详解](https://blog.csdn.net/y1391625461/article/details/124292119)