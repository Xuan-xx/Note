# 连接MySQL

1. systemctl start mysqld.service
2. mysql -u root -p/sudo mycli -u root（命令行增强）



# 选择

- 使用`use`命令
  1. use <database name>
  2. use <table name>



#　展示信息

- 使用`show`命令
  1. show databases;
  2. show tables;
  3. show columns from <table name>
  4. show grants    用来显示授予用户的安全权限
  5. show status    用于显示广泛的服务器状态信息



- 使用`desc`命令（描述结构）



# CRUD

| 功能     | 动词                          |
| -------- | ----------------------------- |
| 数据查询 | select                        |
| 数据定义 | create、drop、alter（修改）   |
| 数据操纵 | insert、update、delete        |
| 数据控制 | grant（特许）、revoke（撤销） |



## 数据查询

```sql
select [ALL|DISTINCT] <目标表达式> AS (别名1),<目标表达式> AS (别名2),... 
from <table name/view name>
[where <条件表达式>]
[group by <列名1> [having <条件表达式>]]
[order by <列名2> [ASC(升序)|DESC(降序)]]
```



- DITTINCT关键字:去重

  - 该关键字会应用于所有的列

- LIMIT子句:限制结果返回行数

  ```sql
  limit 5	//返回前五行数据
  limit 5，5	//返回从第五行开始的后五行数据
  ```



### 排序检索数据

- 使用`order by <排序列名>[ASC|DESC],...`子句
- 可以指定多个列进行排序，执行顺序是先按第一个列排序，第一个列相同再按第二个列，以此类推
- 每个列可以指定`升序(ASC)`或`降序(DESC)`



### 过滤数据

- 使用`where <条件表达式>`子句

- `IS NULL`是一个判断是否为空的where子句

  | 操作符      | 说明             |
  | ----------- | ---------------- |
  | =           | 等于             |
  | !=          | 不等于           |
  | <>          | 不等于           |
  | >           | 大于             |
  | >=          | 大于等于         |
  | <           | 小于             |
  | <=          | 小于等于         |
  | between and | 在指定两个数之间 |

  



#### 数据过滤

- where子句可附加`and`、`or`、`in`、`not`操作符



​	**in的优势**

1. 对于长的选项，in的语法更清晰
2. in操作符一般比or操作符执行更快
3. in可以包含其他select语句



#### 用通配符过滤

- like操作符可以在where子句中使用
- like操作符可使用：
  - `%`:表示任何字符出现任意次
  - `_`:表示任何字符出现一次



- 通配符的处理需要花费更多的时间
- 没有必要时，不要将通配符放到搜索模式的开始处，因为这是最慢的



#### 使用MySQL正则表达式

- 在`where`子句中使用`regexp`操作符
- MySQL中的正则表达式匹配不区分大小写，为区分大小写，可使用`binary`关键字
- 正则表达式中使用`|`表示or，也可以用`[123]`集合这样的形式表达多个or，集合中`^`表示否定
- `[1-9]`、`[a-z]`表示所有的数字和字母
- MySQL中使用`\\`转义，第一个反斜杠由MySQL解释，第二个由正则表达式库解释

| 元字符 | 说明                       |
| ------ | -------------------------- |
| *      | 0个或多个匹配              |
| +      | 1个或多个匹配（等于{1,})   |
| ?      | 0个或1个匹配（等于{0,1}）  |
| {n}    | 指定数目的匹配             |
| {n,}   | 不少于指定数目的匹配       |
| {n,m}  | 匹配数目范围（m不超过255） |





##### 定位符

| 元字符  | 说明       |
| ------- | ---------- |
| ^       | 文本的开始 |
| $       | 文本的结尾 |
| [[:<:]] | 词的开始   |
| [[:>]]  | 词的结束   |



### 创建计算字段

- 计算字段：不实际存在于数据库表中，是运行时在select语句内创建的



#### 拼接字段

