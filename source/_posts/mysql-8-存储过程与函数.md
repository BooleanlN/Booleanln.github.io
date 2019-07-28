---
title: mysql_8_存储过程与函数
date: 2019-04-09 15:09:43
tags:
- mysql

---

### MySQL存储过程与函数

**存储过程（函数）**：事先经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程和函数可简化应用开发人员的很多工作，减少数据在应用服务器与数据库之间的传输，可适当提高数据处理的效率。

*存储过程*与*函数*相比，函数必须有返回值，而存储过程没有，存储过程参数可以是IN、OUT、INOUT类型的，而函数参数类型只能是IN类型。

存储过程与函数虽然可以减少传输量，但在数据库服务器进行大量复杂的运算，会占有服务器的CPU，造成数据库的压力，所以不要在存储过程和函数中进行大量的复杂运算，应尽量将这些运算分摊到应用服务器上执行。

<!--more-->

#### 存储过程/函数的创建

```mysql
create procedure sp_name([proc_parameter[,...]])
	[characteristic]routine_body
create function sp_name([func_parameter[,...]])
	returns type
	[characteristic ...]routine_body
	
-- 存储过程创建
delimiter $$
create procedure proc_getS(in money INT, out counts int)
reads sql data
begin
	select * from salary where salary > money;
    select found_rows() into counts;
end $$
delimiter ;
-- 函数创建
delimiter $$
create function fun_getS(money INT)
returns int
reads sql data
begin 
	return(select count(*) from salary where salary > money);
end $$
delimiter ;
```

**[characteristic]：**

- LANGUAGE SQL：系统默认，说明下面过程的BODY是用SQL语言编写的
- [NOT] DETERMINISTIC：DETERMINISTIC确定的，相同输入必须相同输出
- {CONTAINS SQL|NO SQL|READS SQL DATA|MODIFIES SQL DATA}：这些特征提供子程序使用数据的年内在信息，目前只提供给服务器，并不会造成实际的约束，默认contains，分别对应，不含读写数据语句|不包含SQL语句|包含读|包含写
- SQL SECURITY {DEFINER|INVOKER}：指定子程序该用创建子程序者的权限（definer）来执行还是使用调用者的权限（invoker）来执行
- COMMENT 'STRING' :注释信息

**delimiter**:声明终止符，因为存储过程/函数中常有;，所以另外声明终止符，用于区分

#### 存储过程/函数删除

```mysql
drop {procedure|function}[if exists] sp_name

drop procedure if exists proc_getS;
drop function if exists fun_getS;
```

#### 存储过程/函数调用

```mysql
call sp_name(para...);
select fun_name(para...);
```

#### 查看存储过程/函数

```mysql
show {procedure|function}status[like 'pattern']; -- 查看状态
show procedure status like 'proc_getS'; 
show create {procedure|function} sp_name; -- 查看定义
show create procedure proc_getS;
```

##### 变量使用

###### 变量定义

```
declare var_name[,...] type [default value]
declare last_month_start date;
```

###### 变量赋值

```
set var_name = expr[, var_name=expr]...
select col_name into var_name 
```

##### 条件定义和处理

```mysql
declare condition_name condition for condition_value
conditin_value:
		SQLSTATE[VALUE] sqlstate_value
	|mysql_error_code
-- 条件的处理
declare handler_type handler for condition_value[,...] sp_statement
handler_tyoe:
	CONTINUE|EXIT|UNDO
CONDITION_VALUE:
	SQLSTATE[VALUE] sqlstate_value|CONDITION_NAME|SQLWARING|NOT FOUND|SQLEXCRPTION|mysql_error_code
	
-- 实例，处理主键重复报错
delimiter $$
create procedure inse_emp()
begin
 declare continue handler for sqlstate '23000' set @x2 = 1; -- 跳过
 set @x = 1;
 insert into emp(id,name,hiredate,deptno) values(1,'威威','2000-01-02',6);
 set @x = 2;
insert into emp(id,name,hiredate,deptno) values(5,'五五','2000-01-12',8);
set @x = 3;
end;
$$
delimiter ;
call inse_emp();
select @x,@x2
@x = 3,@x2 = 1;
```

#### 光标

```mysql
-- 声明光标
declare cursor_name CURSOR FOR select_statement
-- OPEN光标
OPEN cursor_name
-- FETCH光标
FETCH cursor_name INTO var_name[,var_name]...
-- CLOSE光标
CLOSE cursor_name

-- 简单使用光标循环遍历每条记录，并根据id奇偶性求得两个相加和

delimiter $$
create procedure payment_sum()
begin 
declare i_staff_id	int;
declare d_amount int;
declare cur_pay cursor for select userid,salary from salary;
declare exit handler for not found close cur_pay;
set @x=0;
set @x2 = 0;
open cur_pay;
repeat
	fetch cur_pay into i_staff_id,d_amount;
		if i_staff_id%2=0 then
			set @x = @x+d_amount;
		else 
			set @x2 = @x2+d_amount;
		end if;
until 0 end repeat;
close cur_pay;
end;
$$
delimiter ;
call payment_sum();
select @x,@x2;
```

###### 流程控制

```
IF、CASE、loop、leave、iterate、repeate、while
```

#### 事件调度器

> 适用于定时收集统计信息、定期清理历史数据、定期数据库检查等
>
> 但避免过多使用，复杂处理更适合用程序实现，开启和关闭调度器须su权限

```mysql
create event event_name 
	on schedule at time + interval interval_time -- on schedule后接执行时间和执行频次
	do
		statement; --具体操作
-- 实例，1分钟后插入一个记录
create event myevent on schedule at current_timestamp + interval 1 minute
do 
insert into emp(name,hiredate,deptno) values('李里','20190402',5);
-- 实例，每5秒后插入一个记录
create event myevent on schedule every 5 second
do 
insert into emp(name,hiredate,deptno) values('李里','20190402',5);
```

##### 管理事件

```mysql
show events; -- 查看事件 
set global event_scheduler = 1; -- 设置事件调度器开启
show variables like '%scheduler%'; -- 查看事件调度器状态
show processlist; -- 查看后台进程
alter event myevent disable; -- 禁用调度器
drop event 	myevent; -- 删除事件
```

