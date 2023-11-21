# GEMM

##  Naive 计算

### mnk VS mkn

![matmul_02](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_02.png)

### nkm VS mkn

![matmul_03](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_03.png)

### kmn VS mkn

![matmul_04](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_04.png)

## 循环4展开

### mnk

- n = 4

![matmul_06](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_06.png)

- m = 4, n = 4, k = 4

  ![matmul_07](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_07.png)

### mkn


- m = 4, n = 4, k = 4

  ![matmul_09](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_09.png)

## SIMD 

###  MNK Intrinsics Neon

![matmul_12](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_12.png)

### Asm

![matmul_13](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_13.png)

## Block MKN

###  block 64， 128， 256

![matmul_14](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_14.png)

### 2层分块 

​	内层32，外层64， 128， 256.

![matmul_15_128](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_15_128.png)

### 3层分块

从内到外 32， 128， 256

![matmul_16_256](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_16_256.png)

## Block KMN

kmn block 128

![matmul_17](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_17.png)

### mknmn 分块

- mknmn 256 128 128 4 4

![matmul_18](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_18.png)

- mknmn 1024 256 384 4 4

![matmul_20](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_20.png)

## kernel 8x12

- mknmn  1024 256 156 12 8

![matmul_23](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_23.png)

- mknmn  1024 512 156 12 8

![matmul_27](../../../project/gemm/mu-optimize-gemm/aarch64/img/matmul_27.png)
