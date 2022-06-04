---
title: "深入理解RDBMS"
date: 2022-06-04T18:41:49+08:00
draft: false
---

# 经典案例
## 关系型数据库事务ACID
- 事务(Transaction): 是由一组SQL语句组成的一个程序执行单元
- ACID： 原子性、一致性、隔离性、持久性

# 发展历史
## 前DBMS时代
人工管理、文件系统

## DBMS数据模型
数据库按数据模型的特点可以分成：网状数据库、层次数据库和关系型数据库

关系型数据库的劣势是关系查询效率不够高，关系必须规范化。

# 关键技术
## 一条SQL的一生
SQL -> Parser -> optimizer -> Executor

## 语法解析器Parser
- 词法分析：获得关键字、表列名、常量、运算符、结束符
- 语法分析：填充一个数据结构Stmt
- 语义分析：检查table/name是否存在，变量是否合法

## optimizer
- 基于规则的优化
- 基于代价的优化

## Executor
- 火山模型
- 向量化
- 编译执行

## 存储引擎 - InnoDB
In-Memory
- Buffer Pool
- Change Buffer
- Adaptive Hash Index
- Log Buffer

On-Disk
- System Tablespace
- General Tablespace
- Undo Tablespace
- Temporary Tablespace
- Redo Log
## 事务引擎
- 通过Undo Log实现事务回退。
- 隔离性通过锁实现
  - 读读 share lock
  - 写写 exclusive lock
  - 读写 share lock | exclusive lock | MVCC读写分离
- 持久性
  1. 事务提交前页面写盘(随机IO、写放大)
  2. WAL(write-ahead logging)，通过redo log 记录页面的变化，如果发生错误，重启后根据redo log 重做

# 企业实践
## 红包
流量大、流量突增、稳定性
### 大流量 - sharding
- 业务数据进行水平拆分
- 代理层进行分片路由

### 流量突增 - 扩容、代理连接池
- 扩容DB物理节点数量
- 利用影子表进行压测

### 稳定性&可靠性
- 多机房
- 代理：读写分离、分库分表、限流、流量调控
  