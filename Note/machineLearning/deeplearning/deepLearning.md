# DeepLearning

> 这篇作为 学习李宏毅视频中深度学习部分的笔记

LiHoneYi 整个课的框架

![](/Users/oliver/Desktop/AI_photo/machinelearning_hongyi/machinelearning.png)

这里有点奇怪就是deeplearning 为什么都是分类问题，咱们之前讲的不是也可以做生成吗

### Keras 实战

- keras vs TensorFlow vs Theano

  Keras 是 tensorflow 和 Theano 上层的封装的结构，也就是keras 其实是使用 TensorFlow 或者 Theano 的模型包，使用者可以更容易的设置模型的一些参数快速搭建模型

  ![](/Users/oliver/Desktop/AI_photo/machinelearning_hongyi/keras.png)

  > Keras 可以在1 hour 内 熟悉

- keras 实战

  - 搭建深度学习步骤

  ~~~python
  # minist 的例子
  # 定义 set of fuction
  model = Sequential()
  ## 输入是28*28 隐层 500个节点
  model.add( Dense(input_dim=28*28),output_dim=500)
  ## 非线性
  model.add (Activation('sigmoid'))
  ## 输出层
  model.add(Dense(output_dim=10))
  # 定义评价方程
  # 定义优化方程
  # 模型保存和评价
  ~~~

  - Mini-batch:我们其实不是真正最小化total loss , 每个batch 指的是训练集的一个划分单元，batch size 指的是每个 batch 中样例的个数，batch size = 1,即是Stochastic gradient descent (随机梯度下降)，每个样例更新一次参数

    使用mini-batch 原因：

    - 性能要求： batch size 越大,时间消耗越小，参数越稳定，应为batch size 比较大，每个样例可以进行并行运算时间消耗少
    - batch 过大：没有随机性，更新参数的数目变少，模型性能变差
    - batch 过小: 极端情况例如随机梯度下降，性能变差

    > 其实 batch size 
    >
    > epoch

  - Keras 2.0

    实例 MINIST 手写识别,输入 28*28,两个隐层，10维输出层

    ~~~python
    model = Sequential()
    model.add(Dense(input_dim=28*28,units=500,activation='relu'))
    model.add(Dense(units=500,activation='relu'))
    model.add(Dense(units=10,activation='softmax'))
    #
    model.compile(loss='categorical_crossentropy',optimizer='adam',
                  metrics=['accuracy'])
    #
    model.fit(x_train,y_train,batch_size=100,epochs=20)
    # save and load model
    # 模型评价
    model.evaluate(x_test,y_test)
    # 模型使用
    score = model.predict
    ~~~

    > x_train 第一个维度标识训练集的第几个,剩下的维度是每个样例x 的维度​​


##Deeplearning tips 

### underfitting (训练不完全)

***

####Train set 的结果并不好

- 梯度消失

  当activation 为sigmoid，网络层数较多

- activation 选择

  当使用sigmoid 



###激活函数

#### RELU

> 非常常用的激活函数

relu 其实就是拿掉一些节点下的线性激活函数，

#### MaxOut

线性变换后组合成多组，每组取组内最大值，relu 可以使maxout的一个特殊情况

MaxOut 的Training：



> 不太明白underfitting 的原因到底是层数的问题，还是activation的关系

首先看到训练的到的test 上accuracy 比较低

去查看在trainset 上性能怎么样， 起码需要在trainset 上有一个很好的性能

loss fuction 对性能影响很大

normalnization  归一化 对结果也很大

### 优化器

####RMsPro

RMSProp: 来自 hinton 的线上课程,在adam 的基础上，可以支持定制化 ，设置$\alpha$

~~~python
RMSProp: epochs 5 number:60000 batch_size= 100 unit=128 loss ='categorical_crossentropy'
Test lose 0.134844 acc 0.968600
Ttrain lose 0.082091 acc 0.978950
Adam (准确率提升速度很快)
Test lose 0.121856 acc 0.967100
Ttrain lose 0.063190 acc 0.982217
SGD (需要多轮训练)
epochs=1 acc = 0.6
epochs=5
Test lose 0.144919 acc 0.960300
Ttrain lose 0.090372 acc 0.971900


~~~

####Momentum

算梯度不只是考虑这个点的grad ，也要考虑之前点的grad ，这样有可能跳出局部最优

#### Adam

Adam : 不用设置学习率

### overfitting

***

Test Set 的结果不好

####解决方案

- Early Stop：在test set 错误率上升了就停止epochs 训练,但是test set 没有loss fuction ，所以加上 evalutate set 评价集是有标注数据

- Regularzation 正则化：为了让fuction 更平滑，加入 参数的平方和,

  Weight Decay

  ==TODO==: 到底为什么正则化能让参数变化，其实通过公式推导会发现正则化能让参数更接近0 

- Bagging：使用若干个模型，整合在一起，再蒸馏

- Drop out： 每次更新的时候只选择部分隐层节点更新，可以让training 的效果变差，但是test 的结果变好

  就是以概率p 丢掉神经单元 

Drop out:方式过拟合

当test set 的结果不好的时候使用 

### local minial 陷入局部最优

其实DNN 有很多参数，所有参数都是最小值的情况概率很小，因此

- Momentum : 算梯度不只是考虑这个点的grad ，也要考虑之前点的grad ，这样有可能跳出局部最优

### 调参

1. 当隐层设置的太多，例如mnist 上设置10 层，结果很差，但是设置两层就很好
2. loss fuction 的选择也差距很大，例如 ’mse‘ 在分类上就不行，而’categorical_crossentropy‘ 就好很多
3. optimizer 优化器的选择也有影响，adam 明显比SGD 正确率上升速度大
4. activation 的影响也很大，例如在用多层的神经网使用sigmoid 就不行，而使用'reLU' 就很好


#### 调参

怎么选择好的超参数

1. 取小数据集，在小的数据集下调参数




dropout

###深度学习环境搭建

https://brickyang.github.io/2017/12/18/%E5%88%A9%E7%94%A8-AWS-%E6%90%AD%E5%BB%BA-Tensoflow-Keras-%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E7%8E%AF%E5%A2%83/

### 参考资料

 [课程主页](http://speech.ee.ntu.edu.tw/~tlkagk/courses_ML17_2.html)