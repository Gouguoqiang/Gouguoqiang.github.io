---
title: Java并发
tags:
  - 多线程
categories:
  - 八股
keywords: 
description: 并发总结。
date: 2022-9-01 11:51:56
---



异常不能跨线程传播, 只能在线程内部本地处理

## 第一章 进程与线程的基本概念

程序有时会由于I/O操作、网络等原因阻塞，所以批处理操作效率也不高。

进程解决了批处理系统时只能存在一个程序问题(单核并发)

线程解决了进程在一段时间只能做一件事情(如果一个进程有多个子任务时，只能逐个得执行这些子任务，很影响效率。)

进程让操作系统的并发性成为了可能,线程让进程的并发成为了可能

**多进程也可以实现并发,为什么我们要使用多线程**

- 进程间通信比较复杂,通常我们需要共享资源,这些资源在线程间通信比较简单
- 进程是重量级

CPU通过为每个线程分配CPU时间片来实现多线程机制。CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到再加载的过程就是一次上下文切换。上下文切换通常是计算密集型的，意味着此操作会消耗大量的 CPU 时间，故线程也不是越多越好。如何减少系统中上下文切换次数，是提升多线程性能的一个重点课题。

**区别**

是否单独占有内存地址空间及其它系统资源（比如I/O）

进程间内存隔离,数据共享复杂,但是同步简单 线程则相反

一个进程出现问题不会影响其他进程,而线程则不同

进程创建不仅需要寄存器和栈信息,还需要资源的分配回收以及页调度,线程只需要保存寄存器和栈信息

另外一个重要区别是进程是操作系统进行资源分配的基本单位,而线程是os进行调度的基本单位即cpu分配时间的单位



## 第二章 Java多线程入门类和接口

Runnable接口最重要的方法—–run方法，使用了**策略者模式**将执行的逻辑(run方法)和程序的执行单元(start0方法)分离出来，使用户可以定义自己的程序处理逻辑，更符合面向对象的思想。

### 2.1 Thread类和Runnable接口

> 我们在程序里面调用了start()方法后，虚拟机会先为我们创建一个线程，然后等到这个线程第一次得到时间片时再调用run()方法。
>
> 注意不可多次调用start()方法。在第一次调用start()方法后，再次调用start()方法会抛出异常。

```java
Runable可函数式编程
@FunctionalInterface
public interface Runnable{public abstract void run();}

new Thread(() -> {            System.out.println("Java 8 匿名内部类");        }).start();

```

### Thread类构造⽅法

查看 Thread 类的构造⽅法，发现其实是简单调⽤⼀个私有的 init ⽅法来实现初 始化。 init 的⽅法签名：

```java
// Thread类源码
// ⽚段1 - init⽅法
private void init(ThreadGroup g, Runnable target, String name,
 long stackSize, AccessControlContext acc,
boolean inheritThreadLocals)
// ⽚段2 - 构造函数调⽤init⽅法
public Thread(Runnable target) {
 init(null, target, "Thread-" + nextThreadNum(), 0);
}
// ⽚段3 - 使⽤在init⽅法⾥初始化AccessControlContext类型的私有属性
this.inheritedAccessControlContext =
 acc != null ? acc : AccessController.getContext();
// ⽚段4 - 两个对⽤于⽀持ThreadLocal的私有属性
ThreadLocal.ThreadLocalMap threadLocals = null;
ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;


```

inheritThreadLocals：可继承的 ThreadLocal ，⻅⽚段4， Thread 类⾥⾯有两 个私有属性来⽀持 ThreadLocal ，我们会在后⾯的章节介绍 ThreadLocal 的概 念。

```java
Thread(Runnable target)
Thread(Runnable target, String name)
```



### Thread类的⼏个常⽤⽅法

这⾥介绍⼀下Thread类的⼏个常⽤的⽅法：

- currentThread()：静态⽅法，返回对当前正在执⾏的线程对象的引⽤； 

- start()：开始执⾏线程的⽅法，java虚拟机会调⽤线程内的run()⽅法； 

- yield()：yield在英语⾥有放弃的意思，同样，这⾥的yield()指的是当前线程愿 意让出对当前处理器的占⽤。这⾥需要注意的是，就算当前线程调⽤了yield() ⽅法，程序在调度的时候，也还有可能继续运⾏这个线程的； 
- sleep()：静态⽅法，使当前线程睡眠⼀段时间；
- join()：使当前线程等待另⼀个线程执⾏完毕之后再继续执⾏，内部调⽤的是 Object类的wait⽅法实现的；

###  Thread类与Runnable接⼝的⽐较

- 由于Java“单继承，多实现”的特性，Runnable接⼝使⽤起来⽐Thread更灵活。
- Runnable接⼝出现更符合⾯向对象，将线程单独进⾏对象的封装
- Runnable接⼝出现，降低了线程对象和线程任务的耦合性。 
- 如果使⽤线程时不需要使⽤Thread类的诸多⽅法，显然使⽤Runnable接⼝更 为轻量。 
- 所以，我们通常优先使⽤“实现 Runnable 接⼝”这种⽅式来⾃定义线程类。

