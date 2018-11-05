# Markdown

### Markdown for Math

加粗符号使用`\boldsymbol`,例如$\beta$ 和$\boldsymbol\beta$

加颜色： 

$\color{green}{Hello World!}$

$\color{red}{Hello World!}$

$\color{fuchsia}{Hello World!}$

$P(a \bigvee b) = P(a)+P(b)-P(a\bigwedge b)$

### 公式

公式标号

~~~markdown
$$
\begin{eqnarray*}
x^n+y^n &=& z^n \tag{1.1} \\
x+y &=& z \tag{1.2}
\end{eqnarray*}
$$
~~~

$$
\begin{eqnarray*}
x^n+y^n &=& z^n \tag{1.1} \\
x+y &=& z \tag{1.2}
\end{eqnarray*}
$$





### 数学

### 希腊字母：

| 字母   | 语法       | 语法         |
| ---- | -------- | ---------- |
| A    | A        | $\alpha$   |
| B    | B        | $\beta$    |
| Γ    | \Gamma   | $\gamma$   |
| Δ    | \Delta   | $\delta$   |
| E    | E        | $\epsilon$ |
| Z    | Z        | $\zeta$    |
| H    | H        | $\eta$     |
| Θ    | \Theta   | $\theta$   |
| I    | I        | $\iota$    |
| K    | K        | $\kappa$   |
| Λ    | \Lambda  | $\lambda$  |
| M    | M        | $\mu$      |
| N    | N        | $\nu$      |
| Ξ    | \Xi      | $\xi$      |
| O    | O        | $\omicron$ |
| Π    | \Pi      | $\pi$      |
| P    | P        | $\rho$     |
| Σ    | \Sigma   | $\sigma$   |
| T    | T        | $\tau$     |
| Υ    | \Upsilon | $\upsilon$ |
| Φ    | \Phi     | $\phi$     |
| X    | X        | $\chi$     |
| Ψ    | \Psi     | $\psi$     |
| Ω    | Omega    | $\omega$   |

### 数学符号：

| 运算符               | 说明       | 代码                                       |
| ----------------- | -------- | ---------------------------------------- |
| +                 | 加        | $ x + y $                                |
| -                 | 减        | $ x - y $                                |
| \times            | 乘        | $ x \times y $                           |
| \cdot             | 乘        | $ x \cdot y $                            |
| \ast              | 乘        | $ x \ast y $                             |
| \div              | 除        | $ x \div y $                             |
| \frac             | 分数       | $ \frac{x}{y} $                          |
| ^                 | 上标       | $ x ^ y $                                |
| _                 | 下标       | $ x _ y $                                |
| \sqrt             | 开二次方     | $ \sqrt x $                              |
| \sqrt             | 开方       | $ \sqrt[x]{y^4+3y-1} $                   |
| \lceil 和 \rceil   | 上取整      | $ \lceil\frac12\rceil $                  |
| \lfloor 和 \rfloor | 下取整      | $ \lfloor\frac12\rfloor $                |
| \pm               | 加减       | $ x \pm y $                              |
| \mp               | 减加       | $ x \mp y $                              |
| =                 | 等于       | $ x = y $                                |
| \leq              | 小于等于     | $ x \leq y $                             |
| \geq              | 大于等于     | $ x \geq y $                             |
| \ngeq             | 不大于等于    | $ x \ngeq y $                            |
| \not\geq          | 不大于等于    | $ x \not\geq y $                         |
| \neq              | 不等于      | $ x \neq y $                             |
| \approx           | 约等于      | $ x \approx y $                          |
| \equiv            | 恒等于      | $ x \equiv y $                           |
| \bigodot          | 定义运算符    | $ x \bigodot y=x+y^2 $                   |
| \bigotimes        | 定义运算符    | $ x \bigotimes y=x+y^2 $                 |
| \in               | 属于       | $ x \in y $                              |
| \notin            | 不属于      | $ x \notin y $                           |
| \subset           | 子集       | $ x \subset y $                          |
| \not\subset       | 非子集      | $ x \not\subset y $                      |
| \subseteq         | 子集       | $ x \subseteq y $                        |
| \supset           | 超集       | $ x \supset y $                          |
| \supseteq         | 超集       | $ x \supseteq y $                        |
| \cup              | 并        | $ x \cup y $                             |
| \cap              | 交        | $ x \cap y $                             |
| \log              | 对数       | $ \log(x) $                              |
| \overline         | 平均数      | $ \overline{x} $                         |
| \overline         | 连线符号     | $ \overline{a+b+c+d} $                   |
| \underline        | 下划线      | $ \underline{a+b+c+d} $                  |
| \overbrace        | 上大括号     | $\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}$ |
| \underbrace       | 下大括号     | $ \underbrace{a+d}_3 $                   |
| \partial          | 部分       | $ \frac{\partial x}{\partial y} $        |
| \lim              | 极限       | $ \lim_{x\to\infty} $                    |
| \displaystyle     | 块公式格式    | $ \displaystyle \lim_{x\to\infty} $      |
| \sum              | 求和       | $ \sum_{i=1}^n $                         |
| \infty            | 极限       | $ \sum_{i=0}^\infty i^2 $                |
| \int              | 积分       | $ \int_0^1 x^2 {\rm d}x $                |
| \iint             | 二重积分     | $ \iint_D f(x,y)d\sigma $                |
| \oint             | 曲面积分     | $ \oint e^{x+y} ds $                     |
| \ldots            | 底端对齐的省略号 | $ 1,2,\ldots,n $                         |
| \cdots            | 中线对齐的省略号 | $ x_1^2 + x_2^2 + \cdots + x_n^2 $       |
| \uparrow          | 上箭头      | $ \uparrow $                             |
| \Uparrow          | 上箭头      | $ \Uparrow $                             |
| \vec              | 向量       | $ \vec{a} $                              |
| \hat              | 拟合值      | $\hat Y = \hat \beta_0 + \hat \beta_1X $ |
| \bot              | 垂直       | $ A \bot B $                             |
| \circ             | 度        | $ 45^\circ $                             |
|                   | 非        |                                          |

