---
title: mysql_9_触发器
date: 2019-04-11 11:20:23
tags:
- mysql
---

### mysql触发器

触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合，触发器可以协助应用在数据库端确保数据的完整性。

#### 触发器创建

```mysql
CREATE TRIGGER trigger_name trigger_time trigger_event ON 
	tbl_name FOR EACH ROW trigger_stmt
-- 实例
delimiter $$
create trigger ins_emp after insert on emp for each row begin
	insert into note(note) values('aftInsert');
    end;
$$
delimiter $$
create trigger ins_emp_n before insert on emp for each row begin
	insert into note(note) values('befInsert');
    end;
$$
delimiter $$
create trigger up_emp after update on emp for each row begin
	insert into note(note) values('aftUpdate');
    end;
$$
delimiter $$
create trigger up_emp_b before update on emp for each row begin
	insert into note(note) values('befUpdate');
    end;
$$
delimiter ;
```

<!--more-->

trigger_time**：BEFORE or AFTER

**trigger_event** ：触发器触发事件，INSERT、UPDATE、DELETE等

对同一个表相同时间的相同触发事件，只能定义一个触发器，**这也是mysql和其他数据库的区别之一**

使用*OLD*以及*NEW*来引用触发器中发生变化的记录内容

目前触发器只支持行级触发，不支持语句级触发

触发器中不能显式或隐式方式开始或结束事务的语句

```mysql
insert into emp(id,name,hiredate,deptno) values(3,'赵温','19971216',5) on duplicate key update name='赵倩',hiredate='19971216',deptno=3;
-- 先before insert触发器，然后before update、after update触发器
```



#### 删除触发器

```mysql
DROP TRIGER [schema_name]trigger_name
drop trigger ins_emp;
```

#### 查看触发器

```
show triggers;
show create trigger ins_emp;
```

