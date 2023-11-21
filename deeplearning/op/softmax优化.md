# Softmax

##  一、公式原理

$$
Softmax(x_i)=\frac{exp(x_j)}{∑_j exp(x_j)}
$$







## 二、优化方向

优化算子实现的时候会从两个方形来思考： 

- 其一数值分析方向，例如DFT(离散傅里叶变换)它的计算复杂度是n2，根据其变换矩阵的特性（或者是蝶形变换方法）我们可以做递归的计算思路将DFT计算量从n2减小到nlogn，这就是我们通常所说的FFT（快速傅里叶变换）。

- 其二体系架构方向，例如采用SIMD(单指令多数据)或者SIMT(多指令多数据)，增加单位时间内运行的指令数。

### 数值方向优化

- 数值稳定

  在计算e^x 的时候很容易超出float32的表示范围而且在数值很大的时候float32的step会很大。超出表示范围很容易溢出得到错误结果，step很大会导致计算精度的下降。为了解决这个问题一般会对分子分母同时处以一个数值，保证数值正确和数值精度。公式如下：

  - 第一步：分子分母上下除以a
  - 第二步：将a整理到exp的参数中， 即b。
  - 第三步：选取合理的b值，一般都是选择这组数中最大值，保证exp的结果小于1。

  $$
  \begin{aligned}
  \operatorname{Softmax}_{\left(x_i\right)} & =\frac{\exp \left(x_i\right)}{\sum_j \exp \left(x_i\right)} \\
  & =\frac{\exp \left(x_i\right) / a .}{\sum_j \exp \left(x_i\right) / a} \\
  & =\frac{\exp \left(x_i\right) / a}{\Sigma_j \exp \left(x_i-a\right)} \\
  & =\frac{\exp \left(x_i-b\right)}{\sum_j \exp \left(x_i-b\right)} \\
  & =\frac{\exp \left(x_i-x_{\text {max }}\right)}{\sum_j \exp \left(x_i-x_{\text {max }}\right)}
  \end{aligned}
  $$

  

- 指数计算

  求解特殊函数的计算方式一般采用级数的方式，例如泰勒多项式，切比雪夫多项式，连分法。泰勒多项式是最简单的一种方法，缺点是收敛比较慢运算量相对比较大，切比雪夫多项式要求数值在（-1,1）之间， 连分法收敛速度最快，但是支持的函数较少。

  由于浮点数存储结构的特点Y*2^M有一种快速计算方法[A Fast, Compact Approximation of the Exponential Function](https://nic.schraudolph.org/bib2html/b2hd-Schraudolph99.html)，可以减少非常多的运算量。

- 

### 体系结构方向优化

- 

## 三、 benchmark

