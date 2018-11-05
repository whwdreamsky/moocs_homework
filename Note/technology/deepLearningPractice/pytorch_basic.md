# pytorch basic

### 环境配置

#### conda

~~~shell
#show env list
conda env list
#conda create env
conda create -n myenv
# create with package
conda create -n myenv python=3.4
#conda active env
source activate myenv
#conda deactive env
source deactivate
~~~

conda with jupyternote book , conda 提供了jupyter notebook 的拓展,只需要`conda install nb_conda`,就可以在notebook 使用conda 的环境

> [conda Document](https://conda.io/docs/user-guide/tasks/manage-environments.html#deactivating-an-environment)

####jupyter notebook

####Pycharm

### 构建神经网

流程:

1. 定义网络的结构,在nn 这包中有大量可用的模型
2. 定义损失函数loss
3. 清空grad , 反向传播backfoward 获得新grad
4. 更新net中的参数parameter

#### Minist example







###参考资料

> [中文文档](https://pytorch-cn.readthedocs.io/zh/latest/notes/autograd/)
>
> [英文文档](http://pytorch.org/docs/master/nn.html?highlight=relu)

