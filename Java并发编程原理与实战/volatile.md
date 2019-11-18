[toc]

# 深入理解volatile原理与使用

## 原理

Volatile称之为轻量级锁，被volatile修饰的变量，在线程之间是可见的

可见：一个线程修改了这个变量的值，在另外一个线程中能够读到这个修改后的值

可见性的前提：多个线程需要的是同一把锁

Synchronized除了线程之间互斥以外，还有一个非常大的作用，就是保证可见性

```Java
public class MultiThreadMain {
    public  volatile boolean run = false;

    public static void main(String[] args) {
        MultiThreadMain m = new MultiThreadMain();

        new Thread(()->{
            for (int i = 1;i<=10;i++){
                System.out.println("执行了第"+i+"次");
                try {
                    Thread.sleep(1000);
                }catch (InterruptedException e){
                    e.printStackTrace();
                }
            }
            m.run = true ;
        }).start();

        new Thread(()->{
            while (!m.run){}
            System.out.println("线程2执行了");
        }).start();
    }
}
```

如果不给run变量加volatile，“线程2执行了”这句话不会被打印出来，因为第二个线程不觉得run变量改变了

如果给run变量加volatile，就会打印出结果，第二个线程会立刻知道run变量改变了。（这就是可见性）

可见性基于编译器的Lock指令

## Lock指令

在多处理器的系统上，将当前处理器缓存行的内容写回到系统内存

这个写回到内存的操作会使在其他CPU里缓存了该内存地址的数据失效

所以多处理器又重新去内存中拿数据，于是就保证了可见性



**volatile只能保证变量的可见性，并不能保证操作的原子性，所以要与synchronized区分开**