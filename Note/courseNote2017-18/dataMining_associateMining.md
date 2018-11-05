# dataMing

###统计学基本概念

- 方差（variance）

  - 样本方差：除n-1，

  - 概率中方差：除n,概率论中方差用来度量随机变量和其数学期望（即均值）之间的偏离程度。

  - 标准差

  - 协方差：协方差就是这样一种用来度量两个随机变量关系的统计量

    协方差矩阵，对大于2维求协方差，就是一个矩阵，其实也就是两两求协方差

    > [资料](http://www.cnblogs.com/eczhou/p/5434996.html)

  - min-max normalization 

  - 皮尔森相关系数

###数据预处理

- 数据归一化

  -  Min-max 标准化

  -  Z-score ：用作归一化，减均值，除标准差
     $$
     \frac{v-u}{\sigma}
     $$


  > [资料]( http://www.cnblogs.com/chaosimple/p/3227271.html)

- 数据相似度计算

  - Euclidean Distance 欧几里得距离

  - Minkowski Distance : 广义欧几里得

    ![](/Users/oliver/Desktop/AI_photo/Minkowski_dataming.png)

  - 二进制向量
    - SMC :也就是把f11,f00 都考虑进去
    - Jaccard 系数，不考虑f00

  - Cosine Similarity

  - 广义Jaccard ：

     ![](/Users/oliver/Desktop/AI_photo/jaccard.png)

    ​

- 数据相关性度量

  - Pearson’s correlation coeﬃcient

  > 这里相关性和相似性是有区别的

  ![](/Users/oliver/Desktop/AI_photo/Pearson’s.png)

  x,y 协方差除以两个变量的标准差，计算的系数 corr 如果等于0不相关，大于0 正相关，小于0负相关

- 独立性检验

  - $x^2$ 独立性检验

    ![](/Users/oliver/Desktop/AI_photo/x^2.png)

    ​

  http://www.voidcn.com/article/p-gzcamzst-qh.html

### 关联分析

- **基本概念**

  **关联关系**：*Given a set of transactions, find rules that will predict the occurrence of an item based on the occurrences of other items in the transaction*，翻译过来也就是给定一些数据条目，找到项集Y 是基于X 出现的，即当X 出现的时候 Y 很有可能出现，也表示为$X\rightarrow Y$

  **关联关系挖掘**：给定一个数据集合，找到支持度和置信度大于规定阀值的关联关系

  > 这里映射代表的共同出现(co-occurrence)而不是因果关系(causality)

  **支持度(support)**：项集出现的频率

  **置信度(confidence)**: 对于关联关系$X\rightarrow Y$,(X,Y) 在 (X) 下出现的概率

  **评估方式**：

  - lift : 解决c 的后件支持度影响问题,如果 lift 小于1 ，证明 A,B 负相连
    $$
    lift = \frac{C(A\rightarrow B)}{S(B)}
    $$
    ​

  -  Interest 
    $$
    Interest = \frac{P(AB)}{P(A)P(B )}
    $$

  - coefficient

  ![](/Users/oliver/Desktop/AI_photo/coefficient.png)

- **频繁项集挖掘**

  - 暴力方法

  - Apriori 算法

    apriori 概念：*If an itemset is frequent, then all of its subsets must also be frequent*，即如果一个项集是频繁的那么它的子项集也是频繁的，这一重要特征发挥在剪枝中，**开创性的使用支持度剪枝**

    算法伪代码

    ~~~python
    #D 是输入的一个个条目  
    # D: type List
    def Apriori(D,minisup):
       
        for item in D:
            items_1.update(item)
        klist = list(items_1)
         # generate k==1
        countSup(klist,suplist)
        for i in range(len(klist)):
                if suplist[i]<minisup:
                    klist.remove(klist[i])
        feqItems.extend(klist)
        # k 项集生成k+1 项集
        while(len(klist)>0):
            #生成新的项集
            produceItems(klist)
            if len(klist)<1:break
            #计算项集支持度
            countSup(klist,suplist)
            #剪枝
            for i in range(len(klist)):
                if suplist[i]<minisup:
                    klist.remove(klist[i])
            feqItems.extend(klist)
         return freqItems
    ~~~

    有以上伪代码，我们知道要解决两个主要问题：

    1. 怎么计算项集的支持度
    2. 怎么又k 项集 生成k+1 项集

    ​

  - FP - growth 算法

    fp-growth : 与Apriori 不同，FP-growth 是先通过建立数据的压缩表示,称为Fp-tree，也就是具有相同前缀的条目有相同的树路径，每个节点表示一个项，然后**自底向上**从压缩表示中提取频繁项集，主要利用分治的方法，把整个求频繁项集的过程转化为求多个以特定项集为后缀的子问题。(因为每个频繁项集都有唯一的后缀，所以我们把问题分解为每个后缀的频繁项集)

    算法步骤：

    1. 算法描述：

       1. 扫描数据库，获得1项集，然后计算其支持度，把每个事务中的项集按支持度的大小从大到小排序

       2. 建立项头表，以频繁1项集建立，每一项接一邻接表,也就是记录了所有该项集出现的位置，方便找到以此项集结尾的子树

       3. 构建FP 树，读入排序后的数据集，插入FP树，插入时按照排序后的顺序，插入FP树中，排序靠前的节点是祖先节点，而靠后的是子孙节点。如果有共用的祖先，则对应的公用祖先节点计数加1。插入后，如果有新节点出现，则项头表对应的节点会通过节点链表链接上新节点。直到所有的数据都插入到FP树后，FP树的建立完成.

       4. 选择项头表第 i 项，通过邻接表，依次找以此项集结尾的子树，递归挖掘得到项头表i结尾的频繁项集，下面是示例一个子过程(书P226 例子)，**从大的FP 树中取得e 结尾的FP 树**

       5.  先判断e 是否为频繁项集，如果是再去判断 de,ce结尾的是否是频繁项集，这样我们从e 树中提取 d 子树，称为e 的条件FP 树，==删除非频繁项+更新前缀上的节点支持度==，递归建立条件FP 树

          > 这个删除非频繁项集具体是怎么操作的？

       6. 重复4知道直到遍历项头表

  - 关联挖掘的

- 频繁项集的压缩表示

  - 极大频繁项集：项集的直接超集都是非频繁的频繁项集

    > 对集合 {a,b},它的超集包括{a,b,c}

    算法 ： MaxMiner Algorithm ,设置两个集合，h 表示前缀，t 表示带扩展的尾部

    1. 初始化，h ={},t = {A,B,C,D} t 也就是全部1 项集

    2. 判断带扩展的N，如果 h ,t 的一个子集是非频繁的那么在扩展前就删除这个子集，

       ==这里有问题，这里算是通过支持度计算剪枝==

    3. 判断 h V t 如果是频繁的则不扩展，直接得到了频繁项集

    4. 全局剪枝，如果h Vt 是极大频繁项集的子集那么直接剪枝

    > 极大频繁项集的条件，认为有2
    >
    > 1. h(n)V t(n) 是频繁的
    > 2. n 的直接超集都为不频繁

    ==TODO==

  - 闭频繁项集：支持度不和它的任何子集相同

  > 关系所有的极大频繁项集都是闭频繁项集

  ​

  ​

####序列挖掘

GSP(Generalized Sequential Pattern (GSP))

基本概念： 子序列的定义

跟之前的相同也是分为

1. 候选生成

   跟之前项集不同，序列的生成满足的条件是k项集a 有长度为k-1的"后子串",出现在k项集b 的前子串

   例如   w 1 =<{1} {2 3} {4}> and w 2 =<{2 3} {4 5}>

2. 候选剪枝：这里是通过生成的k 项集中包含 非频繁 k-1 项集的进行剪枝

3. 支持度计算

4. 根据支持度进行候选消除

SPADE Algorithm

Pseudo-Projection



#### 图挖掘

##### 基本概念

子图的概念：

图挖掘中支持度：包含某个子图的比例

按照之前的思路同样有基于Apriori的方法，有如下关键问题：

- 候选生成:由两个k-1 子图生成k 子图

  - 定点增长
  - 边增长

  > 与之前的区别在于之前都是生成一个k 项集，而这里不同是生成多个k项集

- 剪枝：计算支持度

  判断一个图另一个图的子图是np 难问题，

  子图同构问题:给图设定一个图代码 code,但是同构的图仍然有不同的代码

  > 这里没看懂

  ​

####Gspan

特点 ：Right-most Extension ， 意思就是k 项集扩展只扩展在最右路径上

![书上的描述](/Users/oliver/Desktop/AI_photo/right-most.png)

> 这个最右节点的扩展其实不太懂

避免生成子图的重复问题 

DFS : 对图进行DFS 搜索，也就是由根节点出发，到最右的节点，这个编号也就是DFS 遍历这个图的顺序，

![书上的描述](/Users/oliver/Desktop/AI_photo/DFS.png)

- DFS Code

  前向边：在DFS tree 中的边即 <u,v>表示边是 u<v ，同理有反向边

  边次序：要满足如下条件

  - 两个正向边：(u 1 , v 1 ) and (u 2 , v 2 ), if v 1 < v 2 , then (u 1 , v 1 ) < T (u 2 , v 2 )，按照v 的从大到小排

  - 两个反向边：if u 1 < u 2 , then (u 1 , v 1 ) < T (u 2 , v 2 ) , 也就是按照u 来排，u 相等按v排

  - u1 v1 为正，u2,v2 为反 

    若v1 < = u2  正反

    若 v1 > u2 反正

    ![书上的描述](/Users/oliver/Desktop/AI_photo/DFS_code.png)

    > 注意完整的dfs code 是5元祖，(出发DFS编号,终止DFS编号,出发节点，边，目的节点)
    >
    > 其实也就是 DFS 顺序流程，遇到终点有反向边，就紧接着写反向边

  - 最小DFS code ，定义为词序最小，每个通过的图都有一个相同的最小DFS code，这给剪枝，支持度计算带来了大量的便利

    ![书上的描述](/Users/oliver/Desktop/AI_photo/mini_DFS_code.png)

    > 这里词序不太确定，书上支出，code 的每一部分都可比较
    >
    > 怎么画这个最小DFS

    Gspan

    gSpan Algorithm

    - Scan the database to find all frequent edges

    - For each frequent edge e do

      - Perform a DFS on the subtree rooted at e

      - If a DFS code C visited in the DFS is minimum, then

        uCount the support of C uIf sup(C) >= minsup then

        output C

        Get the children of C by right-most extension

    > 这个算法核心是判断如果不是最小DFS 就可以直接剪枝



### 分类

#### 决策树

主要是那几个选择属性的公式

### 模型评价(决策树)

 


### 实践

  https://github.com/linyiqun/DataMiningAlgorithm

希望利用数据集很好的实践！！！

http://archive.ics.uci.edu/ml/index.php