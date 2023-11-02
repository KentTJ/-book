



# 目录

[TOC]



# WatchDog（死锁+Looper----阻塞恢复机制）

主要参考：

> https://blog.csdn.net/shift_wwx/article/details/81021257/
>
> https://blog.csdn.net/qq_40587575/article/details/133682356   ----------> 比较精确的文章
>
> https://blog.csdn.net/qq_43369592/article/details/123521848
>
> https://blog.csdn.net/qq_27061049/article/details/130138701    Android 系统中的 WatchDog 详解   函数级+ 图

## 目的

CPU级别的WatchDog（硬件）：可以在系统死掉(死锁或者程序跑飞)后重启系统，让系统回到可以工作的状态。------->  系统级恢复

-------->   TODO:  系统死掉，具体指的是啥？

安卓WatchDog（软件，系统服务级别，**线程级别**）： 保护一些重要的**系统服务**，比如[AMS](https://so.csdn.net/so/search?q=AMS&spm=1001.2101.3001.7020)、WMS、PMS

------>  TODO:   比较重要的：为什么不能全部？

Q:   如何保护？系统服务退出，init进程会重启SystemServer的？为什么还需要WatchDog？

> A:   重启SystemServer前提： SystemServer进程中，任何一个线程死掉都可能导致整个系统死掉。
>
> 但是还存在死锁的情况，线程挂不了   ----------->  正是WatchDog设计的意义： 确保AMS、PMS、WMS等服务发生死锁之后，退出SystemServer进程，让init进程重启它，让系统回到可用状态。

## 使用

全是各种服务在使用：

> 

1、向Watchdog注册

```java
 // 死锁监控：
 Watchdog.getInstance().addMonitor(this);
 // 消息队列监控：
 Watchdog.getInstance().addThread(mHandler);
```

2、heartbeat 心跳 回调

## 原理

0层架构图

![在这里插入图片描述](WatchDog.assets/2dd8c0c4df444490a00eb8c286a6ef4d.png)

来源： https://blog.csdn.net/qq_43369592/article/details/123521848

fg 线程





从线程角度：TODO



### Monitor Checker

用于检查 AMS，IMS，WMS PMS 等核心的系统服务 可能发生的死锁

```java
 public void monitor() {
        synchronized (this) { }  //获取AMS的this、wms的mGlobalLock等锁
 }
```

1、检测是否有死锁----------主线程？？？ （android_fg 线程）：

> 回调时，监控 system_server 中几个关键的锁（在 android_fg 线程中尝试加锁），比如AMS的this、wms的mGlobalLock
>
> ------------> 如果有死锁，尝试加锁会阻塞Blocked在这里

2、检测到死锁，kill进程--------Watchdog线程：

```java
TODO
```

### Looper Checker

目的：

> 用于检查线程的消息队列 looper是否长时间处于工作状态

当前适用的一些范围：

> Watchdog自身的消息队列，Ui, Io, Display这些全局的消息队列
>
> 一些重要的线程的消息队列，也会加入到Looper Checker中，譬如AMS, PKMS，powerMS  ------>  TODO: 线程不固定，如何做到的？

原理：

1、监控了哪些？

```java
 //AMS.java
 Watchdog.getInstance().addThread(mHandler);
 
 // Watchdog.java
 private Watchdog() {
     mMonitorChecker = new HandlerChecker(FgThread.getHandler(),
             "foreground thread", DEFAULT_TIMEOUT);
     mHandlerCheckers.add(mMonitorChecker);
     // Add checker for main thread.  We only do a quick check since there
     //  UI running on the thread.
     mHandlerCheckers.add(new HandlerChecker(new Handler(Looper.getMainLooper()),
             "main thread", DEFAULT_TIMEOUT));
     // shared UI thread.
     mHandlerCheckers.add(new HandlerChecker(UiThread.getHandler(),
             "ui thread", DEFAULT_TIMEOUT));
     // check IO thread.
     mHandlerCheckers.add(new HandlerChecker(IoThread.getHandler(),
             "i/o thread", DEFAULT_TIMEOUT));
     // display thread.
     mHandlerCheckers.add(new HandlerChecker(DisplayThread.getHandler(),
             "display thread", DEFAULT_TIMEOUT));
     // animation thread.
     mHandlerCheckers.add(new HandlerChecker(AnimationThread.getHandler(),
             "animation thread", DEFAULT_TIMEOUT));
     // surface animation thread.
     mHandlerCheckers.add(new HandlerChecker(SurfaceAnimationThread.getHandler(),
             "surface animation thread", DEFAULT_TIMEOUT));
 
     // Initialize monitor for Binder threads.
     addMonitor(new BinderThreadMonitor());
 
     .................
 
 }
```

2、判断

```java
 mHandler.getLooper().getQueue().isPolling()  // --------》 返回当前Looper是否没有在处理任务  <https://blog.csdn.net/oHeHui1/article/details/129058587>
```

**Watchdog会不断判断这些线程的Lopper是否空闲，如果一直非空闲，那么必然就被阻塞了。**

其他：

> 保存案发现场
>
> ```java
>  WatchdogDiagnostics.diagnoseCheckers(blockedCheckers);
> ```

TODO:

为啥会和mSFHang，扯上关系？

## WatchDog本身

启动：

> SystemServer.startBootstrapServices 中

本身是一个线程：**无限循环的**       -------->  可以理解为一个服务，但是不是binder