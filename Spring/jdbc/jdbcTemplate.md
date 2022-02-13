# 语句选择

- 对于简单的查询语句，优先使用`query()`和`queryForObject()`，因为只需要提供`SQL语句`、`参数`和`RowMapper`
- 对于更新操作，优先选择`update()`，只需要提供`SQL语句`和`参数`
- 对于复杂的操作使用`execute(ConnectionCallBack)`，获取`Connection()`后可以做任何的JDBC操作