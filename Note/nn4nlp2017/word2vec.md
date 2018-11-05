# NNLM&word2vec

[TOC]

### 1. **A Neural Probabilistic Language Model**

####N-gram 语言模型回顾

回顾,我们的语言模型其实就是要给每个句子分配一个概率$P(w_i\dots w_t)$,N-gram 用前n-1 个词作为context , 第n 个词出现的概率表示$P(w_n|w_0\dots w_{n-1})$,通过极大似然估计这个概率.
$$
P(W)=\prod_{i=1}^{|w|+1} P(w_i |w_0 \dots w_{i-1} )\\
P(w_i|w_1\dots w_{i-1}) \approx P(w_i|w_{i-n+1}\dots w_{i-1}) = \frac{c(w_{i-n+1}\dots w_i)}{c(w_{i-n+1}\dots w_{i-1})}
$$

#### Bengio的神经网络语言模型

##### 模型的目标

目标函数:

![](/Users/oliver/Desktop/AI_photo/photo/w2w1.png)

![](/Users/oliver/Desktop/AI_photo/photo/w2w2.png)

这是计算语言模型的困惑度,困惑度的值越小,句子的概率越大,目标是最小化这个目标函数.

> 这里R 是表示正则化,不代表bias,其中T 代表训练样本的个数
>
> 正则化的概念

#####模型步骤

1. 为在词表中的每一个词分配一个分布式的词特征向量
2. 词序列中出现的词的特征向量表示的词序列的联合概率函数
3. 学习词特征向量和概率函数的参数

