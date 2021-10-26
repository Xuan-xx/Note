# 原理

**没有索引为什么会慢？**

- 没有索引查询要进行全表扫描

**建立了索引为什么会快？**

- 索引运用`树`的数据结构建立，大大提高查询效率

**索引的代价**

- 占用磁盘空间
- 对表进行dml操作(delete,update,insert)的效率影响（修改表会改变索引的结构）



# 类型

1. 主键索引，主键自动的为主索引(primary key)
2. 唯一索引(unique)
3. 普通索引(index)
4. 全文索引(fulltext)[使用于MyISAM]
   - 开发中考虑使用全文搜索Solr和ElasticSearch(ES)，不使用MySQL自带的全文索引



# 创建索引

- 如果某列的值，是不会重复的值，优先使用唯一索引，否则使用普通索引

## 唯一索引

```sql
CREATE UNIQUE INDEX <index name> on <table name>(column name);
```



## 普通索引

方式1:

```sql
CREATE INDEX <index name> on <table name>(column name);
```



方式2:

```sql
ALTER TABLE <table name> ADD INDEX <index name>(column name)
```



## 主键索引

方式1:

- 在创建表时指定`primary key`



方式2:

```sql
ALTER TABLE <table name> ADD PRIMARY KEY (column name)
```



# 删除索引

- 展示索引名字：show index from <table name>



方式1:

```sql
DROP INDEX <index name> ON <table name>
```



方式2（删除主键索引）：

```sql
ALTER TABLE <table name> DROP PRIMARY KEY
```



# 修改索引

- 先删除，后添加



# 查询索引

方式1:

```sql
SHOW INDEX FROM <table name>
```

方式2:

```sql
SHOW INDEXS FROM <table name>
```



方式3:

```sql
SHOW KEYS FROM <table name>
```



方式4:

```sql
DESC <table name>	//不详细
```

