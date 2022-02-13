# 基本用法

`<%-- --%>`:注释

`<% %>`:Java代码

`<%= xxx %>`:快速输出一个变量的值



# JSP内置变量

- out：表示HttpServletResponse的printWriter;
- session：表示HttpSession对象
- request：表示HttpServletRequest对象



# JSP高级功能

- 通过`page`指令引入Java类

  ```java
  <%@ page import="java.io.*" %>
  <%@ page import="java.util.*" %>
  ```



- 使用`include`指令可以引入另一个JSP文件：

  ```java
  <html>
  <body>
      <%@ include file="header.jsp"%>
      <h1>Index Page</h1>
      <%@ include file="footer.jsp"%>
  </body>
  ```

