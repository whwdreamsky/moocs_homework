# 机器学习实验之决策树

### 算法原理

ID3 算法：

决策树有一个根节点，一系列的非叶子节点，叶子节点构成，其实决策树本质是利用非叶子节点中对属性不断测试，不断选择属性再根据属性值对数据进行划分，从数据中学习到一系列的规则最为分类的依据，决策树的生成是一个==递归==的过程，ID3 是通过信息增益的方式进行最优属性选择。

####最优属性选择问题

问题：findOptimizeAtr(A) 要选择最优划分属性

- 信息增益的方法

  - 信息熵的概念:信息熵就是平均而言一个事件发生得到的信息量大小，也就是信息量的期望值
    $$
    Ent(D) = -\sum_{k = 1}^{|y|}p_klog_2p_k\\\
    Gain(D,a)=Ent(D)-\sum_{v=1}^{V}\frac{D_v}{D}Ent(D_v)
    $$

    > Ent ： 这里y 代表的是分类的类别数，Ent(D) 我理解是每个节点的信息熵，信息的含义就是分类为y个类别的纯度，类别越集中熵值越小
    >
    > Gain ： V 代表的是属性a 的值的个数，也就是当前父节点的分支个数，这里也就是算出每个子节点的信息熵再加权取均值，再和父节点的信息熵做差值，这就叫信息增益，ID3 算法也就是优先选择信息增益大的属性

  - 缺陷：对属性值多个的有偏好

- 增益率（C4.5）
  $$
  GainRadio(D,a) = \frac{Gain(D,a)}{IV(a)}\\\
  IV(a ) = -\sum\frac{|D_v|}{|D|}log_2\frac{|D_v|}{|D|}
  $$

  > 其实增益率也就是克服之前信息增益对属性值多的偏好问题，除了一个IV，这个值当属性值种类越多IV 的值也越大，但是这有带来了对属性值种类数较少的偏好，在C4.5中是用`启发`的方式，先选择信息增益大于平均水平的的，再从中选增益率大的

- 基尼指数（CART）

  这个是CART 回归树,使用基尼指数作为属性选择的标准，以下基尼指数表示不纯度。
  $$
  Gini(D) = \sum^{|y|}_{k=1}\sum_{k'\neq k}p_kp_{k'}\\\
  Gini(D)=1- \sum p_k^2
  $$



####属性值划分问题

- 连续值划分

  二分法：找到属性所有取值中信息增益最大的作为划分点，这里属性划分后还可作为后代节点的划分属性

  计算每个值得基尼指数或者熵选择最优的

  要解决的问题：

  1. 属性选择时熵计算

  2. 如何对缺失的条目进行划分

     利用已知条目的划分来估计缺失条目划分，好处是不需要t填充缺失值

- 多变量属性值

  特定：非叶子节点不再是对某个特定属性进行判断，而是属性的线性组合

####过拟合问题优化

关键问题： 什么时候停止划分

在最基础的hunt 中，在如下情况下停止：

1. 输入的数据集D 的类别相同
2. 在属性a的条件b 下 数据集值为空
3. 剪枝

- 预剪枝

  即在生成决策树的时候就决定是否剪枝

- 后剪枝

  即先生成决策树，再自底向上判断节点剪枝前后对决策树精度的影响。这里注意根据奥卡姆剃刀原则，尽管对精度不产生影响也要剪枝

### 问题描述

#### 需求描述

本实验项目要解决自动短回答评分(Automaitc short answer grading)任务，这里简称ASAG，场景是在mooc(在线学习)上根据老师给出这个问题的几个参考答案，对学生关于问题的答案进行分级。是应用语义分析(semantic analysis)在在线学习中，尤其是文本蕴含的问题。

例如，给出一个问题，一个已知的参考答案，一个学生回答，目标是为学生回答打分

这里的难点在于

1. 对学生回答进行判断不能单单从字面上判断，需要外部知识
2. 回复不像其他评分任务，是自然语言的文本，涉及到对语言的理解
3. 回复的长度可长可短
4. 回答受限于设计的问题，领域是未知的

####评分标准：

有三种打分方式，5-way,2-way,3-way，分别是不同粒度的划分，同时难度也随之递减

- 5-way:

  回答主要评价为5类，正确(correct),部分正确(partially_correct_incomplete)，不切题(irrelevant)，不在范围内(non_domain)，矛盾(contradictory)

- 3-way:

  正确(correct) , 矛盾(contradictory),不正确 (incorrect)

- 2-way:

  正确 (correct),不正确 (incorrect)

#### 特征选择

问题理解，简单的解决方案是，优先选择与标准答案中的一个意义最相近的判断为 correct ，其次选择跟问题最相关的，根据这两个方面对学生的回答进行评分。

本次实现选择，问题和学生回答，标准答案和学生回答的相似度指标作为特征，根据来自 perl 的Text::Similarity::Overlaps module 包，选择overlap，cosine ，F1 score，Lesk 四个相似度指标

