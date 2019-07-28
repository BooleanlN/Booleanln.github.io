---
title: Redis_1_1_基础
date: 2019-03-25 14:51:46
tags:
    - Redis
---

## Redis

### 安装

http://redis.io

```shell
wget http://download.redis.io/releases/redis-xxx.tar.gz	
tar xzf redisxxx
cd redisxxx
make
make install
```

<!--more-->

### 命令

#### 字符串类型

1. 赋值与取值

   `set key value`

   `get key`

2. 递增数字

   `incr key`  原子操作

   键的命名：“对象类型:对象ID:对象属性”，如“user:1:friends”,"user:1:articles"

   生成自增ID：对于每一类对象使用名为对象类型(复数类型):count的键来存储当前对象的数量，每增加一个对象，就用incr+键名递增该键的值。

   存储数据时，可以通过序列化函数将数据转化成字符串

   `incrby key incrment`增加指定数值

   `incrbyfloat key incrment`浮点数值

   `decr key`减小

   `decrby key decrement`减小指定数值

3. 向尾部追加值

   `append key value` 返回值为新value的长度

4. 获取字符串的长度

   `strlen key`返回键值的长度，如果不存在则返回0

5. 同时获取/设置多个键值

   ```
   mget key [key...]
   mset key value [key value ...]
   ```

6. 位操作

   ```
   getbit key offset
   setbit key offset value
   bitcount key [start] [end]//键值中位值为1的个数
   bitop peration destkey key [key...]//操作有or,xor,and,not
   ```

#### 散列类型

1. 赋值与取值

   ```markdown
   hset key field value//插入（返回1），更新（返回0）
   HGET key field
   HMSET KEY field value [field value...]
   HMGET key field [field...]
   HGETALL key
   ```

2. 判断字段是否存在

   ```
   hexists key field//存在1，不存在0
   ```

3. 当字段不存在时赋值

   ```
   hsetnx key field value//原子操作
   ```

4. 增加数字

   ```
   hincrby key field increment
   ```

5. 删除字段

   ```
   hdel key field [field...]//返回删除字段个数
   ```

6. 只获取字段名或字段值

   ```
   hkeys key
   hvals key
   ```

7. 获取字段个数

   ```
   hlen key
   ```

#### 列表类型

> *存储一个有序的字符串列表，常用的操作是向列表两端添加元素，或者获得列表的某一个片段，列表内部使用双向链表实现*

1. 向列表两端添加元素

   ```
   lpush key value [value...]
   rpush key value [value...]
   ```

2. 从列表两端弹出（删除）元素

   ```
   lpop key
   rpop key
   ```

3. 获取列表中元素的个数

   ```
   llen key
   ```

4. 获得列表片段

   ```
   lrange key start stop
   ```

5. 删除列表中指定的值

   ```
   lrem key count value
   ```

6. 获得/设置指定索引的元素值

   ```
   lindex key value
   lset key index value
   ```

7. 只保留列表指定片段

   ```
   ltrim key start end
   ```

8. 向列表中插入数据

   ```
   linsert key before|after pivot value//从左至右查找值为pivot的元素，在其之前或之后插入值为value的元素
   ```

9. 将元素从一个列表转到另一个列表

   ```
   rpoplpush source destination//右弹出，左插入
   ```

#### 集合类型

元素不可以重复，基于散列表实现，拉链法解决冲突

1. 增加/删除元素

   ```
   sadd key member [member...]
   srem key member [member...]
   //返回插入或删除的元素个数
   ```

2. 获取集合所有元素

   ```
   smembers key
   ```

3. 判断元素是否在集合中

   ```
   sismember key member //返回1，0
   ```

4. 集合间运算

   ```
   sdiff key [key...]//差集运算
   sinter key [key...]//交集运算
   sunion key [key...]//并集运算
   ```

5. 获得集合元素个数

   ```
   scard key
   ```

6. 进行集合运算并将结果存储

   ```
   sdiffstore destination key [key...]
   sinterstore
   sunionstore
   ```

7. 随机获得集合中的元素

   ```
   srandmember key [count]
   //count为负值则可能取出相同元素，正值则不会
   ```

8. 弹出元素

   ```
   spop key//随机弹出
   ```

#### 有序集合

> *sorted set：散列表+跳跃表实现，读取任意位置速度较快，可调整某个元素的位置，但比列表元素更加耗费内存。*

1. 增加元素

   ```
   zadd key score member [score member ...]//插入或更新一个元素和该元素的分数
   ```

2. 获得元素的分数

   ```
   zscore key member
   ```

3. 获得排名在某个范围的元素列表

   ```
   zrange key start end [withscores]
   //zrange 0 -1 withscores，返回从小到大的顺序，包含两端
   zrevrange key start end [withscores]//从大到小
   ```

4. 获得指定分数范围的元素

   ```
   zrangebyscore key min max [withscores] [limit offset count]
   //zrangebyscore scoreboard 0 90 withscores
   //LOC，在获得元素列表的基础上向后偏移offset个元素，
   ```

5. 增加某个元素的分数

   ```
   zincrby key increment member//返回修改后的分数
   ```

6. 获得集合中元素个数

   ```
   zcard key
   ```

7. 获得指定分数范围内的元素个数

   ```
   zcount key min max
   ```

8. 删除多个元素

   ```
   zrem key member [member...]//返回成功删除个数
   ```

9. 按照范围删除元素

   ```
   zremrangebyrank key start stop//按照排名
   zremrangebyscore key min max//按照分数范围
   ```

10. 获得元素排名

    ```
    zrank key member//从小到大
    zrevrank key member //从大到小
    ```

11. 计算有序集合的交/并集

    ```
    zinterstore dest numbeys key [key ] [weights weight [weight...]] [aggregatesum|min|max]
    zunionstore
    ```

    