### Callable、Future与FutureTask 异步模型

#### Callable

```java
@FunctionalInterface
public interface Callable<V> {
 V call() throws Exception;
}
```

那⼀般是怎么使⽤ Callable 的呢？ Callable ⼀般是配合线程池⼯ 具 ExecutorService 来使⽤的。我们会在后续章节解释线程池的使⽤。这⾥只介 绍 ExecutorService 可以使⽤ submit ⽅法来让⼀个 Callable 接⼝执⾏。它会返回 ⼀个 Future ，我们后续的程序可以通过这个 Future 的 get ⽅法得到结果。

```java
// ⾃定义Callable
class Task implements Callable<Integer>{
 @Override
 public Integer call() throws Exception {
 // 模拟计算需要⼀秒
 Thread.sleep(1000);
 return 2;
 }
 public static void main(String args[]){
 // 使⽤
 ExecutorService executor = Executors.newCachedThreadPool();
 Task task = new Task();
 Future<Integer> result = executor.submit(task);
 // 注意调⽤get⽅法会阻塞当前线程，直到得到结果。
 // 所以实际编码中建议使⽤可以设置超时时间的重载get⽅法。
 System.out.println(result.get());
 }
}

```

#### Future接口

```java
public abstract interface Future<V> {
 public abstract boolean cancel(boolean paramBoolean);
 public abstract boolean isCancelled();
 public abstract boolean isDone();
 public abstract V get() throws InterruptedException, ExecutionException;
 public abstract V get(long paramLong, TimeUnit paramTimeUnit)
 throws InterruptedException, ExecutionException, TimeoutException;
}

```

cancel ⽅法是试图取消⼀个线程的执⾏。 注意是试图取消，并不⼀定能取消成功。因为任务可能已完成、已取消、或者⼀些 其它因素不能取消，存在取消失败的可能。 boolean 类型的返回值是“是否取消成 功”的意思。参数 paramBoolean 表示是否采⽤中断的⽅式取消线程执⾏。 所以有时候，为了让任务有能够取消的功能，就使⽤ Callable 来代替 Runnable 。 如果为了可取消性⽽使⽤ Future 但⼜不提供可⽤的结果，则可以声明 Future 形式类型、并返回 null 作为底层任务的结果。

#### FutureTask类

上⾯介绍了 Future 接⼝。这个接⼝有⼀个实现类叫 FutureTask 。 FutureTask 是 实现的 RunnableFuture 接⼝的，⽽ RunnableFuture 接⼝同时继承了 Runnable 接⼝ 和 Future 接⼝：

在很多⾼并发的环境下，有可能Callable和FutureTask会创建多次。FutureTask能 够在⾼并发环境下确保任务只执⾏⼀次

## 第三章 线程组和线程优先级

Java中的优先级来说不是特别的可靠，Java程序中对线程所设置的优先级只是给 操作系统⼀个建议，操作系统不⼀定会采纳。⽽真正的调⽤顺序，是由操作系统的 线程调度算法决定的

Java提供⼀个线程调度器来监视和控制处于RUNNABLE状态的线程。线程的调度 策略采⽤抢占式，优先级⾼的线程⽐优先级低的线程会有更⼤的⼏率优先执⾏。在 优先级相同的情况下，按照“先到先得”的原则。每个Java程序都有⼀个默认的主线 程，就是通过JVM启动的第⼀个线程main线程。

所以，如果某个线程优先级⼤于线程所在线程组的最⼤优先级，那么该线程的优先 级将会失效，取⽽代之的是线程组的最⼤优先级。

> Thread.currentThread().getThreadGroup().getName()

> todo 

## 第五章 Java线程间的通信

### 四、互斥同步

Java 提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是 JVM 实现的 synchronized，而另一个是 JDK 实现的 ReentrantLock。

### synchronized

**1. 同步一个代码块**  

```java
public void func() {
    synchronized (this) {
        // ...
    }
}
```

它只作用于同一个对象，如果调用两个对象上的同步代码块，就不会进行同步。

对于以下代码，使用 ExecutorService 执行了两个线程，由于调用的是同一个对象的同步代码块，因此这两个线程会进行同步，当一个线程进入同步语句块时，另一个线程就必须等待。

```java
public class SynchronizedExample {

    public void func1() {
        synchronized (this) {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        }
    }
}
```

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func1());
    executorService.execute(() -> e1.func1());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```

对于以下代码，两个线程调用了不同对象的同步代码块，因此这两个线程就不需要同步。从输出结果可以看出，两个线程交叉执行。

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    SynchronizedExample e2 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func1());
    executorService.execute(() -> e2.func1());
}
```

