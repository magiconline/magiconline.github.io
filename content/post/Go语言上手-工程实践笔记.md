---
title: "Go语言上手-工程实践笔记"
date: 2022-05-08T16:56:52+08:00
draft: false
---

# CSP(Communicating Sequential Processes, 通信顺序进程)
1. 通过通信共享内存  
    ![](/img/屏幕截图%202022-05-08%20170129.png)

```GO
ch := make(chan int, 10)
ch <- 1
<-ch
```
2. 通过共享内存实现通信
   ![](/img/屏幕截图%202022-05-08%20171332.png)

```GO
lock := sync.Mutex

lock.Lock()

lock.Unlock()
```

