# ASAG(Automaitc short answer grading)

- 五个特征

  - 需要外部知识库 (recalls external knowledge)
  - response in natural language 
  - length of response variable(可变)
  - focus on content
  - restricted with an objective quesion design

- ASAG 系统流程

  ![](/Users/oliver/Desktop/AI_photo/asag/asag.1.png)

- 四个主流的解决方向

  - concept mapping


  - information extraction

    这里比较关心,这里会用到自然语言处理技术,例如句法分析,句法依存关系抽取等

  - corpus-based methods

  - machine learning

  - initiative in evaluation

- 模型构建的六个模块

  - 数据获取

    - **Semeval 2013**: [The Joint Student Response Analysis and 8th Recognizing Textual Entailment Challenge](https://www.cs.york.ac.uk/semeval-2013/task7.html)

      分析==任务描述,重新定义问题==

      - goal :对学生关于问题的答案进行评分，有三种打分方式，5-way,2-way,3-way

        > The goal of the task is to __produce an assessment of student answers to explanation and definition questions__  typically asked in problems seen in practice 

      - exercises, tests or tutorial dialogue,__textual entailment for e-learning__

      - possible technology : text classification, latent semantic analysis and (语义相似度)other semantic similarity measurements, **textual entailment**（文本完整性）

        >  texual entailement(文本蕴含) : given two text fragments called 'Text' (T) and 'Hypothesis' (H), it is said that T entails H (t ⇒ h) if, typically, a human reading T would infer that H is most likely true (Dagan et al., 2006)
        >
        >  for example:
        >
        >  text: *If you help the needy, God will reward you*.
        >
        >  hypothesis: *Giving money to a poor man has good consequences*.
        >
        >  意思是从文本A中推断H的假设的正确性，有点像阅读理解的感觉

      - textual entailment vs ASAG

        联系：其实对回答评分，一方面也就是找到答案和问题的隐含关系

      - Main Task : **Given a question, a known correct "reference answer" and a 1- or 2-sentence "student answer", the main task consists of assessing the correctness of a student’s answer at different levels of granularity**

        > 跟之前思路差不多，就是给定一个问题，几个正确答案，以及学生答案，给出评级的得分

        等级划分：分为5层，3层，2层

      - Baseline 做法,来自文章 *Towards Effective Tutorial Feedback for Explanation Questions:A Dataset and Baselines*

        - 5层: non_domain,correct,partially_correct_incomplete,contradictory,irrelevant
        - 数据集:SCIENTSBANK data 和 BEETLE II data
        - ==baseline== **lexical similarity**: 选择了4个特征,count of overlapping words,F1,Lesk,cosine scores, 使用决策树做分类器

      - ​

        ​

  - 自然语言处理

  - 建立模型

    这个步骤是将之前自然语言处理过的数据通过**带有知识数据的**模型输出信息.

  - 评分模型

  - 模型评价

  - 效果分析(Effectiveness)

    ​

- ASAG 以往的解决方案

  - text similarity and paraphrasing
    - 老师和学生答案相似度匹配

- 技术点

  ==语义相似度计算==

  数据集：Stanford Natural Language Inference Corpus (SNLI)

  FastText 

  Deep Fusion LSTMs for Text Semantic Matching

- 权威数据集

  1. Semval 13 的数据集
  2. Stanford 

### 参考文献

[1]. The Eras and Trends of Automatic Short Answer Grading

[2]. [维基百科](https://en.wikipedia.org/wiki/Textual_entailment#cite_note-7)

​	