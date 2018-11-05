# Convolutional Neural Networks for NLP

> 学习blog http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/

### CNN vs DNN

CNN 减少参数,不使用全连接的神经网

CNN 的思想，在图像识别中，图中只有一小部分的内容就可以作为特征，而不需要看这个图

CNN 图像识别特性：

1. 有些特征不需要整个图片，而是图片的一小部分
2. 相同的模式(pattern)出现在不同的区域，例如识别不同的鸟，鸟嘴可以出现在图中任何位置
3. 子采样不会对对象造成影响，例如 100\*100像素的图，去掉奇数列，行，变成50\*50

> 这个CNN的属性特征主要是以图像识别为例子，当然也可以扩展到很多其他领域



![](/Users/oliver/Desktop/AI_photo/machinelearning_hongyi/CNNvsDNN.png)

上图是把CNN 卷积的过程转化为神经网络的的方式，右边是把左边6*6 的图像拉平变成36个输入的神经网络，而卷积之后的矩阵就是**隐层**的节点，每个隐层节点连接9个输入，同时保证9个输入按次序的权值相等

> 关于这个权值相等，可以看到图上很清晰画出按顺序连接边的颜色相同，其实这几个连接边也就是filter 里的值，因此CNN 也可以转化为DNN

> CNN 其实比DNN 还要简单,只是相较全连接减少了一些连接的边,同时共享参数

### CNN 架构

Convolution —> Max Pooling —> Convolution —>Max Pooling —>flatten

将输入数据的卷积convolution作为输入, 经过一个ReLU,再到 pooling (subsampling) layers

![Convolutional Neural Network (Clarifai)](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/11/Screen-Shot-2015-11-07-at-7.26.20-AM-1024x279.png)

> we use convolutions over the input layer to compute the output
>
> 之前不理解通过卷积从低维到高维,是通过选择不同的region size 即过滤器矩阵的维度n,以及矩阵中数值的变化,这样可以获得多个卷积之后的结果
>
> Flatten 是把图像打平



### convolution

convolution: 即卷积，是利用filter 提取特征的过程，源图像的一个和filter的size相同的子矩阵和filter 做点积,得到卷积后的矩阵

联想到图像中的应用,图像是个n\*n 的矩阵,卷积是一种 *降维,采集特征*的方式,方式是通过滑动窗口对原始矩阵进行操作,如下图filter 是一个3\*3的矩阵,也叫作kernel,filter,feather detector 依次图中每个3\*3 的矩阵对应每个元素相乘再相加.

stride ： 步长，也就是filter 移动的距离

padding ： 补0，保证图像大小不变

![img](http://deeplearning.stanford.edu/wiki/images/6/6c/Convolution_schematic.gif)

> 对于n * n 的输入数据,m*m 的过滤器,得到卷积过后的矩阵维数是  n-m+1

#### filter 的理解

CNN 学习到什么？

大家对CNN 为什么可以学习到特征,把训练目标改变，反向传播到输入图片


Deep Neural Networks are Easily Fooled 



#### Pooling Layers

在卷积之后二次取样,也就是之前实现 subsamling 的过程,

![](/Users/oliver/Desktop/AI_photo/photo/pooling.png)

二次取样的原因:

1. 输出特定维数的输出,例如上图输入是不同长度的向量,输出都是2维向量,从而作为softmax输入,例如不同的句子长度卷积的输出矩阵长度很可能不同
2. 有效的降维,同时


$$
\begin{array}{ll}
        out(N_i, C_{out_j})  = bias(C_{out_j})
                       + \sum_{{k}=0}^{C_{in}-1} weight(C_{out_j}, k)  \star input(N_i, k)
        \end{array}
$$

3. 有效防止了过拟合

优势：这个做法对nlp 中边长的任务有好处，因为cnn 要连接一个全连接的神经网，必须是定长的输入

缺陷： 这里丢掉了句子的顺序，一个句子的中词的顺序也对语义有很大作用

==NLP 中maxpooling== 

1. MaxPooling Over Time ：意思是对于某个Filter抽取到若干特征值，只取其中得分最大的那个值作为Pooling层保留值，其它特征值全部抛弃，值最大代表只保留这些特征中最强的，而抛弃其它弱的此类特征。

   > 缺陷：对于反复出现的特征，可能只能表示一次，没有体现反复出现特征的重要性

2. Top- K 的max pooling ： 每个filter 选择最重要的k 个特征，保留顺序，

####激活函数Relu

Relu = max{0,x} 一种常用的激活函数，他有

***

##CNN 应用

### CNN FOR NLP

![Illustration of a Convolutional Neural Network (CNN) architecture for sentence classification. Here we depict three filter region sizes: 2, 3 and 4, each of which has 2 filters. Every filter performs convolution on the sentence matrix and generates (variable-length) feature maps. Then 1-max pooling is performed over each map, i.e., the largest number from each feature map is recorded. Thus a univariate feature vector is generated from all six maps, and these 6 features are concatenated to form a feature vector for the penultimate layer. The final softmax layer then receives this feature vector as input and uses it to classify the sentence; here we assume binary classification and hence depict two possible output states. Source: hang, Y., & Wallace, B. (2015). A Sensitivity Analysis of (and Practitioners’ Guide to) Convolutional Neural Networks for Sentence Classification](http://d3kbpzbmcynnmx.cloudfront.net/wp-content/uploads/2015/11/Screen-Shot-2015-11-06-at-12.05.40-PM-1024x937.png)

1. 滑动窗口的宽度是输入词向量的宽度
2. 卷积层可以通过设置多个不同region size的不同数值的过滤器 获得高维数据,例如上图就设置了维度 (2 ,5). (3,5) (4,5),每个维度下又有两种数值的卷积矩阵

### CNN 图像处理

- 图片修改

  **Deep Dream**

  **Deep style**

- playing GO

- 语音识别

- 语义分析

  filter 只在句子的顺序的上移动，不在word embeeding 

### CNN 使用的背景

1. 模式比整个输入小的多，例如鸟嘴在整张图片，在nlp 中例如文本匹配，可能就是几个词匹配
2. 模式出现在不同的区域
3. 子采样：见之前的图像上的背景