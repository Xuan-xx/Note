# java网络编程

## TCP编程

### 服务器端

- Java标准库提供了`ServerSocket`来实现对指定IP和指定端口的监听

- 如果`ServerSocket`监听成功，我们就使用一个无限循环来处理客户端的连接：

  ```java
  for (;;) {
      Socket sock = ss.accept();
      Thread t = new Handler(sock);
      t.start();
  }
  ```

- 客户端程序通过：

  ```
  Socket sock = new Socket("localhost", 6666);
  ```

  连接到服务器端，注意上述代码的服务器地址是`"localhost"`，表示本机地址，端口号是`6666`。如果连接成功，将返回一个`Socket`实例，用于后续通信。



### Socket流

当Socket连接创建成功后，无论是服务器端，还是客户端，我们都使用`Socket`实例进行网络通信。因为TCP是一种基于流的协议，因此，Java标准库使用`InputStream`和`OutputStream`来封装Socket的数据流，这样我们使用Socket的流，和普通IO流类似：

```
// 用于读取网络数据:
InputStream in = sock.getInputStream();
// 用于写入网络数据:
OutputStream out = sock.getOutputStream();
```

最后我们重点来看看，为什么写入网络数据时，要调用`flush()`方法。

如果不调用`flush()`，我们很可能会发现，客户端和服务器都收不到数据，这并不是Java标准库的设计问题，而是我们以流的形式写入数据的时候，并不是一写入就立刻发送到网络，而是先写入内存缓冲区，直到缓冲区满了以后，才会一次性真正发送到网络，这样设计的目的是为了提高传输效率。如果缓冲区的数据很少，而我们又想强制把这些数据发送到网络，就必须调用`flush()`强制把缓冲区数据发送出去。



---

## UDP编程

### 服务器端

- 在服务器端，使用UDP也需要监听指定的端口。Java提供了`DatagramSocket`来实现这个功能

- 要接收一个UDP数据包，需要准备一个`byte[]`缓冲区，并通过`DatagramPacket`实现接收：

  ```java
  byte[] buffer = new byte[1024];
  DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
  ds.receive(packet);
  ```

- 当服务器收到一个DatagramPacket后，通常必须立刻回复一个或多个UDP包，因为客户端地址在DatagramPacket中，每次收到的DatagramPacket可能是不同的客户端，如果不回复，客户端就收不到任何UDP包。

  发送UDP包也是通过`DatagramPacket`实现的，发送代码非常简单：

  ```java
  byte[] data = ...
  packet.setData(data);
  ds.send(packet);
  ```



### 客户端

- 客户端打开一个`DatagramSocket`使用以下代码：

  ```java
  DatagramSocket ds = new DatagramSocket();
  ds.setSoTimeout(1000);
  ds.connect(InetAddress.getByName("localhost"), 6666);
  ```

- 这个`connect()`方法不是真连接，它是为了在客户端的`DatagramSocket`实例中保存服务器端的IP和端口号，确保这个`DatagramSocket`实例只能往指定的地址和端口发送UDP包，不能往其他地址和端口发送。这么做不是UDP的限制，而是Java内置了安全检查。