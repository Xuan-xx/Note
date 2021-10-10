# 网络编程

## InetAddress

- getLocalHost()    获取本机的InetAddress对象
- getByName()    根据指定主机名/域名获取InetAddress对象
- getHostAddress()    通过InetAddress对象获得对应ip地址
- getHostName()    通过InetAddress对象获得对应域名/主机名

---

## Socket(套接字)

1. 通信的两端都要有Socket，是两台机器通信的端点
2. 网络通信就是Socket间的通信
3. Socket允许把网络连接当成一个流，数据在两个Socker之间通过IO传输



- 基于Socket有TCP编程和UDP编程
- 网络编程发送信息需要结束标记，调用方法socket.shutdownOutput()
- 结束标记也可以是writer.newLine()（字符流），不过要求对方使用readLine()接收
- 使用字符流需要flush()才能写入



### ServerSocket

- 构造器接收一个端口，该端口没有被占用
- 这个类可以通过`accept()`方法返回多个Socket

