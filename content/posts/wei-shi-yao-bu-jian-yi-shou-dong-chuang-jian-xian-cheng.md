---
title: 为什么不建议手动创建线程
date: 2022-01-03
tags:
  - Java
---


## 阿里插件提醒

手动创建线程池，效果会更好哦。

## 为什么

更加明确线程池的运行规则，规避资源耗尽的风险。

## `Executors` 返回的线程池有什么弊端？

1. `FixedThreadPool` 和 `SingleThreadPool`

允许请求的队列的长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。

2. `CachedThreadPool`

允许创建的线程的数量为Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。

## 复现 OOM

我们给线程池无限加线程，然后看效果。

```java
public static void main(String[] args) {
  ExecutorService executorService = Executors.newCachedThreadPool();
  int i = 0;
  while (true) {
      executorService.submit(new Task(i++));
  }
}
```

jvm 参数

```java
-Xms10M //堆内存初始化大小
-Xmx10M //堆内存最大大小
```

执行结果

```java
...
pool-1-thread-4071 4070
pool-1-thread-4072 4071
Exception in thread "main" java.lang.OutOfMemoryError: unable to create new native thread
at java.lang.Thread.start0(Native Method)
at java.lang.Thread.start(Thread.java:717)
at java.util.concurrent.ThreadPoolExecutor.addWorker(ThreadPoolExecutor.java:957)
at java.util.concurrent.ThreadPoolExecutor.execute(ThreadPoolExecutor.java:1378)
at java.util.concurrent.AbstractExecutorService.submit(AbstractExecutorService.java:112)
at com.peierlong.concurrency.ThreadPoolOomTest.main(ThreadPoolOomTest.java:18)
Error occurred during initialization of VM
java.lang.OutOfMemoryError: unable to create new native thread
```

## 应该用什么

`ThreadPoolExecutor`，其具有如下构造函数。

```java
public ThreadPoolExecutor(int corePoolSize,	//核心线程数量
                        int maximumPoolSize,	//线程池最大的数量
                        long keepAliveTime,	//空闲线程存货时间
                        TimeUnit unit,
                        BlockingQueue<Runnable> workQueue,	//线程池所使用的缓冲队列
                        ThreadFactory threadFactory,	//线程池创建线程所使用的工厂
                        RejectedExecutionHandler handler)	//线程池对拒绝的任务的处理策略
```

执行流程如下

![](https://peierlong-blog.oss-cn-hongkong.aliyuncs.com/uPic/线程池的执行流程.svg)

## 如何正确定义线程池参数

### CPU 密集型

推荐 CPU+1，CPU 数量获取代码如下

```java
Runtime.getRuntime().availableProcessors()
```

### IO 密集型

CPU 数量 * CPU 利用率 * （1 + 线程等待时间/线程CPU 时间）

## 关于阻塞队列

推荐使用有界队列，避免资源耗尽 OOM。

## 关于拒绝队列

默认直接抛出拒绝异常，有没有更合适的处理办法呢？

- 针对默认拒绝策略，可以捕获并处理。

- 实现 RejectedExecutionHandler 接口自定义拒绝策略。

- 任务不重要，可以使用 DiscardPolicy 和 DiscardOldestPolicy。

## 正确的线程池创建的方式

```java
// 获取处理器数量和创建线程工厂
int threadCnt = Runtime.getRuntime().availableProcessors();
ThreadFactory factory = new ThreadFactoryBuilder().setNameFormat("demo-pool-%d").build();
// Common Thread Pool
new ThreadPoolExecutor(threadCnt, 200, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>(1024), factory, new ThreadPoolExecutor.AbortPolicy());
```
