---
title: Mysql1_sql基础
date: 2019-03-22 15:40:30
tags:
   - Mysql
---

### Mysql_sql基础

1. DDL

   创建/删除数据库

   ```sql
   create database dbname;
   drop database dbname;
   ```

   <!--more-->

   创建/删除数据表

   ```
   create table tablename(
       column name1 columntype constraints,
       column name1 columntype constraints,
   );
   desc tablename;//查看表定义信息
   show create table xxx \G;//查看表定义更完整信息
   drop table tablename;
   ```

   修改表

   ```mysql
   //修改表类型
   alter table tablename modify [column] column_definition [First|after col_name];
   //增加表字段
   alter table tablename add [column] column_definition [First|After col_name];
   //删除表字段
   alter table tablename drop [column] col_name;
   //字段改名及定义
   alter table tablename change [column] old_col_name column_definition [First|After col_name];
   //修改字段排列顺序
   使用 First/After
   //更改表名
   Alter table tablename Rename[to] new_tablename;
   ```

2. DML

   插入记录

   ```mysql
   insert into tablename(field1,field2...) values(data1,data...);
   insert into tablename(field1,field2...) values(data1,data...),(data1,data2...);
   insert into emp(name,hiredate,deptno) values('王三','19971216',1),('李三','1997-02-08',2),('招三',19960523113045,1);
   ```

   更新记录

   ```mysql
   update tablename set field1 = data1,field2 = data2 [where conditon];
   update t1,t2... set field1 = data1,field2 = data3 [where condition]
   ```

   删除记录

   ```mysql
   delete from tablename [where condition];
   delete t1,t2... from t1,t2... [where condition]
   ```

   查询记录

   ```mysql
   select * from tablename;
   select distinct deptno from tablename;
   select * from tablename [where condition];
   select * from emp order by deptno,name;
   select * from emp order by deptno limit 3;
   ```

   聚合

   ```mysql
   select [field_] fun_name from tablename [where condition] [group by field1,field2][with rollup][having where_condition];
   ```

   表连接

   ```mysql
   -- 内连接
   select name,deptname from dept,emp where emp.deptno = dept.deptno;
   -- 右外连接
   select name,deptname from dept right join emp on dept.deptno = emp.deptno;
   -- 左外连接
   select name,deptname from dept left join emp on dept.deptno = emp.deptno;
   ```

   子查询

   ```mysql
   //in、=、exists、not exists、!=
   select name from emp where deptno in(select deptno from dept);
   ```

   联合

   ```mysql
   select deptno from dept union select deptno from emp;
   select deptno from dept union all select deptno from emp;	
   ```

3. DCL

   ```mysql
   grant
   revoke
   ```