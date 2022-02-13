# 多线程



## 线程分类

1. 用户线程
   - 也叫工作线程，当线程的任务执行完或者被通知时结束
2. 守护线程（daemon）
   - 一般是为用户线程服务的，当所有的用户线程结束，守护线程自动结束
   - 常见的守护线程有垃圾回收机制



## 创建线程的两种方式

1. 继承Thread类，重写run()方法

   - 创建一个实例对象，使用start()启动一个线程

2. 实现Runnable接口，实现run()方法

   - 当一个类已经继承了其他类，只能通过实现Runnable接口实现多线程

   - 一个类实现Runnable接口后没有start()方法，要启动一个线程需要新建一个Thread对象，并将该类的一个对象传入新建的Thread对象，再由Thread对象调用start()    ->   使用了一个设计模式[代理模式]

     **[代理模式]**:一个类1实现了一个接口i，但实现的方法不能满足需求。这时有另一个类2实现了同一个接口，实现的方法满足了需求。类1对象可以通过接口i接收一个类2的对象，再调用增强后的方法。（个人理解）

   > Thread有一个构造器可以接收实现了Runnable接口的对象（假定为CiR）
   >
   > Thread调用start()后，start()调用start0()
   >
   > start0()调用run()
   >
   > run()方法源代码如下：
   >
   > ```java
   > @Override
   > public void run() {
   >     if (target != null) {	//target为构造器接收的Runnable CiR
   >         target.run();
   >     }
   > }
   > ```
   >
   > 最终调用了CiR实现的run()方法

   - 使用实现Runnable接口的方式更适合多个线程共享一个资源的情况



- 当一个类继承了Thread，该类就可以当作线程使用
- 我们需要重写run方法，写上自己的业务代码
- Thread类的run实现了Runnable接口的run方法
- 直接调用实例对象的run方法不会创建一个新的线程



## Thread类常用方法

### 第一组

1. static void sleep(int n)     让程序暂停n毫秒

2. Thread.currentThread().getName()    获取当前线程的名字

3. setName()    设置线程的名字

4. setPriority()    设置线程的优先级

   > MIN_PRIORITY     1
   >
   > MAX_PRIORITY     10
   >
   > NORM_PRIORITY    5

5. getPriority()    获取线程的优先级

6. start()    实例对象调用后会创建一个线程，调用run方法

7. interrupt()    中断线程（不是终止线程），一般用于中断正在休眠的线程



### 第二组

1. yield()    线程让出cpu，让其他线程执行，但让出的时间不确定，不一定让出成功
2. join()    线程插队。线程如果插队成功，则肯定先执行完插入的线程的所有任务



## start()分析

1. ​    public synchronized void start() ｛

   ​		start0();

   ​	｝

   ​    private native void start0();

2. start0是本地方法，是JVM调用，底层是C/C++实现

3. 真正实现多线程效果的是start0(),而不是run()



## 线程终止

1. 当线程完成任务后，会自动退出
2. 还可以通过**使用变量**来控制run方法退出的方式停止线程，即通知方式



## 线程状态

- New    Runnable(Ready and Running)    TimedWaiting(超时等待)    Waiting    Blocked     Teminated(结束)

![](java多线程/线程状态图.jpeg)



## 同步(synchronized)

- 保证数据在任何同一时刻，最多只有一个线程访问，已保证数据的完整性

### 同步具体方法

1. 同步代码块

   ```java
   synchronized(AClass){
   	//需要被同步的代码
   }
   ```

   

2. 同步方法

```java
public synchronized void m(String name){
	//需要被同步的代码
}
```



### 互斥锁

- 每个对象都对应于一个可称为“互斥锁”的标记，这个标记用于保证在任一时刻只能有一个线程访问该对象
- 关键字synchronized用来与对象的互斥锁联系。当某个对象用synchronized修饰时，表示该对象在任一时刻只能有一个线程访问
- 局限性：导致性能降低
- 同步方法（非静态的）的锁可以是this（默认），也可以是其他对象（要求是同一对象）
- 同步方法（静态的）的锁为(默认)当前类本身（当前类.class）
- 多个线程的锁为同一个对象即可



### 释放锁

- 下面操作会释放锁

  1. 当前线程的同步方法、同步代码块执行结束
  2. 当前线程在同步代码块、同步方法中遇到break、return
  3. 当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导致异常结束
  4. 当前线程在同步代码块、同步方法中执行了线程对象的wait()方法，当前线程暂停，并释放锁

- 下面操作不会释放锁

  1. 线程执行同步代码块或同步方法时，程序调用sleep()、yield()方法暂停当前线程的执行，不会释放锁

  2. 线程执行同步代码块时，其他线程调用了该线程的suspend()方法将该线程挂起，不会释放锁

     >避免使用suspend()和resume()来控制线程，方法不再推荐使用

 

# volatile(易变的)

- 用该关键字声明的变量的读取和写入一定会同步到主内存里，而不是直接从Cache里读取



## 作用

- 当程序的数据在不同的线程或者CPU核心里面更新时，不同的线程或CPU核心有自己各自的缓存，很可能在A线程的更新，B线程无法看到。这时就需要使用volatile关键字。



# 线程池

- 使用`ExecutorService`表示线程池



**实现类**（创建这些线程池的方法被封装到`Executors`类中）：

- `FixedThreadPool`：线程数量固定的线程池
- `CachedThreadPool`：线程数根据任务动态调整的线程池
- `SingleThreadExecutor`：根据单线程执行的线程池
- `ScheduledThreadPool`：多用于需要反复执行的任务
