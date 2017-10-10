# Word Segmentation#

> Notes on Neubig's tutorial 4 word segmentation

###1. What is word segmentation###

对于中文分词,不像英文,中文没有空格作为分隔符,因此把一个句子切割为一个个词的过程称为分词

使用unicode 处理中文

分词可能有多个候选分词方式,如何选择哪一种是要解决的问题

- 使用unigram language modle 算概率,选择高概率的句子

  存在的问题需要一个长度为n的句子要计算的种数巨大,计算量太大

- viterbi algorithm

  ​



