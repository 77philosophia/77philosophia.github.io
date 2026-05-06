---
title: GPU 的工作原理：从并行像素到 CUDA 加速
date: 2026-05-06 11:08:28
tags:
category:
  - 专业笔记
---



### 1. Gpu 和 cpu

- CPU 尤其擅长处理涉及分支逻辑、操作系统管理和顺序算法的任务。
- **GPU** 是专为**并行计算**而设计的。它不是由几个强大的核心组成，而是由**数千个较小的运算单元**组成，能够同时执行许多运算。

> [!NOTE]
>
> *“CPU 就像几个技艺精湛的大厨烹饪复杂的菜肴，而 GPU 就像成千上万个厨师同时准备简单的菜肴。”*

```
例如，考虑将两个大小为 N 的向量相加。

顺序 CPU 计算：
C[i] = A[i] + B[i]

for i = 1 to N
对于 i = 1 到 N

这需要 N 次顺序操作。

在 GPU 上，N 个独立的线程可以同时计算每个元素：

Thread i 计算：
C[i] = A[i] + B[i]

因此，理论运行时间近似为：
T_gpu ≈ T_cpu / P
其中 P 为并行处理单元的数量。
```



### 2. gpu的架构

1. **流式多处理器（SM）**

   这些是主要的计算单元。每个 SM 包含许多小型算术逻辑单元（ALU）、寄存器和共享内存。

2. **CUDA cores**

   负责执行加法和乘法等算术指令的基本计算单元。

3. **内存层次结构**

   - Global memory 全局内存（容量大但速度相对较慢）
   - Shared memory 共享内存（线程块内线程共享的快速内存）
   - Registers 寄存器（每个线程的超快存储）
   - Constant and texture memory 常量内存和纹理内存（专用缓存内存）



### 3. gpu图形起源

GPU 最初是为**图形渲染而**设计的，特别是用于将 3D 场景转换为 2D 图像等任务。

传统的渲染流程包括以下几个阶段：

1. Vertex processing 顶点处理
2. Geometry transformation 几何变换
3. Rasterization 栅格化
4. Pixel shading 像素着色

每个阶段都会对大量顶点或像素执行操作。由于这些操作大多是独立的，因此可以并行计算。

例如，计算每个像素的颜色涉及评估如下光照方程：

```
I = k_a * I_a + k_d * (N dot L) * I_l
I = k_a * I_a + k_d * (N 点 L) * I_l

where:  在哪里：

I = pixel intensity  I = 像素强度
k_a = ambient coefficient
k_a = 环境系数
k_d = diffuse coefficient
k_d = 扩散系数
N = surface normal  N = 表面法线
L = light direction  L = 光照方向
```

每个像素独立执行此计算，这使得 GPU 成为此类工作负载的理想处理器。

随着时间的推移，工程师们意识到同样的并行架构也可以加速**非图形计算** 。



### 4.gpu效率背后的数学原理

许多计算问题都可以用**线性代数运算**来表示，而 GPU 可以极其高效地执行这些运算。

一个基本的例子是**矩阵乘法** 。

```
给定矩阵 A 和 B：
C = A * B

C 的每个元素计算如下：
C[i,j] = 对 k 求和 A[i,k] * B[k,j]

该操作涉及重复的乘加运算 ，而 GPU 正是针对此类运算进行了专门优化。
如果 A 和 B 是大小为 N x N 的大矩阵，则计算大约需要：
O(N³)
但是，每个元素 C[i,j] 都可以独立计算，从而允许并行执行。
例如，GPU 可能会分配：

One thread per matrix element
每个矩阵元素一个线程
One block per matrix tile
每个矩阵单元一个方块

这种结构能够实现大规模并行处理 ，从而显著缩短运行时间。
诸如此类的矩阵运算构成了以下计算的基础：
neural networks  神经网络
signal processing  信号处理
scientific simulations  科学模拟
wireless communication algorithms
无线通信算法
```



### 5.线程层次：cuda运行模型

为了利用 GPU 的强大功能，开发人员使用 **CUDA（统一计算设备架构）** 等编程模型。

CUDA 将计算组织成一个层次结构：

**Threads → Blocks → Grids
线程 → 块 → 网格**

