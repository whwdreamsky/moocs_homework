#  Decision Tree

> motivation : 一方面好奇这个看似简单的基于规则的机器学习算法的神奇之处，一方面是Semval 13 baseline 甚至state of art 方法采用的，还有机器学习 need to learn 

### 历史

重要人物：**Quinlan（罗斯.昆兰）**

**Hunt**： 

**ID3：**

**C4.5：**

CART: 引入回归树，应对输出Y是连续变量的回归问题，同时使用Gini 指数作为分类树的划分原则

[对各种决策树描述](http://blog.csdn.net/lc013/article/details/55048641)

***

## 决策树学习（西瓜书）

### 决策树基本思想

ID3 算法：

决策树有一个根节点，一系列的非叶子节点，叶子节点构成，其实决策树本质是利用非叶子节点中对属性不断测试，不断选择属性再根据属性值对数据进行划分，从数据中学习到一系列的规则最为分类的依据，决策树的生成是一个==递归==的过程，下图是对决策树基本的算法的总结：

==TODO python 实现==

~~~python
#以下是决策树基本流程的伪代码
#input type:
#数据集:D((x1,y1),(x2,y2).....)
#A 属性列表A = [a1,a2,....]
Tree tree;
Node node;
def TreeGenerate(D,A):
    produceNode()
    tempset = set(D[1,:])
    if len(tempset)==1:
        setNode(tree,tempset[0])
        return
    #isSameValue 判断D中所有条目A属性值都相同
    if len(A)==0||isSameValue(D,A)==True:
        y = D[:,1]
        '''
        ydict = {}
        for item in set(y):
            ydict[item] = y.count(item)
        ''' 
        setNode(tree,findmaxFreq(y))
        return
    a = findOptimizeAtr(A)
    for a in A:
        dv = D[D[:,0].has(a)]
        if len(dv)==0:
            setNode(tree,findmaxFreq(D[:,]))
            return
        else:
            TreeGenerate(dv,A.remove(a))
~~~



### 最优属性选择问题

问题：findOptimizeAtr(A) 要选择最优划分属性

- 信息增益的方法

  - 信息熵的概念:信息熵就是平均而言一个事件发生得到的信息量大小，也就是信息量的期望值

    $$
    \begin{eqnarray*}
    Ent(D) = -\sum_{k = 1}^{|y|}p_klog_2p_k\tag{1.1} \\
    Gain(D,a)=Ent(D)-\sum_{v=1}^{V}\frac{D_v}{D}Ent(D_v) \tag{1.2} \end{eqnarray*}
    $$

    > Ent ： 这里y 代表的是分类的类别数，Ent(D) 我理解是每个节点的信息熵，信息的含义就是分类为y个类别的纯度，类别越集中熵值越小
    >
    > Gain ： V 代表的是属性a 的值的个数，也就是当前父节点的分支个数，这里也就是算出每个子节点的信息熵再加权取均值，再和父节点的信息熵做差值，这就叫信息增益，ID3 算法也就是优先选择信息增益大的属性

  - ID3: 从根节点，计算每个特征下的信息增益，优先选择增益大的属性作为节点特征，并为特征的不同值建立子节点。

  - 缺陷：对属性值多个的有偏好

- 增益率（C4.5）：特征a对数据集D的信息增益 比 训练集D关于特征a的信息熵 值的熵IV(a)，ps Dv 也就是根据某个属性值a的一个划分
  $$
  GainRadio(D,a) = \frac{Gain(D,a)}{IV(a)}\\\
  IV(a ) = -\sum\frac{|D_v|}{|D|}log_2\frac{|D_v|}{|D|}
  $$

  ​

  > 其实增益率也就是克服之前信息增益对属性值多的偏好问题，除了一个IV，这个值当属性值种类越多IV 的值也越大，但是这有带来了对属性值种类数较少的偏好，在C4.5中是用`启发`的方式，先选择信息增益大于平均水平的的，再从中选增益率大的

- 基尼指数（CART）

  假设有K 类，样本点属于k 的概率为Pk，则概率分布的基尼指数定义如下，这里pk 由极大似然统计得到，我们可以看到，Gini 指数描述的是数据的不确定性，Gini 越大表示数据越不确定，当p=1/2 时取最大值
  $$
  Gini = \sum_kp_k(1-p_k) = 1-\sum p_k^2
  $$
  跟之前熵类似，在做属性值选择时，根据比例计算使用该属性以及各个属性值划分之后的基尼指数,假设在D属性下，属性值为D1，D2
  $$
  Gini(D,A)  = \frac{D_1}{D}Gini(D_1)+\frac{D_2}{D}Gini(D_2)
  $$
  ​

  ​

#### 回归树

回归树：之前输入变量（X，Y），Y 往往是要预测的某个类别，而有时需要Y 是个连续的值，以用于回归任务中

预测误差：均方误差 ，把决策树构造出的函数设为 f(x) ，那误差就是 $\sum (y_i-f(x_i))^2$）,这样每个叶子节点的最优值是输入实例的均值

寻找最佳属性和划分点：选择第j 个属性和它取的值s，作为切分变量和切分属性，这样就把空间 划分为 x<=s . x>s ,这样我们加合两部分的损失就是我们的选择属性的损失，通过选择 不同的 j，s 获得最小的损失
$$
loss = min_{j,s}[\min_{c1}\sum_{x<s}(y_i-c_1)^2+\min_{c2}\sum_{x>=s}(y_i-c_2)^2]
$$

> 这里c1 就是所有预测y 的均值，$c_1=ave(y_i|x<s)$,这里是为了计算均方误差



### 属性值划分问题

- 连续值划分

  二分法：找到属性所有取值中信息增益最大的作为划分点，这里属性划分后还可作为后代节点的划分属性

  计算每个值得基尼指数或者熵选择最优的

  > 这里有疑问，如果这样属性就可以被多次划分，理论上可以不断进行划分，知道每个叶子节点只有一条数据，所以在我们就很需要剪枝，但是在后剪枝中应该会因为这个问题，损耗过多的时间和空间
  >
  > ==TODO==

- 缺失值划分

  要解决的问题：

  1. 属性选择时熵计算

  2. 如何对缺失的条目进行划分

     利用已知条目的划分来估计缺失条目划分，好处是不需要t填充缺失值

  ​

  1. 最常值
  2. ​

- 多变量属性值

  特定：非叶子节点不再是对某个特定属性进行判断，而是属性的线性组合

### 过拟合问题优化

关键问题： 什么时候停止划分

在最基础的hunt 中，在如下情况下停止：

1. 输入的数据集D 的类别相同
2. 在属性a的条件b 下 数据集值为空

####剪枝

- 预剪枝

  即在生成决策树的时候就决定是否剪枝

- 后剪枝

  即先生成决策树，再自底向上判断节点剪枝前后对决策树精度的影响。这里注意根据奥卡姆剃刀原则，尽管对精度不产生影响也要剪枝


剪枝的目标函数: 假设叶子节点总数为T，t 为一个叶子节点$N_t$
$$
C_\alpha (T)=\sum_{t=1}^{|T|}N_tH_t(T)+\alpha|T|
$$


### 随机森林 Random Forest

#### 概述

随机森林：随机森林由许多的决策树组成，因为这些决策树的形成采用了**随机**的方法，所以叫做随机森林，每个决策树的构建没有关系，当进入对测试集进行测试，就是让每一个决策树进行预测，得到多个结果，选择类别最多的

随机森林的诞生是因为决策树容易过拟合

####随机采样

- Bagging: 

  1. 使用Bootstrap 自助法，也就是抽取一定数量样本有放回，生成n个样本子集，
  2. 拿每个样本子集独立地训练分类器这样得到m个分类器
  3. 预测的时候通过投票法，平均法 对样本进行预测

  > 这里提到从偏差和方差的方面，这种方法降低了方差

- random forest： 随机森林其实就是在bagging 的基础上进一步加入了，在决策树训练时属性选择的随机性，和普通的直接选择全部属性的最优属性不同，随机森林在随机生成的属性子集中选择最优属性

  > 觉得加入这样的随机策略，目的是之前的思路是让每个分类器有多样性

###sklearn 实战



### 参看资料

http://www.cnblogs.com/pinard/p/6050306.html