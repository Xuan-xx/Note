

# 流

## 文件

- 存放数据的地方

- 文件在程序中是以流的方式传递的



**文件对象File**

- 实现了Serializable接口（可串行化）和Comparable接口（可比较）



**创建文件的相关构造器和方法**

- new File(String pathName)
- new File(File parent,String child)    //根据父目录文件+子路径创建
- new File(String parent,String child)    //根据父目录+子路径创建



- createNewFile()    //创建新文件



### 常用方法

- getName()
- getAbsolutePath()     //绝对路径
- getParent()    //父级目录
- length()    //文件大小（按字节）
- exists()    //文件是否存在
- isFile()    //是不是文件
- isDirectory()    //是不是目录



### 目录

- mkdir()     //创建目录
- mkdirs()    //创建多级目录
- delete()    //删除文件或目录（只能删除空目录）



## IO流

分类

1. 按操作单位：字节流（8bit）    二进制文件、字符流    文件
2. 按流向：输入流、输出流
3. 按流的角色：结点流、处理流\包装流



| 抽象基类 | 字节流       | 字符流 |
| -------- | ------------ | ------ |
| 输入流   | InputStream  | Reader |
| 输出流   | OutputStream | Writer |



### InputStream



**关系图**

![](javaIO/InputStream.jpg)



#### FileInputStream

