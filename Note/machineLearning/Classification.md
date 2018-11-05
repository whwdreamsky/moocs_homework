## Classification

###什么是分类任务

如果使用 regression 做分类任务

未知值问题

##### 怎么解分类任务？

function set—> loss fuction —> find the best fuction 

- 第一步的fuction 貌似是抽象的，就是假设对g(x) （ 这里g(x) 是个用于拟合的函数，可以是SVM ，Logestic 等），若g(x) > 0 为类别1 否则为类别2 
- loss fuction ： 是优化目标，这里提到可以是f(x) != y 的数目，即预测错误数
- find best fuciton  这一步是可以选择多个分类器，选择最优分类器 

> 这里其实不太懂

### Probabilistic Generative Model

####高斯分布(Gaussian Distribution)

高斯分布把 属性值的分布 转为概率分布

分布是什么意思？有什么用？

假设属性值满足高斯分布，找到合适的$\mu,\sigma $,得到分布，以预测位置样本的类别

####模型训练

获得loss fuction 
获得

#### 概率分布函数的选择

- 高斯分布
- Naive Bayes Classifier
- 伯努利分布(Bernoulli distributions)

