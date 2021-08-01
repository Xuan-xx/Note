# java IO

## File对象

- 用于操作文件和目录
- 要构造一个File对象需要传入文件路径（绝对路径、相对路径）

### File对象路径表示方式

1. `getPath()` 构造方法传入的路径

2. `getAbsolutePath` 绝对路径

3. `getCanonicalPath` 规范路径（规范路径就是把`.`和`..`转换成标准的绝对路径后的路径）

   

### 文件和目录

- 构建一个File对象是即使传入的路径文件不存在也不会发生错误，因为创建File对象不会导致任何磁盘操作，只有调用File对象的某些方法时才会对磁盘操作。



### File对象创建和删除文件

- `createNewFile()` 创建文件，`delete()` 删除文件。
- `createTempFile()` 创建临时文件，`deleteOnExit` 在JVM退出时自动删除文件。



### 遍历文件和目录

- File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`提供了一系列重载方法过滤不想要的文件和目录。



---

## InputStream

- InputStream是抽象类，是所有输入流的超类
- 定义了重要方法int read(),方法签名如下：

```java
public abstract int read() throws IOException;
```

- 需要通过`close()`关闭流

- 为了避免发生IO错误时无法关闭流，有以下写法：

  1. `try ... finally`

     ```java
     public void readFile() throws IOException {
         InputStream input = null;
         try {
             input = new FileInputStream("src/readme.txt");
             int n;
             while ((n = input.read()) != -1) { // 利用while同时读取并判断
                 System.out.println(n);
             }
         } finally {
             if (input != null) { input.close(); }
         }
     }
     ```
  
  2. `try(resource)`需要编写`try`语句，让编译器自动为我们关闭资源。推荐的写法如下：
  
     ```java
     public void readFile() throws IOException {
         try (InputStream input = new FileInputStream("src/readme.txt")) {
             int n;
             while ((n = input.read()) != -1) {
                 System.out.println(n);
             }
         } // 编译器在此自动为我们写入finally并调用close()
     }
     ```
  
     实际上，编译器并不会特别地为`InputStream`加上自动关闭。编译器只看`try(resource = ...)`中的对象是否实现了`java.lang.AutoCloseable`接口，如果实现了，就自动加上`finally`语句并调用`close()`方法。`InputStream`和`OutputStream`都实现了这个接口，因此，都可以用在`try(resource)`中。



### 缓冲

在读取流的时候，一次读取一个字节并不是最高效的方法。很多流支持一次性读取多个字节到缓冲区，对于文件和网络流来说，利用缓冲区一次性读取多个字节效率往往要高很多。`InputStream`提供了两个重载方法来支持读取多个字节：

- `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
- `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

利用上述方法一次读取多个字节时，需要先定义一个`byte[]`数组作为缓冲区，`read()`方法会尽可能多地读取字节到缓冲区， 但不会超过缓冲区的大小。`read()`方法的返回值不再是字节的`int`值，而是返回实际读取了多少个字节。如果返回`-1`，表示没有更多的数据了。



### 阻塞

在调用`InputStream`的`read()`方法读取数据时，我们说`read()`方法是阻塞（Blocking）的。它的意思是，对于下面的代码：

```
int n;
n = input.read(); // 必须等待read()方法返回才能执行下一行代码
int m = n;
```

执行到第二行代码时，必须等`read()`方法返回后才能继续。因为读取IO流相比执行普通代码，速度会慢很多，因此，无法确定`read()`方法调用到底要花费多长时间。



### ByteArrayInputStream

- 把一个byte[]在内存中变成一个`InputStream`;
- 主要用于测试（不用真正的在磁盘中导入一个文件，直接在内存中使用ByteArrayInputStream转化为InputStream)



---

## OutputStream

- flush(),可以强制把缓冲区的内容输出



### FileOutputStream

- write()写入文件
- 重载方法write(byte[]),配合String.getBytes()一次写入若干字节



### 阻塞

与InputStream中的read()类似



### 实现类

`ByteArrayOutputStream`可以在内存中模拟一个OutputStream

`ByteArrayOutputStream`实际上是把一个`byte[]`数组在内存中变成一个`OutputStream`，虽然实际应用不多，但测试的时候，可以用它来构造一个`OutputStream`。

同时操作多个`AutoCloseable`资源时，在`try(resource) { ... }`语句中可以同时写出多个资源，用`;`隔开。例如，同时读写两个文件：

```java
// 读取input.txt，写入output.txt:
try (InputStream input = new FileInputStream("input.txt");
     OutputStream output = new FileOutputStream("output.txt"))
{
    input.transferTo(output); // transferTo的作用是?
}
```



---

## Filter模式

- 通过少量类实现多种组合

为了解决依赖继承会导致子类数量失控的问题，JDK首先将`InputStream`分为两大类：

一类是直接提供数据的基础`InputStream`，例如：

- FileInputStream
- ByteArrayInputStream
- ServletInputStream
- ...

一类是提供额外附加功能的`InputStream`，例如：

- BufferedInputStream
- DigestInputStream
- CipherInputStream
- ...

当我们需要给一个“基础”`InputStream`附加各种功能时，我们先确定这个能提供数据源的`InputStream`，因为我们需要的数据总得来自某个地方，例如，`FileInputStream`，数据来源自文件：

```
InputStream file = new FileInputStream("test.gz");
```

紧接着，我们希望`FileInputStream`能提供缓冲的功能来提高读取的效率，因此我们用`BufferedInputStream`包装这个`InputStream`，得到的包装类型是`BufferedInputStream`，但它仍然被视为一个`InputStream`：

```
InputStream buffered = new BufferedInputStream(file);
```

最后，假设该文件已经用gzip压缩了，我们希望直接读取解压缩的内容，就可以再包装一个`GZIPInputStream`：

```
InputStream gzip = new GZIPInputStream(buffered);
```

无论我们包装多少次，得到的对象始终是`InputStream`，我们直接用`InputStream`来引用它，就可以正常读取：

```ascii
┌─────────────────────────┐
│GZIPInputStream          │
│┌───────────────────────┐│
││BufferedFileInputStream││
││┌─────────────────────┐││
│││   FileInputStream   │││
││└─────────────────────┘││
│└───────────────────────┘│
└─────────────────────────┘
```



上述称为**装饰器模式**，用少量类实现各种功能的组合：

```ascii
┌─────────────┐
                 │ InputStream │
                 └─────────────┘
                       ▲ ▲
