# BP Algorithm 反向传播算法总结

## 1.notations:
- $L$ 表示共有$L$个layers
- $S^{(l)}$表示第$l$个layer的神经元个数
- $\Theta_{ij}^{(l)}$表示从**第$l$层第$j$个神经元指向第$l+1$层第$i$个神经元**的参数
- $m$: example的个数。每个example可以表示为
  $(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\cdots (x^{(m)},y^{(m)}).$

## 2.正向传播简介
Steps:
  - 1.输入向量$\vec{x}$, $l=0$
  - 2.给定参数阵$\Theta^{(l)}$.
  - 3.计算$z^{(l)}$. 用的是 
    - 简单变量式 $z^{(l)}_{j}=\theta_{j1}^{(l-1)}+\displaystyle \sum^{s^{(l-1)}}_{t=1}\theta^{(l-1)}_{jt}\cdot a^{(l-1)}_t$.
    - 向量式
  - 4.计算$a^{(l)}$. 一般用$a^{(l)}=g(z^{(l)})$表示。在$K-Classification$ 问题里，$g(x)$一般取$Sigmoid Function$，即$g(x)=\frac{1}{1+\exp(-x)}$.
  - 5.判断$l$是否达到$L$.若达到$L$转$6$.否则 $l \gets l+1$, 转$2$.
  - 6.输出当前的$a^{(l)}$.



## 3.算法描述
这里研究的是$K-Classification$问题，即输出$\vec{y}$ 是一个$K$维向量。

利用定义的 $\delta$, $\Delta$，$D$ 可以帮助计算partial derivatives $\frac{\partial J(\Theta)}{\partial \Theta_{ij}}$, 从而利用梯度下降法得到目标  $min J(\Theta)$.

- 计算过程
  - 定义$\delta^{(l)}$:(此处为向量式)
    - When $l=L$, $\vec{\delta^{(l)}}=\frac{\partial J(\Theta)}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial z^{(L)}}= \cdots = \vec{a^{(L)}}-\vec{y};$
    - When $l \neq L$, $\vec{\delta^{(l)}}=\delta(l+1) \cdot \frac{\partial z^{(l+1)}}{\partial a^{(l)}} \cdot \frac{\partial a^{(l)}}{\partial z^{(l)}}= \cdots \delta^{(l+1)} \cdot [\Theta^{(l)}]^{T} \cdot a^{(l)} \cdot (1-a^{(l)}).$
    - $\delta^{(l)}$是从后往前递归定义的。
  - 定义$\Delta^{(l)}$:(此处为向量式)
    - Set $\Delta^{(l)}=0$
    - $\Delta^{(l)} \gets \Delta^{(l)}+\delta^{(l+1)} \cdot (a^{(l)})^{T}.$
  - 定义$D_{i,j}^{(l)}$:(此处为单变量表达式)
    - When $j=0$, $D_{i,j}^{(l)} \gets \frac{1}{m}\Delta_{i,j}^{(l)}.$
    - When $j \neq 0$, $D_{i,j}^{(l)} \gets \frac{1}{m}\Delta_{i,j}^{(l)}+\lambda \Theta_{i,j}^{(l)}.$
  - 数学上可以证明，$\frac{\partial}{\partial \Theta_{ij}^{(l)}}J(\Theta)=D_{ij}^{(l)}.$
- 说明
  
  链式法则计算中主要用到的式子是
    - $J(\Theta)=-y\log(a^{(l)})-(1-y)\log(1-a^{(l)}).$ 作为logistic Regression的主要代价函数存在
    - $\delta^{(l)}$的那几个式子
    - $z^{(l)}=[\Theta^{(l-1)}]^{T}a^{(l-1)}+\theta_0.$连接第$l-1$和第$l$个layer的线性组合式
    - $a^{(l)}=sigmoid(z^{(l)})$, where $sigmoid(x)=\frac{1}{1+\exp(-x)}.$
    - $D$和$\Delta$的几个式子。注意向量运算等。  

