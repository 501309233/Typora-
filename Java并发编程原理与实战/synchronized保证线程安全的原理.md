[toc]

# synchronized原理与使用

## 理论层面

### 内置锁

多线程的锁，其实本质上就是给一块内存空间的访问添加访问权限，因为Java中是没有办法直接对某一块内存进行操作的，又因为Java是面向对象的语言，一切皆对象，所以具体的表现就是某一个对象承担锁的功能，每一个对象都可以是一个锁。内置锁，使用方式就是使用 synchronized 关键字，synchronized 方法或者 synchronized 代码块。

synchronized 是内置于JDK中的，底层实现是native；同时，加锁、解锁都是JDK自动完成，不需要用户显式地控制，非常方便。

### 互斥锁（同步锁）

当使用synchroinzed锁住多段不同的代码片段， 但是这些同步块使用的同步监视器对象是同一个时，那么这些代码片段之间就是互斥的。多个线程不能同时执行他们。

synchronized 用于同步线程，使线程互斥地访问某段代码块、方法。这就是意味着最多只有一个线程能够获得该锁，当线程A尝试去获得线程B持有的内置锁时，线程A必须等待或者阻塞，直到线程B释放这个锁，如果B线程不释放这个锁，那么A线程将永远等待下去。

**对象锁的粒度要比类锁的粒度要细，引起线程竞争锁的情况比类锁要少的多，所以尽量别用类锁，锁的粒度越少越好。**

### 对象锁和类锁

synchronized 以当前的某个对象为锁，线程必须通过互斥竞争拿到这把对象锁，从而才能访问 临界区的代码，访问结束后，就会释放锁，下一个线程才有可能获取锁来访问临界区（被锁住的代码区域）。synchronized锁 根据锁的范围分为 对象锁 和 类锁。对象锁，是以对象实例为锁，当多个线程共享访问这个对象实例下的临界区，只需要竞争这个对象实例便可，不同对象实例下的临界区是不用互斥访问；而类锁，则是以类的class对象为锁，这个锁下的临界区，所有线程都必须互斥访问，尽管是使用了不同的对象实例；

### 重入锁

```java
public class MultiThreadMain {
    public synchronized void a () {
        System.out.println("a()");
        b();
    }
    public synchronized void b(){
        System.out.println("b()");
    }
}
```

a()和b()都是是对象锁，当线程访问a()时，已经拿到了对象锁，如果再调用b()方法也要有对象锁，而synchronize是允许这种情况出现的，这种规则被称为重入锁。

如果同一个线程拿到对象锁，它可以访问该对象中所有对象锁方法或变量

如果不同线程访问同一对象的不同的加锁方法，”速度慢“的线程要等速度快的线程释放掉对象锁才能访问

如果不同线程访问不同对象的加锁方法，完全不存在任何阻塞问题，除非有线程拿了类锁

### 自旋锁

空转CPU的时间片，等待另外的线程执行完毕释放锁

### 公平锁

公平锁是针对锁的获取而言的，如果一个锁是公平的，那么锁的获取顺序就应该符合请求的绝对时间顺序

### 偏向锁

每次获取锁和释放锁会浪费资源

很多情况下，竞争锁不是由多个线程，而是由一个线程在使用。

只有一个线程在访问同步代码块的场景



### 轻量级锁

自旋

就是让该线程等待一段时间，不会被立即挂起，看持有锁的线程是否会很快释放锁。怎么等待呢？执行一段无意义的循环即可（自旋）

多个线程可以同时

**轻量级锁的目标是，减少无实际竞争情况下，使用重量级锁产生的性能消耗**

### 重量级锁

只允许一个线程执行同步代码块，其他线程想要执行必须等待

### 读写锁

就是共享锁和排他锁

读操作是共享锁允许多个读线程进行，写操作是排他锁只允许一个写线程进行

只有读线程执行完毕，写线程才能执行

读写锁需要保存的状态：

* 读锁的个数
* 写锁重入的次数
* 每个读锁重入的次数

```Java
public class Demo   {
    private Map<String,Object> map = new HashMap<>();
    private ReadWriteLock rwl = new ReentrantReadWriteLock();

    private Lock r = rwl.readLock();
    private Lock w = rwl.writeLock();

    public Object get(String key){
        r.lock();
        try {
            return map.get(key);
        }finally {
            r.unlock();
        }
    }

    public void put(String key,Object value){
        w.lock();
        try{
            map.put(key, value);
        }finally {
            w.unlock();
        }
    }
}
```

* 锁降级：写锁降级为读锁，在写锁没有释放的时候获取读锁，然后再释放写锁
* 锁升级：（Reentrant不支持）读锁升级为写锁，读锁没有释放的时候获取写锁，然后再释放读锁 。

### 修饰普通方法

内置锁就是当前类的实例（对象锁）

### 修饰静态方法

内置锁就是当前的Class字节码对象（类锁）

### 修饰代码块

* object是this，就是对象锁，this指当前对象
* object是一个普通对象实例
  * 如果是静态对象，就是类锁
  * 如果是是非静态对象：成员对象变量、局部变量（甚至可以是方法参数），就是对象锁
* object是一个类的class对象，就是类锁



## jvm层面

进入同步代码块：

​	.class文件中代码执行monitorenter（监视器进入，获取锁）

退出同步代码块：

​	.class文件中代码执行monitorexit（监视器退出，释放锁）

同步代码块：JVM需要保证每一个monitorenter都有一个monitorexit与之相对应，任何对象都有一个monitor与之相关联，当且一个monitor被持有之后，他将处于锁定状态。线程执行到monitorenter指令时，将会尝试获取对象所对应的monitor所有权，即尝试获取对象的锁；

同步方法：synchronized方法则会被翻译成普通的方法调用和返回指令如:invokevirtual、areturn指令，在VM字节码层面并没有任何特别的指令来实现被synchronized修饰的方法，而是在Class文件的方法表中将该方法的access_flags字段中的synchronized标志位置1，表示该方法是同步方法并使用调用该方法的对象或该方法所属的Class在JVM的内部对象表示Klass做为锁对象。(摘自：http://www.cnblogs.com/javaminer/p/3889023.html)


### 任何对象都可以作为锁，那么锁信息又存在对象的什么地方呢？

* 存在对象头中

### 对象头中的信息

	* Mark Word(存hash值和锁信息)
	* class Metadata Address
	* Array Length

