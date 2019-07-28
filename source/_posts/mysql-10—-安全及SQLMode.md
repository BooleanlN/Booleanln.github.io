---
title: mysql_10—_安全及SQLMode
date: 2019-04-11 17:01:07
tags:
- mysql
---

### MySQL安全及SQLMode

SQL安全问题：

**SQL注入** ，应对措施：PrepareStatement+Bind-Variable

SQLMode：

**作用** ：

- 可以完成不同严格程度的数据校验，有效保持数据准确性
- 保证大多数SQL符合标准的SQL语法，利于迁移
- 通过设置SQL Mode使得MySQL上数据更方便地迁移到目标数据库

<!--more-->

**功能实例**：

```
set session sql_mode= 'ANSI';
create table t(d datetime);
insert into t(d) values('2001-04-31');-- warning，且插入表中内容记录值为0
select * from t;
set session sql_mode='traditional';
insert into t(d) values('2001-04-31'); -- Error Code : incorrect dateime value
select * from t;
select @@sql_mode;
set session sql_mode = 'ansi';
select 'bei'||'jing'; -- pipe_as_concat模式
```

**常用的SQL Mode **：

| sql_mode值          | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| ANSI                | 等同于REAL_AS_FLOAT、PIPE_AS_CONCAT、ANSI_QUOTES、IGNORE_SPACE和ANSI的组合模式，该模式使语法和行为更符合SQL标准 |
| STRICT_TRANS_TABLES | STRICT_TRANS_TABLES适用于事务表和非事务表，是严格模式，不允许非法日期，也不允许超过字段长度的值插入字段中，对于插入不正确的值给出错误而不是警告 |
| TRANDITIONAL        | 严格模式，对于插入不正确的值会给出错误而不是警告，可用于事务表和非事务表，用在事务表中，只要出现错误就会立即回滚 |

