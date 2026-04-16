# Lecture 4 Revised

> 按照课程内容整理与重构后的复习笔记。

[TOC]

## 课程主题与整体结构

本讲主题是 **Spatial Filter Design（空间滤波器设计）**，主线如下：

1. 空间滤波基础：相关、卷积、核、可分离、归一化、空域与频域的关系
2. 滤波器设计方法：从数学性质、空间特征、频率响应三个角度设计滤波器
3. 低通滤波：平滑、去噪、减少混叠、区域提取、阴影校正
4. 高通滤波：锐化、边缘检测、不连续检测
5. 带通滤波：由低通和高通组合得到频域选择性滤波
6. 空间增强：将 Laplacian、Sobel、平滑与灰度变换组合成增强流程
7. 补充：最大/最小滤波、Pooling layer

---

## 一、空间滤波基础

### 1. 相关（Correlation）

相关的本质是：**让核在图像上滑动，不翻转核，计算当前位置邻域与核的匹配程度**。

一维形式可写为：

$$
g(x)=\sum_{i=-a}^{a} w(i)f(x+i)
$$

说明：

- 相关不只用于检测边缘，也可用于模板匹配、特征响应计算等
- 当核本身表示“差分”时，相关输出会对灰度跳变、边缘等结构敏感
- `[-1,0,1]` 这类核会对边缘产生明显响应

### 2. 卷积（Convolution）

卷积与相关的核心区别是：**卷积需要先将核翻转（kernel flipped）再进行滑动计算**。

$$
g(x)=\sum_{i=-a}^{a} w(i)f(x-i)
$$

要点：

- 若核是对称核（symmetric kernel），则卷积和相关结果相同
- 常见平滑核如 Box kernel、高斯核通常是对称的，因此实现时两者常表现一致
- 卷积核一般取奇数尺寸，如 `3×3`、`5×5`，以便存在中心像素

### 3. 核（Kernel）与常见术语

`filter`、`mask`、`kernel`、`template`、`window` 等术语在具体语境下常有关联。

核的几个基本概念：

- 核由一组系数（coefficients）构成
- 核大小通常是奇数尺寸
- 核可以设计成检测差分、求平均、突出某种方向结构等

### 4. 边界处理（Boundary Condition）

核在边缘位置滑动时，需要指定边界外像素如何处理。常见有三种：

- `Zero-padding`：边界外补 0
- `Mirror`：镜像扩展
- `Replicated`：复制边界像素

说明：

- `Zero-padding` 常产生暗边，在 Box filter 中尤其明显
- `Mirror` 与 `Replicated` 一般会比补 0 更自然
- 边界处理会直接影响滤波结果，尤其在大核情况下更明显

### 5. 可分离（Separatable）与归一化（Normalized）

基础部分有两个重要性质：

#### 可分离

若二维核可以分解成两个一维核的乘积，则可先按一个方向滤波，再按另一个方向滤波。

优点：

- 计算量更小
- 结果与直接二维卷积一致

高斯核是典型可分离核。

#### 归一化

如果核系数和为 1，则滤波后图像整体亮度更容易保持稳定。

例如 `3×3` 均值滤波核：

$$
\frac{1}{9}
\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

### 6. 空域与频域的关系

需要明确的是：

- 在空间域做卷积（Convolution）
- 在频率域对应乘法（Multiplication）

即：

- 低通、高通、带通都既可以从空域角度理解，也可以从频域角度理解
- 频域里保留低频，对应空域平滑
- 频域里强化高频，对应空域锐化或边缘增强

---

## 二、滤波器设计方法（Design Approach）

常见设计思路有三种。

### 1. 利用数学性质设计

关键词：

- `Difference -> subtraction`
- `Total -> average`

对应理解：

- 若希望检测变化、边缘、不连续，可设计“差分型”滤波器
- 若希望平滑、求局部平均，可设计“求和/平均型”滤波器

### 2. 对具有目标特性的二维空间函数进行采样

即先构造一个连续空间函数，再离散采样成卷积核。

例如：

- 高斯函数采样后得到 Gaussian kernel
- 若关注垂直结构，可设计对 `y` 方向敏感的滤波器

### 3. 按特定频率响应设计

即先决定在频域中“保留什么、抑制什么”，再得到对应滤波器。

这对应：

- 低通滤波器
- 高通滤波器
- 带通滤波器

---

## 三、低通滤波（Low Pass Filter）

