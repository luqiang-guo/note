# Self-attention



## 背景

- 输出输出的sequence 相同或者不同， 例如N->N, N->1, N->N`。
- 输入序列很长，例如语音识别MFCC 序列长度不固定(相同的词)。
- Graph 应用

![image-20220605220759669](https://user-images.githubusercontent.com/9412577/172058123-0a306507-90cb-4275-9cd5-6a6a0bbc0a23.png)



##  Self-attention



## Attention 个人小的理解：

- 没有Attention的时候a1, a2, a3, a4, 得到b1, b2, b3, b4（attention的输入输出，见下图）可以理解为全链接层，权重是Wv矩阵， B = Wv  A。

-  可以有类似 Dropout想法。O1（第一列向量）的结果不一定都来自于A，可以有一个权重系数α来决定占比。α 通过回归训练得到，例如可以将A1 A2 拼接到一起接一个Fc层 得到 结果α。两两拼接的方式不够优雅，而且参数还多。改进下 可以让A1经过线性变换得到向量v1，A2经过线性变换得到向量v2， v1和v2 做内积得到α12。 这里提到的线性变化理解为投影到主成分。
- 其实就是一个特殊的FC层



### **1** 计算两个输入的相关性

![image-20220605222420773](https://user-images.githubusercontent.com/9412577/172058239-a981262a-5d31-49bc-8aab-dc3d952e3bf7.png)



### 2 计算输入之间的相关性

![image-20220605223455909](https://user-images.githubusercontent.com/9412577/172058246-86ad6948-27bc-4393-bbbc-b625ae57d7cb.png)

###  **3** 计算输出b1

![image-20220605224426018](https://user-images.githubusercontent.com/9412577/172058268-43380132-d819-40db-8ce1-6f6d8918e595.png)

![image-20220605224557410](https://user-images.githubusercontent.com/9412577/172058194-7acbcefc-bca0-41ca-ae8a-d4b9402b6c10.png)

## 矩阵描述 Self-attetion

### 计算QKV

![image-20220605224712372](https://user-images.githubusercontent.com/9412577/172058286-5b0e08c5-de03-4692-a44d-96bb7f718a16.png)

### 计算输出A

![image-20220605224829875](https://user-images.githubusercontent.com/9412577/172058296-bc50bf35-6925-456f-ba91-2af69e8ea1c1.png)

### 计算输出

![image](https://user-images.githubusercontent.com/9412577/172058315-be2bf4c7-9a2d-4d26-a3cc-3835319ab45d.png)

### 总结

![image](https://user-images.githubusercontent.com/9412577/172058340-ac220b0d-24a2-47c0-8cba-962e1ae6e1a0.png)



## **Multi-head Self-attention**

可以理解为不止一组α，例如下图计算得到两组α。
![image](https://user-images.githubusercontent.com/9412577/172058350-9a0b12d5-a004-47ae-9b76-12565e37da50.png)

## Positional Encoding 

Self-attention 缺少位置信息，例如a1  a2 距离进， a1 在首部，等等信息。

可以相加一个e， e 具体是什么方法目前还待定！！！

![image](https://user-images.githubusercontent.com/9412577/172058356-5fc283a4-271c-4534-a45a-f425f7492e77.png)



## Self-attention for Image

例如将5x5的范围当做 25个sequence，相当于是cnn的卷积核的基础上增加了attention。

![image](https://user-images.githubusercontent.com/9412577/172058374-3e678ed7-c046-4de3-8afe-2370d40da5b3.png)

![image](https://user-images.githubusercontent.com/9412577/172058393-c2a8e6c5-b1c4-45c0-b26b-ae35775290b3.png)

## Self-attention for Graph

可以借助额外的邻接矩阵的信息，attention 收到这个限制。

![image](https://user-images.githubusercontent.com/9412577/172058417-f420b414-a24e-46e8-8daa-2ef6d3ae5015.png)