1. overlap_SR_RR ：这里取学生回答和标准回答的词重叠的最大值
2. overlap_SR_QE ：这里取学生回答和问题的词重叠
3. cosine_SR_RR:  这里取学生回答和标准回答的词袋cosine的最大值
4. cosine_SR_QE：这里取学生回答和问题的cosine值
5. F1 score_SR_RR：这里取学生回答和标准回答的F1的最大值
6. F1 score_SR_QE：这里取学生回答和问题的F1
7. Lesk score_SR_RR：这里取学生回答和标准回答的 Lesk score的最大值
8. Lesk score_SR_QE：这里取学生回答和问题的F1

#### 数据集描述

本次数据集使用SemVal 13 评测中的 **THE STUDENT RESPONSE ANALYSIS TASK** 这个任务的数据，其中包含了4000条问题回答对，大致的数据格式如下，主要分为三块，问题，参考答案，学生回答

~~~xml
<question qtype="Q_EXPLAIN_SPECIFIC" id="BULB_C_VOLTAGE_EXPLAIN_WHY1" module="FaultFinding" stype="EVALUATE">
   <questionText>Explain why you got a voltage reading of 1.5 for  terminal 1 and the positive terminal.</questionText>
   <referenceAnswers>
   <referenceAnswer category="BEST" id="answer204" fileID="BULB_    C_VOLTAGE_EXPLAIN_WHY1_ANS1">Terminal 1 and the positive terminal     are separated by the gap</referenceAnswer>
   <referenceAnswer category="BEST" id="answer205" fileID="BULB_    C_VOLTAGE_EXPLAIN_WHY1_ANS2">Terminal 1 and the positive terminal     are not connected</referenceAnswer>
   </referenceAnswers>
  <studentAnswers>
  <studentAnswer count="1" answerMatch="answer204" id="FaultFin    ding-BULB_C_VOLTAGE_EXPLAIN_WHY1.sbj3-l1.qa193" accuracy="correct    ">positive battery terminal is separated by a gap from terminal 1    </studentAnswer>
  </studentAnswers>
</question>
~~~

数据类别的分布如下图

| type                         | number | Freq |
| ---------------------------- | ------ | ---- |
| Correct                      | 1157   | 0.42 |
| partially_correct_incomplete | 626    | 0.23 |
| irrelevant                   | 656    | 0.24 |
| non_domain                   | 86     | 0.03 |
| contradictory                | 204    | 0.07 |

### 详细实现

####数据处理

1. 从结构化数据中提取,并计算 overlap , F,lesk,cosine,questionOverlap , questionF,questionLek,questionCosine,`./extractBaselineFeatures.sh`,脚本抽取特征，

   > 这里使用的是词重叠算法

   1. 使用决策树作为分类器，学习之前提取的特征,并保存之前的模型，使用java 的weka 包 		`BaselineClassifier.java`,存下模型

2. 使用学习的模型对之前的所有数据进行预测，获得了模型输出

3. 通过`./extractEvaluationTable.sh` 处理得到标注gold数据，`evaluate.sh` 运行脚本对之前模型输出和标注数据计算指标

获得分类数据如下，4000条具有特征参数的数据。

| name | overlap | F    | lesk | cosine | questionOverlap | questionF | questionLesk | questionCosine    | accuracy |
| ---- | ------- | ---- | ---- | ------ | --------------- | --------- | ------------ | ----------------- | -------- |
| 193  | 7       | 0.67 | 0.1  | 0.6    | 5               | 0.357     | 0.037        | 0.365636212063565 | correct  |





#### ID3算法程序伪代码

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

###结果分析

####实验结果

| fold | precision_macro | recall_macro | f1_macro    |
| ---- | --------------- | ------------ | ----------- |
| 1    | 0.4276555       | 0.50137522   | 0.44957526  |
| 2    | 0.38705993      | 0.43746493   | 0.4006479   |
| 3    | 0.3997114       | 0.43505314   | 0.40890503  |
| 4    | 0.41046657      | 0.37671502   | 0.38927187  |
| 5    | 0.36270728      | 0.3913147    | 0.37303859  |
| 6    | 0.32859919      | 0.37978747   | 0.34863447  |
| 7    | 0.39719891      | 0.41372596   | 0. 40068495 |
| 8    | 0.42081027      | 0. 42062816  | 0. 42050085 |
| 9    | 0.36802488      | 0. 43862554  | 0. 38782814 |
| 10   | 0.38735816      | 0. 45647268  | 0. 41280159 |
| mean | 0.38895920      | 0.425116281  | 0.39918886  |

#### sklearn 实战

使用sklearn(机器学习包)中封装好的C4.5 训练数据，并使用 10-fold 交叉验证给出准确率