每个线程执行一小段计算。线程被分组为**块** ，块构成**网格** 。

```
例如，假设我们要计算 N 个元素的向量加法。

每个 CUDA 线程计算：
C[i] = A[i] + B[i]

线程索引通常按以下方式计算：
i = blockIdx.x * blockDim.x + threadIdx.x
blockIdx.x = block index  blockIdx.x = 区块索引
blockDim.x = number of threads per block
blockDim.x = 每个块的线程数
threadIdx.x = thread index inside the block
threadIdx.x = 代码块内的线程索引

这种索引方式使得数千个线程能够在 GPU 上高效地协调运行。

同一代码块内的线程也可以通过共享内存进行通信，这比使用全局内存要快得多。
```



### 6.内存访问和性能

GPU 性能很大程度上取决于**内存访问模式** 。

全局内存访问速度相对较慢。因此，CUDA 程序会尽可能地利用**共享内存和寄存器** 。

一项关键的优化是**内存合并** 。

如果线程以连续模式访问内存：

A[i], A[i+1], A[i+2], …

GPU 可以将多个内存请求合并成一个事务。

然而，不规则的访问模式会导致许多单独的内存请求，从而降低性能。

实际上，经过良好优化的 GPU 程序与 CPU 实现相比，速度通常可提高 **10 倍到 100 倍** 。



### 7. 机器学习里的gpu

深度学习的兴起极大地提升了 GPU 的重要性。

训练神经网络需要反复进行矩阵运算。

例如，神经网络层计算：

y = W * x + b

where: 在哪里：

W = weight matrix W = 权重矩阵
x = input vector x = 输入向量
b = bias vector b = 偏置向量

在训练过程中，梯度下降需要计算导数：

dL/dW

并执行如下更新：

W = W − eta * dL/dW

这些操作涉及数十亿次浮点运算，而 GPU 由于其并行架构，能够极其高效地处理这些运算。



### 8.gpu在科学计算里的加速

CUDA 允许开发人员使用 C/C++ 的扩展来编写 GPU 加速程序。

一个简单的 CUDA 内核在概念上可能如下所示：

```
__global__ void vectorAdd(float *A, float *B, float *C) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    C[i] = A[i] + B[i];
}

```

关键字 `__global__` 表示该函数在 GPU 上运行。

CPU 启动内核时使用的配置如下：

vectorAdd<<<numBlocks, threadsPerBlock>>>(A, B, C);
vectorAdd<<<numBlocks, threadsPerBlock> >>（A、B、C）；

这将同时启动数千个线程。

现代 GPU 还支持以下高级功能：

- Tensor cores for deep learning
  用于深度学习的张量核心
- Mixed-precision arithmetic
  混合精度算术
- Asynchronous execution 异步执行
- Unified memory 统一内存

这些特性显著加快了矩阵乘法和卷积运算等任务的速度。



### 9.现代gpu创新

现代 GPU 的发展已经远远超越了图形硬件的范畴。

一些关键进展包括：

**A. Tensor Cores**

专为加速神经网络中使用的矩阵乘法运算而设计的专用单元。

D = A * B + C

合并成一条指令。



**B. Ray Tracing Hardware**

用于计算三维环境中光线相交的专用核心。



**C. High Bandwidth Memory (HBM)**

现代 GPU 采用堆叠式内存架构，可提供超过 **1TB/s 的**带宽。

这些改进使得 GPU 能够高效地处理海量数据集。



### 10.gpu不仅处理图形，其他领域——

如今，GPU 为众多领域提供动力：

- Artificial intelligence 人工智能
- Climate simulations 气候模拟
- Genomics 基因组学
- Cryptography 密码学
- Wireless communication research
  无线通信研究
- Computational physics 计算物理学

例如，在信号处理或通信系统中，GPU 可以加速以下操作：

```
快速傅里叶变换（FFT）：
X[k] = sum over n of x[n] * exp(-j * 2pi * k * n / N)
X[k] = ∑_(n) x[n] * exp(-j * 2π * k * n / N)

这种变换出现在 OFDM 系统、雷达处理和频谱分析中
GPU 加速技术即使对于极其庞大的数据集也能实现实时处理。

```

