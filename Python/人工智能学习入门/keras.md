[toc]

## keras实现线性回归

```python
model = keras.Sequential() #初始化一个顺序模型
from keras import layers #可以建立层
# Dense(输出维度1，input_dim=输入维度1)用于建立x和y的函数
# 如y=ax+b 输入x（train数据），输出y（目标数据）
# 简单来说-layers.Dense就是建立y和x的关系 

model.add(layers.Dense(1,input_dim=1))
model.summary() # 显示模型参数
# 开始编译模型
model.compile(optimizer='adam', # 内置的优化算法
              loss='mse' # 均方差
)
#训练模型
model.fit(x, y, epochs=3000) # epochs把所有数据训练3000遍
model.predict(x) # 用训练好的参数来预测x->y
plt.scatter(x, y, c='r') # 原数据画图,c='r'显示红色
plt.plot(x, model.predict(x)) # 训练出的参数画图
```

## keras实现多元线性回归

```python
import pandas as pd
import keras
from keras import layers
data = pd.read_csv('./dataset/Advertising.csv')
data.head() # 看数据的名字和前五行数据
x = data[data.colums[1:-1]] #取出第二列到倒数第二列
y = data.iloc[:,-1] # 取出最后一列的所有数据
model = keras.Sequential() #初始化一个顺序模型
model.add(layers.Dense(1,input_dim=3)) # 输出维度为1，输入维度为3
model.summary() # 显示模型参数
model.compile(optimizer='adam',
              loss='mse' # 均方差
) 
model.fit(x, y, epochs=2000)
model.predict(pd.DataFrame([300,0,0])) # 进行预测
```

## 逻辑回归（Sigmoid函数）

线性回归预测的是一个连续的值

逻辑回归给出的"是"和“否”的回答

对于逻辑回归的话，他的损失函数就不能用均方差，因为数值太小（0-1）

* 交叉熵损失函数

交叉熵刻画的是实际输出（概率）与期望输出（概率）的距离，也就是交叉熵的值越小，两个概率分布就越近

在keras中，我们使用binary_crossentropy来计算二元交叉熵

## softmax分类

对于多个选项的问题，我们可以使用softmax函数

他说对数几率回归在N个可能不同的值上的推广

神经网络的原始输出不是一个概率值，实质上只是输入的数值做了复杂的加权和与非线性处理之后的一个值而已，那么如何将这个输出变为概率分布？那就使用softmax

在keras里，对于多分类问题我们使用

* categorical_crossentropy
* sparse_categorical_crossentropy

来计算softmax交叉熵

```python
import keras
from keras import layers
import matplotlib.pyplot as plt
import keras.datasets.mnist as mnist # keras内置的手写字数据集
(train_image, train_label),(test_image, test_lable)=mnist.load_data()
train_image.shape    #（60000,28,28)60000张28*28的图片
plt.imshow(train_image[0]) # 显示第一章图片
train_label.shape # (60000,)
train_label[0] # 5
test_image.shape,test_label.shape # ((10000,28,28),(10000))
model = keras.Sequential()
model.add(layers.Flatten()) #第一层把它展开为二维的形式(60000,28,28)-->(60000,28*28)
model.add(layers.Dense(64,activation='relu'))
model.add(layers.Dense(10, activation="softmax"))
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy',
              metrics=['acc']
)
model.fit(train_image, train_label, epochs=50, batch=512) # 分批次每批次512个数据进入训练，所有数据训练50遍
model.evaluate(test_image, test_label) #查看得分[2.0327, 0.8713]
model.evaluate(train_image, train_label) # 查看训练集的得分[1.86586, 0.8825]
# 综合说明对数据的拟合不够
import numpy as np
np.argmax(model.predict(test_image[:10]), axis=1) # 可以查看预测前10个数据
# array([7,2,1,9,4,1,4,9,5,9])
# 然后对比一下正确答案
test_label[:10]
# array([7,2,1,0,4,1,4,9,5,9])
#可以发现第四个数据预测错误（训练的还不够）



# 模型优化
model = keras.Sequential()
model.add(layers.Flatten()) #第一层把它展开为二维的形式(60000,28,28)-->(60000,28*28)
model.add(layers.Dense(64,activation='relu'))
model.add(layers.Dense(64,activation='relu'))
model.add(layers.Dense(64,activation='relu'))
model.add(layers.Dense(10, activation="softmax"))
model.compile(optimizer='adam', 
        loss='sparse_categorical_crossentropy',
              metrics=['acc']
)
model.fit(train_image, train_label, epochs=50, batch=512, validation_data=(test_image, test_label))

```



