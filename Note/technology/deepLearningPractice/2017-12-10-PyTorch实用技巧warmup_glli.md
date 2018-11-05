### PyTorch实用技巧warmup

[TOC]

> 下面的所有内容请在`ipython`中执行，默认`ipython`的`sys.path`中包含有安装好的`torch`包。

#### 1. 创建张量

```python
import torch

# 创建形状为[3, 4]的张量，并根据内存中先前使用剩余的内容作为其值
t_d2 = torch.FloatTensor(3, 4) # 在ipython中初步尝试nn模块提供的接口时，根据接口的input定义张量形状

print(t_d2.shape)
print(t_d2.size()) # 二者等价
# 输出
torch.Size([3, 4])
torch.Size([3, 4])

print(t_d2)
# 输出
-6.7273e+21  4.5855e-41  4.3903e-37  0.0000e+00
-6.4562e+20  4.5855e-41 -4.9929e+17  4.5855e-41
-6.5225e+20  4.5855e-41 -6.5226e+20  4.5855e-41
[torch.FloatTensor of size 3x4]
# 可以看出，没有初始化会使得，某些值很大或很小（溢出边缘）例如：e+21，所以对神经网进行初始化是一个好习惯

# 根据randn创建：会创建一个形状为[2, 3, 4]的，初始值呈现“均值为0，协方差为1”的高斯分布的Float类型张量
t_d3 = torch.randn(2, 3, 4)

print(t_d3)
# 输出
(0 ,.,.) =
 -0.1568  0.8220  0.1211  0.1031
  1.6405  0.6280  0.6611 -1.4922
  0.3528  0.7176  0.2340 -0.2679

(1 ,.,.) =
 -1.5914 -1.4284 -0.9995 -0.3267
 -1.1582  0.6701 -0.0795 -0.4648
 -0.4903 -0.2778 -0.0343 -1.1904
[torch.FloatTensor of size 2x3x4]
# 可以看出，torch是通过索引（indexing）与切片（slicing）的形式呈现一个大于二维的张量的，且二维数组的形式可视化的是该张量末两维度的值；例如上述[2, 3, 4]的张量，第0维长度为2，可以索	引出两个元素，每个元素各自是一个3x4的二维数组

# torch中定义的张量类型有：DoubleTensor、FloatTensor、LongTensor、IntTensor、ByteTensor等，张量之间可以方便的进行类型转换
t_d2_float = t_d2.randn(2, 3)
print(t_d2_float)
# 输出（这个输出对于每次试验是不同的，各自对照自己当次试验结果就好）
 0.6793 -1.1584 -0.4410
-0.1093  0.3811  0.5659
[torch.FloatTensor of size 2x3]

t_d2_double = t_d2.double()
t_d2_long = t_d2.long()
t_d2_int = t_d2.int()
t_d2_byte = t_d2.byte()

# 这部分不再贴出打印的结果，请读者自行尝试，并观察类型转换后，张量值与类型的变化
print(t_d2_double)
print(t_d2_double.type())

print(t_d2_long)
print(t_d2_long.type())

print(t_d2_int.type())
print(t_d2_byte.type())
```

可以通过索引的方式访问`torch.Size`类型：

```python
import torch

t_4d = torch.randn(2, 3, 4, 5)
print(t_4d)
# 输出
(0 ,0 ,.,.) =
  0.2084 -0.3735  0.8949 -0.9461  0.3664
  0.3704  1.7382 -1.1168  0.3083  0.6597
  1.3117  0.9615 -0.4789  0.5684 -1.9550
  0.8449 -0.6687  1.2223 -0.9024 -1.7909

(0 ,1 ,.,.) =
  0.8346 -0.6174 -1.7080  2.3141  0.7818
 -0.0894 -1.0520 -1.2819 -0.0391 -0.3994
 -0.9701  1.7124  0.0875 -1.8019 -1.3514
 -0.6972 -1.9935 -0.7833  0.8665  1.0206

(0 ,2 ,.,.) =
 -0.1655  0.5759  1.0542  0.1451 -1.1671
  0.5874 -0.0959  0.5467 -1.5355  0.0455
 -0.1955  1.2217 -1.7846  0.7019 -0.4318
 -0.1670 -0.1454 -1.3186 -1.3960  0.4600

(1 ,0 ,.,.) =
  1.2927  0.5163  1.0580  0.1411  0.2814
  0.0015  1.0082 -0.3804 -1.6301  1.1014
 -1.3178 -0.6732  2.7941  0.4077  0.0913
  2.6667 -0.6396  0.6444 -0.4971 -1.7782

(1 ,1 ,.,.) =
  0.1096 -0.1797 -1.1645  0.9764  1.0895
  0.3449 -0.6491 -0.2139  2.3230  1.3064
 -0.2717 -0.0559 -1.0729  0.3795  1.2107
  1.2643 -0.5325 -3.0741 -1.4562  0.9454

(1 ,2 ,.,.) =
  1.6353 -0.4192  0.3039  2.0171 -0.7752
  1.4946 -0.5339 -0.4419 -2.0552 -0.2278
  1.9852  0.8678 -1.7960  2.0115 -0.8417
 -0.1589  0.4533  0.3299  0.4785 -0.2447
[torch.FloatTensor of size 2x3x4x5]

# 以方位列表的下标索引方式访问t_4d.size()对象
dim0 = t_4d.size(0) # python原生的 int 类型
dim1 = t_4d.size(1)
dim2 = t_4d.size(2)
dim3 = t_4d.size(3)

print(dim0, dim1, dim2, dim3)
# 输出
(2, 3, 4, 5)
```

