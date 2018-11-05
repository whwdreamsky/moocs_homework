# Mooc讨论区回答文本理解与分析

##实验设计方案与实现

- ###问题定义

- ###实验方案

  ![](/home/oliver/Desktop/tmp/流程mooc1.png)

- ###数据获取

  - 数据爬取

  - 数据清洗

    去重,去掉同一个id 的相同条目,去除回复中的英文数字以及英文标点	

通过对中国大学mooc的数据爬取,针对一个问题"怎么研究网络?",获取了3300条数据,

经过过滤,去重获得到1300条数据,并包含用户名;评论数;投票

### 文本表示

### 词表示

词的表示是训练语言模型的副产物

> 这里结合之前学习的n-gram语言模型,那词的表示就是那些概率

- **one hot encoding**

  *Bag-of-words model*: 为每个词分配一个词表维度的向量,但因为高维度,可以使用稀疏存储

  - 优点:模型简单,易于理解
  - 缺点:词和词之间没有关联

  使用词袋模型+TF-IDF,即每个向量的加和表示一个文本

  > 典型应用:http://www.ruanyifeng.com/blog/2013/03/cosine_similarity.html

- **Distributed representation**

  使用低纬向量表示词,同时又能表示相近词之间的语义关联

  word2vec:

  - CBOW
  - skipgram

  ![img](http://cdn1.infoqstatic.com/statics_s2_20171024-0600/resource/articles/machine-learning-automatic-classification-of-text-data/zh/resources/11.png)

素材文档(http://www.infoq.com/cn/articles/machine-learning-automatic-classification-of-text-data)

### 语义匹配

- 问题和由标注集合构成的文档的匹配

  - TF-IDF模型

    思想:如果一个词w在文章a 中出现的高,在其他文章中出现的少,认为词w有很好的区分能力

    - TF:词w在文章d中的词频tf(Term Frequency),词在d中出现的次数count(w,d),与总词数的比值
      $$
      tf(w,d) = \frac {count(w,d)}{size(d)}
      $$

    - IDF:词在整个文档中出现的逆向文档频率idf(inverse Document Frequency),即文档总数与词w所出现的文件数docs(w,D)比值的对数

      ​
      $$
      idf = log(\frac{n}{docs(w,D)})
      $$
      最终TF-IDF根据tf和idf为每个文档d和由关键词组成的查询串q计算一个权值表示查询串q和文档d的匹配程度

      ​TF-IDF = TF *IDF

      baseline : http://www.ruanyifeng.com/blog/2013/03/cosine_similarity.html

      > 我们想要的是一系列query 只对应一个文档,对于监督学习的训练数据来书,想要的是(mathch,0or1)

      结合vsm 做相似度  (Vector Space Model)

- 问题和句子做文本相似度学习

  - 分词:使用jieba,删去标点,停用词
  - word embedding ,:使用中文维基百科训练好的word2vec,给每个分词的一个向量表示
  - 相似度计算:用gensim 的 n_similar 计算句子之间的相似度,通过对每个词的向量加和取平均,进而得到句子的相似度,通过求解回答和问题的相似度
  - 以(相似度,类别)作为输入数据,构造逻辑斯地回归,构造得到一个二分类的分类器

  > 缺陷:
  >
  > 1. 在表示句子的时候,没有考虑词之前的顺序,进而无法得到词与词之间的语义联系
  > 2. 没有考虑到想"我不知道"这种表达,也是与问题相关
  > 3. 对于长句子,不相关的长句子也会判定其为相关,猜测这种句子的表达形式对长句子有优势

  模型改进:

  1. 针对长句子这一问题,我选择对句子提取关键词作为句子的表示,例如最多提取5个关键词,再做相似度计算
  2. 针对有些表达与问题不一定相关,对于通用的表达,我们可以设置一些常用表达库,例如我们想知道那些同学是掌握的不好的,我们获取掌握不好的,作为新的一个类别,针对这个类别,构造一个三分类器
  3. 问题和答案的相似度是在一定程度体现相关性,但是考虑和提供的答案做
  4. 做泛化实验,当

  其中回头想句子的表示:

  1. 使用句子中每个词向量的平均
  2. 使用word2vec 向量使用tf-idf score

- fastText

- CNN

- Topic model

### 模型评价

交叉验证(cross validation)

- 评估方法

  理想方案是对候选模型的泛化误差进行评估

  1. 留出发:将样本直接划分为训练集和测试集,通常选择样本的2/3 ~ 4/5 作为训练集
  2. 交叉验证:将样本集合D分层划分为k 个集合,每次选择k-1个子集的并集 作为训练集合,剩下一个作为验证集,这样得到k 组训练集+验证集,最终结果取k 组的均值,这种称为 **k折交叉验证**

- 性能度量

1. **准确率** **(Aaccuracy)**: 分类器对整个样本的判定能力,即将正的判定为正，负的判定为负: A = (TP + TN)/ (TP + FN + FP + TN)

   > a、真正(True Positive , **TP**)：被模型预测为正的正样本
   >
   > b、假正(False Positive , **FP**)：被模型预测为正的负样本
   >
   > c、假负(False Negative , **FN**)：被模型预测为负的正样本
   >
   > d、真负(True Negative , **TN**)：被模型预测为负的负样本

   | 真实情况 | 预测结果 |      |
   | :--: | :--: | :--: |
   |      |  正例  |  反例  |
   |  正例  |  TP  |  FP  |
   |  反例  |  FN  |  TN  |

   ​

2. 查全率:R = TP/(TP+FN)

3. 查准率:P = TP/(TP+FP)



实验结果:

|                  | TFidf+word2vec | mean+word2vec  | onehot        |
| ---------------- | -------------- | -------------- | ------------- |
| has stopword     | 0.777142857143 | 0.785714285714 |               |
| without stopword | 0.771946564885 | 0.781488549618 | 0.68893129771 |

>  对tfidf 准确率下降的原因,其实在计算tfidf时的预料来自训练数据,这时出现一个问题,就是训练数据中大多是这个问题下的对应回复正确的标记值,这样使用tfidf时真正的主题词例如 '研究' '网络' 的tfidf 的数值就很低,这样造成加权的效果不好
>
>  ont hot 有大量 0  的结果

> 资料
>
> [模型评估选择--来自西瓜书](http://bealin.github.io/2017/02/27/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%B3%BB%E5%88%97%E2%80%942-%E6%A8%A1%E5%9E%8B%E8%AF%84%E4%BC%B0%E4%B8%8E%E9%80%89%E6%8B%A9/)

### 模型优化



http://blog.csdn.net/busycai/article/details/6159109





参考文献

1. Short Text Similarity with Word Embeddings 