### Operators

| Symbol | Script |
| ------ | ------ |
| coscos | \cos   |
| sinsin | \sin   |
| limlim | \lim   |
| expexp | \exp   |
| →→     | \to    |
| ∞∞     | \infty |
| ≡≡     | \equiv |
| modmod | \bmod  |
| ××     | \times |

### Power and Indices

| Symbol   | Script    |
| -------- | --------- |
| kn+1kn+1 | $k_{n+1}$ |
| n2n2     | $n^2$     |
| k2nkn2   | $k_n^2$   |

### Fractions and Binomials

| Symbol               | Script                    |
| -------------------- | ------------------------- |
| n!k!(n−k)!n!k!(n−k)! | \frac{n!}{k!(n-k)!}       |
| (nk)(nk)             | \binom{n}{k}              |
| x1x−yx1x−y           | \frac{\frac{x}{1}}{x - y} |
| 3/73/7               | ^3/_7                     |

### Roots

| Symbol | Script      |
| ------ | ----------- |
| k√k    | \sqrt{k}    |
| k√nkn  | \sqrt[n]{k} |

### Sums and Integrals

| Symbol                               | Script                                   |
| ------------------------------------ | ---------------------------------------- |
| ∑10i=1ti∑i=110ti                     | \sum_{i=1}^{10} t_i                      |
| ∫∞0e−xdx∫0∞e−xdx                     | \int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x |
| ∑∑                                   | \sum                                     |
| ∏∏                                   | \prod                                    |
| ∐∐                                   | \coprod                                  |
| ⨁⨁                                   | \bigoplus                                |
| ⨂⨂                                   | \bigotimes                               |
| ⨀⨀                                   | \bigodot                                 |
| ⋃⋃                                   | \bigcup                                  |
| ⋂⋂                                   | \bigcap                                  |
| ⨄⨄                                   | \biguplus                                |
| ⨆⨆                                   | \bigsqcup                                |
| ⋁⋁                                   | \bigvee                                  |
| ⋀⋀                                   | \bigwedge                                |
| ∫∫                                   | \int                                     |
| ∮∮                                   | \oint                                    |
| ∬∬                                   | \iint                                    |
| ∭∭                                   | \iiint                                   |
| ∫⋯∫∫⋯∫                               | \idotsint                                |
| ∑0<i<m\0<j<nP(i,j)∑0<i<m\0<j<nP(i,j) | \sum_{\substack{0<i<m\0<j<n}} P(i, j)    |
| ∫ab∫ab                               | \int\limits_a^b                          |

| Symbol           | Script               |
| ---------------- | -------------------- |
| a′a′a′a′         | a` a^{\prime}        |
| a″a″             | a’’                  |
| â a^            | hat{a}               |
| a¯a¯             | \bar{a}              |
| à a`            | \grave{a}            |
| á a´            | \acute{a}            |
| $\dot{a}         | \dot{a}              |
| a¨a¨             | \ddot{a}             |
| ⧸a⧸a             | \not{a}              |
| a˚a˚             | \mathring{a}         |
| AB−→AB→          | \overrightarrow{AB}  |
| AB←−AB←          | \overleftarrow{AB}   |
| a‴a‴             | a’’’                 |
| aaa⎯⎯⎯⎯⎯⎯⎯⎯⎯aaa¯ | \overline{aaa}       |
| aˇaˇ             | \check{a}            |
| a⃗ a→            | \vec{a}              |
| a⎯⎯a_            | \underline{a}        |
| xx               | \color{red}x         |
| ±±               | \pm                  |
| ∓∓               | \mp                  |
| ∫ydx∫ydx         | \int y \mathrm{d}x   |
|                  | \,                   |
|                  | \:                   |
|                  | \;                   |
| !!               | !                    |
| ∫ydx∫ydx         | \int y\, \mathrm{d}x |
| ……               | \dots                |
| ……               | \ldots               |
| ⋯⋯               | \cdots               |
| ⋮⋮               | \vdots               |
| ⋱⋱               | \ddots               |

### 页面跳转

1. 先定义一个标识（即下面这行代码），并把它放到要跳转的地方。
   <div id="footer">123</div>
2. 然后把跳转链接放在任意地方，当点击这个跳转链接的时候变回跳到当前标识所在的位置。
   [跳转到数学公式](#footer)
   例如我把标识代码放在数学公式的上一行，当点击[跳转到数学公式](http://blog.csdn.net/bone_ace/article/details/46400975#footer)的时候便会实现跳转。