- 拼接：把值连接在一起构成单个值
- MySQL使用`Concat()`函数拼接
- MySQL使用`Rtrim()`函数去除串右边的空格



#### 别名

- 在select子句后用`AS`关键字赋予每个列别名



#### 算术计算

- MySQL可以对检索出的数据进行计算，输入列名的算术表达式即可



### 使用数据处理函数

#### 文本处理函数

| 函数        | 说明             |
| ----------- | ---------------- |
| Left()      | 返回串左边的字符 |
| Length()    | 返回串的长度     |
| Locate()    | 找出串的一个子串 |
| Lower()     | 将串转换为大写   |
| LTrim()     | 去除左边空格     |
| Right()     | 返回串右边的字符 |
| RTrim()     | 去除串右边的空格 |
| Soundex()   | 返回读音相近的值 |
| SubString() | 返回子串的字符   |
| Upper()     | 将串转换为大写   |



#### 日期和时间处理函数

| 函数       | 说明                   |
| ---------- | ---------------------- |
| AddDate()  | 增加一个日期           |
| AddTime()  | 增加一个时间           |
| CurDate()  | 返回当前日期           |
| CurTime()  | 返回当前时间           |
| Date()     | 返回日期时间的日期部分 |
| DateDiff() | 计算两个日期之差       |



#### 数值处理函数

| 函数   | 说明               |
| ------ | ------------------ |
| Exp()  | 返回一个数的指数值 |
| Mod()  | 返回除操作的余数   |
| Rand() | 返回一个随机数     |
| Sqrt() | 返回一个数的平方根 |



### 聚集函数

| 函数    | 说明             |
| ------- | ---------------- |
| AVG()   | 返回某列的平均值 |
| SUM()   | 返回某列的和     |
| COUNT() | 返回某列的行数   |
| MAX()   | 返回某列的最大值 |
| MIN()   | 返回某列的最小值 |



### 分组数据

- 使用`group by`子句可以对数据进行分组，并且聚集函数会对每组数据执行一次
- group by子句可以包含任意数目的列，可以对分组进行嵌套
- group by子句中不能使用别名
- 除聚集计算语句，select语句中的每个列都必须在group by子句中给出，否则会丢失数据



#### 过滤分组

- 使用`having`子句，having非常类似于where，差别在于where过滤行，having过滤分组。



### 使用子查询

#### 在from子句中使用

- 作为临时表

#### 在where子句中使用

- 对于能嵌套的子查询数目没有限制
- 会降低性能
- 应保证子查询中返回的列与where中的列数目相匹配



#### 作为计算字段使用

```sql
select cust_name,cust_state,
                              -> (select count(*)
                              -> from orders
                              -> where orders.cust_id = customers.cust_id) as orders
                              -> from customers
                              -> order by cust_name;
                              
+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         | 2      |
| E Fudd         | IL         | 1      |
| Mouse House    | OH         | 0      |
| Wascals        | IN         | 1      |
| Yosemite Place | AZ         | 1      |
+----------------+------------+--------+
```

- 该SQL语句中的子查询共执行了5次，因为检索出了五个客户



#### ALL和ANY

- 可以在子查询中使用`ALL`和`ANY`操作符



#### 多列子查询

- 在where子句中使用

  ```sql
  where (column1,column2)=(
  	select colunm1,column2
  	...
  )
  ```

  

### 联结表

- 外键：为某个表中的一列，它包含了另一个表的主键值，定义了两个表之间的关系

- 可以在`where`子句中创建联结

