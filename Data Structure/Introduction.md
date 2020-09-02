# Chapter 1 : 绪论

* 计算 = 信息处理 = 借助某种工具，遵照一定规则，以明确而机械的形式进行

* 计算模型 = 计算机 = 信息处理工具
* 所谓算法，即特定计算模型下，旨在解决特定问题的指令序列

## 1. 计算模型

同一问题通常有多种算法，如何评价其优劣？

不同的算法，可能受问题规模、输入类型的影响，同一算法，可能由不同的程序员、不同的编程语言、运行在不同的体系结构、操作系统上，为了给出**客观**的评判，需要抽象出一个**理想**的平台或模型。

### 1.1. 图灵机

![](https://raw.githubusercontent.com/gaofanfei/figurebed/master/img/20200820113841.png)

* 纸带：依次均匀地划分为单元格，各存有某一字符，初始均为'#'
* 头：总是对准某一单元格，并可读取或改写其中的字符。每经过一个节拍，可转向左侧或右侧的邻格
* 状态： TM总是处于有限种状态中的某一种，每经过一个节拍可按照规则转向另一种状态。统一约定，'h' = halt
* 从启动至停机，所经历的**节拍数目**，即可用以度量计算的成本，亦等于Head累计的**移动次数**

### 1.2. RAM

![](https://raw.githubusercontent.com/gaofanfei/figurebed/master/img/20200822130744.png)

* 寄存器顺序编号，总数没有限制
* 可通过编号直接访问任意寄存器
* 每一基本操作仅需常数时间

## 2. 渐进复杂度

问题规模**足够大**之后，计算成本的**增长趋势**。

![daChb4.png](https://s1.ax1x.com/2020/08/22/daChb4.png)

![daPKGq.png](https://s1.ax1x.com/2020/08/22/daPKGq.png)

![daPuin.png](https://s1.ax1x.com/2020/08/22/daPuin.png)

![daPMR0.png](https://s1.ax1x.com/2020/08/22/daPMR0.png)

渐进复杂度的层次级别：

![daP3sU.png](https://s1.ax1x.com/2020/08/22/daP3sU.png)

![daPJZ4.png](https://s1.ax1x.com/2020/08/22/daPJZ4.png)

对数复杂度：

* 常底数无所谓
* 常数次幂无所谓
* 复杂度无限接近于常数

从指数复杂度到多项式复杂度，是**有效算法**到**无效算法**的分水岭。

## 3. 复杂度分析

主要方法：

* 迭代（级数求和）
* 递归（递归跟踪 + 递归方程）
* 实用（猜测 + 验证）

#### 3.0. 级数

* 算术级数：与**末项平方**同阶

![daPQzV.png](https://s1.ax1x.com/2020/08/22/daPQzV.png)

* 幂方级数：比幂次高出**一阶**

![daP1MT.png](https://s1.ax1x.com/2020/08/22/daP1MT.png)

* 几何级数：与**末项**同阶

![daP8LF.png](https://s1.ax1x.com/2020/08/22/daP8LF.png)

* 调和级数

![daPYdJ.png](https://s1.ax1x.com/2020/08/22/daPYdJ.png)

* 对数级数

![daPto9.png](https://s1.ax1x.com/2020/08/22/daPto9.png)

#### 3.1. 迭代

迭代+算术级数

```c
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        O1op(const i, const j);
```

迭代+级数

```c
for (int i = 0; i < n; i++)
    for (int j = 0; j < i; j++)
        O1op(const i, const j);

for (int i = 0; i < n; i++)
    for (int j = 0; j < i; j += 2020)
        O1op(const i, const j);
```

#### 3.2. 递归

**递归跟踪**：绘出计算过程中出现过的所有递归实例（及其调用关系），它们各自所需时间之总和，即为整体运行时间。

```c
// 减而治之
int sum(int A[], int n)
{
    return n < 1 ? 0 : sum(A, n-1) + A[n-1];
}
```

![daPaJ1.png](https://s1.ax1x.com/2020/08/22/daPaJ1.png)

本例中，共计n+1个递归实例，各自只需O(1)时间，故总体运行时间为T(n) = O(1) * (n+1) = O(n)

**递归方程**：

* 从递推的角度看，为求解规模为n的问题sum(A, n)，需 //T(n)

- 递归求解规模为n-1的问题sum(A, n - 1)，再 //T(n-1)
- 累加上A[n - 1] //O(1)

T(n) = T(n-1) + O(1), T(0) = O(1)



```c
// 分而治之
int sum(int A[], int hi, int lo)
{
    if (hi - lo < 2)
        return A[lo];
    int mi = (hi + lo) >> 1;
    return sum(A, lo, mi) + sum(A, mi, hi);
}
```

递归跟踪：

![daPdRx.png](https://s1.ax1x.com/2020/08/22/daPdRx.png)

T(n) = 各**层**递归实例所需时间之**和**

![daPBQK.png](https://s1.ax1x.com/2020/08/22/daPBQK.png)

递推方程：

![daPwz6.png](https://s1.ax1x.com/2020/08/22/daPwz6.png)

#### 3.3. 主定理

![daPDsO.png](https://s1.ax1x.com/2020/08/22/daPDsO.png)