低通滤波保留低频、抑制高频，核心作用是：

- 减少灰度的锐利过渡（Reduce sharp transitions in intensity）
- 降噪（Reduce noise）
- 减少混叠（Reduce aliasing）
- 模糊化（Blurring）
- 在目标提取前减少细节
- 弥合小间隙（Bridge small gaps）

### 1. 低频的物理含义

可概括为：

> Low frequency -> slow variant pattern

可理解为：

- 灰度变化缓慢的大结构属于低频
- 大面积区域、平滑背景、缓慢变化的阴影往往属于低频成分
- 边缘、细纹理、噪声等快速变化结构更偏向高频

### 2. Box Kernel（均值滤波）

Box kernel 的本质是局部平均。

例如 `3×3` 均值核：

$$
\frac{1}{9}
\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

特点：

- 输出像素是邻域平均值
- 核越大，平滑越明显
- 噪声、细节、局部突变都会被削弱
- 边缘也会被模糊

从 `3×3`、`11×11`、`21×21` 的对比可以看出，核越大，模糊越重。

注意：

- `Zero-padding` 会导致暗边（dark border）
- 这不是均值滤波本身的性质，而是边界处理方式带来的结果

### 3. Gaussian Kernel（高斯滤波）

高斯滤波也是一种加权平均，但中心像素权重更高，离中心越远权重越小。

二维高斯函数形式：

$$
G(x,y)=\frac{1}{2\pi\sigma^2}\exp\left(-\frac{x^2+y^2}{2\sigma^2}\right)
$$

特点：

- 中心权重大，周围逐渐衰减
- 圆对称（circularly symmetric）
- 滤波结果通常比 Box filter 更自然、更平滑
- 高斯核是可分离核，计算效率更高

#### 核大小的选取

“若为 7，则取 43（21x2+1）”这种写法不准确，逻辑也不清楚。

更合理的记法是：

- 高斯核理论上无限大，实际中需要截断（crop）
- 常见经验是截断到约 `[-3\sigma, 3\sigma]`
- 因此核尺寸常近似写为：

$$
(2\lceil 3\sigma \rceil + 1) \times (2\lceil 3\sigma \rceil + 1)
$$

`43×43`、`85×85` 等具体核大小只是某个参数设置下的实例，不应机械写成固定公式。

#### 重复高斯平滑

有如下提示与思考题：

> Smooth and smooth again? G1=2mm, G2=3mm, G?

这表示：

- 连续做两次高斯平滑，结果仍等价于某个更宽的高斯平滑
- 多次高斯平滑会使整体更模糊
- 因此“高斯再高斯”不是产生新类型滤波，而是得到更强的平滑效果

### 4. Box 与 Gaussian 的比较

可以概括为：

- `Box`：平均范围固定且权重一致
- `Gaussian`：中心权重大，边缘过渡更平滑（smooth edge transition）

因此一般认为：

- Box filter 更简单直接
- Gaussian filter 的视觉效果更自然
- 在很多图像处理中，高斯平滑更常用

### 5. 相对尺寸与分辨率（Relative Size / Resolution）

这一点与分辨率、物理分辨率密切相关，需要单独说明。

**同样大小的核，放在不同分辨率或不同 FOV（视野范围）的图像上，其物理意义可能完全不同。**

例如：

- 对 `4096×4096` 图像使用 `187×187` 核
- 对 `1024×1024` 图像也使用 `187×187` 核

若两者对应的实际物理尺寸不同，则滤波强度的实际含义不同。

因此，分析滤波效果时必须同时考虑：

- 图像像素分辨率
- 像素对应的物理尺寸
- 核的像素大小与实际物理尺度

### 6. 低通滤波的应用

#### 区域提取（Region Extraction）

以细胞荧光显微图像为例：

- 区域通常表现为连续成片、缓慢变化的结构
- 先用低通滤波可减少细碎高频细节
- 有助于后续区域分割或目标提取

#### 阴影校正（Shading Correction）

低频部分往往对应图像中的不均匀照明或缓慢变化背景。

思路是：

- 估计低频背景
- 再用原图减去或校正这个低频背景
- 从而减轻亮度不均匀问题

### 7. 顺序统计滤波（Order-statistic Filters）

这一部分可进一步规范如下。

关键词：

- `Nonlinear`
- `Ranking`
- `Median filter`
- `Max filter`
- `Min filter`

