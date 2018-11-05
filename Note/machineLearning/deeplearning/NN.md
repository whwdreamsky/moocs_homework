 	# 	 Neural Networks

> Week4 : from course of machine learning on the cousera 

###1. **背景**

特征过多: 当一个问题特征较多,导致线性回归,逻辑斯地回归的不可用,因为此时的计算量是巨大的,例如在做一个汽车图片的识别器中,对50 X 50 的图片,.要把每个像素点作为特征,对于灰度图片(非RGB),有2500个维度,这样单单选择二次特征,就有$C_n^2​$ 种,接近3百万个,这样计算量是巨大的.神经网络的方法就是为了解决这个困境.![4-1](/Users/oliver/Desktop/AI_photo//photo/4-1.png)

![4-3](/Users/oliver/Desktop/AI_photo//photo/4-3.png)

###2. 神经网络的模型表示

使用神经网络的原理表示假设函数(*hypothesis fuction*),特征$x_1,\dots,x_n$就是神经网络中的树凸,输出就是假设函数的值,其中我们通常要设置一个$x_0$作为偏置单元(*bias unit*),这里我们可以使用sigmoid 作为激活函数(*activation function*),其中之前的参数theta 有时也被称为权重

- 简单的表示形式,$x_0,x_1,x_2$作为输入层,最终输出假设函数,是输出层,中间是  隐层(hidden layer)

$$
\begin{bmatrix}x_1 \newline x_2 \newline x_3 \newline \end{bmatrix}\rightarrow\begin{bmatrix}\ \ \ \newline \end{bmatrix}\rightarrow h_\theta(x)
$$

- 使用$a_i^{(j)}$表示第j 个隐层中的第i个激活单元的激活方程,假设我们设置隐层为三个节点(unit),就有如下的表达形式
  $$
  \begin{bmatrix}x_1 \newline x_2 \newline x_3\end{bmatrix}\rightarrow \begin{bmatrix}{a_1^{(2)}\\\\a_2^{(2)}\\a_3^{(2)} }\end{bmatrix} \rightarrow h_\theta (x)
  $$

  > 想知道隐层节点的数目是如何选取的?
  >
  > 这个过程的的输出是为了得到目标函数的参数吗? 这个过程就相当于之前的有变量xi 输入获取估计的y 值,同样$a_i^{(j)}$也应该是一个值,因为在思考这个g(g(x))的嵌套问题,估计是之后通过梯度下降的方式估计参数矩阵$\Theta^{j}$ 

  每个激活节点(*activation node*)的函数表示如下,其中$\Theta^{j}$层次j 到j+1 的参数矩阵,令Si 表示每层的节点数目,则第j 层的参数矩阵为的维度 为 (S(j-1)+1) * S(j),其中+1 来自偏置,例如第一层的x0

  ​
  $$
  \begin{align*} a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \newline a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \newline a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) \newline h_\Theta(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)}) \newline \end{align*}
  $$

  > 之前理解偏置x0 只是输入层要添加,其实不然如果是多隐层,也要加上a0=1
  >
  > 疑问:这种表示方式是如何对参数降维,猜测是将隐层的数目< 输入层,达到降维的作用

  ![4-2](/Users/oliver/Desktop/AI_photo//photo/4-2.png)

- 向量化表示

  令$z^{(2)}_i = \Theta_{i0}^{(1)}x_0+ \Theta_{i1}^{(1)}x_1+\Theta_{i2}^{(1)}x_2+ \Theta_{i3}^{(1)}x_3$,通过向量计算我们可以得到$z^{(2)}=\Theta^{(1)}x$,因此得到$a^{(2)}=g(z^{(2)})$,进而得到输出$h_\Theta(x)=g(\Theta^{(2)}a^{(2)})$
  $$
  z^{(j+1)}=\Theta^{(j)}a^{(j)}\\ h_\Theta = a^{j+1}=g(z^{(j+1)})
  $$

  > 这里$\Theta ^{j}$ 的维数$S_{j+1}+ (n_{j}+1)$,$S_{j+1}$ 表示这一层的激活节点数目,n 表示上一层的节点数,因为有一个偏置参数,所以+1
  >
  > 神经网络与逻辑斯地回归的区别
  >
  > 1. 神经网络的输出层j -> j+1,就是逻辑斯地回归,只是输入不再是直接输入的值,而是上一个隐层的输出结果,通过隐层,我们可以得到更多的参数,构造更为复杂的非线性假设函数

***

### 3. 神经网络的应用

- 前向计算

使用神经网络模型,表示 y = x1 or x2  , 设置x1 x2 输入的权值,以及偏置x0 ,表示关系,就是前馈神经网的前向计算

![4-4](/Users/oliver/Desktop/AI_photo//photo/4-4.png)

![4-5](/Users/oliver/Desktop/AI_photo//photo/4-5.png)



- 多分类任务

  作业中

  使用逻辑斯地回归​


### 参考资料

https://hit-scir.gitbooks.io/neural-networks-and-deep-learning-zh_cn/content/