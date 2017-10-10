# Logistic Regression

> machine learning course from Andrew Ng week3

##Classification and Representation ##

什么是分类任务 

使用liner regression 应用在分类任务的问题

- 例如在二分类问题中,y=0 or 1 , 对于训练好的模型h(x), 尽管所有训练数据中yi是0或1, 可能h 的值 大于1或小与0,这样的性质对分类问题是效果很不稳定
- 分类本身不能用线性方程表示,*(可能简单理解是到某个值区间就是一个类别,应该收敛,而不是持续的增加)* 仅仅是个 ==回归问题==

> attempt classification, one method is to use linear regression and map all predictions greater than 0.5 as a 1 and all less than 0.5 as a 0. However, this method doesn't work well because classification is not actually a linear function.
>
> The classification problem is just like the regression problem, except that **the values we now want to predict take on only a small number of discrete values.** For now, we will focus on the **binary classification** **problem** in which y can take on only two values, 0 and 1. (Most of what we say here will also generalize to the multiple-class case.) For instance, if we are trying to build a spam classifier for email, then x(i) may be some features of a piece of email, and y may be 1 if it is a piece of spam mail, and 0 otherwise. Hence, y∈{0,1}. 0 is also called the negative class, and 1 the positive class, and they are sometimes also denoted by the symbols “-” and “+.” Given x(i), the corresponding y(i) is also called the label for the training example.

###hypothesis representation###

 假设我们的预测值是离散的例如二分类中的0,1,希望h(x)的输出值[0,1]构造得到sigmoid function

$$
h_\theta (x)=\frac {1}{1+e^{-\theta ^T x}}
$$

> $$h_\theta (x) $$ 表示y  =1 在x 上的概率,与线性回归类似,也是有多个参数决定了y的估计值,因此应用$$h_\theta = g(\theta ^Tx)$$

![img](https://raw.githubusercontent.com/whwdreamsky/moocs_homework/master/photo/1.png)

Decision Boundary

即分类的判断条件,

> 例如在二分类h(x) 值在什么范围代表类别1 or 0,我们定义h(x)大于等于0.5为1,小于0.5为0,通过计算得到$$\theta ^Tx\geq 0  $$为1,当取一元方程,x0,x1 构造的空间就有一条直线代表分类边界eg:x1+x2>0

Non-linear deciosion boundaries

$$\theta ^TX$$可能是非线性方程(eg $$z = \theta _0+\theta_1x_1^2+\theta_2x_2^2$$),边界不是条直线,而是个椭圆或者其他图形

###cost fuction###

损失函数的定义,如何估计参数$$\theta$$的值,联想到线性方程的损失函数,若类似使用如下作为损失函数,logistic fuction 的输出不是个凸函数,有多个局部最优点,而我们希望损失函数是个凸函数,因此不能选择欧式距离做为损失函数

![img](http://upload-images.jianshu.io/upload_images/2884841-47c82f902ac701c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

linear regression
$$
J(\theta )=\frac {1}{m} \sum{i=1}^{m}\frac {1}{2}(h\theta(x^i)-y)^2
$$
对于二分类的logistic regression
$$
Cost(h_\theta (x),y)=-log(h_\theta(x)\\

Cost(h_\theta (x),y)=-log(1-h_\theta (x))
$$


![img](https://raw.githubusercontent.com/whwdreamsky/moocs_homework/master/photo/3-1.png)

*TODO:这里修改为两个并列的图一个是当y=1,一个是当y=0*

简化形式,讲连个方程变为一个方程
$$
J(\theta )=- \frac {1}{m}\sum _{i=1}^m(y \cdot log h_\theta (x)+(1-y)log(1-h_\theta (x)))
$$
为了最小话损失函数,要对$$\theta$$ 求偏导,获得梯度下降估计函数

![img](http://upload-images.jianshu.io/upload_images/2884841-38796d26d49a5e0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

获得向量的求解方程



###高级优化



fim

> 可以参考下人家的笔记哈
>
> http://www.jianshu.com/p/f374de37efc3?utm_campaign=maleskine&utm_content=note&utm_medium=pc_all_hots&utm_source=recommendation

