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

## numpy的索引

## numpy的array合并

## numpy的array分割

## numpy的copy&deep copy

# pandas

## pandas的生成序列、矩阵以及查看矩阵、排序

```python
import pandas as pd
import numpy as np
s = pd.Series([1,3,6,np.nan,44,1]) # 生成一个序列
dates = pd.date_range('20160101',periods=6) # 生成6个日期的序列
df = pd.DataFrame(np.random.randn(6,4),index=dates,columns=['a','b','c','d']) # 生成6行4列数据，每一行数据名为dates，每一列的数据名为，a b c d
df1 = pd.DataFrame(np.arange(12).reshape((3,4))) # 默认生成3行4列的矩阵，行名为0-n的数字，列名也一样
df2 = pd.DataFrame({
  'A':1., # A为列名，1为此列的所有内容为1
  'B':pd.Timestamp('20130102'),
  'C':pd.Series(1,index=list(range(4)),dtype='float32'),
  'D':np.array([3]*4,dtype='int32'),
  'E':pd.Categorical(['test','train','test','train']),
  'F':'foo'
})
#结果为
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo

df2.dtypes # 打印所有列的类型
df2.index # 返回所有行名
df2.columns # 返回所有列名
df2.values # 返回矩阵中所有值
df2.describe() # 返回矩阵中数字构成的列的count,mean,std方差,min,占比,max
df2.T # 矩阵反转，行转列，列转行
df2.sort_index(axis=1,ascending=False) # axis=1表示针对行排序=0表示针对列，ascending=False表示倒序
df2.sort_values(by='E') # 根据E列的值排序
```

## pandas选择数据

```python
import pandas as pd
import numpy as np
dates = pd.date_range('20130101', periods=6)
df = pd.DataFrame(np.arange(24).reshape((6,4)),index=dates,columns=['A','B','C','D'])
df['A'] # 输出A列数据
df.A # 同上
df[0:3] # df从0行到2行到数据
df['20130102':'20130104'] # 筛选20130102行到20130104行
df.loc['20130102'] # 20130102这个序列的数据
df.loc[:,['A','B']] # A和B列数据
df.iloc[3] # 第四行的数据
df.iloc[3,1] # 第四行第二个数据
df.iloc[3:5,1:3] # 第4-6行，第2-4列
df.iloc[[1,3,5],1:3] # 第2，4，6行的2-4列
df.ix[:3,['A','C']] # 筛选1-3行，A，C列的数据 已弃用
df.[df.A>8] # 矩阵中，A列数据>8的所有数据
```

## pandas设置值

```python
import pandas as pd
import numpy as np
dates = pd.date_range('20130101', periods=6)
df = pd.DataFrame(np.arange(24).reshape((6,4)),index=dates,columns=['A','B','C','D'])
df.iloc[2,2] = 111 # 这个位置的值改为111
df.loc['20130101','B'] = 2222
df[df.A>4] = 0 # A列值大于4的所有值包括其他列都为0
df.A[df.A>4] = 0 # A列值大于4的A列值都改为0
df.B[df.A>4] = 0 # A列值大于4的B列值都改为0
df['F'] = np.nan # 添加F列，所有值为nan
# 定义一个列，然后加上去
df['E'] = pd.Series([1,2,3,4,5,6],index=pd.date_range('20130101',periods=6))
```

## pandas处理丢失数据

```python
import pandas as pd
import numpy as np
dates = pd.date_range('20130101', periods=6)
df = pd.DataFrame(np.arange(24).reshape((6,4)),index=dates,columns=['A','B','C','D'])
df.iloc[0,1] = np.nan
df.iloc[1,2] = np.nan

df.dropna(axis=0, how='any') # how={'any','all'},any为出现nan就删除，all为全部为nan才删除，axis=0表示删除行，axis=1表示删除列
df.fillna(value=0) # 所有nan填充为0
df.isnull() # 返回矩阵每个数值是否为nan
np.any(df.isnull()) == True # 这个矩阵是否包含True这个值(df矩阵是否存在nan)
```

## pandas导入导出数据

```python
import pandas as pd
data = pd.read_csv('student.csv') # 读取
data.to_pickle("student.pickle") # 导入
```

## pandas合并concat

```python
import pandas as pd
import numpy as np
# concatenating
df1 = pd.DataFrame(np.ones((3,4))*0,columns=['a','b','c','d'])
df2 = pd.DataFrame(np.ones((3,4))*1,columns=['a','b','c','d'])
df3 = pd.DataFrame(np.ones((3,4))*2,columns=['a','b','c','d'])
res = pd.concat([df1,df2,df3],axis=0) # axis=0纵向合并，axis=1横向合并
res = pd.concat([df1,df2,df3],axis=0，ignore_index=True) # index（行名）会重新排序
```

```python
import pandas as pd
import numpy as np
df1 = pd.DataFrame(np.ones((3,4))*0,columns=['a','b','c','d'],index=[1,2,3])
df2 = pd.DataFrame(np.ones((3,4))*1,columns=['b','c','d','e'],index=[2,3,4])
res = pd.concat([df1,df2],join='outer')
# join为outer为扩展，inner为裁切
     a    b    c    d    e
1  0.0  0.0  0.0  0.0  NaN
2  0.0  0.0  0.0  0.0  NaN
3  0.0  0.0  0.0  0.0  NaN
2  NaN  1.0  1.0  1.0  1.0
3  NaN  1.0  1.0  1.0  1.0
4  NaN  1.0  1.0  1.0  1.0
# 裁切
res = pd.concat([df1,df2],join='inner',ignore_index = True)
     b    c    d
0  0.0  0.0  0.0
1  0.0  0.0  0.0
2  0.0  0.0  0.0
3  1.0  1.0  1.0
4  1.0  1.0  1.0
5  1.0  1.0  1.0

# axis=1 左右合并，join_axes以df1的index为准，没有的填充nan
res = pd.concat([df1, df2], axis=1, join_axes=[df1.index]) # 可能会报错
# TypeError: concat() got an unexpected keyword argument 'join_axes'
res = pd.concat([df1, df2.reindex(df1.index)], axis=1) #这个不会报错
     a    b    c    d    b    c    d    e
1  0.0  0.0  0.0  0.0  NaN  NaN  NaN  NaN
2  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0
3  0.0  0.0  0.0  0.0  1.0  1.0  1.0  1.0

res = df1.append([df2,df3],ignore_index=True)

```

## pandas合并merge

```python
import pandas as pd
left = pd.DataFrame({'key':['k0','k1','k2','k3'],
                    'A':['a0','a1','a2','a3'],
                    'B':['b0','b1','b2','b3']})
right = pd.DataFrame({'key':['k0','k1','k2','k3'],
                    'C':['c0','c1','c2','c3'],
                    'D':['d0','d1','d2','d3']})
res = pd.merge(left,right,on = 'key') # 基于key进行合并

```

## pandas plot画图

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

#Series
data = pd.Series(np.random.randn(1000),index = np.arange(1000))
data = data.cumsum()
data.plot()
plt.show()

#DataFrame
data = pd.DataFrame(np.random.randn(1000,4),
                   index=np.arange(1000),
                   columns=list('abcd'))
data = data.cumsum()
data.plot()
plt.show()
#plot methods:
# 'bar'条状，'hist' ,'box', 'kde' 'area' 'scatter'点
data=data.cumsum()
ax = data.plot.scatter(x='a',y='b',color='DarkBlue',label='Class 1')
data.plot.scatter(x='a',y='b',color='DarkGreen',label='Class 2',ax=ax)
plt.show()
```