![img](http://images.cnitblog.com/blog/590456/201409/012056252978173.png)

分为三个层次

1. 输入层,将one-hot 表示的词向量,映射到m 维度的向量,

   图中最低的是输入 映射层 

   输入是当前词w_t的前n-1个词 (w_t-n+1~w_t-1) 经过C matrix 映射后到了映射层，模型训练的开始C matrix可以随机初始化，*they could be initialized using prior knowledgeof semantic feature*,最终模型反向传播后C matrix也更新。 

   - 词向量concat成矩阵,这里是将N个词特征向量拼接起来
     ![这里写图片描述](http://img.blog.csdn.net/20170830172312697?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d3X2p1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

     ![img](http://opigwu3vh.bkt.clouddn.com/17-8-22/26190754.jpg)

2. 隐含层 和输出层
   如图是简单的有一层隐层的神经网络，这一部分就是通过网络结构表示条件概率函数,目的就是计算出下一个词的概率,这里分为两部分,先是计算y表示词w_t出现的概率,也就是未正则化的log 概率值,在通过softmax 归一化的得到概率值.

   ![](/Users/oliver/Desktop/AI_photo/photo/w2w5.png)

   ![](/Users/oliver/Desktop/AI_photo/photo/w2w6.png)
   $$
   y=q+Wx+Utanh(Wx_w+p)
   $$

   > 论文中写的公式如上,为什么要加上W,原因是什么呢,跳边,防止梯度丢失
   >
   > 加入隐层的目的是什么的?增加神经网络参数,提升模型的学习能力

   参数的含义,W

   > tanh is a rescaled logistic sigmoid function
   >
   > $tanh(x) = \frac {sinhx}{coshx}=\frac{e^x-e^{-x}}{e^x+e^{-x}}$,范围[-1,1]
   >
   > 使用tanh 的原因是什么?

3. softmax 归一化(保证概率之和的概率值为1)
   由输出层归一化指数函数softmax,最终获得在词表的分布列,进而可以计算语言模型的困惑度
   ![](http://img.blog.csdn.net/20170830173708320?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3d3X2p1bg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


#####NNLM 模型效果

dog,cat 的例子,提升语言模型泛化能力,这是n-gram 做不到的

![](/Users/oliver/Desktop/AI_photo/photo/w2w20.png)

#### N-gram vs NNLM

相似性:这里NNLM(from Bengio)又被称为 N-gram NNLM,都是利用前n 个词预测下一个词出现的概率来构建语言模型,有相同的目标结构,输入相同,即$P(w_i|w_0\dots w_{i-1})\approx P(w_i|w_{i-n+1}\dots w_{i-1})$

差异性:

1. NNLM:在学习句子的联合概率的同时,又学习了更好的词的表示,得益于词的维度下降,模型能够提高n的上限,能有更多的训练样本.
2. 在平滑方面,在对每个样本训练的时候N-gram 是通过计数统计,NNLM 中的参数是共享的,每个样本的训练会细微更改参数,这样让概率变化更为平滑.
3. 在应对unknown word 的时候,因为相似词有相似的词表示,进而提升模型的泛化性,eg,当词`apple`未出现在上下文`eat`中出现,而与`apple`相似的`orange`存在`eat orange`的样本,那样`eat apple`的概率也会比较大,而在n-gram `eat apple`的概率为0.


#### 词向量表示

N-gram 采用one-hot,也就是词的离散向量表示.

![](/Users/oliver/Desktop/AI_photo/photo/w2w3.png)

NNLM使用distribute分布式表示

![](/Users/oliver/Desktop/AI_photo/photo/w2w4.png)

分布式解决了两个问题:

1. 维度灾难,对于one-hot,如果我们要对n=10 的语言模型建模时,当词表是$10^5$,那组合数就是$(10^5)^{10}$-1,也就是$10^{50}$的量级,造成的原因是词向量维度太高,使得参数估计更难,而分布式的方式可以利用神经网络享参数,同时降低维度也可以表示词的含义.
2. one-hot 中词与词之间是独立的,无法比较词之间的相似度,而分布式的可以通过训练模型中词相似的上下文,获得相似的向量


### 2.word2vec 

#### word2vec vs NNLM

1. 针对隐层和输出层之间的矩阵向量运算这一计算瓶颈,提升计算能力,通过构造层次的softmax极大的降低了降低计算复杂度
   $$
   Q=N * D+N*D*H+H*V \rightarrow N*D+D*log_2(V)
   $$







####Two novel Model 

![](/Users/oliver/Desktop/AI_photo/photo/w2w7.png)

思想:

cbow:使用 $w_t$ 的前c,后c 个词预测当前词的概率,这里的思想是句子中的词 $w_t$不仅和之前的词相关,也和这个词后边的词相关吗

> 这里可以看做  输入参数是 2c 个，输出是softmax 中 概率最大的词，也就是优化让softmax给出训练集的真实中间字的概率最大

skip-gram: 使用$w_t$ 预测 前后context的词

> 这里看似这个思想很简单,这里的创新点在哪呢?
>
> *We’re going to train a simple neural network with a single hidden layer to perform a certain task, but then we’re not actually going to use that neural network for the task we trained it on! Instead, the goal is actually just to learn the weights of the hidden layer–we’ll see that these weights are actually the “word vectors” that we’re trying to learn.*
>
> 这个是[Chris McCormick](http://mccormickml.com/)的一个博客中的一句话,就是Word2vec 不是通常的通过构造神经网络去训练得到词向量,而是仅仅通过学习神经网络中的参数,就能表示词向量

#####Continuous Bag-of-Words Model

模型整体的目标函数如下,对每一个样本的训练目标函数是最大化 损失函数   E = - log(p(word|context))
$$
L = -\frac{1}{T}\sum log(p(word|context))
$$

![](/Users/oliver/Desktop/AI_photo/photo/w2w9.png)

模型分为 三个层次,输入层,隐层,输出层, x 为 |V|*1 的one-hot 向量, 有C 个上下文词的个数,在隐层H 相当于取了c个词的特征向量加合取平均.后面输出层计算log概率再softmax 归一.

![](/Users/oliver/Desktop/AI_photo/photo/w2w10.png)
$$
y_i = W'^T*h\\\
p(x_t|x_1\dots x_c) = \frac{e^{y_t}}{\sum_{i=1}^V e^{y_i}}
$$

> 这里W 还是W' 是词向量矩阵呢?
>
> 其实这里W 叫输入词向量矩阵,W' 叫输出词向量矩阵,都可以表示词向量,并且同一词的输出词向量和输入词向量是一致的,是对称关系

#####skip-gram Model

![](/Users/oliver/Desktop/AI_photo/photo/w2w11.png)

skim-gram 单次损失函数,不严谨的写法为 $E = -log(P(context|word))$,与cbow 不同目标不再是升高预测词输出分布列中$p(w_t)$的概率,而是提升$P(w_1,w_2\dots w_c|w_t)$(c个上下问,$w_t$是给出的中间词)
$$
E=-logP(w_1,w_2,\dots w_c|w_t)\\
P(w_1,w_2\dots w_c |w_t) = \prod _{j=1}^{c}\frac{ e^{y_j}}{\sum_{i=1} ^V e^{y_i}}
$$

#### Hierarchical Softmax

**局限**:之前的模型构造了更好的词表示,同时通过消除隐层让模型变得更简单,消除了N\*D\*H 的计算复杂度,计算复杂度最大的一项来自最后的隐层->输出层,也就是刻画P(word|context) 的部分, H*V, 这时word2vec 提供了层次softmax 的方式 重新刻画 P(word|context).

**层次softmax**: 根据词频构建Huffman 树, 词作为树的叶子节点,有唯一的路径从根节点到某个叶子节点,这样也就表示了每个词,每个非叶子节点相当于是一个二分类,因此从根节点到叶子节点的每个二分类的概率之积表示了这个词的概率.这样实现了将 复杂度有 H\*V -> $H*log_2V$

![](/Users/oliver/Desktop/AI_photo/photo/w2w12.png)

![](/Users/oliver/Desktop/AI_photo/photo/w2w13.png)

叶子节点的概率公式如下:

![](/Users/oliver/Desktop/AI_photo/photo/w2w14.png)

![](/Users/oliver/Desktop/AI_photo/photo/w2w17.png)

![](/Users/oliver/Desktop/AI_photo/photo/w2w18.png)

> $n(w_2,p)$ 表示从根节点到w2叶子节点的第p个节点,对于计算从节点j-> j+1,[] 中判定下一个节点是不是左结点是就为1,不是就是-1,代表就是选左结点概率为P,右结点为1-P

##### 哈夫曼树的构建

哈夫曼树的构建是从底向上构建，每次选择两个权值最大的节点合并为一个节点，从集合中除去选择过的两个节点，把新合成的节点加入树中，这样构建哈夫曼树

实际例子:

![](/Users/oliver/Desktop/AI_photo/photo/w2w15.png)

#### 负采样

1. 什么叫负采样？

   对于CBOW，而言相对真实的中心词$(context(w_0),w_0)$，按照词频分布，取 neg 个 不是w 的词，构成$(context(w_0),w_i)$,根据

2. 通过负采样进行模型训练

   一个正例和多个负例，构造逻辑斯蒂回归分类器，通过SGD，获得每个词的向量表示

   每次优化的就是对数似然，让 P(1-P) 的概率最大

3.  优势：层次softmax ，遇到生僻词，可能要下深入到很深的树深，其次要维护比较大的哈夫曼树

> 这个负采样，可以告诉这个什么样的上下文和这个词相关，什么词和上下文无关

负采样中的二元逻辑回归：求解参数



###3. word2vec 可视化

可视化中orange 和apple 的例子,这是论文*word2vec Parameter Learning Explained* 提供的可视化

![](/Users/oliver/Desktop/AI_photo/photo/w2w16.png)

做报告的板书,也是note

![](/Users/oliver/Desktop/AI_photo/photo/w2w_note.jpg)



### word2vec VS Glove

glove :又叫全局的词向量表示， glove 基于计数(count based) 的词向量模型，首先得到每一行表示一个单词，每一列表示context(也就是词的组合) ，构造出 单词*上下文大小的矩阵，因为矩阵特别大，我们需要对其进行PCA 降维

> glove 的思想和skip-gram 有点像，是使用词上下文来表示这个词



###参考资料

> 中文翻译版本http://www.cnblogs.com/Dream-Fish/p/3950024.html
>
> 代码注释http://blog.csdn.net/EnochX/article/details/52852271
>
> word2vec 中的数学原理详解http://www.cnblogs.com/peghoty/p/3857839.html
>
> explain ppt https://docs.google.com/presentation/d/1yQWN1CDWLzxGeIAvnGgDsIJr5xmy4dB0VmHFKkLiibo/edit#slide=id.ge77999220_0_0
>
>  [Efficient Estimation of Word Representations in Vector Space](http://arxiv.org/pdf/1301.3781.pdf)
>
> [Distributed Representations of Words and Phrases and their Compositionality](http://arxiv.org/pdf/1310.4546.pdf)
>
>  [Linguistic Regularities in Continuous Space Word Representations](http://research.microsoft.com/pubs/189726/rvecs.pdf)
>
> [word2vec Parameter Learning Explained](https://arxiv.org/abs/1411.2738)
>
> [Chris McCormick skip-gram](http://mccormickml.com/2016/04/19/word2vec-tutorial-the-skip-gram-model/)
>
> [word2vec 可视化](https://ronxin.github.io/wevi/)

