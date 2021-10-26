# 约束

## 1. not null

- 非空 不能为null

## 2. unique

- 字段的值不能重复
- 如果没有指定not null，约束为unique的列可以有多个null
- 一张表可以有多个unique

## 3. primary key

### 创建

- 定义列数据类型时: 

  ```sql
  <column name> <data type> PRIMARY KEY
  ```

- 定义列数据类型后：

  ```sql
  PRIMARY KEY(column1,column2...)
  ```

  



### 注意事项

1. 主键不能重复且不能为null
2. 一张表只能有一个主键，可以有复合主键

## 4. foreign key

- 用于定义主表和从表的关系：外键约束定义在从表上，主表则必须具有主键约束或是unique约束，当定义外键约束后，要求外键列数据必须在主表的主键列存在或是为null



### 细节

1. 外键指向的表的字段，约束要求是primary key或unique
2. 只有`innodb`类型的表才支持外键
3. 从表外键字段的类型要与主表主键字段类型一致（长度可以不同）
4. 一旦建立主外键关系，数据就不能随意删除了
5. 当定义外键约束后，要求外键列数据必须在主表的主键列存在或是为null（前提是外键列可以为null）

### 创建

```sql
FOREIGN KEY (column name) references <table name>(<column name>)
```



## 5. check

- 用于强制行数据必须满足的条件
- MySQL 5.7不支持check（只支持语法，不会运行）