#### 中值滤波（Median Filter）

做法：

- 取一个窗口
- 将窗口内像素值排序
- 取中位数作为输出

特点：

- 非线性滤波，不是“乘权重求和”
- 对脉冲噪声（impulse noise）尤其有效
- 比均值滤波更不容易模糊边缘
- 会让异常像素更接近周围邻域的主流水平

在椒盐噪声（salt-and-pepper noise）下，Median 通常比 Gaussian 更有效。

#### 最大滤波（Max Filter）

作用可理解为：

- 输出邻域中的最大值
- 强化亮目标
- 去除小黑点
- 填补亮区域内的小黑洞

#### 最小滤波（Min Filter）

作用可理解为：

- 输出邻域中的最小值
- 强化暗目标
- 去除小白点
- 压制暗背景中的小亮斑

### 8. 低通滤波小结

可归纳为：

- 概念：低频表示缓慢变化模式
- 基本滤波器：Box、Gaussian、Median
- 注意事项：分辨率/相对尺度、边界处理

---

## 四、高通滤波（High Pass Filter）

主旨是：

- 低通更接近求和 / 积分（summation / integration）
- 高通更接近差分 / 微分（differentiation）

高通滤波主要用于：

- 锐化（Sharpening）
- 边缘检测（Edge）
- 不连续检测（Discontinuities）
- 噪声响应分析

并且：

> High frequency -> fast change component

即高频成分对应快速变化的结构。

### 1. 一阶与二阶

先区分：

- 一阶导数型方法（First order）
- 二阶导数型方法（Second order）

一般理解：

- 一阶导数更适合刻画变化率和方向
- 二阶导数更强调快速变化和尖锐过渡

### 2. 一阶、二阶离散微分基础

#### 一阶差分

$$
\frac{\partial f}{\partial x} \approx f(x+1)-f(x)
$$

或也常写成中心差分形式：

$$
\frac{\partial f}{\partial x} \approx f(x+1)-f(x-1)
$$

#### 二阶差分

$$
\frac{\partial^2 f}{\partial x^2} \approx f(x+1)+f(x-1)-2f(x)
$$

图像处理中，这些差分都可以写成卷积形式，因此“求导”在离散图像里通常通过卷积核实现。

### 3. Laplacian 算子

`Laplacian` 与梯度不是同一个概念。

#### 定义

Laplacian 是 **二阶导数算子**，通常写成：

$$
\nabla^2 f = \frac{\partial^2 f}{\partial x^2}+\frac{\partial^2 f}{\partial y^2}
$$

展开后可写为：

$$
\nabla^2 f = f(x+1,y)+f(x-1,y)+f(x,y+1)+f(x,y-1)-4f(x,y)
$$

常见离散模板：

$$
N_4:
\begin{bmatrix}
0 & 1 & 0 \\
1 & -4 & 1 \\
0 & 1 & 0
\end{bmatrix}
\qquad
N_8:
\begin{bmatrix}
1 & 1 & 1 \\
1 & -8 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

说明：

- `N4` 只考虑上下左右
- `N8` 连对角方向也考虑
- `N8` 对方向更均衡，但也可能更容易放大噪声

#### Laplacian Enhancement

核心理解是：

> Laplacian enhancement -> highlights sharp transitions

典型形式：

$$
g(x,y)=f(x,y)+c\,\nabla^2 f(x,y)
$$

实际使用时要注意模板中心系数符号，不同模板写法下，增强公式可能写成加号或减号，本质目标都是：

- 让快速变化部分被强调
- 让边缘和细小结构更突出

### 4. Unsharp Masking（反锐化掩膜）

它是另一类常见锐化方法。

基本思想：

1. 先对原图做低通平滑，得到模糊图 `f_L`
2. 原图减去模糊图，得到细节部分 `f_h`
3. 将细节按比例放大后加回原图

公式可写为：

$$
f_h = f - f_L
$$

$$
f_{enhance} = f + kf_h
$$

与 Laplacian enhancement 的区别：

- Laplacian 直接基于二阶导数响应增强高频
- Unsharp masking 通过“原图 - 模糊图”提取高频细节
- 两者都能增强细节，但对噪声的敏感性、视觉效果和参数控制方式不同

### 5. 梯度（Gradient）

梯度是 **一阶导数构成的向量**，不是 Laplacian。

定义：

