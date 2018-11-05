### cnn for classification

####预处理

> 主要调用 keras.preprocessing 

1. 把句子转化为Dense 的 BOW 向量

   ~~~python
   text1='some thing to eat'
   text2='some thing to drink'
   texts=[text1,text2]
   # 把句子转换为向量
   tokenizer = Tokenizer(num_words=10,split=" ")
   #根据texts,创建词典
   tk.fit_on_texts(texts)
   #注意这里传入的必须是二维的list，也就是句子列表[[]]
   s = tk.texts_to_sequences(texts)

   ~~~

2. padding 把密集的词向量补零

```python
from keras.preprocessing import sequence
# 把sequence 转换为固定维度的向量 用于后面的Embedding 层
s_padded= sequence.pad_sequences(s,maxlen=100)
```

3. 获取句子每个词的词向量，Embbeding

~~~python
#这里获取每个句子的embbeding 可以加入一个embbeding 层，也可以使用预先训练好的词向量
# 这里input_dim 也就是词表大小，output_dim 也就是词向量大小，input_length也就是输入的长度
model.add(Embedding(input_dim,output_dim,input_length=100))
~~~



### 神经网构建

~~~python
###这里只设置了一个卷积
model.add(Conv1D(filters=10,padding='valid',activation='relu',kernel_size=3,strides=1,input_shape=(100,20)))
model.add(MaxPooling1D(pool_size=2))
model.add(Flatten())
model.add(Dropout(0.2))

###接一个全连接神经网络
model.add(Dense(50,activation="relu"))
model.add(Dense(1,activation="sigmoid"))
model.compile(loss="binary_crossentropy", optimizer="adam", metrics=["accuracy"])
model.fit(X_train, y_train, batch_size=64, epochs=5,
          validation_data=(X_test, y_test), verbose=2
~~~





### 模型使用

#### 模型训练完成后预测

~~~python
model.predict(np.array(tk.texts_to_sequences(text)))


~~~





#### 模型存储

model.save("123.h5")





### 模型调参

A Sensitivity Analysis of (and Practitioners’ Guide to) Convolutional
Neural Networks for Sentence Classification

### 代码学习

[代码学习](https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras/blob/master/sentiment_cnn.py)

