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

---

## 使用ReadWriteLock

- 创建一个`ReadWriteLock`实例，然后分别获取读锁和写锁，把读写操作分别用读锁和写锁来加锁，读取时，多个线程可以同时获取读锁，这样大大提高了并发读的效率。
- 使用条件：同一个数据，有大量线程读取，但仅有少量线程修改。

```java
public class Counter {
    private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
    private final Lock rlock = rwlock.readLock();
    private final Lock wlock = rwlock.writeLock();
    private int[] counts = new int[10];

    public void inc(int index) {
        wlock.lock(); // 加写锁
        try {
            counts[index] += 1;
        } finally {
            wlock.unlock(); // 释放写锁
        }
    }

    public int[] get() {
        rlock.lock(); // 加读锁
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            rlock.unlock(); // 释放读锁
        }
    }
}
```



*潜在问题*：这是一种悲观的读锁，在读数据时，不能写入数据。必须等待读线程释放读锁后写线程才能获取写锁。

---

## 使用StampedLock

- 解决`ReadWriteLock`悲观读的问题
- 和`ReadWriteLock`相比，写入的加锁基本一致。
- 读取要先通过通过`tryOptimisticRead()`获取一个乐观读锁，并返回版本号。接着进行读取，读取完成后，我们通过`validate()`去验证版本号，如果在读取过程中没有写入，版本号不变，验证成功，我们就可以放心地继续后续操作。如果在读取过程中有写入，版本号会发生变化，验证将失败。在失败的时候，我们再通过获取悲观读锁再次读取。由于写入的概率不高，程序在绝大部分情况下可以通过乐观读锁获取数据，极少数情况下使用悲观读锁获取数据。



***代价***:

1. 代码更复杂
2. `StampedLock`是一个不可重入锁。



```java
public class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock(); // 获取写锁
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp); // 释放写锁
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
        // 注意下面两行代码不是原子操作
        // 假设x,y = (100,200)
        double currentX = x;
        // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
        double currentY = y;
        // 此处已读取到y，如果没有写入，读取是正确的(100,200)
        // 如果有写入，读取是错误的(100,400)
        if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
            stamp = stampedLock.readLock(); // 获取一个悲观读锁
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp); // 释放悲观读锁
            }
        }
        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```



---

## 使用Concurrent集合

- `java.util.concurrent`包提供了对应的并发集合类

| interface | non-thread-safe         | thread-safe                              |
| :-------- | :---------------------- | :--------------------------------------- |
| List      | ArrayList               | CopyOnWriteArrayList                     |
| Map       | HashMap                 | ConcurrentHashMap                        |
| Set       | HashSet / TreeSet       | CopyOnWriteArraySet                      |
| Queue     | ArrayDeque / LinkedList | ArrayBlockingQueue / LinkedBlockingQueue |
| Deque     | ArrayDeque / LinkedList | LinkedBlockingDeque                      |



因为所有的同步和加锁的逻辑都在集合内部实现，对外部调用者来说，只需要正常按接口引用，其他代码和原来的非线程安全代码完全一样。即当我们需要多线程访问时，把：

```
Map<String, String> map = new HashMap<>();
```

改为：

```java
Map<String, String> map = new ConcurrentHashMap<>();
```



---

## 使用Atomic

- Java的`java.util.concurrent`包除了提供底层锁、并发集合外，还提供了一组原子操作的封装类，它们位于`java.util.concurrent.atomic`包。
- 我们以`AtomicInteger`为例，它提供的主要操作有：
  - 增加值并返回新值：`int addAndGet(int delta)`
  - 加1后返回新值：`int incrementAndGet()`
  - 获取当前值：`int get()`
  - 用CAS方式设置：`int compareAndSet(int expect, int update)`



我们利用`AtomicLong`可以编写一个多线程安全的全局唯一ID生成器：

```java
class IdGenerator {
    AtomicLong var = new AtomicLong(0);

    public long getNextId() {
        return var.incrementAndGet();
    }
}
```



---

## 使用线程池

- `ExecutorService`接口表示线程池
- 因为`ExecutorService`只是接口，Java标准库提供的几个常用实现类有：
  - FixedThreadPool：线程数固定的线程池；
  - CachedThreadPool：线程数根据任务动态调整的线程池；
  - SingleThreadExecutor：仅单线程执行的线程池。
- `submit()`提交任务，`shutdown()`会等待正在执行的任务完成然后关闭线程池，`shutdownNow()`会立刻停止正在执行的任务，`awaitTermination()`则会等待指定的时间让线程池关闭



如果我们把线程池改为`CachedThreadPool`，由于这个线程池的实现会根据任务数量动态调整线程池的大小，所以6个任务可一次性全部同时执行。

如果我们想把线程池的大小限制在4～10个之间动态调整怎么办？我们查看`Executors.newCachedThreadPool()`方法的源码：

```
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                    60L, TimeUnit.SECONDS,
                                    new SynchronousQueue<Runnable>());
}
```

因此，想创建指定动态范围的线程池，可以这么写：

```
int min = 4;
int max = 10;
ExecutorService es = new ThreadPoolExecutor(min, max,
        60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
```



### scheduledThreadPool

- 应对需要定期反复执行的任务

创建一个`ScheduledThreadPool`仍然是通过`Executors`类：

```
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
```

我们可以提交一次性任务，它会在指定延迟后只执行一次：

```
// 1秒后执行一次性任务:
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);
```

如果任务以固定的每3秒执行，我们可以这样写：

```
// 2秒后开始执行定时任务，每3秒执行:
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);
```

如果任务以固定的3秒为间隔执行，我们可以这样写：

```
// 2秒后开始执行定时任务，以3秒为间隔执行:
ses.scheduleWithFixedDelay(new Task("fixed-delay"), 2, 3, TimeUnit.SECONDS);
```

注意FixedRate和FixedDelay的区别。FixedRate是指任务总是以固定时间间隔触发，不管任务执行多长时间：

```ascii
│░░░░   │░░░░░░ │░░░    │░░░░░  │░░░  
├───────┼───────┼───────┼───────┼────>
│<─────>│<─────>│<─────>│<─────>│
```

而FixedDelay是指，上一次任务执行完毕后，等待固定的时间间隔，再执行下一次任务：

```ascii
│░░░│       │░░░░░│       │░░│       │░
└───┼───────┼─────┼───────┼──┼───────┼──>
    │<─────>│     │<─────>│  │<─────>│
```

因此，使用`ScheduledThreadPool`时，我们要根据需要选择执行一次、FixedRate执行还是FixedDelay执行。



---

## Future

`Runnable`接口有个问题，它的方法没有返回值。如果任务需要一个返回结果，那么只能保存到变量，还要提供额外的方法读取，非常不便。所以，Java标准库还提供了一个`Callable`接口，和`Runnable`接口比，它多了一个返回值：

```
class Task implements Callable<String> {
    public String call() throws Exception {
        return longTimeCalculation(); 
    }
}
```

并且`Callable`接口是一个泛型接口，可以返回指定类型的结果。



如果仔细看`ExecutorService.submit()`方法，可以看到，它返回了一个`Future`类型，一个`Future`类型的实例代表一个未来能获取结果的对象



一个`Future<V>`接口表示一个未来可能会返回的结果，它定义的方法有：

- `get()`：获取结果（可能会等待）
- `get(long timeout, TimeUnit unit)`：获取结果，但只等待指定的时间；
- `cancel(boolean mayInterruptIfRunning)`：取消当前任务；
- `isDone()`：判断任务是否已完成。



---

## 使用ForkJoin

//TODO



---

