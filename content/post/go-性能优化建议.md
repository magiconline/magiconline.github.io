---
title: "Go性能优化建议"
date: 2022-05-11T20:55:03+08:00
draft: false
---
# 高性能编程
## 时间性能优化
### Slice
尽量预分配内存，减少append操作扩容的频率。
Slice扩容时会复制原数组到新数组。
可使用 ```go test --bench=. --benchmem``` 查看内存分配次数、分配容量。

Slice切片时不会新建数组，而是引用已有的数组。
可能导致一个小Slice切片自一个大Slice时，大Slice的内存不会被释放，可使用```copy```替代```[:]```操作。

### map
与Slice相同，可通过提前分配内存，减少内存拷贝和Rehash的消耗。

### 字符串
字符串是不可变类型，使用的内存是固定的，每次用`+`都会重新分配内存。
使用```strings.Builder```拼接大量字符串，底层是`[]bytes`数组，不会每次都重新分配内存。
`bytes.Buffer`最后会重新创建一个新的字符串，因此稍慢。

还可以使用`builder.Grow(n)`提前分配内存，减少申请内存次数。

## 空间性能优化
### 使用空结构体节省内存
空结构体不占据任何内存空间，可作为各种场景下的占位符。

应用场景：`map[int]struct{}`，通过map实现了set。

## 多线程
### 线程安全
使用`atomic`包性能会比`Mutex`性能高。
因为`Mutex`通过操作系统实现，`atomic`操作通过硬件实现。

`Mutex`用来保护一段逻辑，`atomic`用来保护一个变量。
# 性能优化
## 性能优化原则
- 要依靠数据而不是猜测。
- 要定位最大瓶颈而不是细枝末节
- 不要过早优化
- 不要过度优化

## 性能分析工具 pprof
首页：http://localhost:6060/debug/pprof/
### 查看CPU
```
go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/profile?second=10"
top
list Eat
web
```

### 查看堆内存
```
go tool pprof -http=:8080 "http:localhost:6060/debug/pprof/heap"
```
可切换当前/累计 内存/对象数据，查找未使用的内存。

### goroutine
```
go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/goroutine"
```
通过graph或火焰图分析。

### mutex
```
go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/mutex"
```

### block

### ThreadCreate

## pprof采样原理
### CPU
在程序启动时，向操作系统加入一个10ms定时器。
到达时间后，记录一次CPU信息。
每100ms将记录写入输出流。

### 堆内存
通过内存分配器在堆上分配和释放内存，每分配512KB记录一次。
记录时间从程序运行开始到采样时。

### 协程和线程


### 阻塞和锁
采样阻塞操作的次数和耗时，阻塞耗时超过阈值才会被记录，锁只记录固定比例的锁操作。


# 性能调优案例
## 业务服务优化
- 简历服务性能评估手段
- 分析性能数据，定位性能瓶颈
- 重点优化项改造
- 优化效果验证
  
## 基础库优化
- 分析基础库的核心逻辑和性能瓶颈
- 内部压测验证
- 推广业务服务落地验证
  
## Go语言优化
编译器&运行时优化
- 优化内存分配策略
- 优化代码编译流程
- 内部压测验证
- 推广业务服务落地验证