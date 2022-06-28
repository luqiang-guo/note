# 量化







## 量化范围

###  Min-Max 

这不会导致截断误差。但是这种方法对异常值很敏感，因为强异常值可能会导致过多的舍入误差(强异常值会使得量化步长偏大)。

### **Mean squared error**

均方误差(MSE)，缓解强离群值问题的一个方法是使用基于MSE的范围设置。在这种范围设置方法中，我们找到qmin和qmax，使原始张量和量化张量之间的MSE最小化。

![image-20220618160432322](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220618160432322.png)

### **Cross entropy**

在分类的最后一层效果好

### **BN based range setting** 

没看懂这个操作意义？





## Q&A

### 摘要
- 1 QAT需要微调和带标签的数据，但是可以在更低位宽时取得更有竞争力的结果， 低带宽时为什么能去的更好的结果？
- 矩阵乘法的消耗则降低为原来的1/16， 为什么是1/16？
- 神经网络已经被证明对量化有着比较好的鲁棒性， 怎么证明的？
### 基础知识
- 矩阵乘法实现，计算流程是什么，（内积？）  需要多少个时钟，溢出操作怎么处理？ ![img](https://pic4.zhimg.com/80/v2-b94f85e47c487bff11948e26018cadbb_1440w.jpg)

- Sw 和 Sx 每一层都一样吗
- <font color=red>目前大多数的定点累加器都不支持这类操作，因此本文中不会考虑这种量化方案</font>， 没看懂？
- <font color=red>将线性层的结果写回内存然后又加载到非线性层进行计算，这个操作是很浪费的。</font> why？
- <font color=red>test</font>
- <font color=red>**逐点相加(Element-wise addition)**</font>  这里类似Q格式， 两个Q格式相加要先统一到同样的量化范围(Q15 + Q18 -> Q15 << 3  + Q18 = Q18)
- <font color=red>反量化</font>， 举一个例子解释，解释其目的
- <font color=red>激活的按通道量化则很难实现</font>
- <font color=red>test</font>
- <font color=red>test</font>
- 