[toc]

# Happens-before

* Happens-before是用来指定两个操作之间的执行顺序，提供跨线程的内存可见性
* 在java内存模型中，如果一个操作执行的结果需要对另一个操作可见，那么这两个操作之间必然存在happens-before关系。
* Happens-before规则如下：
  * 程序顺序规则
  * 监视器锁规则
  * volatile变量规则
  * 传递性
  * start规则
  * join规则



## 程序顺序规则

单个线程中对每个操作，总是前一个操作happens-before于该线程中的任意后续操作

## 监视器规则

对一个锁的解锁，总是happens-before于随后对这个锁的加锁

## volatile变量规则

对一个volatile域的写，happens-before于任意后续对这个volatile域的读

## 传递性

A happens-before B

B happens-before C

则A happens-before C

## start规则

A线程中startB线程，那么A线程happens-beforeB线程

## Join规则

A线程JoinB线程，那么B线程happens-beforeA线程



