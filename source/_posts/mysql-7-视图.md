---
title: mysql_7_视图
date: 2019-04-08 16:52:55
tags:
- mysql
---

### Mysql_视图

#### 视图概述

视图是一种虚拟存在的表，对于使用视图的用户来说基本上是透明的。视图并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

优势：

- 简单，视图用户无需考虑后面的表结构
- 安全，访问权限管理可以限制到某个行某个列
- 数据独立，屏蔽表结构变化对用户的影响

<!--more-->

#### 视图操作

##### 创建视图

```mysql
create [or replace][algorithm={undefined|merge|temptable}]
	view view_name [(column_list)]
	AS select_statement
	[WITH[CASCADED|LOCAL]CHECK OPTION]
alter [algorithm={undefined|merge|temptable}]
	view view_name [(column_list)]
	AS select_statement
	[WITH[CASCADED|LOCAL]CHECK OPTION]
-- 实例	
begin;
create view salary_view as select * from salary where salary>1000;
select * from salary;
select * from salary_view;
alter view salary_view as select * from salary where salary<1000;
commit;
```

MySQL视图限制：

- FROM关键字后不能包含子查询
- 不可更新：
  - 包含：聚合函数、DISTINCT、GROUP BY、HAVING、UNION或者UNION ALL
  - 常量视图 (create view pi as select 3.1415926 as pi)
  - join
  - select中包含子查询
  - from一个不可更新的视图
  - where字句的子查询引用了from字句中的表

[WITH[CASCADED|LOCAL] CHECK OPTION]：

- cascaded级联，满足所有针对该视图的所有视图的条件才可以更新
- local 只要满足本视图条件就可以更新

##### 删除视图

```mysql
DROP VIEW [IF EXISTS] view_name[,view_name2]...[RESTRICT|CASCADE]

-- 删除、查看视图操作
drop view salary_view restrict;
show tables;
show table status;
show create view salary_view;
```



