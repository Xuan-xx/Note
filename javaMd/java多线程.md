# java多线程

## 创建多线程

- 实例化一个`Thread`，然后调用它的start()方法；
- 新线程执行指定代码的两种方法：
  1. 从`Thread`派生一个自定义类，然后覆写run()方法；
  2. 创建`Thread`实例时，传入一个`Runnable`实例；（或者用lambda语法）
- Thread.setPriority(int n)方法可以设定线程的优先级

---

## 线程的状态

线程的状态有以下几种：

- New:新创建的进程，尚未执行；
- Runnable:运行中的进程，正在执行`run()`方法的java代码；
- Waiting:运行中的进程，因为某些操作正在等待；
- Timed Waiting:运行中的进程，因为执行`sleep()`正在计时等待；
- Blocked:运行中的进程，因为某些操作被阻塞而挂起；
- Terminated：线程已终止，因为`run()`方法执行完毕。

用一个状态转移图表示如下：

```ascii
         ┌─────────────┐
         │     New     │
         └─────────────┘
                │
                ▼
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
 ┌─────────────┐ ┌─────────────┐
││  Runnable   │ │   Blocked   ││
 └─────────────┘ └─────────────┘
│┌─────────────┐ ┌─────────────┐│
 │   Waiting   │ │Timed Waiting│
│└─────────────┘ └─────────────┘│
 ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
                │
                ▼
         ┌─────────────┐
         │ Terminated  │
         └─────────────┘
```



一个线程还可以等待另一个线程直到其运行结束。例如，`main`线程在启动`t`线程后，可以通过`t.join()`等待`t`线程结束后再继续运行；



---

## 中断线程

1. 在其他线程对目标线程调用interrupt()方法，目标线程需要反复检验自身状态是否为interrupted状态，如果是就立刻结束运行。

2. 设置一个标志位表示线程是否中断，通常为`running`。外部线程将running标记为false可以中断线程。

   - 标志位boolean running要使用`volatile`关键字

   - 为什么要对线程间共享的变量用关键字`volatile`声明？这涉及到Java的内存模型。在Java虚拟机中，变量的值保存在主内存中，但是，当线程访问变量时，它会先获取一个副本，并保存在自己的工作内存中。如果线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时间是不确定的！

     ```ascii
     ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
                Main Memory
     │                               │
        ┌───────┐┌───────┐┌───────┐
     │  │ var A ││ var B ││ var C │  │
        └───────┘└───────┘└───────┘
     │     │ ▲               │ ▲     │
      ─ ─ ─│─│─ ─ ─ ─ ─ ─ ─ ─│─│─ ─ ─
           │ │               │ │
     ┌ ─ ─ ┼ ┼ ─ ─ ┐   ┌ ─ ─ ┼ ┼ ─ ─ ┐
           ▼ │               ▼ │
     │  ┌───────┐  │   │  ┌───────┐  │
        │ var A │         │ var C │
     │  └───────┘  │   │  └───────┘  │
        Thread 1          Thread 2
     └ ─ ─ ─ ─ ─ ─ ┘   └ ─ ─ ─ ─ ─ ─ ┘
     ```

     这会导致如果一个线程更新了某个变量，另一个线程读取的值可能还是更新前的。例如，主内存的变量`a = true`，线程1执行`a = false`时，它在此刻仅仅是把变量`a`的副本变成了`false`，主内存的变量`a`还是`true`，在JVM把修改后的`a`回写到主内存之前，其他线程读取到的`a`的值仍然是`true`，这就造成了多线程之间共享的变量不一致。

     因此，`volatile`关键字的目的是告诉虚拟机：

     - 每次访问变量时，总是获取主内存的最新值；
     - 每次修改变量后，立刻回写到主内存。

     `volatile`关键字解决的是可见性问题：当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

     如果我们去掉`volatile`关键字，运行上述程序，发现效果和带`volatile`差不多，这是因为在x86的架构下，JVM回写主内存的速度非常快，但是，换成ARM的架构，就会有显著的延迟。

---

## 守护线程（Daemon Thread）

