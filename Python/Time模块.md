[TOC]

# Time模块

包含了以下内置函数，既有事件处理，也有转换时间格式：

| 函数                          | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| time.altzone                  | 返回格林威治西部的夏令时地区的偏移秒数。                     |
| time.asctime([tupletime])     | 接受时间元组并返回一个可读的形式为"Tue Dec 11 18:07:14 2008"（2008年12月11日 周二18时07分14秒）的24个字符的字符串。 |
| time.clock()                  | 用以浮点数计算的秒数返回当前的CPU时间。用来衡量不同程序的耗时，比time.time()更有用。 |
| time.ctime([secs])            | 将时间戳转换为时间字符串，如果没有给参数则返回当前时间的时间字符串 |
| time.gmtime([secs])           | 接收时间戳并返回格林威治天文时间下的时间元组t。注：t.tm_isdst始终为0 |
| time.localtime([secs])        | 接收时间戳并返回当地时间下的时间元组t（t.tm_isdst可取0或1，取决于当地当时是不是夏令时） |
| time.strptime(string[format]) | 根据指定的格式把一个时间字符串解析为时间元组。               |
| time.mktime(tupletime)        | 接受时间元组并返回时间戳                                     |
| time.sleep(secs)              | 推迟调用线程的运行，secs指秒数。（就是运行中暂停几秒）       |
| time.strftime(fmt)            | 返回以可读字符串表示的当地时间，格式由fmt决定。              |
| time.time()                   | 返回当前时间的时间戳                                         |
| time.tzset()                  | 根据环境变量TZ重新初始化时间相关设置。                       |

时间戳：是指1970纪元之后经过的浮点秒数

time模块中时间表现的格式主要有三种：

1. timestamp时间戳，
2. struct_time时间元组，共有九个元素组
3. format time格式化时间，已格式化的结构使时间更具可读性，包括自定义格式和固定格式

## 格式转换

引入模块

```python
import time,datatime
```

### str类型的日期转换为时间戳

```python
# 字符类型的时间
tss1 = '2013-10-10 23:40:00'
# 转为时间数组
timeArray = time.strptime(tss1, "%Y-%m-%d %H:%M:%S")
print timeArray     
# timeArray可以调用tm_year等
print timeArray.tm_year   # 2013
# 转为时间戳
timeStamp = int(time.mktime(timeArray))
print timeStamp  # 1381419600


# 结果如下
time.struct_time(tm_year=2013, tm_mon=10, tm_mday=10, tm_hour=23, tm_min=40, tm_sec=0, tm_wday=3, tm_yday=283, tm_isdst=-1)
2013
1381419600
```

### 更改str类型日期的显示格式

```python
tss2 = "2013-10-10 23:40:00"
# 转为数组
timeArray = time.strptime(tss2, "%Y-%m-%d %H:%M:%S")
# 转为其它显示格式
otherStyleTime = time.strftime("%Y/%m/%d %H:%M:%S", timeArray)
print otherStyleTime  # 2013/10/10 23:40:00

tss3 = "2013/10/10 23:40:00"
timeArray = time.strptime(tss3, "%Y/%m/%d %H:%M:%S")
otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
print otherStyleTime  # 2013-10-10 23:40:00
```

### 时间戳转换为指定格式的日期

```python
# 使用time
timeStamp = 1381419600
timeArray = time.localtime(timeStamp)
otherStyleTime = time.strftime("%Y--%m--%d %H:%M:%S", timeArray)
print otherStyleTime   # 2013--10--10 23:40:00
# 使用datetime
timeStamp = 1381419600
dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
otherStyleTime = dateArray.strftime("%Y--%m--%d %H:%M:%S")
print otherStyleTime   # 2013--10--10 15:40:00
```

### 获取当前时间并且用指定格式显示

```python
# time获取当前时间戳
now = int(time.time())     # 1533952277
timeArray = time.localtime(now)
print timeArray
otherStyleTime = time.strftime("%Y--%m--%d %H:%M:%S", timeArray)
print otherStyleTime    

# 结果如下
time.struct_time(tm_year=2018, tm_mon=8, tm_mday=11, tm_hour=9, tm_min=51, tm_sec=17, tm_wday=5, tm_yday=223, tm_isdst=0)
2018--08--11 09:51:17


# datetime获取当前时间，数组格式
now = datetime.datetime.now()
print now
otherStyleTime = now.strftime("%Y--%m--%d %H:%M:%S")
print otherStyleTime  

# 结果如下：
2018-08-11 09:51:17.362986
2018--08--11 09:51:17
```

## 获取当前时间

* time.time()函数返回当前时间的时间戳
* time.localtime([secs])函数接收时间戳作为参数，并返回当前时间的struct_time元组

![1565684555246](E:\Typora笔记\Python\assets\1565684555246.png)

根据以上的结果参考struct_time元组的属性

| 属性     | 值                                   |
| -------- | ------------------------------------ |
| tm_year  | 四位数的年                           |
| tm_mon   | 1到12                                |
| tm_mday  | 1到31                                |
| tm_hour  | 0到23                                |
| tm_min   | 0到59                                |
| tm_sec   | 0到61（60或61 是闰秒）               |
| tm_wday  | 0到6（0是周一）                      |
| tm_yday  | 1到366                               |
| tm_isdst | -1，0，1，-1是决定是否为夏令时的旗帜 |

从示例1中获取到的当前时间元组可读性较差，这时可以使用time.asctime()、time.ctime()函数把时间元组、时间戳转化为简单可读的时间字符串模式

* time.asctime([tupletime])函数接收struct_time元组作为参数，并返回形式为"Tue Dec 11 18:07:14 2008"的时间字符串
* time.ctime([secs])函数接收时间戳作为参数，返回时间戳对应的时间字符串，如果参数为空则返回当前时间的时间字符串

![1565684965177](E:\Typora笔记\Python\assets\1565684965177.png)

## 格式化时间

time.strftime(fmt)函数返回一个可读字符串表示的当地时间，格式由fmt决定

python中时间日期格式化符号：

%y 两位数的年份表示（00-99）

%Y 四位数的年份表示（000-9999）

%m 月份（01-12）

%d 月内中的一天（0-31）

%H 24小时制小时数（0-23）

%I 12小时制小时数（01-12） 

%M 分钟数（00=59）

%S 秒（00-59）

 

%a 本地简化星期名称

%A 本地完整星期名称

%b 本地简化的月份名称

%B 本地完整的月份名称

%c 本地相应的日期表示和时间表示

%j 年内的一天（001-366）

%p 本地A.M.或P.M.的等价符

%U 一年中的星期数（00-53）星期天为星期的开始

%w 星期（0-6），星期天为星期的开始

%W 一年中的星期数（00-53）星期一为星期的开始

%x 本地相应的日期表示

%X 本地相应的时间表示

%Z 当前时区的名称

%% %号本身 

![1565685280702](E:\Typora笔记\Python\assets\1565685280702.png)

time 模块的内置函数还可以将指定的格式化字符串时间再转化为时间元组 或者 时间戳

* time.strptime(string[,format])函数根据指定格式把一个时间字符串解析为时间元组，string是时间字符串。format是格式化字符串，返回struct_time对象
* time.mktime(tupletime)函数把时间元组转化为时间戳

![1565685546813](E:\Typora笔记\Python\assets\1565685546813.png)

## 推迟调用线程的运行

time.sleep(secs)函数推迟调用线程的运行，可通过参数secs指秒数，表示进程挂起的时间

![1565685625896](E:\Typora笔记\Python\assets\1565685625896.png)

