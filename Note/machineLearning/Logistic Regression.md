# Logistic Regression

> machine learning course from Andrew Ng week3

##Classification and Representation ##

####什么是分类任务 

使用 regression 做分类任务

#####怎么解分类任务？

function —> loss fuction —> find the best fuction 

- 第一步的fuction 貌似是抽象的，就是假设对g(x) （ 这里g(x) 是个用于拟合的函数，可以是SVM ，Logestic 等），若g(x) > 0 为类别1 否则为类别2 
- loss fuction ： 是优化目标，这里提到可以是f(x) != y 的数目，即预测错误数
- find best fuciton  这一步是可以选择多个分类器，选择最优分类器 

> 这里其实不太懂



高斯分布(Gaussian Distribution)
分布是什么意思？有什么用？

假设属性值满足高斯分布，找到合适的$\mu,\sigma $,得到分布，以预测位置样本的类别

如何 获得：

***

使用liner regression 应用在分类任务的问题

- 例如在二分类问题中,y=0 or 1 , 对于训练好的模型h(x), 尽管所有训练数据中yi是0或1, 可能h 的值 大于1或小与0,这样的性质对分类问题是效果很不稳定
- 分类本身不能用线性方程表示,*(可能简单理解是到某个值区间就是一个类别,应该收敛,而不是持续的增加)* 仅仅是个 ==回归问题==


###hypothesis representation###

 假设我们的预测值是离散的例如二分类中的0,1,希望h(x)的输出值[0,1]构造得到sigmoid function

$$
h_\theta (x)=\frac {1}{1+e^{-\theta ^T x}}
$$

> $$h_\theta (x) $$ 表示y  =1 在x 上的概率,与线性回归类似,也是有多个参数决定了y的估计值,因此应用$$h_\theta = g(\theta ^Tx)$$

![img](https://raw.githubusercontent.com/whwdreamsky/moocs_homework/master/photo/1.png)

#####为什么叫logestic Regression

来源于逻辑斯蒂分布

==补充==

> 来自李航

逻辑斯蒂回归，是Y =1 下 对数几率 (P/(1-P)) 的线性回归


$$

$$





Decision Boundary

即分类的判断条件,

> 例如在二分类h(x) 值在什么范围代表类别1 or 0,我们定义h(x)大于等于0.5为1,小于0.5为0,通过计算得到$$\theta ^Tx\geq 0  $$为1,当取一元方程,x0,x1 构造的空间就有一条直线代表分类边界eg:x1+x2>0

Non-linear deciosion boundaries

$$\theta ^TX$$可能是非线性方程(eg $$z = \theta _0+\theta_1x_1^2+\theta_2x_2^2$$),边界不是条直线,而是个椭圆或者其他图形

### Decision Boundary

根据之前的定义当函数h(x) 的值>= 0.5 得到 最终预测结果 为1 ,如果我们想找到一个分界,分开0,1,我们根据如下等式,对于一次线性的特征,例如在二维下,即可得到 例如 $\theta _0x_0+\theta _1x_1+\theta _2x_2 =0$就是一条直线
$$
h_\theta(x)=\frac{1}{1+e^{-\theta ^Tx}} =0.5\rightarrow e^{-\theta^Tx}=1\rightarrow \theta^Tx=0
$$




###cost fuction###

损失函数的定义,如何估计参数$$\theta$$的值,联想到线性方程的损失函数,若类似使用如下作为损失函数,logistic fuction 的输出不是个凸函数,有多个局部最优点,而我们希望损失函数是个凸函数,因此不能选择欧式距离做为损失函数

![img](http://upload-images.jianshu.io/upload_images/2884841-47c82f902ac701c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

linear regression
$$
J(\theta )=\frac {1}{m} \sum_{i=1}^{m}\frac {1}{2}(h\theta(x^i)-y)^2
$$
对于二分类的logistic regression
$$
Cost(h_\theta (x),y)=-log(h_\theta(x))\\

Cost(h_\theta (x),y)=-log(1-h_\theta (x))
$$

> 这里第一个是 y =1 的损失函数，因为y =1 是 cost(1) = 0 , cost(0) 无穷大
>
> 第二个式子是 y =0 的损失函数

![img](https://raw.githubusercontent.com/whwdreamsky/moocs_homework/master/photo/3-1.png)

*TODO:这里修改为两个并列的图一个是当y=1,一个是当y=0*

简化形式,讲连个方程变为一个方程
$$
J(\theta )=- \frac {1}{m}\sum _{i=1}^m(y \cdot log h_\theta (x)+(1-y)log(1-h_\theta (x)))\\\
grad = \frac {\partial}{\partial\theta}J(\theta)=\frac {1}{m}\sum((h_\theta(x)-y)x)
$$
> 可以练习推到一下grad , 其次grad 是一个矩阵,表示每个$\theta$的梯度

为了最小话损失函数,要对$$\theta$$ 求偏导,获得梯度下降估计函数

![img](http://upload-images.jianshu.io/upload_images/2884841-38796d26d49a5e0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

获得向量的求解方程

### 多分类问题



###高级优化

直接使用随机梯度下降函数	`fminunc`, 无需自己设置学习率,更有效的收敛,如下代码中可以对fminunc 梯度下降过程进行参数设置,GradObj 代表我们的costfuction 会返回损失和梯度,maxiter 代表最大迭代次数,其中@(t) 表示对函数costfuction 的标记,也就是给costfuction 起个别名(short-hand)

> Find minimum of *unconstrained* multivariable function

```matlab
%  Set options for fminunc
options = optimset('GradObj', 'on', 'MaxIter', 400);

%  Run fminunc to obtain the optimal theta
%  This function will return theta and the cost 
[theta, cost] = ...
	fminunc(@(t)(costFunction(t, X, y)), initial_theta, options);
%
```

> 这里的t 是costFunction 的句柄 *the handle of the objective function*

### cross-entropy





### 逻辑斯蒂回归多分类






可以参考下人家的笔记哈

http://www.jianshu.com/p/f374de37efc3?utm_campaign=maleskine&utm_content=note&utm_medium=pc_all_hots&utm_source=recommendation

