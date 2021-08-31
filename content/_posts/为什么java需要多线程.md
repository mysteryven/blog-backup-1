---
title: "为什么java需要多线程"
date: 2020-03-08T16:49:00+08:00
draft: false
---

<!--more-->

我们的 Java 程序默认是单线程的，优点是程序的逻辑非常自然，但缺点也很明显，他在某些情况下有明显的性能劣势。

假设一个计算机的主频是 3G 的，那 CPU 完成一个指令所需要的时间大约要 3ns 左右。而从内存中读取 1MB 数据所需的时间大约要花费 2.5 _ 10^5 ns，同一个数据中心跑一个来回的话就更慢了，要 5 _ 10^5 ns。如果我们的程序一直是单线程的，会出现的情况就是 CPU 的工作很快就完成了，但是却一直等着外接返回的结果，等待的时间就都浪费了。

上面这种情况是我们不能接受的，也就是说，对于一些 IO 密集型的任务，使用单线程会有明显的性能劣势，这时候就需要我们使用多线程了。

我们可以使用 `Thread` 这个类去创建一个线程。每多开一个线程就是多一个执行流。如果我们一次性的开了多个线程，除了他们内部的局部变量是私有的外，其他的像外部的静态变量、类变量都是被所有线程所共享的。

当然了，万物都有其两面性，我们在享受多线程给我们带来的便利的同时，也要忍受它给我们带来的一些令人烦恼的问题。

问题之一就是多线程造成的数据错误。举个例子来说的话，我们在多线程中同时对一个 HashMap 进行赋值操作的话，很容易就出现重复赋值的状况。可见[这篇文章](https://coolshell.cn/articles/9606.html)。也可以看下面这个例子，也会引发数据错误的问题。

```java

// 生成 1-10 的随机数
public static void putRandomValueToMap() {
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    int r = new Random().nextInt(10);
    if (!map.containsKey(r)) {
        map.put(r, r);
        System.out.print(r);
    }
}

public static void main(String[] args) {
    for (int i = 0; i < 100; i++) {
        new Thread(A::putRandomValueToMap).start();
    }

    for (Integer i : map.keySet()) {
    }

}
// 按照逻辑，这个程序是不会出现重复的值，但事实并非如此
// 某次的结果是： 7 5 3 4 5 5 6 8 9 2 0 1
```

问题之二就是死锁。我们也用两个线程重现一下这个代码。这里有两个线程 Thread1、Thread2。它们在争夺锁的使用过程中就出现了死锁这个问题。

```java
public static Object lock1 = new Object();
public static Object lock2 = new Object();

static class Thread1 extends Thread {
        @Override
        public void run() {
            synchronized (lock1) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    synchronized (lock2) {}
                }
            }
        }
    }

static class Thread2 extends Thread {
    @Override
    public void run() {
        synchronized (lock2) {
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                synchronized (lock1) {}
            }
        }
    }
}


public static void main(String[] args) {
    new Thread1().start();
    new Thread2().start();
}
```

以上就是多线程容易引发的两个问题。虽然比较容易遇到问题，但是也不能不去使用它，遇到问题解决问题才对！

解决多线程问题，我们大概有以下方法：

1. 使用不可变类。
2. 使用同步块，可以让我们在某个时间点只能有一个线程执行它
3. 使用 JUC（java.utils.concurrent）里面专为并发设计的类

在 JUC 中，提供了很多帮我们实现线程安全的类，如 AtomicInteger、ConcurrentHashMap、ReentrantLock、BlockingQueue。需要在实践中去慢慢探索。

以上就是关于 Java 多线程的一些简单介绍。
