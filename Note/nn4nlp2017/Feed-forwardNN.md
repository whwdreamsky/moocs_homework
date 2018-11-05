# Feed-forward Neural Network

cross-entropy(or log loss)

###sigmoid vs softmax

*The [sigmoid function](https://en.wikipedia.org/wiki/Logistic_function) is used for the two-class logistic regression, whereas the [softmax function](https://en.wikipedia.org/wiki/Softmax_function) is used for the multiclass logistic regression (a.k.a. MaxEnt, multinomial logistic regression, softmax Regression, Maximum Entropy Classifier).*

sigmoid
$$
P(y=1)=\frac {1}{1+e^{-\theta ^T x}}\\
P(y=0)=\frac {e^{-\theta^Tx}}{1+e^{-\theta ^T x}}
$$
softmax 公式


$$
y_c=\zeta(\textbf{z})_c = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} \text{  for } c=1, \dots, C
$$

>  其中C 表示有C 种类别,Z 表示维度为C的输入,每个输入也就是对应每个类别的wx+b 的预测值,而y 为输出的维度为C 的每个类别的概率.对于softmax 每个类别的概率$\sum y[i]=1$ 
>
> [参考](http://shuokay.com/2016/07/20/softmax-loss/)

sigmoid 和softmax 的关系
$$
\begin{align}
\Pr(Y_i=0) &= \frac{e^{\boldsymbol\beta_0 \cdot \mathbf{X}_i}} {~\sum_{0 \leq c \leq K}^{}{e^{\boldsymbol\beta_c \cdot \mathbf{X}_i}}} = \frac{e^{\boldsymbol\beta_0 \cdot \mathbf{X}_i}}{e^{\boldsymbol\beta_0 \cdot \mathbf{X}_i} + e^{\boldsymbol\beta_1 \cdot \mathbf{X}_i}} = \frac{e^{(\boldsymbol\beta_0 - \boldsymbol\beta_1) \cdot \mathbf{X}_i}}{e^{(\boldsymbol\beta_0 - \boldsymbol\beta_1) \cdot \mathbf{X}_i} + 1}  = \frac{e^{-\boldsymbol\beta_ \cdot \mathbf{X}_i}} {1 +e^{-\boldsymbol\beta \cdot \mathbf{X}_i}} \\ \, \\
\Pr(Y_i=1) &= \frac{e^{\boldsymbol\beta_1 \cdot \mathbf{X}_i}} {~\sum_{0 \leq c \leq K}^{}{e^{\boldsymbol\beta_c \cdot \mathbf{X}_i}}} = \frac{e^{\boldsymbol\beta_1 \cdot \mathbf{X}_i}}{e^{\boldsymbol\beta_0 \cdot \mathbf{X}_i} + e^{\boldsymbol\beta_1 \cdot \mathbf{X}_i}} = \frac{1}{e^{(\boldsymbol\beta_0-\boldsymbol\beta_1) \cdot \mathbf{X}_i} + 1} = \frac{1} {1 +e^{-\boldsymbol\beta_ \cdot \mathbf{X}_i}}  \, \\
\end{align}
$$


> 这里$\boldsymbol\beta=-(\beta_0-\beta_1)$,但是这里的减操作没太理解,难道是之前使用二分类逻辑斯回归,输出的是一个节点,而如果用两个节点就要用softmax
>
> [参考资料](https://stats.stackexchange.com/questions/233658/softmax-vs-sigmoid-function-in-logistic-classifier)

