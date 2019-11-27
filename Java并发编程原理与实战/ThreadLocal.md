每个线程访问这个变量（如：count），与其他线程的访问毫不相干。

* 实现原理：用map来存储，key为线程，value为值

```java
public class DemoLocalThread {
    ThreadLocal<Integer> count = new ThreadLocal<Integer>(){
        @Override
        protected Integer initialValue() {

            return new Integer(0);
        }
    };

    public int getNext(){
        Integer value = count.get();
        value++;
        count.set(value);
        return value;
    }

    public static void main(String[] args) {
        DemoLocalThread d = new DemoLocalThread();
        new Thread(()->{
            while (true) {
                System.out.println(Thread.currentThread().getName() + ":::" + d.getNext());
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
        new Thread(()->{
            while (true) {
                System.out.println(Thread.currentThread().getName() + ":::" + d.getNext());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
        new Thread(()->{
            while (true) {
                System.out.println(Thread.currentThread().getName() + ":::" + d.getNext());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }

}
```