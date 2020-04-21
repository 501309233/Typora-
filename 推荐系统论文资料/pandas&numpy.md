[toc]

# numpy

## numpy属性

```python
import numpy as np
array = np.array([[1,2,3],[2,3,4]])
print(array) # 输出一个矩阵
print('number of dimation:', array.ndim) # 输出这是几维矩阵 2
print('shape:',array.shape) # 输出行数和列数 （2，3）
print('size:',array.size) # 一共有多少个元素 6

```

## numpy创建array

```python
import numpy as np

a = np.array([2,23,4], dtype = np.int) # 定义它的类型,也可以为int32,float,float32
print(a) # [2 23 4]
print(a.dtype) # int64

b = np.array([[2,23,4],[2,23,4]])
print(b) # 输出一个两行三列的矩阵
c = np.zeros((3,4)) # 输出一个3行4列全为0的矩阵
d = np.ones((3,4), dtype=np.int16) #输出一个3行4列全为1的矩阵
e = np.empty((3,4)) # 生成一个3行4列全部极端接近于0的矩阵
f = np.arange(10,20,2) # 生成[10 12 14 16 18]
g = np.arange(12).reshape((3,4)) # 生成一个从0-11 3行4列的矩阵
h = np.linspace(1,10,20) # 生成一个从1-10的一共有20个数字（步长相同）的数列
i = np.linspace(1,10,6).reshape((2,3)) # 生成一个2行3列步长相同的从1-10的矩阵
```

## numpy的基础运算

```python
import numpy as np
a = np.array([10,20,30,40])
b = np.arange(4)

#加减，三角函数，比较
c = a - b # 逐个相减 [10,19,28,37]
c = b**2 # b的平方 [0,1,4,9]
c = 10*np.sin(a) # 对a逐个进行三角函数sin运算然后乘10，也可以进行cos,tan
print(b<3) # 输出一个列表，每个元素是否<3, [True True True False]

# 矩阵乘法
a = np.array([[1,1],[0,1]])
b = np.arange(4).reshape((2,2))
c = a*b # 矩阵点乘
c_dot = np.dot(a,b) #矩阵叉乘 也可以写成 c_dot_2 = a.dot(b)

# 求和，最大值，最小值
a = np.random.random((2,4)) # 随机生成2行4列0-1的数字矩阵
np.sum(a)	# 矩阵内数字求和
np.min(a) # 矩阵内最小数字
np.max(a) # 矩阵内最大数字
np.sum(a,axis=1) # axis=1每行分别求和，输出一个数组每一个数字代表该行求和
np.min(a,axis=0) # axis=0寻找每列的最大值


a = np.arange(2,14),reshape((3,4))
np.argmin(a) # a的最小索引0
np.argmax(a) # a的最大索引11
np.mean(a) # 输出平均值7.5
np.mean(a,axis=0) # 对每一列计算平均值生成一个列表
a.mean() # 同上
np.median(a) # a的中位数7.5
np.cumsum(a) # 逐步相加，第3位数为前3个数加起来的值，第9位数为前9个数加起来的值
np.diff(a) # 生成一个3行3列的矩阵，每个数为原矩阵对应位置 后面-当前
np.nonzero(a) # 生成两个列表，第一个表示行，第二个表示列，以指示原矩阵非0数
np.sort(a) # 逐行排序
np.transpose(a) # 行列反转，将列变成行，行变成列
a.T # 同上
a.T.dot(a) # 矩阵反转之后与原矩阵叉乘
np.clip(a, 5, 9) # 原矩阵中>5和<9的数不变，<5的变成5，>9的变成9



```

