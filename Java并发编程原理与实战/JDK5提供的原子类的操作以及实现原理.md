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

```java
private int[] s = {1,43,2,5,3};
AtomicIntegerArray a = new AtomicIntegerArray(s);
//让数组中2位置的数字返回然后自增
a.getAndIncrement(2);
//让数组中3位置的数字返回然后增加10
a.getAndAdd(3,10);
return value.getAndIncrement();
```

## 原子更新抽象类型



## 原子更新字段

```java
User u = new User();
AtomicIntegerFieldUpdater<User> old =AtomicIntegerFieldUpdater.newUpdater(User.class,"old");
//把a=u.old+2变成了原子操作
int a = old.addAndGet(u,2);
```

