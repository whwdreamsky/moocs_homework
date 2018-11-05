# <center>数据挖掘实验报告</center>

> 第一题： Apriori algorithm and the FP-Growth algorithm for frequent itemset mining

### 前言

本实验报告是解决使用Apriori 和 FP-Growth 算法对频繁项集挖掘的题目，主要是介绍数据生成，算法原理，以及参数性能分析，Apriori 算法性能分析四个方面。

### 1. 数据生成

使用 [IBM Synthetic Data Generator](https://github.com/zakimjz/IBMGenerator) 生成要求的数据，控制参数获得多个测试数据集，使用命令

`./gen lit -ntrans 100 -tlen 10 -nitems 1 -npats 1000 -patlen 4 -fname T10I4D100K -ascii`

| 变参            | 定参                       | 变参设置区间(设10个点)                 |
| ------------- | ------------------------ | ----------------------------- |
| 事务数目(ntrans)  | 事务长度=10 ;1项集个数 = 1,000   | [10,000 ,100,1000]  10,100,10 |
| 事务长度(tlen)    | 事务数目= 10,000;1项集个数=1,000 | [5,50] 5,50,5                 |
| 1项集个数(nitems) | 事务长度=10；事务个数=10,000      | [1,000 , 10,000]1,10,1        |

### 2. 算法原理

####2.1 Apriori 算法

apriori 概念：*If an itemset is frequent, then all of its subsets must also be frequent*，即如果一个项集是频繁的那么它的子项集也是频繁的，这一重要特征发挥在剪枝中，**开创性的使用支持度剪枝**

算法原理简介：采用广度优先搜索算法进行搜索并采用树结构来对候选项目集进行高效计数。它通过长度为k-1的候选项目集来产生长度为k的候选项目集，然后从中删除包含不常见子模式的候选项。根据向下封闭性引理,该候选项目集包含所有长度为k的频繁项目集。之后，就可以通过扫描交易数据库来决定候选项目集中的频繁项目集。

算法流程描述：

1. 从事务中提取长度为1 的频繁项集
2. 由长度K的项集生成长度K+1 项集
3. 剪枝，删除K+1项集中K子集不在频繁K 项集中的项集
4. 计算剪枝过后的支持度
5. 删除非频繁项集，重复2~5 ，直到不再生成新的频繁项集

算法伪代码如下：

```python
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
        produceItems(klist,k1list)
        if len(k1list)<1:break
        # 借助频繁K剪枝
        pruneByK(klist,k1list)
        #计算项集支持度
        countSup(k1list,suplist)
        # 借助支持度剪枝
        for i in range(len(k1list)):
            if suplist[i]<minisup:
                k1list.remove(k1list[i])
        feqItems.extend(k1list)
        klist = list(k1list)
     return freqItems
```

有以上伪代码，我们知道要解决两个主要问题：

1. 由k 项集 生成k+1 项集,Apriori 中k项集合并成k+1 项集要求k 项集的前k

   ~~~python
      def produceItems(self,itemsk,itemsk1,iter):
           for i in range(len(itemsk)):
               tmp = list(itemsk[i])
               for j in range(i+1,len(itemsk)):
                   if iter==1:
                       # 1项集特殊处理
                       tmp.append(itemsk[j][0])
                       #tmp.sort()
                       itemsk1.append(list(tmp))
                       tmp.pop()
                   else:
                       #前k-1个项集相同的合并
                       if itemsk[i][:len(itemsk[i])-1] == itemsk[j][:len(itemsk[j])-1]:
                           tmp.append(itemsk[j][-1])
                           itemsk1.append(list(tmp))
                           tmp.pop()
   ~~~

   ​

2. 根据长度K 项集进行对K+1 项集进行剪枝

   ~~~python
   def joinSet(itemSet, length):
           """Join a set with itself and returns the n-element itemsets"""
           return set([i.union(j) for i in itemSet for j in itemSet if len(i.union(j)) == length])
   ~~~

3. 计算项集的支持度

   使用建立hash树枚举K-项集的方式，当对K 项集计算支持度时扫描一遍数据库，构建一个K 项集hash树

   主要思想：构造hash 函数，h(p) = p mod n ,用来在节点根据前缀产生n个分支，当对k 项集计算支持度时，扫描一遍数据库用之前hash 选择的机制枚举所有的k 项集，这样对多个k 算支持度只用扫描一次数据库，性能大大提升。

####2.2 FP-Growth

fp-growth : 与Apriori 不同，FP-growth 是先通过建立数据的压缩表示，也就是具有相同前缀的条目有相同的树路径，每个节点表示一个项，然后**自底向上**从压缩表示中提取频繁项集，主要利用分治的方法，把整个求频繁项集的过程转化为求多个以特定项集为后缀的子问题。

算法描述：

1. 扫描数据库，获得1项集，然后计算其支持度，把每个事务中的项集按支持度的大小从大到小排序
2. 建立项头表，以频繁1项集建立，每一项接一邻接表,也就是记录了所有该项集出现的位置，方便找到以此项集结尾的子树
3. 构建FP 树，读入排序后的数据集，插入FP树，插入时按照排序后的顺序，插入FP树中，排序靠前的节点是祖先节点，而靠后的是子孙节点。如果有共用的祖先，则对应的公用祖先节点计数加1。插入后，如果有新节点出现，则项头表对应的节点会通过节点链表链接上新节点。直到所有的数据都插入到FP树后，FP树的建立完成.
4. 选择项头表第 i 项，通过邻接表，依次找以此项集结尾的子树，递归挖掘得到项头表i结尾的频繁项集
5. 重复4知道直到遍历项头表

主要要解决的问题：

1. FP 树的构建

   - FP 数据结构，以下是每个树节点设计

     ~~~python
     class FPNode(object):
         """A node in an FP tree."""

         def __init__(self, tree, item, count=1):
             self._tree = tree
             self._item = item
             self._count = count
             self._parent = None
             self._children = {}
             self._neighbor = None
          
     ~~~

   -  构建函数，主要是节点添加

     ~~~python
      def add(self, transaction):
             """Add a transaction to the tree."""
             point = self._root
             for item in transaction:
                 next_point = point.search(item)
                 if next_point:
                     next_point.increment()
                 else:
                     next_point = FPNode(self, item)
                     point.add(next_point)
                     self._update_route(next_point)
                 point = next_point
     ~~~

2. FP 树频繁项集的挖掘

   建立FP 子树，在给定要求下要建立一个FP 的子树以挖掘以某个特定item为结尾的频繁项集

   - 将item按逆序排列，准备逐个开始寻找以该item结尾的频繁项集。
   - 从根节点向上寻找到所有的前缀路径，计算前缀路径中的支持度。
   - 建立条件FP树，更新支持度计数，删除支持度不足的节点。
   - 根据条件FP树找出对应的频繁项集
   - 递归执行c,d步骤，直到没有频繁项集产生。

   ~~~python
   def conditional_tree_from_paths(paths):
       """Build a conditional FP-tree from the given prefix paths."""
       tree = FPTree()
       condition_item = None
       items = set()

       # Import the nodes in the paths into the new tree. Only the counts of the
       # leaf notes matter; the remaining counts will be reconstructed from the
       # leaf counts.
       for path in paths:
           if condition_item is None:
               condition_item = path[-1].item
           point = tree.root
           for node in path:
               next_point = point.search(node.item)
               if not next_point:
                   # Add a new node to the tree.
                   items.add(node.item)
                   count = node.count if node.item == condition_item else 0
                   next_point = FPNode(tree, node.item, count)
                   point.add(next_point)
                   tree._update_route(next_point)
               point = next_point
       assert condition_item is not None
       # Calculate the counts of the non-leaf nodes.
       for path in tree.prefix_paths(condition_item):
           count = path[-1].count
           for node in reversed(path[:-1]):
               node._count += count
       return tree
   ~~~

   ​

### 3 参数性能分析

​	本节分析minsup(支持度阀值)，transactions(事物数)，transactions length事务长度，item nums（一维项集数）对算法耗时的影响。

#### 3.1 事务数

​	如下图所示，本次选择10个节点数据如下表，很明显Fp-growth 的性能要优于Apriori，两种算法都是随着事务的增加时间消耗增加，可以大概判断，事物数和时间是线性正相关关系。从算法的角度，apriori,Fp-growth 都存在遍历数据集的过程，因此数据集的增加对两种算法都有影响。

![](/Users/oliver/Desktop/AI_photo/datamingproject/figure_ntrans.png)

| Ntrans(1000s) | Apriori | FP-growth |
| ------------- | :-----: | :-------: |
| 10            |  1.19   |   0.14    |
| 20            |  2.86   |   0.29    |
| 30            |   4.2   |   0.37    |
| 40            |  5.76   |   0.53    |
| 50            |  7.32   |   0.57    |
| 60            |  8.45   |   0.69    |
| 70            |   10    |   0.76    |
| 80            |  11.5   |   0.85    |
| 90            |   13    |   0.93    |
| 100           |  14.33  |   1.04    |

#### 3.2 事务长度

​	如下图所示，消耗时间随着，事务的长度而增长的过程，根据图分析，事务的长度很大的影响了时间消耗，从算法的角度，事务长度的增加项集的组合数目也大大增加，可以知道事务长度增加对时间消耗成正比关系，同时之前表现很好的Fp-Growth,也对

![](/Users/oliver/Desktop/AI_photo/datamingproject/figure_tlen.png)



| Item  | Apriori | FP-growth |
| :---: | :-----: | :-------: |
| **1** |  13.5   |   1.02    |
| **2** |  37.6   |   12.8    |
| **3** |  140.5  |   76.2    |
| **4** |  271.5  |  187.43   |
| **5** |  457.5  |   380.3   |
| **6** |   664   |    669    |



####3.3 一项集数目

​	如下图所示是Apriori ，Fp-growth两种算法随着项集数目增长，耗时增长的图示，我们看到项集大小对Apriori的影响很大，而对Fp-growth 影响并不大，这很有趣，也就是说项集的增大，Fp-growth 的时间复杂度不受影响，这跟Fp-growth 算法中使用树结构压缩数据这一特性有关。

![](/Users/oliver/Desktop/AI_photo/datamingproject/figure_1.png)

| nitem（1000） | Apriori | FP-growth |
| :---------: | :-----: | :-------: |
|    **1**    |  13.46  |   1.01    |
|    **2**    |  16.13  |   0.675   |
|    **3**    |  19.75  |   0.63    |
|    **4**    |  21.7   |   0.62    |
|    **5**    |  23.7   |   0.68    |
|    **6**    |  26.3   |   0.62    |
|    **7**    |  26.7   |   0.64    |
|    **8**    |  28.2   |   0.62    |
|    **9**    |  29.4   |   0.64    |
|   **10**    |  29.79  |   0.67    |

####3.4 最小支持度大小

​	如下图所示，是时间随着最小支持度的大小增大时间消耗的变化，我们可以看到，随着支持度的增大，时间消耗急剧下降，原因是随着支持度的下降，**频繁项集**的数目急剧减少，因此后面可供继续查找的的项集也同时减少，我们可以看到最小支持度与时间消耗成反相关

![](/Users/oliver/Desktop/AI_photo/datamingproject/figure_minisup.png)

| minisup(0.01) | Apriori | FP-growth |
| :-----------: | :-----: | :-------: |
|       1       |  81.69  |   11.46   |
|       2       |  22.12  |    3.6    |
|       3       |   7.6   |    1.6    |
|       4       |   3.0   |    0.5    |
|       5       |   1.5   |   0.16    |
|       6       |  1.36   |   0.09    |
|       7       |  1.06   |   0.06    |
|       8       |  0.99   |   0.06    |
|       9       |  0.97   |   0.06    |
|      10       |  0.97   |   0.05    |

### 4 .Apriori 算法性能分析

​	这一节我简单测试实现的Apriori 在候选生成，候选剪枝，支持度计算的消耗时间。这里举例生成候选耗时，0.01，计算支持度12.7，剪枝2.6，可以大概判断出计算支持度花费的时间最长，计算支持度的最长，从算法的角度上说，计算支持度的过程需要遍历整个数据库，这一操作耗时最大。

### 5.参考文献

- 数据挖掘导论，第六章

- 维基百科 Apriori 算法

- 维基百科 FP-growth

- github Fp-growth  https://github.com/enaeseth/python-fp-growth

  作者:enaeseth

###6.附录

这里只给出Apriori 的实现代码，Fp-growth的实现是参考github 上的代码

~~~python
# -*- coding: utf-8 -*-
# D 是输入的一个个条目
# D: type List
import time
from argparse import ArgumentParser
class Apriori:
    def __init__(self):
        self.data = []
        self.T = 0
        self.itemsdict = {}
        self.itemsize= 0
        self.freqItem = {}
        #优化,测试发现差别并不是很大，希望在大的数据集上有较大差别
        self.id_to_name = {}
    # 用id 替换item 以优化
    def convertToID(self,item):
        for i in range(len(item)):
            item[i] = self.itemsdict[item[i]]
        item.sort()

    def adddict(self,temp):
        for item in temp:
            if item not in self.itemsdict:
                self.itemsdict[item] = self.itemsize
                self.id_to_name[self.itemsize] = item
                self.itemsize+=1


    def getDataSet(self,filename):
        with open(filename) as f:
            for line in f:
                temp = line.strip('\n').split()
                if '' in temp :
                    temp.remove('')
                self.adddict(temp)
                self.convertToID(temp)
                self.data.append(list(temp))
        self.T = len(self.data)

    def getItemById(self,id):
        '''
        :param id:int
        :return:
        '''
        #可以优化这里，选择那种可以一一对应的数据结构
        for key,value in self.itemsdict.iteritems():
            if value == id:
                return key
        return 'NAN'
    def getItemById_optimized(self,id):
        return self.id_to_name[id]

    def countSup(self,itemset):
        '''
        :param itemset: set
        :return: float
        '''
        count = 0
        for item  in self.data:
            if itemset.issubset(set(item))==True:
                count+=1
        return float(count)/self.T

    def countSupBatch(self,itemlist,supdict):
        for item in itemlist:
            item.sort()
            # join 不能加int
            str_item = ','.join(str(x) for x in item)
            #supdict["123"] = "123"
            itemset = set(item)
            supdict[str_item] = self.countSup(itemset)

    #TODO 优化这里的代码
    def produceItems(self,itemsk,itemsk1,iter):
        for i in range(len(itemsk)):
            tmp = list(itemsk[i])
            for j in range(i+1,len(itemsk)):
                if iter==1:
                    # 1项集特殊处理
                    tmp.append(itemsk[j][0])
                    #tmp.sort()
                    itemsk1.append(list(tmp))
                    tmp.pop()
                else:
                    #前k-1个项集相同的合并
                    if itemsk[i][:len(itemsk[i])-1] == itemsk[j][:len(itemsk[j])-1]:
                        tmp.append(itemsk[j][-1])
                        itemsk1.append(list(tmp))
                        tmp.pop()
    def pruneItmes(self,items,supdict,minisup):
        for key,value in supdict.iteritems():
            if value< minisup:
                tmp = str(key).split(',')
                tmp_int = [int(x) for x in tmp]
                tmp_int.sort()
                items.remove(tmp_int)
    def addFreqItem(self,items,supdict):
        for item in items:
            idstr = ",".join([str(x) for x in item])
            namelist = []
            for i in item:
                namelist.append(self.getItemById(i))
                #namelist.append(self.getItemById_optimized(i))
            self.freqItem[",".join(namelist)] = supdict[idstr]

    def apriori(self,minisup):
        # generate k==1
        items_1 = list(self.itemsdict.values())
        #这里要保证对1项集排序
        items_1.sort()
        temp = []
        # supdict 格式：'id1,id2,...':float
        supdict = dict()
        k1list = []
        temp = []
        iter = 1
        #TODO 优化这里写法
        for item in items_1:
            temp1 = []
            temp1.append(item)
            temp.append(list(temp1))
        items_1 = list(temp)
        # 对1项集剪枝
        self.countSupBatch(items_1,supdict)
        self.pruneItmes(items_1,supdict,minisup)
        self.addFreqItem(items_1,supdict)
        klist = list(items_1)
        # k 项集生成k+1 项集
        while (len(klist) > 0):
            # 生成新的项集
            k1list = []
            supdict.clear()
            self.produceItems(klist,k1list,iter)
            if len(k1list) < 1: break
            # 计算项集支持度
            self.countSupBatch(k1list,supdict)
            # 剪枝
            self.pruneItmes(k1list,supdict,minisup)
            # 加入频繁项集
            self.addFreqItem(k1list,supdict)
            klist = list(k1list)
            iter +=1

#测试运行时间
start = time.clock()
##start
parser = ArgumentParser()
parser.add_argument('datafile', type=str, help = 'data file "item seperated by space ')
parser.add_argument('minisup', type=float, help = 'minisup')
args = parser.parse_args()

apri = Apriori()
apri.getDataSet(args.datafile)
apri.apriori(args.minisup)
# print Freq Item
for key,value in apri.freqItem.iteritems():
    print key,value
##stop
elapsed = (time.clock() - start)
print "Time cost:" ,elapsed
~~~

