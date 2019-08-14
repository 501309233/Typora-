[TOC]

# pickle模块

python标准库提供pickle模块，pickle模块实现了基本的数据序列和反序列化

通过pickle模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储，也可以简单的讲字符进行序列化。通过pickle模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象，也可以将字符进行反序列化

pickle可以存储什么类型的数据：

1. 所有python支持的原生类型：布尔值，整数，浮点数，复数，字符串，字节，None
2. 由任何原生类型组成的列表，元组，字典，集合
3. 函数，类，类的实例

## 四个基本功能

pickle模块提供dumps、loads、dump、loap四个基本功能：

* dumps和loads用于python对象和字符串间的序列化和反序列化
* dump和loap用于对文件进行序列化和反序列化，python数据持久化用的比较多

使用pickle模块，需要先导入

## 与json的区别

tips：pickle和json的功能类似，与json不同的是：

1. json更适合跨语言，可以处理字符串，基本数据类型；
2. pickle python专有，更适合处理复杂类型的序列化

## 将对象存储在变量中

### pickle.dumps(obj)

将数据通过特殊的形式转换为只有python语言认识的字符串

![1565664371495](E:\Typora笔记\Python\assets\1565664371495.png)

### pickle.loads(Str)

loads将pickle数据转换为python的数据结构

![1565664425052](E:\Typora笔记\Python\assets\1565664425052.png)

## 将对象存储在文件中

### pickle.dump(obj,file)

将数据通过特殊的形式转换为只有python语言认识的字符串，并写入文件

### pickle.load(file)

从数据文件中读取数据，并转换为python的数据结构

![1565664541137](E:\Typora笔记\Python\assets\1565664541137.png)

![1565664554842](E:\Typora笔记\Python\assets\1565664554842.png)

## 存储函数，类，类的实例

pickle不仅可以写入字符串，字典等数据，还可以写入类，以及类的实例

![1565664777494](E:\Typora笔记\Python\assets\1565664777494.png)

![1565664849404](E:\Typora笔记\Python\assets\1565664849404.png)

