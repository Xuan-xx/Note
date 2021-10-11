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
3. Socket允许把网络连接当成一个流，数据在两个Socket之间通过IO传输



- 基于Socket有TCP编程和UDP编程

### TCP编程

- 构造Socket要传入`ip`和`端口号`
- Socket只能接收字节流
- 网络编程发送信息需要结束标记，调用方法socket.shutdownOutput()（字节流）
- 结束标记也可以是writer.newLine()（字符流），不过要求对方使用readLine()接收
- 使用字符流需要flush()才能写入
- 写入时先调用`newLine()`再调用`flush()`



#### ServerSocket

- 构造器接收一个端口，该端口没有被占用
- 这个类可以通过`accept()`方法返回多个Socket



### UDP编程

1. 类`DatagramSocket`和`DatagramPacket`[数据包/数据报]
2. DatagramPacket对象封装了UDP数据报，在数据报中包含了发送端的ip地址和端口号以及接收端的ip地址和端口号
3. UDP协议中每个数据包给定了完整的地址信息，因此`无需`建立发送方和接收方的连接



- 没有明确的服务端和客户端，演变成数据的发送端和接收端（可以互相转化）
- 接收和发送数据是通过DatagramSocket对象完成
- 将数据封装到DatagramPacket对象/装包
- 当接收到DatagramPacket对象需要拆包
- DatagramSocket对象可以指定在哪个端口接收数据
