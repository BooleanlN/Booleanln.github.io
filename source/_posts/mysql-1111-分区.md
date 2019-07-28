---
title: mysql_1111_分区
date: 2019-04-11 20:54:42
tags:
- mysql
---

### MySQL分区

**分区**：是指根据一定规则，数据库将一个表分解成多个更小的、更容易管理的部分。有利于管理非常大的表，采用了“分而治之”的逻辑，分区引入了分区键的概念，分区键用于根据某个区间值、特定值列表或者HASH函数值执行数据的聚集，让数据根据规则分布在不同的分区中。

**分区的优点 **：

- 相比单个磁盘或文件系统分区，可以存储更多数据
- 优化查询
- 对于已过期或不需要保存的数据，可通过删除与这些数据有关的分区来实现
- 跨多个磁盘来分散数据查询，以获得更大的查询吞吐量

#### 分区类型

- RANGE分区：基于一个给定连续区间范围
- LIST分区：类似RANGE分区，区别在LIST分区是基于枚举出的值列表分区，RANGE是基于给定的联系区间范围分区
- HASH分区：基于给定的分区个数，把数据分配给不同的分区
- KEY分区：类似于HASH分区

若表有主键，则主键必须包含分区键，无主键则无此约束

<!--more-->

##### Range分区

```mysql
create table partEmp(
	c1 int default NUll,
    c2 varchar(30) default NULL,
    c3 date default NULL)engine=InnoDB partition by range(year(c3))(
    partition p0 values less than (1995),
    partition p1 values less than(1996),partition p2 values less than(1997),
    partition p3 values less than(1998),partition p4 values less than(1999),
    partition p5 values less than(2000),partition p6 values less than(2001),
    partition p7 values less than(2002),partition p8 values less than(2003),
    partition p9 values less than(2004),partition p10 values less than(2010),
    partition p11 values less than maxvalue);
-- maxvalue代表可能的最大值
```

适用于经常运行分区键的查询，如上表，经常根据C3的日期值查询记录，删除过期数据时也十分合适

##### LIST分区

```mysql
create table expenses(
	expense_date date not null,
    catagory int,
    amount decimal(10,3)
    )partition by list(catagory)(
    partition p0 values in (3,5),
     partition p1 values in (1,10),
	 partition p2 values in (4,9),
	partition p3 values in(2)
);
```

##### HASH分区

```mysql
create table hashemp(
	id int not null,
    ename char,
    deptno int not null
    )partition by hash(deptno) partitions 4;	
```

主要用来分散热点读，确保数据在预先确定个数的分区中尽可能平均分布

mysql支持两种hansh分区，常规hash分区（取模）和线性hash分区（2的幂）

```mysql
create table linhashemp(
	id int not null,
    ename char,
    deptno int not null
    )partition by linear hash(deptno) partitions 4;	
```

#### 分区管理

##### 分区删除

```mysql
alter table partEmp drop partition p2;
select count(*) from partEmp where c3>date('1996-01-01') and c3<date('1996-12-31');
-- 返回0
-- 若新写入内容应该在p2，则保存至p3（range分区，list分区则无法再保存	）
```

##### 分区与未分区查询速度比较

```mysql
-- 插入大量数据
delimiter $$
create procedure ins_emp()
begin
declare v int default 0;
while v < 800000
do 
insert into partEmp(c1,c2,c3) values(v,null,adddate('1995-01-01',(rand(v)*36520)mod 3652));
set v = v+1;
end while;
end
$$
delimiter ;
call ins_emp();
insert into nopartemp select * from partEmp;
explain select count(*) from nopartemp where c3>date('1995-01-01') and c3<date('1995-12-31');
-- '1', 'SIMPLE', 'nopartemp', NULL, 'ALL', NULL, NULL, NULL, NULL, '50436', '11.11', --- 'Using where'
-- 可以看出，查询了所有行
explain select count(*) from partEmp where c3>date('1995-01-01') and c3<date('1995-12-31');
-- '1', 'SIMPLE', 'nopartemp', NULL, 'ALL', NULL, NULL, NULL, NULL, '10336', '11.11', --- 'Using where'
-- 可以看出，只查询了一个分区，查询性能提升
```

