---
title: Java多线程详解笔记（一）
tags:
  - Java入门
  - 多线程
categories:
  - - Java
mathjax: true
copyright: true
abbrlink: 8083204b
date: 2021-06-12 19:19:24
---

## 1.线程简介

参考视频：狂神多线程详解，尚硅谷Java入门视频。

学习建议：先学完IO流再来学习多线程。

<!--more-->

线程（thread）

![image-20210612193035124](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103605.png)

进程（process）

![image-20210612212103470](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103735.png)

程序、进程和线程的区别：

![image-20210612212228192](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103308.png)

通常在一个进程中可以包含若干个线程，一个进程至少有一个线程。线程数CPU调度和执行的单位。

![image-20210612212444861](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103010.png)

核心概念：

![image-20210612212658925](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103568.png)

一个Java应用程序Java.exe，至少有三个线程，main()主线程，gc()垃圾回收线程，异常处理线程。当然发生异常时，会影响主线程。

并行与并发：

并行： 多个 CPU 同时执行多个任务。比如：多个人同时做不同的事 。

并发： 一个 CPU（采用时间片）同时执行多个任务。比如：秒杀、多个人做同一件事。

何时需要多线程？

程序需要同时执行两个或多个任务。

程序需要实现一些需要等待的任务时，如用户输入、文件读写操作、网络操作、搜索等。

需要一些后台运行的程序时。

## 2.创建线程

有三种创建线程的方式：

1. Thread class：继承java.lang.Thread类**（重点）**
2. Runnable接口：实现Runnable接口**（重点）**
3. Callable接口：实现Callable接口（了解）

## 3.Thread

1.自定义线程类继承Thread类

2.重写run()方法，编写线程执行体

3.创建线程对象，调用start()方法启动线程（作用：启动当前线程，调用当前线程的run方法）

```Java
public class Thread_1 extends Thread{
    // 创建线程方式1：创建一个继承Thread类的子类，重写run方法，通过此对象调用start启动线程
    // 总结：线程开启不一定立即执行，由CPU调度执行
    @Override
    public void run() {
        // run方法线程体
        for (int i = 0; i < 200; i++) {
            System.out.println("我在看源码..."+i);
        }
    }

    public static void main(String[] args) {
        // main线程，主线程

        // 创建一个线程对象
        Thread_1 th1 = new Thread_1();
        // 调用start方法，开启线程
        // 看源码和学多线程是并发运行的
        th1.start();// 并不是直接调用run方法
		// 不能用th1执行2次start，会报异常，需要新创建一个线程对象执行start
        for (int i = 0; i < 1000; i++) {
            System.out.println("我在学习多线程..."+i);
        }
    }
}
```

```Java
// 通过创建Thread类的匿名子类来启动多线程
new Thread(){
    @Override
    public void run() {// 重写run方法
        for (int i = 0; i < 200; i++) {
            System.out.println(Thread.currentThread().getName()+":"+i);
        }
    }
}.start();
```

![image-20210726113402721](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103348.png)

![image-20210726114809437](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232103960.png)

```Java
class helloThread extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            //if (i % 3 == 0){
            //    yield();// 释放当前CPU的执行权，可能再次抢到
            //}
            if (i % 40 == 0){
                try {// 只能用try-catch处理异常
                    sleep(1000);// 睡眠1000ms
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println(Thread.currentThread().getName()+":"+i);
        }
    }
    public helloThread(String name){
        super(name);
    }
    public helloThread(){
    }
}
public class ThreadTest {
    public static void main(String[] args) {
        helloThread h1 = new helloThread();
        helloThread h2 = new helloThread("myThread");// 通过带参构造器给线程命名
        h1.setName("helloThread");// 也可以通过setName方法实现
        h1.start();
        h2.start();
        Thread.currentThread().setName("mainThread");
        for (int i = 0; i < 1000; i++) {
            System.out.println(Thread.currentThread().getName()+":"+i);
            if (i == 20){
                try {
                    h1.join();// 在线程a中调用线程b的join()
                    //此时线程a就进入阻塞状态，直到线程b完全执行完才结束阻塞状态
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

