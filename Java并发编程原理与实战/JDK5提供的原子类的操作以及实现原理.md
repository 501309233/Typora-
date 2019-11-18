[toc]

# 原子类的操作以及实现

## 原子更新基本类型

```java
public class MultiThreadMain {
  	//原子基本数据类型
    private AtomicInteger value = new AtomicInteger(0);
    public int getNext(){
      //也可以value.getAndAdd(10);每次+10
        return value.getAndIncrement();
    }

    public static void main(String[] args) {
        MultiThreadMain m = new MultiThreadMain();
        new Thread(()->{
            while (true){
                System.out.println(Thread.currentThread().getName()+":"+m.getNext());

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
        new Thread(()->{
            while (true){
                System.out.println(Thread.currentThread().getName()+":"+m.getNext());

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    new Thread(()->{
            while (true){
                System.out.println(Thread.currentThread().getName()+":"+m.getNext());

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
```



## 原子更新数组

## 原子更新抽象类型

## 原子更新字段