- 可以在`from`子句中用`<table name1> INNER JOIN <table name2> ON (联结条件)`指定

  ```sql
  select vend_name,prod_name,prod_price
                                -> from vendors inner join products
                                -> on vendors.vend_id = products.vend_id
                                -> order by vend_name,prod_name;
                                
  +-------------+----------------+------------+
  | vend_name   | prod_name      | prod_price |
  +-------------+----------------+------------+
  | ACME        | Bird seed      | 10.00      |
  | ACME        | Carrots        | 2.50       |
  | ACME        | Detonator      | 13.00      |
  | ACME        | Safe           | 50.00      |
  | ACME        | Sling          | 4.49       |
  | ACME        | TNT (1 stick)  | 2.50       |
  | ACME        | TNT (5 sticks) | 10.00      |
  | Anvils R Us | .5 ton anvil   | 5.99       |
  | Anvils R Us | 1 ton anvil    | 9.99       |
  | Anvils R Us | 2 ton anvil    | 14.99      |
  | Jet Set     | JetPack 1000   | 35.00      |
  | Jet Set     | JetPack 2000   | 55.00      |
  | LT Supplies | Fuses          | 3.42       |
  | LT Supplies | Oil can        | 8.99       |
  +-------------+----------------+------------+
  ```

  - 可以联结多个表，不过联结的表越多，性能下降越厉害
  - 没有指定联结条件的两个表返回笛卡尔积



### 创建高级联结

- 可以为表指定别名（在from子句中用as）



#### 自联结

- 使用表的别名区分开两个自身表

#### 自然联结

- 排除相同的列多次出现，使每个列只返回一次

#### 外部联结

- 联结时包含那些没有关联行的行
- 使用`[LEFT | RIGNT] OUTER JOIN`完成（左外联结/右外联结）



### 组合查询

- 使用`UNION`关键字

**UNION规则**

1. 必须由两条或以上的select语句组成
2. 每个查询必须包含相同的列、表达式、或聚集函数
3. 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含转换的类型
4. UNION默认取消重复行，如果想返回所有行，可以使用`UNION ALL`
5. 使用UNION时只能使用一条order by 子句排序



## 插入数据

- ```sql
  INSERT INTO <table name> (column name1,column name2,...) VALUES (value1,value2,...),(value1,value2,...)
  ```

  1. 插入完整的行
  2. 插入行的一部分
  3. 插入多行
  4. 插入某些查询结果

- 使用insert时可以省略某些列，省略的列要满足以下某个条件：

  1. 该列定义为允许NULL值
  2. 在表的定义中给出默认值

- 可以在INSERT和INTO中间插入`LOW_PRIORITY`降低MySQL中INSERT的优先级



### 插入检索出的数据

```sql
INSERT INTO <table name1> (column name1,column name2,...)
select (column name1,column name2,...)
from <table name2>;
```

- insert和select的列名可以不匹配，MySQL会按列的位置插入数据
- select子句选择当前表是自我复制



## 更新和删除数据

### 更新数据

**更新方式**：

1. 更新表中特定行
2. 更新表中所有行

```sql
update <table name>
set <column name1> = <value>,<column name2> = <value>,...
[where <condition>]
```

- 没有指定where子句更新会更新到所有行
- 可以在UPDATE语句中使用子查询



### 删除数据

**删除方式**：

1. 从表中删除特定行
2. 从表中删除所有行

```sql
delete
from <table name>
where <condition>
```



- 如果想删除表中所有行，不要使用delete，应使用`truncate table`(truncate 缩短)
- 使用强制实施引用完整性的数据库，这样MySQL将不允许删除具有与其他表相关联的数据行



# 流程控制函数

| 函数                                                         | 效果                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| IF(expr1,expr2,expr3)                                        | 如果expr1为T，返回expr2，否则返回expr3                       |
| IFNULL(expr1,expr2)                                          | 如果expr1不为NULL则返回expr1，否则返回expr2                  |
| SELECT CASE when expr1 then expr2 when expr3 the expr4 else expr5 END; | 如果expr1为T，返回expr2，否则判断expr3，expr3为T，返回expr4，都不为T，返回expr5.（类似switch） |

# 创建和操纵表

## 查询表的结构

```sql
desc <table name>
```