~~~python
# coding: utf-8
from sklearn.tree import DecisionTreeClassifier as dtc
import numpy as np
from sklearn.model_selection import cross_validate
from sklearn.metrics import recall_score
with open('beetleFeature.copy') as f:
    data = []
    for line in f:
        line = line.strip()
        tmplist = line.split(',')
        data.append(tmplist)
data_np = np.array(data)
y = data_np[:,-1]
x = data_np[:,1:8]
scoring = ['precision_macro', 'recall_macro','f1_macro']
clf = dtc()
socres = cross_validate(clf,x,y,scoring=scoring,cv=10,return_train_score=False)
socres
clf.predict(x)
~~~



### 附录

~~~python
# coding: utf-8
import matplotlib.pyplot as plt

# 使用文本注解绘制树结点
decisionNode = dict(boxstyle="sawtooth", fc="0.8")
leafNode = dict(boxstyle="round4", fc="0.8")
arrow_args = dict(arrowstyle="<-")

def plotNode(nodeText, centerPt, parentPt, nodeType):
    createPlot.ax1.annotate(nodeText, xy=parentPt, xycoords='axes fraction',\
        xytext=centerPt, textcoords='axes fraction', va='center', ha='center', \
        bbox=nodeType, arrowprops=arrow_args)

# matplot绘图函数
# def createPlot():
#   fig = plt.figure(1, facecolor='white')
#   fig.clf()
#   createPlot.ax1 = plt.subplot(111, frameon=False)
#   plotNode('Decision Node', (0.5, 0.1), (0.1, 0.5), decisionNode)
#   plotNode('Leaf Node', (0.8, 0.1), (0.3, 0.8), leafNode)
#   plt.show()


# 获取叶子节点的数目和树的层数
def getNumLeaves(myTree):
    numLeaves = 0
    firstStr = myTree.keys()[0]     # 决策树的第一个键值
    secondDict = myTree[firstStr]   # 字典中第一个key所对应的value，即下面的目录
    for key in secondDict.keys():
        if type(secondDict[key]).__name__=='dict':  # 如果是字典类型，则继续递归
            numLeaves += getNumLeaves(secondDict[key])
        else:   # 如果不是字典类型，则停止递归
            numLeaves += 1
    return numLeaves

def getTreeDepth(myTree):
    maxDepth = 0
    thisDepth = 0
    firstStr = myTree.keys()[0]
    secondDict = myTree[firstStr]
    for key in secondDict.keys():
        if type(secondDict[key]).__name__=='dict':
            thisDepth += getTreeDepth(secondDict[key])
        else:
            thisDepth = 1
        if(thisDepth > maxDepth):
            maxDepth = thisDepth
    return maxDepth

# 预先存储树的信息，方便读取测试
def retrieveTree(i):
    listOfTrees =[{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}},
                  {'no surfacing': {0: 'no', 1: {'flippers': {0: {'head': {0: 'no', 1: 'yes'}}, 1: 'no'}}}}
                  ]
    return listOfTrees[i]

def plotMidText(centerPt, parentPt, txtStr):
    xMid = (parentPt[0] + centerPt[0]) / 2.0
    yMid = (parentPt[1] + centerPt[1]) / 2.0
    createPlot.ax1.text(xMid, yMid, txtStr)

def plotTree(myTree, parentPt, nodeTxt):
    firstStr = myTree.keys()[0]
    numLeaves = getNumLeaves(myTree)
    depth = getTreeDepth(myTree)
    centerPt = (plotTree.xOff + (1.0 + float(numLeaves))/2.0/plotTree.totalW, plotTree.yOff)
    # centerPt = (0.5, plotTree.yOff)
    plotMidText(centerPt, parentPt, nodeTxt)
    plotNode(firstStr, centerPt, parentPt, decisionNode)
    secondDict = myTree[firstStr]
    plotTree.yOff -= 1.0 / plotTree.totalD  # 由于绘图是自顶向下的，所以y的偏移要递减
    for key in secondDict.keys():
        if type(secondDict[key]).__name__ == 'dict':
            plotTree(secondDict[key], centerPt, str(key))
        else:
            plotTree.xOff += 1.0 /  plotTree.totalW
            plotNode(secondDict[key], (plotTree.xOff, plotTree.yOff), centerPt, leafNode)
            plotMidText((plotTree.xOff, plotTree.yOff), centerPt, str(key))
    plotTree.yOff += 1.0 / plotTree.totalD # 在绘制了所有子节点后，增加y的偏移



def createPlot(inTree):
    fig = plt.figure(1, facecolor='white')
    fig.clf()
    axprops = dict(xticks=[], yticks=[])
    createPlot.ax1 = plt.subplot(111, frameon=False, **axprops)
    plotTree.totalW = float(getNumLeaves(inTree))
    plotTree.totalD = float(getTreeDepth(inTree))
    plotTree.xOff = -0.5/plotTree.totalW
    plotTree.yOff = 1.0
    plotTree(inTree, (0.5, 1.0), '')
    plt.show()
~~~





