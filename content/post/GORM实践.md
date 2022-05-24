---
title: "GORM实践"
date: 2022-05-16T14:53:41+08:00
draft: true
---

# 1 简介

## 1.1 DB连接的几种类型

直接连接/Conn

预编译/Stmt

事务/Tx

## 1.2 处理返回数据的几种方法

Result

Rows

Row



# 2 GORM

## 2.1 CRUD

操作数据库

```go
// 创建
db.AutoMigrate(&Product{})
db.Migrator().CreateTable(&Product{})


// 插入
db.Create(&user)


// 读取
db.First(&product)
db.Find(&users, []int{1,2,3})

// 更新
db.Model(&product).Update("Price", 200)


// 删除
db.Delete(&product)
```

## 2.2 Model 定义

## 2.3 惯例约定

## 2.4 关联操作



# 3 GORM 设计原理

## 3.1 SQL生成

SQL是怎么生成的

```sql
SELECT * FROM * WHERE * ORDERBY * LIMIT * FOR *
// 必选：SELECT FROM 
// 可选：WHERE ORDERBY LIMIT
```

```go
db.Where().Where().Limit().Order().Find()
// Chain Method + Finisher Method
```



## 3.2 插件扩展

Callbacks

## 3.3 ConnPool



## 3.4 Dialector

# 4 GORM 最佳实践

## 4.1 最佳实践

数据序列化与SQL表达式

批量数据操作

关闭默认事务

SkipHooks

Prepare Statement

代码分页、分库分表

混沌工程、压测