## 创建表

```sql
create table <table name>
(
	<column name1> <data type> <constraint>,
	<column name2> <data type> <constraint>,
	...
	primary key (column name)	//指定主键
)engine=[InnoDB|MEMORY|MyISAM]
```

### Constraint

- 可指定`NULL` 或 `NOT NULL`，主键不允许指定NULL
- 可指定`AUTO_INCREMENT`,插入新行时MySQL自动对该列增量（通常配合primary key或unique使用）
- 对于指定了AUTO_INCREMENT的列，insert时指定一个值会覆盖自动生成的值，后续增量会从该值开始
- 对于AUTO_INCREMENT生成的值，可使用`last_insert_id()`函数获取
- 使用`default`指定默认值



### 引擎类型(engine)

1. InnoDB:一个可靠的事务处理引擎，不支持全文本搜索
2. MyISAM:一个性能极高的引擎，支持全文本搜索，但不支持事务处理
3. MEMORY:功能等同于MyISAM,但数据存储在内存中，速度很快（适合建立临时表）



- 引擎类型可以混用，但外键不能跨引擎，即使用一个引擎的表不能引用具有使用不同引擎的表的外键



## 更新表

```sql
alter table <table name1>
[add <column name> <data type> <constraint>]
[add constraint <constraint name> 
foreign key (column name1) references <table name2> (column name2)]

[drop column <column name> [cascade|restrict]]	//删除列
[modify column <column name> <data type>]	//修改列的数据类型
```



## 删除表

```sql
drop table <table name>
```



# 视图

- 视图是一个虚拟的表，只包含使用时动态检索数据的查询，本身不包含数据

## 为什么使用视图

1. `重用`SQL语句
2. 简化复杂的SQL操作
3. 使用表的组成部分而不是 整个表
4. 保护数据。可以给用户赋予表的特定部分数据的访问权限而不是整个表的访问权限
5. 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据



## 视图的规则和限制

1. 视图必须唯一命名
2. 对于可创建的视图没有数量限制
3. 创建视图必须要有足够的权限
4. 视图可以嵌套
5. order by可以用在视图中，但select该视图时也有一个order by，后者会覆盖前者
6. 视图不能索引，也不能有关联的触发器和默认值
7. 视图可以和表一起使用



## 使用视图

1. 视图用`CREATE VIEW`语句创建

   ```sql
   create view <view name> as
   <seleect ...>
   ```

2. 使用`SHOW CREATE VIEW <view name>`来查看创建视图的语句

### 好处

1. 利用视图简化复杂的联结
2. 用视图重新格式化检索出的数据
3. 用视图过滤不想要的数据
4. 使用视图简化计算字段



## 更新视图

- 通常情况下，视图是可更新的（即可对视图使用insert,update,delete）
- 更新一个视图将更新其`基表`
- 但有视图定义中有以下操作则不能对视图更新：
  1. 分组（使用group by和having）；
  2. 联结；
  3. 子查询；
  4. 并（union）；
  5. 聚集函数；
  6. DISTINCT；
  7. 导出（计算）列；



# 使用存储过程

- 类似其他编程语言中的函数



## 为什么要使用

1. 将复杂操作封装起来，直接调用
2. 防止错误
3. 简化对变动的管理
4. 提高性能（使用存储过程比使用单独的SQL语句要快）



- 总结：简单、安全、高性能

**缺陷**

1. 编写复杂
2. 需要创建存储过程的安全访问权限



## 使用存储过程

### 执行存储过程

- 使用`CALL`语句

  ```sql
  CALL <procedure name>([@parameter1],[@parameter2]...)
  ```



### 创建存储过程

```sql
create procedure <procedure name>()
BEGIN
	<some SELECT...>
END;
```



### 删除存储过程

``` sql
drop procedure productpricing;
```



### 使用参数

**参数类型**：

