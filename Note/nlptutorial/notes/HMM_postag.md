# Hidden Markov Models 

### 隐马尔科夫模型基本构成



####三要素

三元组 $\lambda=(A,B,\pi)$ ,N表示状态的个数，M 表示可能的观测数

-  $A$ 表示状态转移矩阵，$A=[a_{ij}]_{N*N}$ 
- $B$ 表示观测概率矩阵(发射矩阵)，$B = [b_j(k)]_{N*M}$ ,在状态j 下生成观测k 的概率
- $\pi$ 表示初始概率向量

####两个基本假设

- 齐次马尔科夫假设：假设隐藏马尔科夫链在任意时刻t 的状态只依赖于之前一个时态的状态 t-1 , 与其他状态无关
- 观测假设：任意观测只依赖于该时刻状态

### 三个基本问题

1. 概率计算：给定模型 $ \lambda (A,B,\pi$) 和观测序列 $O$，给出$P(O|\lambda)$ 

   >  简单来说，概率计算就是给定一个句子去判定其是否符合句法结构，给出这个句子的概率，也就是语言模型干的事

2. 学习问题：根据序列使用极大似然估计，去估计模型参数，也就是求解三元组

3. 预测问题：也就是解码问题, 给定模型 和观测序列，找到概率最大的状态序列 I             



#### 概念计算

- 前向计算：简单的思路是穷举每个状态 I 序列，求和状态序列下观测序列的概率 $\sum_{I\in \lambda} P(O|I)$

  使用递推的思想降低算法复杂度，利用之前的结果重复计算，递推公式
  $$
  \alpha _{t+1}(i)=P(o_{t+1}|o_1,o_2,\dots o_t,i,\lambda)=(\sum\alpha_t(j)a_{ji})\cdot b_i(o_{t+1})\\\
  P(O_t|\lambda)=\sum_{i=1}^N \alpha_t(i)
  $$

  > 注意这可不是维特比算法，因为这里不是在计算最大概率，没有最优结构
  >
  > 这里只是为了较少计算，重复利用子结构而已
  >
  > 这其实也就是一个全概率公式

- 后向计算:和前向计算类似，但是给定的是$O_{t+1},O_{t+2},\dots O_{}$ , 求第t 时刻概率，因为t+1时刻概率只跟第t 时刻概率相关，下面相当于求一个边缘概率，第t 个状态是i的概率，$P_t(i_t|O_{t+1}) =\sum_{j}^N P_{t+1}(i_t,j_{t+1}|O_{t+1}) $
  $$

  $$
  ​

  > 后向计算的初始概率为啥是1 

后向计算：

####预测算法

目的是根据观测序列和模型，求解概率最大的状态序列

典型应用： 在进行序列标注的时候，给定一个句子,也就是给定一个观测序列O，模型训练的过程是通过已标记的序列，通过极大似然估计来估计A，B，也就是通过计数比值的方式得到。预测序列的词性

**维特比算法**

首先引入变量$X_t(i)$ ,指时间t , 观测为$o_t$下,状态 i 的最大概率
$$
X{t}(i) = max[X_t(j)\cdot a{ji}]*b_i(o_t)
$$
引入 变量 Y_t(i) ,若表示t 时刻状态为i，t-1 时刻的最大概率上的点，主要用处是记录最大概率路径上的点
$$
Y_t(i)=arg max{X_{t-1}(j)a_{ji}}
$$
在回溯Y_t 的路径 就是最优路径

### Discriminative

### Hidden Markov Models(HMMs) for POS Tagging

要判断某个词是某个tag,即得到这个词在所有标签下的概率最大值,应用贝叶斯定义,对于某个确定的词,即$$P(word)==1$$,
$$
P(pos|word) = \frac{P(pos\cdot word)}{P(word)}\\
P(word|pos) = \frac{P(pos\cdot word)}{P(pos)}\\
P(pos|word) = P(word|pos)\cdot \frac {P(pos)}{P(word)}=P(word|pos)\cdot P(pos)
$$

> 贝叶斯公式:

###Find the best path(Viterbi)



CRF

https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjKoLHMooLYAhUU22MKHZXTCesQFggpMAA&url=http%3A%2F%2Fwww.cs.cmu.edu%2F~epxing%2FClass%2F10708-07%2FSlides%2Flecture12-CRF-HMM-annotation.pdf&usg=AOvVaw3aL35-7GrryY4NZJmC5yLz