$$
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x} \\
\frac{\partial f}{\partial y}
\end{bmatrix}
=
\begin{bmatrix}
g_x \\
g_y
\end{bmatrix}
$$

梯度幅值常写为：

$$
M(x,y)=\sqrt{g_x^2+g_y^2}\approx |g_x|+|g_y|
$$

含义：

- 平坦区域灰度变化小，梯度小
- 边缘处灰度变化快，梯度大
- 梯度还能给出变化最剧烈的方向

### 6. Roberts Cross-Gradient Operators

梯度部分先介绍 `Roberts cross-gradient operators`。

其特点是：

- 基于一阶差分
- 使用很小的邻域
- 能检测局部快速变化
- 对噪声更敏感

这一部分需要补充掌握。

### 7. Sobel Operators

梯度部分进一步给出 Sobel 算子。

常用写法为：

$$
S_x=
\begin{bmatrix}
-1 & 0 & 1 \\
-2 & 0 & 2 \\
-1 & 0 & 1
\end{bmatrix}
\qquad
S_y=
\begin{bmatrix}
-1 & -2 & -1 \\
0 & 0 & 0 \\
1 & 2 & 1
\end{bmatrix}
$$

注意：有些资料会把 `g_x` 与 `g_y` 的模板记法互换，但本质上只是方向命名不同；为避免混乱，建议按上式记忆。

Sobel 的思想是：

- 一个方向做差分，计算变化率
- 垂直方向做加权平均，降低噪声影响

因此它兼具：

- 边缘检测能力
- 一定的抗噪性
- 方向信息表达能力

### 8. 高通滤波小结

这一部分可总结为：

- Sharpening 对应高频增强
- 一阶方法：Gradient、Roberts、Sobel
- 二阶方法：Laplacian
- 细节增强方法：Unsharp mask

---

## 五、带通滤波（Band Pass Filter）

这一部分需要单独补充。

### 1. 基本概念

带通滤波的目标是：

- 抑制过低频成分
- 抑制过高频成分
- 只保留某一段频率范围内的信号

相关关系如下：

- Low pass
- High pass
- Band reject
- Band pass

即带通/带阻可以理解为在频域上进一步限制通过频率范围。

### 2. 与低通、高通的关系

带通滤波通常不是独立凭空出现，而是建立在低通和高通思想之上。

可以这样理解：

- 低通：保留低频
- 高通：保留高频
- 带通：只保留中间某个频段
- 带阻：只抑制中间某个频段

### 3. 理解要点

这一节的关键词包括：

- `Spatial frequency increase`
- `Low pass filter`
- `Seperatable`
- `Isotropic`
- `Frequency domain filter`
- `Basic low pass filter`

可作如下理解：

- 带通滤波通常从频域角度更容易理解
- 许多带通滤波器可由基本低通滤波器组合、相减或变形得到
- `Isotropic` 表示频率响应只与频率半径有关，与方向无关
- `Separable` 表示若结构允许，可拆成多个方向上的处理

### 4. 学习时应掌握的最低要求

这一部分至少要掌握：

- 知道带通是“保留中间频率”的滤波
- 知道它与低通/高通/带阻的区别
- 知道它更多从频域响应上理解
- 知道很多带通滤波器来自基本低通滤波器的组合

---

## 六、空间增强（Spatial Enhancement）

这一部分的重点不是单个算子，而是 **多个空间滤波步骤的组合增强流程**。

### 1. 目标

关键词：

- `Enhance details`
- `A rather noisy sharpened image is expected`

说明：

- 单独做锐化往往会把噪声也一起增强
- 因此实际增强流程通常不是“一步完成”
- 需要将锐化、边缘信息、平滑和灰度拉伸组合使用

### 2. 增强思路

从第 38 到 42 页内容可整理出如下流程：

1. 对原图做 Laplacian 增强，突出锐利变化
2. 计算 Sobel gradient，得到边缘/变化信息
3. 对梯度图做平滑（如 averaging filter）以降低噪声影响
4. 将平滑后的梯度信息与 Laplacian 增强结果相乘，得到 masked image
5. 再与原图组合形成 sharpened / sharpen enhanced 图像
6. 最后配合 `gamma` 或 `power-law stretch` 做灰度动态范围调整

### 3. 为什么需要组合增强

需要注意：

- Laplacian 对噪声和细小结构都很敏感，单独使用时结果可能较噪
- Gradient 对噪声和细碎细节的响应通常低于 Laplacian
- 进一步对梯度图进行平滑，可以继续抑制噪声
- 把不同算子的结果组合起来，比只用一个算子更稳定

