---
title: mysql常用操作
tags:
  - mysql，数据库
url: 168.html
id: 168
categories:
  - 数据库
date: 2018-07-19 13:38:18
---

1.创建表
CREATE TABLE table\_name (column\_name column_type);
CREATE TABLE IF NOT EXISTS \`runoob_tbl\`(
   \`runoob\_id\` INT UNSIGNED AUTO\_INCREMENT,
   \`runoob_title\` VARCHAR(100) NOT NULL,
   \`runoob_author\` VARCHAR(40) NOT NULL,
   \`submission_date\` DATE,
   PRIMARY KEY ( \`runoob_id\` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;	 
2.查看表结构：
describe tablename;
show columns from tablename;
explain tablename;
3.添加列
alter table tablename add column columnname varchar(30);
4.删除列
alter table tablename drop column columnname;
5.修改列名
alter table tablename change columnname newname varchar(30);
6.修改列属性
alter table tablename modify columanname varchar(22);
7.添加主键约束：
alter table 表名 add constraint 主键 （形如：PK_表名） primary key 表名(主键字段);
8.添加外键约束：
alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);
9.删除主键约束：
alter table 表名 drop primary key;
10.删除外键约束：
alter table 表名 drop foreign key 外键（区分大小写）;
11.修改表名：
alter table t_book rename to bbb;
12.select按降序：
select * from tablename order by columnname desc;
13.删除增加主键约束
alter table tablename drop primary key;
alter table tablename add primary key(xxx,xxx);

##### 给已有字段设置默认值

ALTER TABLE test ALTER COLUMN case_status SET DEFAULT 'A';

##### mysql编码格式

MYSQL写入数据时报错ERROR 1366 (HY000): Incorrect string value: '\\xE8\\x8B\\xB1\\xE5\\xAF\\xB8...' for c 插入中文不能插入
解决：
	1.删除之前的表
	2.重新创建，并添加语句 ‘ DEFAULT CHARSET=utf8’
一些查看和修改字符集的命令：
查看mysql的字符集：show variables where Variable_name like '%char%';
查看某一个数据库字符集：show create database enterprises;(注：enterprises为数据库)
查看某一个数据表字符集：show create table employees;(注：employees为数据表)
修改mysql的字符集：

              mysql> set character\_set\_client=utf8;

              mysql> set character\_set\_connection=utf8;

              mysql> set character\_set\_database=utf8;

              mysql> set character\_set\_results=utf8;

              mysql> set character\_set\_server=utf8;

              mysql> set character\_set\_system=utf8;

              mysql> set collation_connection=utf8;

              mysql> set collation_database=utf8;

              mysql> set collation_server=utf8;
修改数据库enterprises的字符集：

alter database enterprises character set utf8

修改数据表employees的字符集：

alter table employees character set utf8

修改字段的字符集

alter table employees change name name char(10) character set utf-8;