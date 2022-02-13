# Servlet API

- 用于处理HTTP请求，并响应数据给客户端

```ascii
                 ┌───────────┐
                 │My Servlet │
                 ├───────────┤
                 │Servlet API│
┌───────┐  HTTP  ├───────────┤
│Browser│<──────>│Web Server │
└───────┘        └───────────┘
```



- 实现类总是继承`HttpServlet`，然后覆写`doGet`和`doPost`方法
- 使用注解`@WebServlet(urlPatterns="")`配置处理的请求路径



## doGet

- `doGet()`方法传入了`HttpServletRequest`和`HttpServletResponse`两个对象，分别代表HTTP请求和响应。



### 发送响应对象

- 使用`setContentType()`方法设置响应类型
- 使用`getWriter()`方法获取输出流并写入响应内容