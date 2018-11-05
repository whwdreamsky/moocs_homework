# 贝叶斯 学习

###贝叶斯基础知识

贝叶斯公式: 往往我们是给定观察属性B，要求求解类别 A 的概率，往往P(B) 可以通过全概率公式+归一化(如下)解决，不需要计算，下面就是估计P(B|A)和P(A)
$$
P(A|B)=\frac{P(B|A) \cdot P(A)}{P(B)}
$$
推广：这里e 说是一种条件，不太明白 ? 
$$
P(A|B,e)=\frac{P(B|A,e) \cdot P(A|e)}{P(B|e)}
$$
全概率推理，以下是全概率公式
$$
P(A)=\sum_e P(A|e)\cdot P(e)
$$
这里我们给个例子，cavity “牙洞”，代表类别属性，toothache“牙痛” 代表观测属性，当我们要计算 P(cavity|toothache) 也即是当症状牙痛，结果是牙洞的概率，有如下推导。
$$
P(cavity|toothache) =\frac{P(cavity ,toothache)}{P(toothcache)}\\\
P(cavity|toothache) =\frac{P(cavity ,toothache)}{P(toothcache)}
$$
可以不知道 P(toothache)  的概率，直接使用两个分量的比值，再归一化就可以计算得到， 这样我们就把==条件概率转化为联合概率==

边缘化

条件化

###朴素贝叶斯(Naive Bayes)

**条件独立性假设**：$P(B_1 , B_2 ,\dots, B_n |A_j ) = P(B_1|A_ j ) P(B_2 |A_ j ) \cdots P(B_n|A_ j )$,从这个公式也可以看到，贝叶斯假设很天真的认为属性之间是独立的。

> 这也就是把复杂的问题简单化了

**估计 $P(A_i|C_j)$**：

- 对于离散变量，我们使用极大似然估计，$P(A_i|C_j)  = \frac{|A_{ij}|}{C_j}$, Aij 代表 观察属性为Ai 且类别为Cj 的实例数目

- 对连续变量：

  - 离散化，具体意思是分割完整的区间成一个个小区间
  - 二划分
  - 概率密度估计：常见满足正太分布，考虑每个点(Ai,Cj) 求出 $\sigma,\mu$

- **数据平滑化**：对概率为0的P(A|C) ：
  $$
  P(A|C) = \frac{N+1}{N_c+c}
  $$




###贝叶斯网络

不确定下进行推理的网络模型,对比之前的朴素贝叶斯中属性独立的假设，这里拓宽了这一限制，考虑属性之间的关系，这样属性构成了有向无环图

贝叶斯网构建

1. 每个节点代表一个变量，边是有向边，由父节点指向孩子节点，例如 这里 Exercise 是 Heart Disease 的父节点，有个节点有个条件概率表(CPT),每个表描述了 父节点对当前的节点的影响，例如 Heart Disease 节点受两个父节点影响，表中也就是 P(HD|E,D) 的概率分布

![](/Users/oliver/Desktop/AI_photo/bayes.png)

2. 贝叶斯网络的语义

   表示了完全联合概率分布，也就是贝叶斯网络构建出来了之后，能计算任何联合概率的值

### 贝叶斯精确计算

这个公式是利用完全概率公式，将联合分布化为一个个条件概率的乘积
$$
P(x_i\dots,x_n)=\Pi P(x_i|parent(x_i))\\\
P(X|e) = \alpha P(X,e) =\alpha \sum_yP(X,e,y)
$$

通过观察值 结合 贝叶斯网进行精确推理, 下面一个公式很重要，是把条件概率转化为联合概率的推理，其中X 表示变量，e 表示证据(也就是观测属性),y 是未观测变量，之后的通过枚举的方法就是按照这个公式推导

举例按照书上例子,下面是一个贝叶斯网络的例子，要求计算特定的条件概率

![](/Users/oliver/Desktop/AI_photo/bayes.2.png)

![](/Users/oliver/Desktop/AI_photo/bayes.3.png)

变量消元

下面是上面计算的具体过程，具体过程用树的形式表示出来，重点是在这个计算过程中有 j m 的重复路径，每个圈代表一次计算，因此，我们要优化这个过程。

![](/Users/oliver/Desktop/AI_photo/bayes.4.png)



###贝叶斯网的近似推理



吉布斯采样算法



实践https://www.kancloud.cn/digest/machine-learning-dm/125820

