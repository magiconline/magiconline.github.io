---
title: "生成唯一ID的方式"
date: 2022-05-09T14:37:37+08:00
draft: false
---

# ID生成的要求
全局唯一：

递增：数据库中的索引需要主键有序才能保证高性能

有序：满足事务、增量消息、排序等特殊需求

安全：不能为简单的连续ID，防止爬虫等

包含时间信息：可以根据ID提取时间信息

# UUID
包含32个16进制数字，分为8-4-4-4-12的形式，性能高。
问题是UUID为无序数据，数据库插入性能差，作为主键过长。

# 数据库自增主键

# 基于redis INCR原子操作

# 雪花算法
按照时间有序生成，结果是64bit的整数。

结构：1bit符号位（一般为正数、固定为0），41bit时间戳（毫秒），10bit工作进程位（共1024个节点），12bit序列号（一个节点一毫秒内最多产生4096个ID）。

生成的ID按时间趋势递增。