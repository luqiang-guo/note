# ShuffleChannel



## 背景 ShuffleNet



- **标准卷积**

  ![image-20230406154819381](ShuffleChannel.assets/image-20230406154819381.png)

- **分组卷积**

  ![image-20230406154858535](ShuffleChannel.assets/image-20230406154858535.png)

如果全部是分组卷积，会导致通道之间无法信息交流。所以就有了一个shuffle channel 的操作。

![image-20230406154202245](ShuffleChannel.assets/image-20230406154202245.png)

##  参考

https://blog.yani.ai/filter-group-tutorial/





## C/C++