┌────────────────────┐ │ │ ┌─────────────────┐
│  FileInputStream   │─┤ └─│FilterInputStream│
└────────────────────┘ │   └─────────────────┘
┌────────────────────┐ │     ▲ ┌───────────────────┐
│ByteArrayInputStream│─┤     ├─│BufferedInputStream│
└────────────────────┘ │     │ └───────────────────┘
┌────────────────────┐ │     │ ┌───────────────────┐
│ ServletInputStream │─┘     ├─│  DataInputStream  │
└────────────────────┘       │ └───────────────────┘
                             │ ┌───────────────────┐
                             └─│CheckedInputStream │
                               └───────────────────┘
```

类似的，`OutputStream`也是以这种模式来提供各种功能：

```ascii
                  ┌─────────────┐
                  │OutputStream │
                  └─────────────┘
                        ▲ ▲
┌─────────────────────┐ │ │ ┌──────────────────┐
│  FileOutputStream   │─┤ └─│FilterOutputStream│
└─────────────────────┘ │   └──────────────────┘
┌─────────────────────┐ │     ▲ ┌────────────────────┐
│ByteArrayOutputStream│─┤     ├─│BufferedOutputStream│
└─────────────────────┘ │     │ └────────────────────┘
┌─────────────────────┐ │     │ ┌────────────────────┐
│ ServletOutputStream │─┘     ├─│  DataOutputStream  │
└─────────────────────┘       │ └────────────────────┘
                              │ ┌────────────────────┐
                              └─│CheckedOutputStream │
                                └────────────────────┘
```



### 编写FilterInputStream

我们可以自己编写FilterInputStream，用到InputStream中



---

## Reader

- 与InputStream的不同是，InputStream是字节流，Reader是字符流
- Reader.read()类似inputStream.read()
- charArrayReader()、StringReader()类似byteArrayInputStteam()



### inputStreamReader

- 将inputStream转换为Reader



## Writer

- FileWriter()类似FileReader()
- charArrayWriter()、StringWriter()类似charArrayReader()、StringReader()
- OutputStreamWriter()类似inputStreamWriter()
