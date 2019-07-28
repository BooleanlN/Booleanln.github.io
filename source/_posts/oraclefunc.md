---
title: Oracle函数
tags:
  - ORACLE
url: 140.html
id: 140
categories:
  - 数据库
date: 2018-07-13 14:27:15
---

create or replace function return_sal(eno Jsj425.Emp.Empno%type)--声明或覆盖一个函数
return number --需要先声明返回值
    is  --is/as
v_sal Jsj425.Emp.sal%type;
--function body
--查找员工号是eno的sal
begin
  select sal into v_sal from Jsj425.Emp where empno = eno;
  return v_sal;
exception 
  when NO\_DATA\_FOUND then
    dbms\_output.put\_line('无此员工信息');
  when others then 
    dbms\_output.put\_line('其他未知错误');
 end return_sal;
 ---函数调用
 declare 
  v_sal number(7,2);
  begin
    v\_sal:=return\_sal(7369);
    dbms\_output.put\_line(v_sal);
  end;