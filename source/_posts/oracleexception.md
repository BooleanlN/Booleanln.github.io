---
title: Oracle异常
tags:
  - ORACLE
url: 139.html
id: 139
categories:
  - 数据库
date: 2018-07-13 14:26:07
---

### Oracle异常

*   预定义的异常Predefined,
    
*   非预定义的异常
    
*   用户定义的异常
    
    *   declare xxx exception;
        
    *   raise xxx;
        
    *   exception Handleing
        

begin 
declare  
      dept_error exception; --自定义异常
      v_comm emp.comm%type; -- 类型为emp表comm字段类型
      k number(3):='sss';
begin
      --update emp set comm=600.0 where empno = 7369;
      dbms\_output.put\_line(v_comm||k);
      select comm into v_comm from emp where empno = 7369;
      if v\_comm = 0 or v\_comm is null then
         raise dept_error; --抛出异常
      else
         dbms\_output.put\_line(v_comm);
      end if;
      exception
          when NO\_DATA\_FOUND then --预定义异常
               dbms\_output.put\_line('无7369职工');
          when dept_error then  --用户自定义异常
               dbms\_output.put\_line('无佣金!');
          when others then --其它异常处理
               dbms\_output.put\_line('无佣金!');
             
end;
exception --用于捕获声明处的异常
   when others then
      dbms\_output.put\_line('声明错误!');
end;