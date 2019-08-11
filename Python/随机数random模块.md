[TOC]

#Random模块

##生成随机数

| 函数                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| random.random()                   | 用于生成一个0-1的随机浮点数                                  |
| random.uniform(a,b)               | 用于生成一个指定范围内的随机浮点数，两个参数其中一个是上限，一个是下限，生成的随机数在此范围内 |
| random.randint(a,b)               | 用于生成一个指定范围内的整数，其中参数a是下限，参数b是上限   |
| random.randrange(start,stop,step) | 从指定范围内，按指定基数递增的集合中获取一个随机数           |

![1565318332400](E:\Typora笔记\Python\assets\1565318332400.png)

##生成随机字符

###random.choice()

该方法随机返回序列中的一个元素

```python
random.choice(sequence)
```

* 参数sequence表示一个序列

![1565319185166](E:\Typora笔记\Python\assets\1565319185166.png)

###random.sample()

从序列中选择n个随机且独立的元素

```python
random.sample(sequence,k)
```

* 参数sequence表示一个序列，k表示元素个数

返回：返回多个随即元素组成的列表。该方法不会修改原有序列

![1565319280627](E:\Typora笔记\Python\assets\1565319280627.png)

##列表洗牌

###random.shuffle()

返回：一个新的对象，是打乱元素的列表

![1565319454219](E:\Typora笔记\Python\assets\1565319454219.png)

