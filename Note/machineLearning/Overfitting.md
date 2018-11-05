# Overfitting

过拟合:当参数过多或者对特征使用过多的高阶多项式表示时,学习算法会尽可能符合训练数据,但是过度的学习不广泛的特征,导致模型的泛化能力变差.

> Logestic 多项式如何选择?

![3-3](/Users/oliver/Desktop/AI_photo//photo/3-3.png)

解决方法:

###减少变量的数目

1. 模型选择算法,即只选择影响结果的因素

###正则化(Regularization) 

思想:希望在损失函数加入额外项限制参数，可以看做对损失函数的惩罚

1. 保留所有参数,但是减小模型参数$\theta$的值

   修改损失函数:
   $$
   \displaystyle J(\theta) = \frac{1}{2m}\left[\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})^2 + \lambda\sum_{j=1}^n\theta_j^2\right]
   $$
   $\lambda$的值选择问题,如果选择的过大,会导致欠拟合,因为当很大时 参数$\theta_j$的值趋近于0,这样导致目标函数$h_\theta(x)=\theta_0$

   > 这里添加一个参数的平方相加项,相当于加了一个方差,这里通常不加入$\theta_0$因为通常他的值很大,而且对结果的影响很小
   >
   > 但是为什么是平方呢? 答 使用的L2 正则化项

   ==L1 vs  L2 正则化==

   - L1正则化是指权值向量 $w$ 中各个元素的**绝对值之和**，通常表示为$||w||_1$

     特点：就是加入了权值的一个求和，会让很多权值等于0，进而形成**稀疏矩阵**，便于进行特征选择

   - L2正则化是指权值向量 $w$ 中各个元素的**平方和然后再求平方根**

     特点：主要用于防止过拟合，让权值趋近于0，但不是等于0, 可以看到👇 线性回归的参数更新的式子，每次 $\theta$ 都乘上一个$1 - \alpha\frac{\lambda}{m}$ ，也就是一个小于1 的因子，因此会使$\theta$ 的值不断变小

   总结: L1 和 L2 是两种正则化的手段，都是损失函数上加入惩罚项对参数进行约束，不同的地方是L1是去参数绝对值之和，优化过程会是很多权值等于0，进而形成稀疏矩阵，便于特征选择，而L2 是加入平方项，在进行梯度下降时，参数会乘上一个小于1 的值，进而减小参数的值

   ​

2. 线性回归正则化

   a. 根据之前损失函数,结合之前梯度下降的公式,得到正则化梯度下降公式如下
   $$
   \begin{align*} & \text{Repeat}\ \lbrace \newline & \ \ \ \ \theta_0 := \theta_0 - \alpha\ \frac{1}{m}\ \sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})x_0^{(i)} \newline & \ \ \ \ \theta_j := \theta_j - \alpha\ \left[ \left( \frac{1}{m}\ \sum_{i=1}^m (h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)} \right) + \frac{\lambda}{m}\theta_j \right] &\ \ \ \ \ \ \ \ \ \ j \in \lbrace 1,2...n\rbrace\newline & \rbrace \end{align*}\\\\
   \theta_j := \theta_j(1 - \alpha\frac{\lambda}{m}) - \alpha\frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}
   $$

   > 这里$\theta_j$变成一次是因为对theta 求了偏导,从而获得关于theta 的等式

   b. 从等式的角度获得参数方程
   $$
   X\cdot \theta+ \lambda \cdot L\cdot\theta= Y\\X^T \cdot X\cdot\theta = X^T\cdot Y
   $$

   $$
   \begin{align*}& \theta = \left( X^TX + \lambda \cdot L \right)^{-1} X^Ty \newline& \text{where}\ \ L = \begin{bmatrix} 0 & & & & \newline & 1 & & & \newline & & 1 & & \newline & & & \ddots & \newline & & & & 1 \newline\end{bmatrix}\end{align*}
   $$

   > 这里有疑问,按照之前的定义应该是我写的等式,我明白等式左侧同乘一个$X^T$的目的是,为了让theta 脱离出来,同时构造一个可以求逆矩阵的n\*n矩阵,这里给的解释是 *X 为m\*n的矩阵,且m<n 此时逆不存在,加上一个$\lambda\cdot L$,这样逆就存在*,这里不理解

3. 逻辑斯地回归正则化

   跟之前线性回归相似,在损失函数中加入参数平方项

   cost fuction
   $$
   J(\theta) = - \frac{1}{m} \sum_{i=1}^m \large[ y^{(i)}\ \log (h_\theta (x^{(i)})) + (1 - y^{(i)})\ \log (1 - h_\theta(x^{(i)}))\large] + \frac{\lambda}{2m}\sum_{j=1}^n \theta_j^2
   $$
   gradient
   $$
   \theta_j := \theta_j(1 - \alpha\frac{\lambda}{m}) - \alpha\frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}
   $$
   > 注意这里正则化有要求,不要加入$\theta_0$,这个问题在ex3的作业中出错了,公式里也强调了$\theta$ 是从0开始的

   ex2 作业

   ```matlab
   %注意这里对theta0不进行正则化,因此让theat0 置0
   regression_theta = theta;
   regression_theta(1,1)=0;
   grad = (1/m)*X'*(sigmoid(X*theta)-y)+regression_theta*lambda/m;
   J = (-1/m)*(y'*log(sigmoid(X*theta))+(1-y)'*log(1-sigmoid(X*theta)))+(lambda/(2*m))*(regression_theta'*regression_theta);
   ```

   ​

   ​

   不使用regularied 即 lamda = 0, accuracy = 86.440678

   ![3-3](/Users/oliver/Desktop/AI_photo//photo/3-5.png)

      使用regularied:明显平滑一些,能更好的适应估计 accuracy = 83.1 lamda = 1

![3-3](/Users/oliver/Desktop/AI_photo//photo/3-4.png)

​	 使用regularied : 设置大的lamda =100,不能很好的拟合原数据

​	![3-6](/Users/oliver/Desktop/AI_photo//photo/3-6.png)








