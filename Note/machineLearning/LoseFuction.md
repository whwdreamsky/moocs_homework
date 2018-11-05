# 损失函数

> 这里简单总结一下常用的损失函数

### 熵(entropy)

熵的定义来源于信息论，是用作描述信息量，熵值越大表示信息量越大，需要消耗记录的长度(Bit) 也越长，以下的计算也就是最小编辑距离
$$
H(x) = -\sum_iP(x_i)log_bP(x_i)
$$
**经验熵,经验条件熵**：当熵和条件熵由数据估计得到(特别是极大似然估计)时，熵和条件熵称为经验熵和经验条件熵

### 交叉熵(cross-entropy)

用作衡量真实分布p和非真实分布q 的相似程度，作为目标函数，让非真实分布更趋近与真实分布，香农信息量就是 (log 1/p(i)) 的期望，也就是下面1式，而用q 来编码分布p就是二式，也就是交叉熵

> 这里编码我理解就是描述p 分布的函数

$$
H(p) = \sum_i p(i) \cdot log \frac{1}{p(i)} \\\
H(p,q) = \sum_i p(i) \cdot log \frac{1}{q(i)}
$$

==应用==：经常在神经网络的分类问题作为损失函数，原因是本身我们的目标是让KL 散度越小越好，也就让D(p||q) 越小越好，但是由于p的分布已经给定，也就是训练集，所以优化目标也就是让H

非对称性：即 $H(p,q) \neq H(q,p)$,因为有所谓的参考系，可以从公式上看也不同

#### 极大似然vs交叉熵

1. 在分类问题上，负极大对数似然本质等于交叉熵
2. 任何一个由**负对数似然**组成的损失都是定义在训练集上的经验分布和定义在模型上的概率分布之间的交叉熵



###KL 散度

> 用于计算两个分布p,q 的不同
>
> 也正是机器学习的目标，就是让KL 散度越小越好

按照之前的定义，我们使用q 来编码真实 p 分布多于的编码长度，也就是相对熵，也称为KL 散度, 他表示两个函数的差异越大相对熵就越大
$$
D(p||q)=H(p,q)-H(p) = \sum_i p(i) \cdot log \frac{p(i)}{q(i)}
$$
应用 TF-IDF：也即是看词在本文的词频分布和在全文档中词频分布的差异

> 复习下：TF-IDF，计算的是词在文档中的频率(TF) 和 出现词的文档数/总文档数 的倒数 (逆文档频率) 之积
>
> 这里可以看做 p(i) 是词在本文档中分布，q(i) 是词在全部文档中的分布

### 相对熵

在随机变量X 给定条件下的随机变量Y 的不确定性，定义为H(Y|X)
$$
H(Y|X) = \sum_{i=1}^{n} p_i H(Y|X=x_i) 
$$
信息增益：指得到X 特征下使得类Y 信息不确定性减少的程度
$$
g(D,A) = H(D)-H(D|A)
$$

### 正则化

http://blog.csdn.net/jinping_shi/article/details/52433975