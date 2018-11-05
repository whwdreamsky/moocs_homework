### 20171223 重新理清思路

1. Semval 13 的评测是重点需要参考的，因此首先实现baseline

   需要的技术：决策树，特征选择

   提到一个关键词，其实这个任务的本质是`partial textual entailment`

2. 参考 Semval 13 最终得到的效果很好的模型

   - ==SoftCardinality==:   soft cardinality as an extension to classical cardinality，总体是使用决策树

   - UKP-BIU : combining multiple textual similarity measures together using DKPro Similarity

   - ==ETS== : stacking (also seen in some top ASAP ’12 SAS systems) and domain adaptation as a technique to apply non-uniform weights to the features in any model.

   - Levy 13 : 

     > 这个是方法是在Semval 13 之后实现的，不过获得了ACL ，值得参考

   > 评价：主要使用以下三类解决方案，bag-of-words model, lexical inference with semantically related words, syntactic inference with dependency trees。
   >
   > 从效果上看softCardinality  和 ETS 具有较好的性能，而Levy 没有在评测中，需要之后看他的论文

3. 结合当前的深度学习方法，最近的==MatchZoo==，给了文本深度匹配的工具箱，其中还实现了很多之前成功的模型，大多使用CNN，LSTM

   [MatchZoo](https://github.com/faneshion/MatchZoo)

   > 这也可以当做学习深度学习的很好motivation 和 参考

***

1. 重新定义问题

   感觉跟信息检索相关,看信息检索的教材找到相应的解决办法

   - **完成了布尔搜索**,使用`whoose`也就是简单的关键词匹配,但是关键点可能不在这里,更多的是看泛化的方式
   - 看到最近2016 的有一个关于短文本主题模型的论文深入去看看别人是怎么做的,感觉短文本的主题模型在问答,搜索等情景都有应用,挺有用的

2. ~~使用LDA~~

   感觉也不太是主题模型的问题,当然LDA 也很有用

   LSI

3. 基于知识库的语义相似度

4. 问题核心还是短文本相似度

   ==我获得了比较权威的英文的标记文本数据==,下面可以开始对==文本相似度计算进行实现==(2017.12.14),明天主要干的事.

5. 通过ASAG 的论文评价,我知道Levy et al,*Recognizing partial textual entailment* , 这篇文章是当时在2014年最好的系统方案.

6. 最后建议使用 echart 搭建一个网站，展示功能



###Semeval 2015 

有个任务就是文本相似度,我从中获得了文本相似的标记数据

总算找到一个关键名词

### Automated Short-Answer Grading

*A functional semantic similarity evaluator could automatically grade short-answer questions by evaluating the similarity of a student answer with the corresponding correct answer*







