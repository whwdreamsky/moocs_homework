# Environment for DeepLearning

### Conda

> conda 是python的包管理器

#### install

mniconda

anaconda

- 环境变量设置

  ~~~shell
  vim ~/.brash
  ~~~

  ​

- 更改更新源 

  - 更新conda 源

  - 更新pip 源

    ~~~
    linux下，修改 ~/.pip/pip.conf (没有就创建一个)， 修改 index-url至tuna，内容如下：
     [global]
     index-url = https://pypi.tuna.tsinghua.edu.cn/simple

    ~~~

    ​

#### 命令

conda create -n myenv python=3.6

conda -info

####加入conda 到系统路径

export PATH=/home/oliver/anaconda2/bin:$PATH

### Tensorflow

> 谷歌神经网络基础包

```
pip install --upgrade tensorflow
```

####install

### Keras 

> 神经网络模型包，是TensorFlow 上层包

安装很简单，`sudo pip install keras`，注意之前要安装TensorFlow

### 参考

[博客](http://inmachineswetrust.com/posts/deep-learning-setup/)