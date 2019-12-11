

[toc]

# 任务

1.10个线程来处理大量的任务

* ThreadPoolExecutor

```java 
public class Demo {
    public static void main(String[] args) {
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(10,10,0, TimeUnit.MILLISECONDS,new LinkedBlockingQueue<>());
        while (true){
            threadPoolExecutor.execute(()->{
                System.out.println(Thread.currentThread().getName());
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
    }
}
```



* Executors.newFixedThreadPool
* Executors.newCachedThreadPool

```java 
public class Demo {
    public static void main(String[] args) {
        ExecutorService threadPoolExecutor = Executors.newFixedThreadPool(10);
        while (true){
            threadPoolExecutor.execute(()->{
                System.out.println(Thread.currentThread().getName());
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
    }
}
```

## 源码解析

## newFixedThreadPool

```Java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

一目了然，Executors.newFixedThreadPool只是将ThreadPoolExecutor的参数设为默认参数而已。

## newCachedThreadPool

```
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

理论上能创建Integer.MAX_VALUE个线程，60s不用就销毁

## newSingleThreadPool

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

只创建一个线程，如果线程因为各种原因关闭了，线程池会再创建一个线程执行任务，保证有且仅有一个线程在运行

## newScheduledThreadPool

* 构造过程

```java
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
    return new ScheduledThreadPoolExecutor(corePoolSize);
}
```

```java
public ScheduledThreadPoolExecutor(int corePoolSize) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue());
}
```

super就是下面这堆代码

```java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue) {
    this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
         Executors.defaultThreadFactory(), defaultHandler);
}
```

重点关注那个DelayedWorkQueue，用延迟队列来实现Schedule的功能

* 延迟5秒执行任务

```java
public class Demo {
    public static void main(String[] args) {
        ScheduledExecutorService threadPoolExecutor  = Executors.newScheduledThreadPool(10);
        while (true){
            threadPoolExecutor.schedule(()->{
                System.out.println(Thread.currentThread().getName());
            },5,TimeUnit.SECONDS);
        }
    }
}
```

##线程池的submit

```java
public class Demo {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        Future <Integer>[] f=new Future[10];
        for (int i = 0 ; i < 10; i++){
              f[i] = executorService.submit(new Callable<Integer>() {
                 @Override
                 public Integer call() throws Exception {
                     System.out.println(Thread.currentThread().getName()+"正在计算...");
                     int j = new Random().nextInt(10);
                     return j;
                 }
             });
        }

        for (Future<Integer> t: f
             ) {
            try {
                Integer s = t.get();
                System.out.println(s);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
        }
    }
}
```