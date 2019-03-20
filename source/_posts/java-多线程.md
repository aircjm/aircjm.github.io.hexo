---
title: java 多线程
date: 2016-06-06 23:24:00
tags:
- java
- 笔记
---

学习java多线程的一下知识的总结，写在这里，防止自己遗忘，也是为了复习。

<!--more-->

### JVM的启动是单线程还是多线程的？？

JVM启动的时候不仅仅启动了主线程，而且启动了垃圾回收线程，所以JVM的启动是多线程的。

### 如何实现多线程？

两种方式：

1. 继承Thread类
   1. 自定义MyThread类继承了Thread类
   2. 重写MyThread类中的run()方法
   3. 创建MyThread对象
   4. 启动线程对象


1. 实现Runable接口
   1. 自定义MyRunnable类实现了Runnable接口
   2. 重写MyRunnable类中的run()方法
   3. 创建MyRunnable对象
   4. 创建Thread对象，把MyRunnable对象对象作为构造函数的入参






对于继承Thread来说，是通过重写了`run`方法来实现的，因为一个类的代码并不是都需要多线程执行的，只会执行`run`方法里面的代码。

#### 为什么有两张方式来实现多线程？？？

1. 由于Java只支持单继承，避免单继承导致的多线程的局限性
2. 适合多个相同程序的代码去处理同一个资源，吧线程相同程序的代码，数据有效分离，体现了Java面向对象的设计思想。

如果重复调用`run`会发现，还是单线程执行，原因是`run`方法它是单行程的。

可以通过`start`方法来执行，如果调用两次`start`方法，并不能进行多线程操作，由于已经执行了`start`方法，再次执行`start`会报错,，报错信息如下：

![连续执行对象的线程start方法报错.png](http://upload-images.jianshu.io/upload_images/1013655-0a96d1ad2c5294ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以可以实例化两个对象，分别启动对象的线程，会发现他们两个确实是多线程执行的。看到两个for循环是同时执行的，并没有顺序执行。





代码示例：

```java
package com.aircjm.thread;

public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println(i);
        }
    }
}

```



```java
package com.aircjm.thread;

public class ThreadTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();

        //由于run()方法时单线程的，所以还是按照单线程执行，一个run()执行完了，另一个run()才会执行
        //myThread.run();
        //myThread.run();

        //可以通过start()方法来启动线程
        //两者之间的区别，start是使该线程开始执行，JVM调用该线程的run()方法
        //myThread.start();
        //myThread.start();

        MyThread myThread1 = new MyThread();
        MyThread myThread2 = new MyThread();

        myThread1.start();
        myThread2.start();


    }
}
```



### 如何获取继承Thread的线程对象的名称

```java
public final String getName() //获取线程的名词
```

### 线程控制的几个方法：

休眠线程：
`public static void sleep(long millis) throws InterruptedException`

在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。该线程不丢失任何监视器的所属权。
启动线程：
start

`public void start()`
使该线程开始执行；Java 虚拟机调用该线程的 run 方法。
结果是两个线程并发地运行；当前线程（从调用返回给 start 方法）和另一个线程（执行其 run 方法）。

多次启动一个线程是非法的。特别是当线程已经结束执行后，不能再重新启动。

抛出：
IllegalThreadStateException - 如果线程已经启动。
另请参见：
run(), stop()
run

`public void run()`
如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回。
Thread 的子类应该重写该方法。

加入线程：

礼让线程：

守护线程：

中断线程：



#### 启动线程用的是哪个方法？

start()

#### run()和start()方法的区别？

1. run直接调用，调用的是普通方法
2. start()方法，是启动线程，然后JVM调用了run()方法



#### 如何让线程安全？？

给需要线程安全的地方进行加锁

```java
public class SellTicket implements Runnable {
    private Integer ticketNum = 100;
    public void run() {
        while (true){
            synchronized (ticketNum){
                if (ticketNum>0){
                    System.out.println("now is sell "+ ticketNum-- +" ticket");
                }
            }
        }
    }
}
```

加锁的方式有多种，可以给代码块加锁（锁对象是任意对象），可以给方法加锁（锁对象是this），也可以给静态方法加锁（锁对象是字节码文件）



学习到的线程安全的对象有：

```java
StringBuffer stringBuffer = new StringBuffer();
Vector<String> vector = new Vector<String>();
Hashtable<String, Object> hashTable = new Hashtable<String, Object>();
```

```java
// 即使线程安全但是也不推荐使用
Vector<String> vector = new Vector<String>();
Hashtable<String, Object> hashTable = new Hashtable<String, Object>();

// 如何使用线程安全的List呢??
List<String> list = Collections.synchronizedList(new ArrayList<String>()); // 线程安全
List<String> list1 = new ArrayList<String>(); //线程不安全
```

通过静态方法获取到线程安全的List集合









