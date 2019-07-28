---
title: mysqlql_4_常用函数
date: 2019-03-26 09:39:37
tags: 
  - mysql
---

### 常用函数

#### 日期和时间函数

| 函数                              | 功能                   |
| --------------------------------- | ---------------------- |
| curdate()                         | 返回当前日期           |
| curtime()                         | 返回当前时间           |
| now()                             | 日期+时间              |
| unixtimestamp()                   | 当前日期对应的时间戳   |
| from_unixtime()                   | 时间戳对应的日期       |
| week(date)                        | date对应的周数         |
| year(date)                        | date对应的年份         |
| hour(time)                        | time对应的小时值       |
| minute(time)                      | time对应的分钟值       |
| mouthname(date)                   | 日期对应的月份英文名称 |
| date_format(date,fmt)             | 格式化date值           |
| date_add(date,interval expr type) |                        |
| datediff(expr,expr2)              |                        |

<!--more-->

```mysql
select unix_timestamp(now()),from_unixtime(unix_timestamp(now())),week(now()),year(now()),hour(curtime()),minute(curtime()),monthname(now()),month(now());
-- 结果：
1553564335	2019-03-26 09:38:55	12	2019	9	38	March	3
```

```mysql
select date_format(now(),"%M,%D,%Y");
-- 结果：		
	March,26th,2019
```

```mysql
select now() current,date_add(now(),interval 31 day) after31days;
-- 结果：
	2019-03-26 09:49:33	2019-04-26 09:49:33
-- interval，间隔类型关键字 type为间隔类型
```

```mysql
select datediff('2008-08-08',now());
-- 结果:
	-3882
-- 返回现在距离2008-08-08的天数
```

#### 数值函数

| 函数          | 功能                                       |
| ------------- | ------------------------------------------ |
| abs(x)        | 取绝对值                                   |
| ceil(x)       | 返回大于x的最小整数值                      |
| floor(x)      | 返回小于x的最大整数值                      |
| mod(x,y)      | 返回x/y的模                                |
| rand()        | 返回0~1内的随机值                          |
| round(x,y)    | 返回参数x的四舍五入的有y位小数的值         |
| truncate(x,y) | 返回参数x截断为y位小数的值，不进行四舍五入 |

#### 字符串函数

| 函数                  | 功能                                              |
| --------------------- | ------------------------------------------------- |
| concat(s1,s2...)      | 连接s1,s2...                                      |
| insert(str,x,y,instr) | 将字符串从x开始，y个字符长的子串替换成字符串instr |
| lower(str)            | 将字符变为小写                                    |
| upper(str)            | 将字符变为大写                                    |
| left(str,x)           | 返回str最左边x个字符                              |
| right(str,x)          | 返回str最右边x个字符                              |
| lpad(str,n,pad)       | 用pad对str左边进行填充，直到总长度为n             |
| rpad(str,n,pad)       | 用pad对str右边进行填充，直到总长度为n             |
| ltrim(str)            | 去掉字符串左侧空格                                |
| rtrim(str)            | 去掉字符串右侧空格                                |
| repeat(str,x)         | f返回str重复x次的结果                             |
| replace(str,a,b)      | y用字符串b替换str中出现的字符串a                  |
| strcmp(s1,s2)         | b比较字符串s1和s2（ASCII值）                      |
| trim(str)             | q去掉字符串行首行尾的空格                         |
| substring(str,x,y)    | c从x位置起y个长度的子串                           |

Null与任何字符串连接结果为NULL

#### 流程函数

| 函数                                                  | 功能                                             |
| ----------------------------------------------------- | ------------------------------------------------ |
| IF(value,t,f)                                         | 如果value为真，返回t，否则返回f                  |
| IFNULL(value1,value2)                                 | 如果value1不为空，返回value1，否则返回value2     |
| CASE WHEN[value1]THEN[result1]...ELSE[result]END      | r如果value1为真，返回result1，否则返回result     |
| CASE[expr]WHEN[value1]THEN[result1]...ELSE[result]END | r如果expr等于value1，返回result1，否则返回result |

```mysql
create table salary(
	userid int,
    salary decimal(9,2)
);
insert into salary values(1,1000),(2,2000),(3,3000),(4,4000),(5,5000),(1,null);
select * from salary;
select if(salary>2000,'high','low') from salary;
select ifnull(salary,0) from salary;
```

#### 其他常用函数

| 函数           | 功能                             |
| -------------- | -------------------------------- |
| database()     | 返回数据库名                     |
| version()      | 数据库版本                       |
| user()         | 当前登录用户名                   |
| inet_aton(IP)  | 返回IP地址的数字表示             |
| inet_ntoa(num) | 返回数字代表的ip地址             |
| password(str)  | 返回str的加密版本//用于mysql系统 |
| mad(str)       | str的md5版本//用于应用           |

```mysql
select inet_aton('192.168.1.1'), inet_ntoa(3232235777);
	3232235777	192.168.1.1

```