#### 2. 张量的形变——`view`，`transpose`，`t`，`squeeze/unsqueeze`以及广播机制（Broadcast）

下面，我们要得到一个三维的张量`h`，每一个维度代表的语义是`[batch_size, seq_len, hid_size]`，该语义的典型代表是通过`torch.nn.RNN`、`torch.nn.LSTM`、`torch.nn.GRU`输出的循环神经网**每一时刻**的隐层状态。然后，我们对其进行一些形变操作。

```python
import torch
import torch.nn as nn
from autograd import Variable as Var

import numpy as np

# 首先根据PyTorch的RNN接口，创建一个RNN：每时刻输入向量的长度为6，计算得到的隐层向量长度为4
rnn = nn.RNN(
	input_size=6,
    hidden_size=4
)

batch_size = 2
seq_len = 5
emb_size = 6 # 需要与上面定义的rnn的输入长度一致

# 创建一个语义为[batch_size, seq_len, emb_size]的张量，该张量将作为上述rnn对象的输入，而得到输出h
# input = torch.randn(batch_size, seq_len, emb_size) # [2, 5, 6]
# 为了使后面的计算结果不受随机性影响，我们这里确定的给出输入的张量值，并且初始化上述rnn所有权值为1
input_np = np.arange(2 * 5 * 6).reshape(2, 5, 6).astype('float32')
input = torch.from_numpy(input_np)
for p in rnn.parameters():
    shape = p.size()
    ones = torch.ones(shape)
    p.data = ones
# 作为nn模块输入的变量类型均为Variable类型
input_var = Var(input) # 形状为[2, 5, 6]
h_var, _ = rnn(input_var)
print(h_var)
# 输出
Variable containing:
(0 ,.,.) =
  1  1  1  1
  1  1  1  1
  1  1  1  1
  1  1  1  1
  1  1  1  1

(1 ,.,.) =
  1  1  1  1
  1  1  1  1
  1  1  1  1
  1  1  1  1
  1  1  1  1
[torch.FloatTensor of size 2x5x4]

# 使得h_var的形状变为[2 * 5, 4]
h_var_1 = h_var.contiguous().view(-1, h_var.size(2)) 
# contiguous()是使得h_var中的元素在内存中存放顺序是相邻的，view函数只能针对内存中相邻存储的元素组成的张量进行视图变换，view(-1, h_var.size(2)),-1是希望view函数自行推断出第0维的大小啊，因为view返回的视图只有2维，又指定了第1维，第0维即可根据（2 * 5 * 4 / 4）推断出来

# 使得h_var的形状变为[4, 5, 2]，可以发现该形状是原来形状[2, 4, 5]第0维和第2维转置后得到的
h_var_2 = h_var.transpose(0, 2)

# 是的h_var_1这个2维的张量进行转置得到[4, 2 * 5]的张量
h_var_3 = h_var_1.t() # t()仅仅适用于2维的张量，但transpose()适用于高维的张量

# 现在有一个形状为[2, 4]的张量变量，我们希望将其加到h_var上，使得对于h_var的第1维,所加的元素是一样的
add_np = np.arange(2 * 4).reshape(2, 4).astype('float32')
add_var = Var(torch.from_numpy(add_np)) # add_var.size() ==> [2, 4]
print(add_var)
# 输出
Variable containing:
 0  1  2  3
 4  5  6  7
[torch.FloatTensor of size 2x4]

# 即我们希望将每一个add_var的元素[i, j]，加到对应的那个三维的张量[2, 5, 4]的[i, k, j]元素上
# pytorch实现了broadcast机制，只要相加的张量具有：1). 相同的维度；2). 在不匹配的维度上有一者大小为1
# 两个性质即，可具体请参看：
# http://pytorch.org/docs/master/notes/broadcasting.html?highlight=broadcast
result = add_var + h_var
# 输出（报错）
RuntimeError: inconsistent tensor size, expected r_ [2 x 4], t [2 x 4] and src [2 x 5 x 4] to have the same number of elements, but got 8, 8 and 40 elements respectively at /pytorch/torch/lib/TH/generic/THTensorMath.c:887

# 正确的做法是
result = add_var.unsqueeze(1) + h_var # [2 x 1 x 4] + [2 x 5 x 4] =broadcast=> [2 x 5 x 4] + [2 x 5 x 4]
print(result)
# 输出

Variable containing:
(0 ,.,.) =
  1  2  3  4
  1  2  3  4
  1  2  3  4
  1  2  3  4
  1  2  3  4

(1 ,.,.) =
  5  6  7  8
  5  6  7  8
  5  6  7  8
  5  6  7  8
  5  6  7  8
[torch.FloatTensor of size 2x5x4]
```



