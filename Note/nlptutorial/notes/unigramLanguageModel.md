
# Unigram Language Models
###probabilistic language Models

**concept**: language models assign a probability to each sentence
$$
P(W)=\prod_{i=1}^{|w|+1} P(w_i |w_0 \dots w_{i-1} )
$$
这里W 表示某个句子的概率,例如speech recognition system,这里wi 表示整个句子从左到右每个片段的概率*,从i =1 开始原因是假定起始终止分别为< s > < /s > 
how to caculate ?

> 在Graham Neubig 的教程中,应用的例子是口语识别系统,例如说了"speech recognition system",可能但从语音上有一些候选项目,例如,speech cognition system,这时就要通过判断哪个句子更有可能,下面从概率的角度判断

####maximum likelihood estimation(极大似然估计)
**concept** : 通过对训练集估计统计模型参数,这里是通过对训练集中出现次数估计出现的概率,即用有限的 观察数据估计概率,当然概率不一定准确特别是测试集少的时候,下面是[极大似然官方解释](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation)
>maximum likelihood estimation (MLE) is a method of estimating the parameters of a statistical model given observations, by finding the parameter values that maximize the likelihood of making the observations given the parameters. MLE can be seen as a special case of the maximum a posteriori estimation (MAP) that assumes a uniform prior distribution of the parameters, or as a variant of the MAP that ignores the prior and which therefore is unregularized.	

$$
P(w_i|w_1\dots w_{i-1}) = \frac{c(w_1\dots w_{i})}{c(w_1\dots w_{i-1})}
$$

###Unigram Model

**concept**:使用词在整个出现词中的频率对词的概率进行估计$$P(w_i|w_1\dots w_{i-1})$$

$$
P(w_i|w_1\dots w_{i-1}) \approx P(w_i) = \frac{c(w_i)}{\sum_wc(w)}
$$

>那不是失去了句子的关联性,难道这就是词袋模型 

###Unkown word problem

**problem** : 当训练集较小,当出现新的句子时尽管词都出现过但是按照极大似然估计,概率为0,因此引入Unigram Language Model

解决: 加入**smooth**,给unknown word 加入概率,$$\lambda_i$$的值自己取,其中$$1-\lambda_1$$为unknown word 的概率.N表示词空间
$$
P(wi)=\lambda_1P_{ML}(w_i)+(1-\lambda_1)\frac {1}{N}
$$

- 加一平滑法
  $$
  P(w_i|w_1,w_2,\dots w_{i-1})=\frac {c(w_1 \dots w_i)+1}{c(w_1\dots c_w{i-1})+N}
  $$

- Good-Turing

  ​

###Evaluating Models

- likelihood

  使用chain rule 的概率估计,$$W_{test}$$表示测试集数据,M 表示unigram 模型,P(w|M)即每个词的概率

  $$
  P(W{test}|M) = \prod _{w\subseteq W{test}}P(w|W)
  $$

  > 如何用这个概率值评价模型呢,概率大了代表什么,概率小了代表什么?

- log likelihood

  因为直接使用概率的值作为评价标准,值通常十分小,接近于0,计算机难以存储(underflow),因此使用log 值更直观表示数据
  $$
  P(W{test}|M) = \prod _{w\subseteq W{test}}P(w|W)
  $$

- 熵（Entropy）

  > average negative log 2 likelihood per word,使用这里其实求解每个词的log估计的平均值,这时熵值的定义,这个熵为什么能表示模型的好坏呢

  $$
  H(W_{test}|M)=\frac {1}{|W_{test}|}\sum_{w|M}-log_2P(w|M)
  $$

  ​

  ​

