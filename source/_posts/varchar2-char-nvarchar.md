---
title: varchar2、char、nvarchar
tags:
  - ORACLE
  - 变量语法
  - 数据库
url: 137.html
id: 137
categories:
  - 数据库
date: 2018-07-13 14:20:22
---

#### varchar2、char、nvarchar

区别：

*   char的长度是固定的，varchar2长度是可变化的
    
*   char的效率比varchar2效率稍高
    
*   目前varchar是varchar2的同义词，varchar2是oracle自己开发的一个数据类型，它将在数据库中varchar列可以存储空字符的特性改为存储NULL值
    

 

VARCHAR2比CHAR节省空间，在效率上比CHAR会稍微差一些，即要想获得效率，就必须牺牲一定的空间，这也就是我们在数据库设计上常说的‘以空间换效率’。 
VARCHAR2虽然比CHAR节省空间，但是如果一个VARCHAR2列经常被修改，而且每次被修改的数据的长度不同，这会引起‘行迁移’(Row Migration)现象，而这造成多余的I/O，是数据库设计和调整中要尽力避免的，在这种情况下用CHAR代替VARCHAR2会更好一些。

nvarchar：包含n个字符的可变长度Unicode字符数据。n的值介于1与4000之间。字节的存储大小是所输入字符个数的两倍