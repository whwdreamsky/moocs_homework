

### CNN for NLP

> 在/machineLearning 的目录下有对卷积基本知识的解释
>
> 下面主要关注CNN 在NLP 任务上的应用

http://mp.weixin.qq.com/s/gpEtffuOLiqzrtEWusaX0A

情感分析的例子

BOW -> CBOW -> Deep BOW



- Time-delay networks 相当于 1D convolution 
- CNN
  - Padding
  - Pooling

###CNN For Sentence classification

> Kim 《Convolutional Neural Networks for Sentence Classiﬁcation》

1. 证明了词向量的可用性
2. 证明了词向量作为输入进过简单的CNN 就能达到很好的效果

使用 Conv1D

==pading==

固定长度20个词，词不够的补0 ，进行padding

#### max pooling 的作用

出了减少参数，提取公共特征，还有吗？

1. max pooling 总是选择一个窗口的最大值，也就是选择最重要的特征

2. 应对与处理边长数据

   ​


### CNN for Sentence Pair

#### Arch 1

思想：参考之前CNN 对句子进行建模，先分别对句子1，句子2，进行卷积，maxpooling 操作，获得固定长度的句子的表示，再打平两个向量表示，接一个MLP (全连接神经网络)

缺陷：分来对句子进行表示，不能很好的获得句子之间的特征，直到生成句子表示向量之后才进行语义交互，丧失了语义交互信息。

####Arch 2

思想: 在Arch 1 缺陷的基础上，我们先让句子之间进行交互，假设句子为$S_x,S_y$

1. 取固定的卷积窗口k，这里卷积是1D 的，然后遍历$S_x$, $S_y$ 所有组合形成的二维矩阵进行卷积，每个二维矩阵输出一个值，因此获得了一个二维的矩阵
2. 对二维矩阵进行简单的max-pooling ， size 2 *2
3. 进行二维卷积操作
4. 动态maxpooling : **k-max池化**（ 从filter 中选择top k 大小的特征，并保留顺序）， 而动态 k maxpooling 则是根据 句子的长度提取相应数量的特征
5. 打平，接MLP 全连接神经网

> 重要的参考资料 ：http://www.jeyzhang.com/cnn-apply-on-modelling-sentence.html


### CNN 结果理解

PCA 降维：PCA/t-SNE Embedding  of Feature Vector

Convolutional Matching Model: 这篇是比较关心的CNN 应用在问答对匹配





### 参考资料

[paperWeekly](http://mp.weixin.qq.com/s/gpEtffuOLiqzrtEWusaX0A)

Neubig CNN 课程

卷积与池化在NLP中的应用 from teaEvening 