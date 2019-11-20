[toc]

# 解决多线程安全问题的解决方案

* Synchronized-性能低下
* Volatile-只能保证可见性，不能保证原子操作
* Atomic-功能有限

都有局限性

与Lock相比

Lock需要显式地获取和释放锁--繁琐但灵活--可以在一种锁的内部释放另外一个锁，释放锁的操作还可以放在finally代码块中

Synchronized不需要显式地获取和释放锁--简单

使用Lock可以方便的实现公平性

非阻塞的获取锁

能被中断的获取锁

超时获取锁

```java
public class MultiThreadMain {
    private int value;
  	//定义两个锁
    Lock lock = new ReentrantLock();
    Lock lk1 = new ReentrantLock();

    public int getNext(){
        lk1.lock();
        System.out.println("从前有一个小女孩");
        lock.lock();
        int a = value++;
        lk1.unlock();
        lock.unlock();
        return a;
    }
}
```

# 自己实现一个锁Lock

