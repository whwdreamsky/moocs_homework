# DyNet

***

DYNet 想比tensorflow 是使用dynamic network, 而tensorflow 是使用static flow,区别在于dynet 不是通过建立一个network 通过set 方法来修改input , dynet 是为每个训练样本建立一个netword

> Dynamic networks are very similar to static ones, but instead of creating the network once and then calling “set” in each training example to change the inputs, we just create a new network for each training example.
>
> 并不理解这样做的优势,之后可以了解

大概可以分为几个步骤:

1. 设定模型参数,通过`ParameterCollection`,`add_parameters`,设置具体维度的参数
2. 利用模型参数,设定模型(方程),通过dy 提供的`Expression`设置模型方程,可以利用自带的logistic函数
3. 设置损失函数loss
4. 声明训练器trainer, 我这里用到的是`SimpleSGDTrainer`,通过不断迭代,调用loss.backward() 反向传播,通过`train.update()`跟新权值


> 自己写了一个XOR的计算图

