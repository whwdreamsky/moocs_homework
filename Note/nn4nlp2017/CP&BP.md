

### 计算图(compute graphs)

计算图是一个有向无环(Directed Acyclic Graph)的计算过程,其中图中有边(edge),和节点(node),其中边代表一个函数声明(*argument*),或者是数据依赖关系(*data dependency*),出发点,边以及到达点,构成了一个以出发点为参数的函数方程

> from nn4nlp 
>
> 节点代表数学运算,或是变量约束,边代表中间结果(上一个节点的输出)和

用处: 其实神经网络就是一个计算图,前向计算用于表示模型进而可以预测,反向计算是用于训练,通过对训练数据进行训练修改每个节点方程的参数的值

### backpropagation algorithm

算法:核心就是找到权值对最后损失函数的影响程度,本质是通过正向的结果反向校正权重的值,最终是损失最小化

1. chain-rule differentiation:链式微分,通过计算最后的损失E,不断的求解偏导,获得E 关于 w 的偏导值
2. 然后对权重w 的 值进行更新,$w_{ij}' = w{ij}-\alpha\frac{\partial E}{w_{ij}}$

> 对于之前在感知机的时候,通过先计算 y 对w,b的偏导,当预测不对是通过偏导对w,b 进行修改,这个求偏导的股过程是人工的,其实也是反向传播算法,但是对于复杂的运算,人就无法很快的得到最终的y 和权重之间的关系,需要借助BP  快速求偏导

[a example for bp](https://mattmazur.com/2015/03/17/a-step-by-step-backpropagation-example/) 

[LeCun](yann.lecun.com/exdb/publis/pdf/lecun-98b.pdf) 论文比较详细介绍了BP

#### 使用 DyNet 构建计算图

1. 什么是softmax
2. 几个函数的用法`dy.AdamTrainer`,`add_lookup_parameters`

scale 

vetor 

tensor