1. `OUT`:返回给调用者
2. `IN`:传递给存储过程
3. `INOUT`:对存储过程传入和传出



- 可通过一系列select语句检索值然后通过`INTO`关键字保存到相应的变量
- 可使用`delimiter`改变语句分隔符
- MySQL变量要以`@`开头



**EXAMPLE**

```sql
delimiter //
create procedure productpricing(
                              -> out pl decimal(8,2),
                              -> out ph decimal(8,2),
                              -> out pa decimal(8,2)
                              -> )
                              -> begin
                              -> select min(prod_price)
                              -> into pl
                              -> from products;
                              -> select max(prod_price)
                              -> into ph
                              -> from products;
                              -> select avg(prod_price)
                              -> into pa
                              -> from products;
                              -> end //
             
delimiter ;
```



```sql
call productpricing(@pl,@ph,@pa);
select @pl,@pw,@pa;
+------+--------+-------+
| @pl  | @pw    | @pa   |
+------+--------+-------+
| 2.50 | <null> | 16.13 |
+------+--------+-------+
```



### 建立智能存储过程

- `--`:表示注释
- `declare <var name> <data type>`:定义一个变量
- `if <boolean var> then`:类似其他编程语言的if



- `comment`为procedure添加一个comment值，不是必须的，但添加了会在show procedure status的结果中显示



**EXAMPLE**

```sql
create procedure orderTotal(
                              -> in ornum int,
                              -> in taxable boolean,
                              -> out total decimal(8,2))
                              -> begin
                              -> declare temp_total decimal(8,2);
                              -> declare taxRate int default 6;
                              -> select sum(item_price*quantity)
                              -> from orderitems
                              -> where order_num = ornum
                              -> into temp_total;
                              -> if taxable then
                              ->     select temp_total+temp_total/100*taxRate into temp_total;
                              -> end if;
                              -> select temp_total into total;
                              -> end;

```



```sql
--result                    
call orderTotal(20005,1,@total);
select @total;
+--------+
| @total |
+--------+
| 158.86 |
+--------+

call orderTotal(20005,0,@total);
select @total;
+--------+
| @total |
+--------+
| 149.87 |
+--------+
```

### 检查存储过程

- `show create procedure <procedure name> `:显示创建一个存储过程的create子句
- `show procedure status`:获得包括何时、由谁创建等详细信息的存储过程列表



# 触发器

- MySQL响应以下任意语句自动执行的一条MySQL语句（或位于BEGIN和END语句之间的一组语句）：
  - delete
  - update
  - insert

- 每个表最多设置`6`个触发器

## 创建触发器

在创建触发器时，需要给出4条信息：

- 唯一的触发器名
- 触发器关联的表
- 触发器响应的活动(delete,update,insert)
- 触发器何时执行(after,before)



```sql
CREATE TRIGGER <trigger name> 
[AFTER|BEFORE] [INSERT|DELETE|UPDATE] ON <table name>
```



## 删除触发器

```sql
DROP TRIGGER <trigger name>;
```



## 使用触发器

### INSERT触发器

- 该触发器内可以引用一个名为`NEW`的虚拟表，访问被插入的行
- 在`BEFORE INSERT`触发器中，`NEW`中的值也可以被更新（允许更改被插入的值）
- 对于`AUTO_INCREMENT`列，NEW在INSERT执行之前包含0，在INSERT执行之后包含新的自动值





# 安全管理

## 创建用户

```sql
CREATE USER <user name>@<log in host> IDENTIFIED BY <password>
```



## 删除用户

```sql
DROP USER <user name>@<log in host>
```



## 设置访问权限

```sql
GRANT <> ON <database name>.<table name> TO <user name>
```



- GRANT的反操作为`revoke`,用来撤销特定的权限

```sql
REVOKE <> ON <database name>.<table name> FROM <user name>
```





## 更改密码

```sql
SET PASSWORD FOR <user name> = Password(<psw>)
```

