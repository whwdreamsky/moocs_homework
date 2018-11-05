# Machine Learning 

### 决策树

- 决策树ID3算法文字描述，要加入过拟合

  ![书上的描述](/Users/oliver/Desktop/AI_photo/ml_1.png)

- GS 算法

- AQ 算法

- 扩张矩阵

- ==最小属性子集==

  > 完全不明白这个在说什么

>  主要总结见machineLearning 下决策树的总结，这里只关注考点

### 概念学习(concept learning)

概念学习指的是

> 反正大概意思就是通过给出的特殊训练实例给出一般概念，跟之前决策树相同也就是根据实例得到规则

教材第二章

1. 一些基本概念：假设空间，假设函数H,目标函数，

2. Find-S 算法

   ~~~python
   #1.将h 初始化为H中最特殊假设,也即是全部为"空集"(一种都不取)
   #2.对每个正例，若h(x) != c(x),用满足x 的 h 极小普化式代替h
   #3.输出假设H
   ~~~

   > 现在才明白，Find-S 是 General to Special , 有点怀疑那个最小属性值的题指的是Find-S

3. 列表后消除算法

   大概的意思就是穷举所有可能的假设，然后遍历数据集，把不满足假设的条目删除

4. ==候选消除算法==（考第一题重要)

   很可惜，这两个移除的过程没看懂

   ​

### 人工神经网络

- 梯度下降
- 反向传播

### 遗传算法



### 基于实例学习

1. KNN

### 观察与发现学习

概念聚类：

### 规则学习

- 写FOIL算法

### 贝叶斯学习

- 朴素贝叶斯分类器

### 强化学习

1. 写出**消极学习**的ID3算法