# BaseLine for SemEval 2013

### data

### 总体思路

1. 处理XML 格式化数据

   数据格式如下

   ~~~XML
   <question qtype="Q_EXPLAIN_SPECIFIC" id="BULB_C_VOLTAGE_EXPLAIN_W    HY1" module="FaultFinding" stype="EVALUATE">
      <questionText>Explain why you got a voltage reading of 1.5 for     terminal 1 and the positive terminal.</questionText>
      <referenceAnswers>
      <referenceAnswer category="BEST" id="answer204" fileID="BULB_    C_VOLTAGE_EXPLAIN_WHY1_ANS1">Terminal 1 and the positive terminal     are separated by the gap</referenceAnswer>
      <referenceAnswer category="BEST" id="answer205" fileID="BULB_    C_VOLTAGE_EXPLAIN_WHY1_ANS2">Terminal 1 and the positive terminal     are not connected</referenceAnswer>
      </referenceAnswers>
     <studentAnswers>
     <studentAnswer count="1" answerMatch="answer204" id="FaultFin    ding-BULB_C_VOLTAGE_EXPLAIN_WHY1.sbj3-l1.qa193" accuracy="correct    ">positive battery terminal is separated by a gap from terminal 1    </studentAnswer>
     </studentAnswers>
   </question>
   ~~~

   ==特征选择==：

   定义 ： 学生回答为SR，标准回答为RR，问题为QE

   相似度四个特征：overlap，cosine ，F1 score，Lesk 都是来自 perl 的Text::Similarity::Overlaps module 包，分别对 SR&RR，和SR&&QE 取两组特征，这就有8个特征

   在特征都取目标和

   1. overlap_SR_RR ：这里取学生回答和标准回答的词重叠的最大值

   2.  overlap_SR_QE ：这里取学生回答和问题的词重叠

   3. cosine 学生回答和标准答案的cosine 角

   4.  F1 score

      *F-measure, which is a normalized value between 0 and 1. Normalization can be turned off by specifying --no-normalize, in which case the raw_score is returned, which is simply the number of words that overlap between the two strings.*

      ```
      F-measure = 2 * precision * recall / (precision + recall)
      ```

   5.  Lesk score (Lesk 算法)

      ```
      Lesk = sum of the squares of the length of phrasal matches  
      ```

   6. 还可以想到的特征

      - 编辑距离，莱文斯坦距离
      - http://wetest.qq.com/lab/view/276.html

### Tools

- the Text::Similarity::Overlaps module

  counting the number of overlapping words and phrases between the two files, and is (optionally) normalized by the length of the files. Phrasal matches are scored more highly

  用于计算相似度

- Lingua-Stem

  用于提取词干

- Lesk score





### 步骤

5-way

1. 从结构化数据中提取,并计算 overlap , F(这个不太清楚),lesk,cosine,questionOverlap , questionF,questionLek,questionCosine,`./extractBaselineFeatures.sh`,脚本抽取特征，

   > 这里使用的是词重叠算法

   2. 使用决策树作为分类器，学习之前提取的特征,并保存之前的模型，使用java 的weka 包 		`BaselineClassifier.java`,存下模型

2. 使用学习的模型对之前的所有数据进行预测，获得了模型输出

   > 这里貌似是用所有之前的数据，这样来做测试应该吗，不是应该用新的数据，这样怎么扩展这个模型呢
   >
   > ==疑问==

3. 通过`./extractEvaluationTable.sh` 处理得到标注gold数据，`evaluate.sh` 运行脚本对之前模型输出和标注数据计算指标

   > 这里micro,macro,不清楚具体意思，==明天看下==

验证 ： use 10-fold cross-validation

还要给出类别分布

macro-averaged recall：这里C 代表类别，这里平均了所有类别的recall 值
$$
R_{macro}=\frac{1}{|C|}\sum_{c\in C} R(c)
$$
micro-averaged recall ： Nc 表示类别c 中的元素个数，考虑了类别的大小
$$
R_{micro}=\sum_{c\in C}\frac {1}{N_c}R(c)
$$


### 实验结果

#### 5-way

|                              | precision | recall    | fmeasure  |
| ---------------------------- | --------- | --------- | --------- |
| correct                      | 0.7479115 | 0.9064920 | 0.8196015 |
| partially_correct_incomplete | 0.6806527 | 0.6347826 | 0.6569179 |
| contradictory                | 0.7636849 | 0.5521236 |           |
| irrelevant                   | 0.6744186 | 0.2612613 |           |
| non_domain                   | 0.6601562 | 0.8666667 |           |
|                              |           |           |           |
|                              |           |           |           |

correct                      0.7479115 0.9064920 0.8196015

partially_correct_incomplete 0.6806527 0.6347826 0.6569179

contradictory                0.7636849 0.5521236 0.6408964
irrelevant                   0.6744186 0.2612613 0.3766234
non_domain                   0.6601562 0.8666667 0.7494457
macroaverage                 0.7053648 0.6442652 0.6486970
microaverage                 0.7299448 0.7297640 0.7186986