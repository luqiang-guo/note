# CodedVTR

Codebook-based Sparse Voxel TRansformer with Geometric Guidance

 [链接](https://a-suozhang.xyz/codedvtr.github.io/)



**还需要再看看录像总结总结**



## Background



- 卷积网络我们先天的给了他一些**归纳偏置**(卷积核的位置，卷积核的大小)

- Transformer 具有较小的归纳偏置，可以通过数据集来学习当前任务的**归纳偏置**。

- Transformer 需要很大的数据集，在较小的数据集上容易overfits；训练慢，对超参数敏感，网络参数初始化敏感。（写distribution normal算子的时候BUG导致 BERT训练  NAN）

  

### Self Attention 在图像上的应用

简单的理解为带有加权系nn网络，[详细链接](https://github.com/luqiang-guo/note/blob/master/deeplearning/self-attention.md)

![image-20220623223638816](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623223704944.png)

在大量的数据情况下self attention 是要好于CNN

![image-20220623230030280](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623230030280.png)

### 在3D上的应用

作者介绍在3D数据下引入 Transformer 会导致结果变差，作者在最后也解释到，可能是他的数据量少，他还没有在大规模数据及上做测试。（根据2D 图像的经验 self attention 在数据量少的时候确实效果不好，可以认为是学到的归纳偏置差与我们设定的CNN归纳偏置）

![image-20220623225846111](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623225846111.png)

## 作者提出的方案





###  新的 attention 机制

原来的 self attention 作者说是continuous space（没看懂），提出了一个新的方法codebook-based self-attention 

![image-20220623232729087](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623232729087.png)

codebook-based self-attention 介于一般卷积核 self-attention之间。

![image-20220623232941674](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623232941674.png)

### 通过attention机制 来选择不同的卷积核



根据输入的featuremap   来决定选择那种卷积核， 如下图3D点云图，墙角和平面的卷积核明显不一样。 (红色表示这种卷积的所在的空间位置) 这个可以认为是学出来**归纳偏置**。

![image-20220623233250302](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623233250302.png)

在无人驾驶的点云数据下

 可以通过输出的feature map决定采用的   Dilation的参数。



![image-20220623233536340](C:\Users\mu\AppData\Roaming\Typora\typora-user-images\image-20220623233536340.png)