### 4. 流程图式总结

可将增强过程记为：

$$
\text{Original}
\rightarrow \text{Laplacian enhanced}
\rightarrow \text{Sobel gradient}
\rightarrow \text{Box-filter smoothed}
\rightarrow \text{Masked image}
\rightarrow \text{Sharpen enhanced}
\rightarrow \text{Power-law stretch}
\rightarrow \text{Output}
$$

最终方案 **depends on the application / the user**，即增强流程没有绝对固定答案，要根据图像类型、任务目标和主观视觉要求来调整。

---

## 七、补充内容

### 1. Max / Min / Median Filter 的补充说明

最大、最小、中值滤波并不只是低通部分的小知识点，而是很常用的局部非线性处理工具。

其中：

- `Max filter` 偏向扩张亮结构
- `Min filter` 偏向扩张暗结构
- `Median filter` 更适合处理脉冲噪声

### 2. Pooling Layer

这一部分的表述需要进一步规范。

核心含义是：

- `Downscaling image by extracting most important feature`
- `Removing invariances like shift, rotational and scale`

即池化层的作用是：

- 通过保留局部最重要特征来下采样图像或特征图
- 在一定程度上提升对平移、旋转、尺度变化的鲁棒性

#### Max Pooling

这里重点掌握 `Max pooling`。

其特点：

- 在局部窗口中取最大值
- 更容易保留强响应特征，如亮边缘、显著结构
- 会降低分辨率，但保留局部最强激活

#### 关于“最小化池”的说法

“有最大化池与最小化池”这种写法并不是这里的重点表述。

更准确的写法应当是：

- 这里主要讲的是 **pooling 的概念** 与 **max pooling 示例**
- 不应把前面讲的 `Max/Min filter` 与这里的 `Pooling layer` 混为一谈
- 池化层是下采样/特征汇聚操作；最大最小滤波是空间滤波操作，两者用途和语境不同

---

## 八、本课应掌握的核心知识点清单

### 必须会区分的概念

- 相关与卷积的区别：是否翻转核
- 低通、高通、带通的含义
- 梯度与 Laplacian 的区别：一阶向量 vs 二阶标量
- 空间滤波与频域乘法之间的对应关系
- 最大/最小/中值滤波 与 pooling 的区别

### 必须理解的滤波器

- Box filter
- Gaussian filter
- Median filter
- Laplacian
- Roberts gradient
- Sobel gradient
- Unsharp masking
- Band pass filter（概念层面）

### 必须注意的问题

- 边界处理方式会影响结果
- 核大小必须结合图像分辨率和物理尺寸理解
- 锐化会放大噪声
- 实际增强往往需要多种算子组合

---

## AI总结

本讲《Spatial Filter Design》主要讨论数字图像处理中如何设计和理解空间滤波器，以及如何利用这些滤波器完成平滑、锐化、边缘检测和图像增强。内容从最基础的相关、卷积、核、可分离、归一化以及空域与频域的对应关系出发，建立空间滤波的统一框架；随后介绍滤波器设计的三种基本思路，即基于数学性质、基于目标空间函数采样、基于特定频率响应设计。接着系统讲解低通滤波，包括 Box filter、Gaussian filter、Median filter 及其在降噪、去细节、减少混叠、区域提取、阴影校正中的作用，同时强调分辨率和边界处理的重要性。之后进入高通滤波，讲解一阶与二阶导数思想、Laplacian、Roberts、Sobel、Unsharp masking 等典型方法，并说明它们如何用于锐化、边缘检测和高频增强。随后补充带通滤波的频域意义，说明其与低通、高通、带阻的关系。最后给出一个更实际的空间增强流程：将 Laplacian、Sobel、平滑和灰度拉伸组合起来，以在突出细节的同时尽量控制噪声；并通过最大/最小滤波和池化层补充说明局部非线性操作与特征下采样的基本思想。总体而言，本讲涵盖的知识点包括：相关、卷积、核、可分离、归一化、边界处理、空域与频域关系、低通滤波、高通滤波、带通滤波、均值滤波、高斯滤波、中值滤波、最大最小滤波、Laplacian、梯度、Roberts、Sobel、反锐化掩膜、区域提取、阴影校正、空间增强流程、灰度拉伸以及池化层。
