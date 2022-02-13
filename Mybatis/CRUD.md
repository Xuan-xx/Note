# CRUD

1. 使用与SQL语句同名的标签定义
2. DML语句需要手动提交
3. DML语句绑定的抽象方法可以返回`Integer` `Long` `Boolean` `void`类型



# insert

## 获取自增主键值

- 使用`useGeneratedKeys`开启功能
- 使用`keyProperty`表示封装到哪一个属性