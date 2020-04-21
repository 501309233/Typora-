[toc]

# tf.keras概述

## tf实现一个简单的线性回归

```python
import tensorflow as tf
#查看版本-tf.__version__
import pandas as pd #用来处理文件
data = pd.read_csv('./dataset/Income1.csv')
import matplotlib.pyplot as plt #用来生成图像
#  %matplotlib inline #在使用jupyter notebook时直接在console中生成图像
plt.scatter(data.Education, data.Income) #绘制成散点图
x = data.Education
y = data.Income
model = tf.keras.Sequential()
#Dense(输出维度，输入维度为1)
model.add(tf.keras.layers.Dense(1, input_shape(1,)))
model.summary() #输出model的模型
model.compile(optimizer='adma',#编译,optimizer优化方法，常用adma
             loss='mse')#损失函数，均方差（mse）
#导入数据，训练5000次
history = model.fit(x, y, epochs=5000)
#用模型进行预测
model.predict(x)  # 传入数据x，类型为pd.Series
#例子model.predict(pd.Series([20]))-->50.430862
```



# 多层感知器的实现

```python
import tensorflow as tf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('dataset/Advertising.csv')
plt.scatter(data.TV,data.sales)
x = data.iloc[:, 1:-1] #取所有数据，从第一列到最后一列
y = data.iloc[:, -1]  #取最后一列
model = tf.keras.Sequential([tf.keras.layers.Dense(10, input_shape(3,), activation='relu'),tf.keras.layers.Dense(1)])
model.summary()
model.compile(optimizer='adam', loss='mse')
model.fit(x, y, epochs=100)
test = data.iloc[:10, 1:-1]	#前十行，
model.predict(test)
```

# 逻辑回归与交叉熵

sigmoid函数是一个概率分布函数

给定某个输入，它将输出为一个概率值

平方差所惩罚的是与损失为同一数量级的情形

对于分类问题，我们最好使用交叉熵损失函数会更有效

交叉熵会输出一个更大的“损失”

keras交叉熵-在keras中，我们使用binary_crossentropy来计算二院交叉熵

```python
import tensorflow as tf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('dataset/credit-a.csv', header = None)  # 没有头部属性名
data.head()
data.iloc[:, -1].value_counts() # 取数据最后一列全部数据，分类并统计个数
x = data.iloc[:, :-1] # 取出除最后一列的所有数据
y = data.iloc[:, -1].replace(-1,0) # 取出最后一列所有数据并将-1替换成0
model = tf.keras.Sequential()
model.add(tf.keras.layers.Dense(4,input_shape=(15,), activation='relu'))
model.add(tf.keras.layers.Dense(4, activation='relu'))
model.add(tf.keras.layers.Dense(1, activation='sigmoid'))
model.summary()
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['acc'] # 成功概率
)
history = model.fit(x, y, epochs=100)
history.history.keys() #返回dict_keys(['loss','acc'])
plt.plot(history.epoch, history.history.get('loss')) # 输出训练次数与损失的关系图
plt.plot(history.epoch, history.history.get('acc')) # 输出训练次数与成功率的关系图


```

# softmax分类

对数几率回归（sigmoid）解决的是二分类问题

对于多个选项的问题，我们可以使用softmax函数

它是对数几率回归在N个可能不同的值上的推广

* softmax层的作用就是

  ​	神经网络的原始输出不是一个概率值，实质上是输入的数值做了复杂的加权和非线性处理之后的一个值而已，softmax把这些值变成概率分布

* softmax样本分量之和为1，当两个类别时与对数几率回归结果相同

* 对于损失值使用softmax交叉熵

  * categorical_crossentropy
  * sparse_categorical_crossentropy

## Fashion MNIST数据集

MNIST数据集包含手写数字(0,1,2等)的图像

Fashion MNIST比常规MNIST手写数据集更具挑战性

* 这两个数据集都相对较小，用于验证某个算法能否如期正常进行。它们都是测试和调试代码的良好起点

* 可以从TensorFlow直接访问Fashion MNIST, 只需导入和加载数据

```python
import tensorflow as tf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

(train_image, train_label), (test_image, test_label) = tf.keras.datasets.fashion_mnist.load_data()

```