```html
0 0 1 1 2 2 3 3 4 4 5 5 6 6 7 7 8 8 9 9
```


**2. 同步一个方法**  

```java
public synchronized void func () {
    // ...
}
```

它和同步代码块一样，作用于同一个对象。

**3. 同步一个类**  

```java
public void func() {
    synchronized (SynchronizedExample.class) {
        // ...
    }
}
```

作用于整个类，也就是说两个线程调用同一个类的不同对象上的这种同步语句，也会进行同步。

```java
public class SynchronizedExample {

    public void func2() {
        synchronized (SynchronizedExample.class) {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        }
    }
}
```

```java
public static void main(String[] args) {
    SynchronizedExample e1 = new SynchronizedExample();
    SynchronizedExample e2 = new SynchronizedExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> e1.func2());
    executorService.execute(() -> e2.func2());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```

**4. 同步一个静态方法**  

```java
public synchronized static void fun() {
    // ...
}
```

作用于整个类。

总结 类(.class对象)锁 对象锁

### ReentrantLock

ReentrantLock 是 java.util.concurrent（J.U.C）包中的锁。

```java
public class LockExample {

    private Lock lock = new ReentrantLock();

    public void func() {
        lock.lock();
        try {
            for (int i = 0; i < 10; i++) {
                System.out.print(i + " ");
            }
        } finally {
            lock.unlock(); // 确保释放锁，从而避免发生死锁。
        }
    }
}
```

```java
public static void main(String[] args) {
    LockExample lockExample = new LockExample();
    ExecutorService executorService = Executors.newCachedThreadPool();
    executorService.execute(() -> lockExample.func());
    executorService.execute(() -> lockExample.func());
}
```

```html
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9
```


### 比较

**1. 锁的实现**  

synchronized 是 JVM 实现的，而 ReentrantLock 是 JDK 实现的。

**2. 性能**  

新版本 Java 对 synchronized 进行了很多优化，例如自旋锁等，synchronized 与 ReentrantLock 大致相同。

**3. 等待可中断**  

当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。

ReentrantLock 可中断，而 synchronized 不行。

**4. 公平锁**  

公平锁是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁。

synchronized 中的锁是非公平的，ReentrantLock 默认情况下也是非公平的，但是也可以是公平的。

**5. 锁绑定多个条件**  

一个 ReentrantLock 可以同时绑定多个 Condition 对象。

> todo

### 使用选择

除非需要使用 ReentrantLock 的高级功能，否则优先使用 synchronized。这是因为 synchronized 是 JVM 实现的一种锁机制，JVM 原生地支持它，而 ReentrantLock 不是所有的 JDK 版本都支持。并且使用 synchronized 不用担心没有释放锁而导致死锁问题，因为 JVM 会确保锁的释放。

### 五、线程之间的协作

当多个线程可以一起工作去解决某个问题时，如果某些部分必须在其它部分之前完成，那么就需要对线程进行协调。

### await() signal() signalAll()

java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。

使用 Lock 来获取一个 Condition 对象。

```java
public class AwaitSignalExample {

    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void before() {
        lock.lock();
        try {
            System.out.println("before");
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public void after() {
        lock.lock();
        try {
            condition.await();
            System.out.println("after");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

```java
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();
    AwaitSignalExample example = new AwaitSignalExample();
    executorService.execute(() -> example.after());
    executorService.execute(() -> example.before());
}
```

```html
before
after
```





## 第六章 Java内存模型基础知识

## 第七章 重排序与happens-before

## 第八章 volatile

## 第九章 synchronized与锁

## 第十章 乐观锁和悲观锁

## 第十一章 AQS

## 第十二章 线程池原理

### 12.1为什么要使用线程池

1. 创建/销毁线程需要消耗系统资源,线程池可复用已创建的线程
2. 控制并发数量(主要原因),并发数量过多可能会导致资源消耗过多导致服务器崩溃
3. 可以对线程做统一管理

### 12.2 线程池原理

线程池顶层接口是Executor,TreadPoolExecutor是实现类

#### 12.2.1 ThreadPoolExecutor提供的构造方法

一共四个构造方法 

涉及到5~7个参数

都有的 5 个

1. 核心线程数
2. 最大线程数
3. 非核心线程闲置超时时长
4. 时长单位
5. 阻塞队列,维护等待执行的Runnable任务对象

两个非必须参数

1. 指定线程工厂
   1. 默认defaultThreadFactory
2. 拒绝处理策略
   1. 线程数量大于最大线程数就会采用拒绝处理策略
      1. 默认AbortPolicy 丢弃任务并抛出异常

## 第十三章 阻塞队列

## 第十四章 锁接口和类

## 第十五章 并发集合容器简介

## 第十六章 CopyOnwrite

## 第十七章 通信工具类

## 第十八章 Fork/Join框架

## 第十九章 Java8 Stream并行计算原理

## 第二十章 计划任务

