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

