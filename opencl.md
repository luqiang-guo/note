# OpenCL

## 简介



**OpenCL**全称为Open Computing Language（开放计算语言），先由Apple设计，后来交由Khronos Group维护，是异构平台并行编程的开放的标准，也是一个编程框架。OpenCL支持数据并行任务并行。同时OpenCL内建了多GPU并行的支持。这使得OpenCL的应用范围比CUDA广。为了能适用于一些更低端的嵌入式设备（如DSP+单片机这种环境），OpenCL API基于纯C语言进行编写。Khronos Group是一个非盈利性技术组织，维护着多个开放的工业标准，并且得到了业界的广泛支持。OpenCL的设计借鉴了CUDA的成功经验，并尽可能地支持多核CPU、GPU或其他加速器。

OpenCL包含两个部分

- 一是OpenCL C语言（OpenCL 2.1将开始使用OpenCL C++作为内核编程语言）和主机端API
- 二是硬件架构的抽象。

为了使C程序员能够方便简单地学习OpenCL，OpenCL只是给C11进行了非常小的扩展，以提供控制并行计算设备的API以及一些声明计算内核的能力。软件开发人员可以利用OpenCL开发并行程序，并且可获得比较好的在多种设备上运行的可移植性。

为了使得OpenCL程序能够在各种硬件平台上运行，OpenCL提供了一个硬件平台层。同时各种不同设备上的存储器并不相同，相应地，OpenCL提供了一个存储器抽象模型。与CUDA相似，OpenCL还提供了执行模型和编程模型。

OpenCL不但包括一门编程语言，还包括一个完整的并行编程框架，通过编程语言、API以及运行时系统来支持软件在整个平台上的运行。

相比CUDA，OpenCL的优点在于它提供了一种能够在不同平台上可移植的编程方式，另外其原生支持的多设备并行也是一大亮点。





- 平台模型（platform model）
- 存储器模型（memory model）
- 执行模型（execution model）
- 编程模型（programming model）