- Java程序入口就是由JVM启动`main`线程，`main`线程又可以启动其他线程。当所有线程都运行结束时，JVM退出，进程结束。

  如果有一个线程没有退出，JVM进程就不会退出。所以，必须保证所有线程都能及时结束。

- 守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。

- 创建守护线程：在调用`start()`前调用`setDaemon(true)`;



---

## 线程同步

- 多个线程同时读取共享变量，会出现数据不一样的问题
- 使用`synchronized`加锁解决
- 我们来概括一下如何使用`synchronized`：
  1. 找出修改共享变量的线程代码块；
  2. 选择一个共享实例作为锁；
  3. 使用`synchronized(lockObject) { ... }`。



### 不需要synchronized的操作

JVM规范定义了几种原子操作：

- 基本类型（`long`和`double`除外）赋值，例如：`int n = m`；
- 引用类型赋值，例如：`List<String> list = anotherList`。
- `long`和`double`是64位数据，JVM没有明确规定64位赋值操作是不是一个原子操作，不过在x64平台的JVM是把`long`和`double`的赋值作为原子操作实现的。



单行赋值语句不需要加锁，但多行赋值语句也需要加锁。

---

## 同步方法

我们知道Java程序依靠`synchronized`对线程进行同步，使用`synchronized`的时候，锁住的是哪个对象非常重要。

让线程自己选择锁对象往往会使得代码逻辑混乱，也不利于封装。更好的方法是把`synchronized`逻辑封装起来。



### 线程安全类

- 有部分类在设计时就设计为线程安全类
- 一些不变类，成员变量都是`final`，多线程访问时只能读不能写，也是线程安全的
- 类似`Math`这类只提供静态方法，没有成员变量的类也是线程安全的



*大部分的类都是线程不安全的，但如果只读取不写入，也是可以在线程间安全共享的*



当我们锁住的是`this`实例时，实际上可以用`synchronized`修饰这个方法。下面两种写法是等价的：

```
public void add(int n) {
    synchronized(this) { // 锁住this
        count += n;
    } // 解锁
}
public synchronized void add(int n) { // 锁住this
    count += n;
} // 解锁
```

因此，用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁。

我们再思考一下，如果对一个静态方法添加`synchronized`修饰符，它锁住的是哪个对象？

```
public synchronized static void test(int n) {
    ...
}
```

对于`static`方法，是没有`this`实例的，因为`static`方法是针对类而不是实例。但是我们注意到任何一个类都有一个由JVM自动创建的`Class`实例，因此，对`static`方法添加`synchronized`，锁住的是该类的`Class`实例。上述`synchronized static`方法实际上相当于：

```java
public class Counter {
    public static void test(int n) {
        synchronized(Counter.class) {
            ...
        }
    }
}
```



---

## 死锁

- java的锁是可重入锁（能被同一个线程反复获取的锁）
- 可能会产生死锁



---

## wait()和notify

### wait()

1. 是一个定义在`Object`类的`native`方法（由JVM的c代码实现）；
2. 必须在`synchronized`块中才能调用（`wait()`方法调用时，会*释放*线程获得的锁，`wait()`方法返回后，线程又会重新试图获得锁）；



### notify()

- 唤醒一个等待线程
- notifyAll()唤醒所有等待线程

---

## 使用ReentrantLock

- 是一个可重入锁，可以代替synchronized，更加安全
- 必须先获取锁，再进入try｛...｝代码块，最后在`finally`中释放锁
- 可以使用tryLock()尝试获取锁



---

## 使用Condition

- 实现`synchronized`中的`wait`和`notify`
- `Condition`提供的`await()`、`signal()`、`signalAll()`原理和`synchronized`锁对象的`wait()`、`notify()`、`notifyAll()`是一致的，并且其行为也是一样的：
  - `await()`会释放当前锁，进入等待状态；
  - `signal()`会唤醒某个等待线程；
  - `signalAll()`会唤醒所有等待线程；
  - 唤醒线程从`await()`返回后需要重新获得锁。
- 和`tryLock()`类似，`await()`可以在等待指定时间后，如果还没有被其他线程通过`signal()`或`signalAll()`唤醒，可以自己醒来
