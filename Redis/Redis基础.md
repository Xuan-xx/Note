# Redis基础

## Redis的作用

- Redis是一个`NoSQL`(Not only SQL)数据库，数据采用`KEY-VALUE`的方式存储，性能较好，适用于频繁访问的场景

## Redis-key键操作

- keys * ：查看当前库所有key
- exists \<key> ：判断某个key是否存在
- type \<key> ：查看key的类型
- del \<key> ：删除指定的key数据
- unlink \<key> ：根据value选择非阻塞删除（仅将keys从keysspace元数据中删除，真正的删除会在后续异步操作）
- expire \<key> ：为给定的key设置过期时间
- ttl \<key> ：查看key的还有多少秒过期；-1表示永不过期，-2表示已过期

## Redis常用命令

- get \<key> : 查询对应键值
- append \<key> value : 将value追加到原值的后面
- strlen \<key> : 获得值的长度
- setnx \<key> value : 只有key不存在时才设置key的值
- incr \<key> : 将key中存储的数字值增一，只能对数字值操作，如果为空，新增值为1
- decr \<key> : 将数字值减一，其他同上
- incrby/decrby \<key> 步长 : 将key存储的值自增/减，数值自定义

---
**以下为原子性操作：**

- mset \<key1> \<value1> \<key2> \<value2>... : 同时设置一个或多个key-value对
- mget \<key1> \<value1> \<key2> \<value2>... : 同时获取一个或多个key-value对
- msetnx \<key1> \<value1> \<key2> \<value2>... : 同时设置一个或多个key-value对，当且仅当所有给定key都不存在

---

- getrange \<key> \<start> \<end> : 获得值的范围，类似Java中的substring`前包、后包`
- setex \<key> \<过期时间> \<value> : 设置键值的同时设置过期时间
- getset \<key> \<value> : 以新换旧，设置了新值同时得到旧值

## Redis其他命令

- select db：切换数据库
- dbsize：查看当前数据库的key的数量
- flushdb：清空当前库
- flushall：清空所有数据库的数据

---

## Redis常用数据类型

### Redis字符串（String）

- String是Redis中最基本的类型
- String类型是二进制安全的。可以存储jpg图片或其他可序列化对象
- String的value最多是512M

#### 数据结构

Redis中的String的数据结构为简单动态字符串(Simple Dynamic String,SDS)。内部结构实现上类似Java的ArrayList。
> 字符串的长度小于1M时，扩容时内存加倍。字符串的长度大于1M时，每次扩容内存增加1M，最大为512M.

---

### Redis列表(List)

- 一个key可以存多个value
- 底层为`双向链表`，对两端操作性能很高

#### 常用命令

- `lpush/rpush <key> <value1> <value2>` : 在列表左边或右边加入多个数据
- `lpop/rpop <key>` : 在列表左边或右边取出数据，取出列表的所有值时列表消失
- `lrange <key> <start> <end>` : 从左边查看开始到结束索引内列表的值
- `rpoplpush <k1> <k2>` : 从k1列表右边取出一个值放入k2的左边
- `lindex <key> <index>` : 按照下标获得元素（从左到右）
- `llen <key>` : 获得列表长度
- `linsert <key> before <value> <newvalue>` : 在\<value>后面插入\<newvalue>
- `lrem <key> <n> <value>` : 从左边删除n个value
- `lset <key> <index> <value>` : 将列表key下标为index的值替换成value

#### 数据结构

List的数据结构为quickList（满足了快速的插入删除性能，又不会出现太大的空间冗余）

- 首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是`ziplist`，即压缩列表（它将所有的元素紧挨着一起存储，分配的是一块连续的内存）
- 当数据量比较多的时候会改成`quicklist`（把多个压缩列表组成一个**双向链表**）

---

### Redis集合(Set)

- 一个key可以存储多个元素
- 提供判断某个成员是否在一个Set集合里面的接口
- Redis的Set是一个String类型的无序集合，它的底层是一个value为null的hash表，添加、查找、删除的时间复杂度都是O(1)

#### 常用命令

- `sadd <key> <value1> <value2> ...` : 将一个或多个member元素添加到集合key中，自动忽略重复的元素
- `smembers <key>` : 取出该集合的所有值
- `sismemeber <key> <value>` : 判断集合\<key>是否有元素\<value>，有返回1，没有返回0
- `scard <key>` : 返回\<key>中元素的个数
- `srem <key> <value1> <value2>...` : 移除集合中一个或多个元素
- `spop <key>` : 随机从集合中吐出一个值
- `srandmember <key> <n>` : 随机从集合中取出n个值，不删除
- `smove <source> <destination> <value>` : 把一个集合中的一个值移动到另一个集合
- `sinter <key1> <key2>` : 返回两个集合的**交集**
- `sunion <key1> <key2>` : 返回两个集合的**并集**
- `sdiff <key1> <key2>` : 返回两个集合的**差集**(key1与key2的差)

#### 数据结构

- Set底层是哈希表

---

### Redis哈希(hash)

- hash是一个键值对集合，是一个String类型的`field`和`value`的映射表

#### 常用命令

- `hset <key> <field> <value>` : 为key设定一对field-value
- `hget <key> <field>` : 得到key中指定\<field>的值
- `hmset <key1> <field1> <value1> <field2> <value2> ...` : 为key设定多对field-value
- `hexists <key> <field>` : 查看key中给定的field是否存在
- `hkeys <key>` : 列出key中所有的field
- `hvals <key>` : 列出key中所有的value
- `hincrby <key> <field> <increment>` : 为key中指定的field加上增量值increment
- `hsetnx <key> <field> <value>` : 为key设定一对field-value，当且仅当field不存在

#### 数据结构

Redis中hash对应的数据结构有两种，ziplist和hashtable。当field-value长度较短且数据较少时使用ziplist，否则使用hashtable。

---

### Redis Zset(sorted set)

- zset类似于set，是一个没有重复元素的字符串集合。
- 不同之处在于zset中的每个成员都关联了一个`score`，这个评分被用来按照从最低分到最高分的方式排序集合元素（评分可重复）

#### 常用命令

- `zadd <key> <score1> <value1> <score2> <value2>...` : 为zset添加成员
- `zrange <key> <start> <stop> [WITHSCORE]` : 取出key中下标在start到stop中的元素，带withscore表示同时取出score
- `zrangebyscore <key> <min> <max> [WITHSCORE]` : 返回key中score在min到max之间的值，并按score值从小到大排序
- `zrevrangebyscore <key> <max> <min> [WITHSCORE] [LIMIT OFFSET COUNT]` : 同上，但按score从大到小排序
- `zincrby <key> <increment> <value>` : 为元素的score加上增量
- `zrem <key> <value>` : 删除该集合下指定值的元素
- `zcount <key> <min> <max>` : 统计该集合分数区间内元素的个数
- `zrank <key> <value>` : 返回该值在集合中的排名，从0开始

#### 数据结构

Redis中zset底层使用了两个数据结构
1. hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值
2. 跳跃表，跳跃表的目的在于为value排序，根据score的范围获取元素列表