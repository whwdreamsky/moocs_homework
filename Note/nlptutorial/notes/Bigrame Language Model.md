# Bigram to ngram Language Model

unigram ignore the context,这样就会出现句子的概率由词的概率决定导致不合乎语法规则的句子,因此要考虑**context**,根据考虑之前词个数的多少,我们得到Bigram , Trigram , 以及n-gram

$$P(w_i|w_1\dots w_{i-1} \approx P(w_i) = \frac{c(w_i)}{\sum_wc(w)}$$

**However,Bigrame add one word of context**

$$P(w_i|w_0\dots w_{i-1})\approx P(w_i|w_{i-1})$$

**Trigrame add two word of context**

$$P(w_i|w_0\dots w_{i-1})\approx P(w_i|w_{i-2}w_{i-1})$$

### Maximum Likelihood Estimation of N-gram

### 稀疏问题

$$\lambda$$ 的选择方法

- grid search:平均取,0.1,0.2....0.9
- context dependent smoothing : 解决


