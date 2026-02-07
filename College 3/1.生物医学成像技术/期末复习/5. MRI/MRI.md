[TOC]

# MRI

## 核磁共振的物理原理

核磁共振 (NMRI) 研究的<span style="color:#CC0000; font-weight:bold;">原子核的性质</span>

### 自旋的磁场特性

#### 自旋

自旋 Spin 是粒子所具有的内在性质。每种基本粒子都有特定的自旋量子数 `I`。重点关注 `H, O, F, Na, P`

**有自旋**
<span style="vertical-align:super;">1</span>H (I = ½), <span style="vertical-align:super;">17</span>O (I = 3/2), <span style="vertical-align:super;">19</span>F (I = ½), <span style="vertical-align:super;">23</span>Na (I = 3/2), and <span style="vertical-align:super;">31</span>P (I = ½). 

**无自旋**
$^4He$, $^{12}C$, $^{16}O$ and <span style="vertical-align:super;">32</span>S.
<span style="vertical-align:super;">14</span>N (I = 1), <span style="vertical-align:super;">2</span>H (deuterium, I = 1)

#### 自旋的磁场

自旋角动量：$\Phi = \displaystyle \frac{h}{2 \pi} I$

微观磁场的磁矩：$\mu = \gamma\cdot \Phi$

表现：在没有外加磁场时，所有核自旋方向随机分布，导致无宏观磁场，施加外部磁场 B 后，粒子的势能会依据不同的自旋角动量投影发生 **能级分裂**，其自旋向上的量子态将比自旋向下的量子态<span style="background:#FFCCCC;">能量更低</span>，宏观上产生<span style="color:#004C99; font-weight:bold;">磁化矢量M0</span>

#### 自旋的进动

粒子自旋在磁场中的产生进动，即<span style="color:#FF3399; font-weight:bold;">拉莫进动</span>。

由公式推导，得 拉莫频率为
$$
\omega_0 = \gamma B_o
$$

#### 核磁共振的频率

$$
\begin{aligned}
 & f =\gamma/2\pi \cdot B_0 = 42.6MHz/T\times B_0 \\
 & \mathrm{for~}1.5\mathrm{T},= 64\mathrm{MHz};
\end{aligned}
$$

再结合电磁波谱，可知道，<span style="color:#FF3333; font-weight:bold;">MRI没有电离辐射</span>

### 化学位移

化学位移是由于化学环境不同导致的原子核 **拉莫频率变化**。例如，水氢（1H）与不同分子结合时会产生不同的化学偏移效应

### 宏观磁化矢量的进动

我们分成 xyz 三个方向

对于宏观磁化矢量 $M=M(x,t)$， 其与宏观角动量 J 的关系满足 $M=\gamma J$， 对于特定位置/体素 $M=M(t)$, 其进动过程可以描述为

$$
\frac{dM(t)}{dt}=\gamma M(t)\times B(t)
$$

当 $ \boldsymbol{B}(t)=B_0,\boldsymbol{M}(0) $ 与 z 轴成夹角 $\alpha$ 时

$$
M_{x}(t)= M_{0}sin\alpha\cos(-\gamma B_{0}t+\phi)
$$

$$
M_y(t)= M_0sin\alpha\sin(-\gamma B_0t+\phi)
$$

$$
M_z(t)= M_0cos\alpha
$$

分成 z 轴的纵向和 xy 平面的横向

### 射频激发

在测量 M 时，由于各个自旋的进动相位相互独立，而与 B0 场相比，M 非常微弱难以测量。
因而人们通常使用一个 ==垂直于主磁场方向的== 额外射频磁场 B1 将 M 从 B0 中分离出来，从而被接收线圈检测到。

#### 翻转角

M旋转的角度叫做**翻转角**。翻转方向符合“***<u>左手定则</u>***”（见PPT关于翻转角从介绍）
$$
\alpha=\gamma\int_0^{\tau_p}B_1^e(t)dt
$$

### 信号接收

原理是法拉第电磁感应定律。

### 弛豫-Relaxation

#### 横向弛豫

横向弛豫也称为自旋-自旋弛豫（spin-spinrelaxation）。

由邻近自旋引 1 起磁场扰动，这种相互作用使得自旋加速或减速，改变它们的相位这种去相位（dephasing）会导致自旋系统产生的 RF 波失去相干性，信号衰减

**自由感应衰减信号 FID**

指数衰减，有参数<span style="color:#FF8000; font-weight:bold;">横向弛豫时间</span> $T_2$

$$
M_{xy}(t)= M_0sin\alpha \cdot e^{j\phi}\cdot e^{-t/T_2}
$$

#### 纵向弛豫

纵向弛豫又称自旋-晶格弛豫（spin-lattice）。

是纵向磁化矢量逐渐恢复至平衡磁化矢量的过程，分子的运动频率越接近拉莫频率，恢复越快，其时间常数为<span style="color:#FF8000; font-weight:bold;">纵向弛豫时间</span> $T_1$
$$
M_{z}(t)= M_{0}\left(1-e^{-\frac{t}{T_{1}}}\right)+M_{z}(0^{+})e^{-t/T_{1}}
$$

### 布洛赫方程

超级牛逼的微分方程
$$
\frac{dM(t)}{dt}=\gamma M(t)\times B(t)-R\{M(t)-M_0\}
$$
其中
$$
B(t)= B_0+B_1(t) \\
R =
\begin{pmatrix}
1/T_2 & 0 & 0 \\
0 & 1/T_2 & 0 \\
0 & 0 & 1/T_1
\end{pmatrix}
$$

方程分解，则
$$
\frac{d}{dt}
\begin{pmatrix}
M_x(t) \\
M_y(t) \\
M_z(t)
\end{pmatrix}=\gamma
\begin{pmatrix}
M_y(t)B_z(t)-M_z(t)B_y(t) \\
-M_x(t)B_z(t)+M_z(t)B_x(t) \\
M_x(t)B_y(t)-M_y(t)B_x(t)
\end{pmatrix}-
\begin{pmatrix}
\frac{1}{T_2}M_x(t) \\
\frac{1}{T_2}M_y(t) \\
\frac{1}{T_1}(M_z(t)-M_{0z})
\end{pmatrix}
$$



## MRI 硬件设备

### 主磁体

单位: 1Tesla = 10000Gauss 

#### 进动

**磁化矢量**
$$
M_0 = \frac{N\gamma^2(h/2\pi)^2B_0I_z(I_z+1)}{3kT} \\
M_0 = \Delta n\frac23\gamma(h/2\pi)I_z(I_z+1)
$$
**进动**

绕着 $B_0$ 旋转的进动
静止坐标系: $(\frac{dM}{dt})_{lab} = (\frac{dM}{dt})_{rot}+\omega\times M$
旋转坐标系: $(\frac{dM}{dt})_{rot} = \gamma M\times B_{eff}$; $B_{eff} = B+\omega/\gamma$.
$$
B = B_0 + \frac\omega\gamma(ratating)\\
\frac{dM}{dt} = M\times\gamma(B+B_1(t))\\
B_1(t) = B_1e^{-i\omega_0t}\\
\omega_1 = \gamma B_1\\
\alpha = \omega_1 t(flip\ angle)
$$
射频脉冲制造 B1

#### 拉莫尔公式

$\omega = \gamma B$, $\mu$ 围绕主磁场以拉莫尔频率 $\omega$ 旋转, $\omega$ 为拉莫尔频率.

### 射频脉冲

#### 激励

用的是翻转线圈
翻转角度: $\alpha = \gamma B_1 t = \omega_1t$

#### 接收

**弛豫**

公式: $\frac{dM}{dt} = M(t)\gamma B(t)-R(M(t)-M_0)$.

纵向: $M_z = M_{z}|_{t=0}\times e^{-t/T_1} + M_0(1-e^{-t/T_1})$. $T_1$ 为恢复到 $1-\frac1e$ 时的时间, B0 大, T1 更长.
横向: $M_{xy} = M_{xy}|_{t=0}e^{-t/T_2}$. $T_2$ 为衰减到 $\frac1e$ 时的时间. z 分量波动的影响往往主导 T2 弛豫过程.
$T_1,T_2$ 长短正相关

**信号接收**

法拉利公式: 线圈接收的是横向的分量
$S = M_0(1-e^{-TR/T1})e^{-TE/T2}$
TR 全周期的重复时间, TE 回波(检测信号的)时间

**对比度**
减短 TR→T1 恢复时间短, 值更大 →T1 对比度上升.
减短 TE→ 影响 T2 对比度, 增加信噪比(噪声减少)

三种成像: T1 加权; 质子密度; T2 加权
T1: TR TE 短, TE 短 T2 影响小, 信号幅度高.
T1: TR TE 长, TR 长 T1 影响小, 信号幅度低.
质子: TR 长, TE 短, T1T2 影响都小.

**自由感应衰减 FID**
自由感应衰减: 正余弦 × 指数衰减

傅里叶变换后: 余弦信号 $\frac12[\delta(\omega-\omega_0)+\delta(\omega+\omega_0)]$.
指数信号: $\frac{a+i\omega}{a^2+\omega^2},a = 1/T_2$.

==幅度和频率, 第四份 PPT==

**化学位移**
$B_{eff} = B_0(1-\sigma), \omega_{eff} = \omega_0(1-\sigma)$ → $\Delta f = \frac\gamma {2\pi}B_0\times \Delta\delta\times10^{-6}$
结论: $B_0$ 和尖峰的间隔之间有线性关系.
化学位移: 由于轨道电子的屏蔽而引起的共振频率的小位移.

### 梯度线圈

#### 线圈

z 方向梯度: 麦克斯韦线圈, 同螺线管绕在环上
xy 方向梯度: 格雷线圈, 方形框贴在环上

参数: Gmax 最大梯度, dB/dx; slew rate 上升的最大值的速度.

#### 选层

$f_{RF} = \gamma(B_0+GL)$.
选层位置: G 不变更改 RF 中心频率; RF 不变更改 G 大小
选层厚度: G 不变, RF 和厚度正比; RF 不变, 厚度和 G 反比.

#### 编码

**频率编码**: 编码后经过傅里叶变换, 不同频率对应不同的锋.
**相位编码**: 编码相位

y 方向为相位编码, 优先利用 $G_y$ 生成, 生成后关闭, 保留固定相位; x 方向为频率编码, 打开 $G_x$ 后即产生.

y 方向数量决定重复采集的次数(k 空间的列数), 频率编码采集到可以区分(每次填充了 k 空间的一行).

具体过程案例: 打开 $G_z$, 给予 RF 脉冲, 给完 $G_z$ 给予负值完成抵消. 打开 $G_y$, $G_x$ 负值抵消, 生成相位差. $G_x$ 给予正值, 等待采集(时刻为 TE).
若为 Spin Echo, 则先 90° 脉冲, 时间为 TE/2 时给予 180° 脉冲, 而后开始编码.

#### K 空间

$F(k_x,k_y) = \sum_{x,y}f(x,y)e^{-i2\pi(k_xx+k_yy)}$, $f(x,y)$ 对应信号

坐标轴: $k_x,k_y$.
$$
\Delta\omega = \gamma xG, \Delta\phi = \gamma xGt = 2π(kx)\\
k_x = \frac{\gamma}{2\pi}\int_0^tG_x(\tau)d\tau, k_y = \frac{\gamma}{2\pi}\int_0^tG_y(\tau)d\tau
$$
中心部分低频, 对应模糊图像, 可见基本轮廓.决定对比度
外周部分高频, 对应精细细节, 但不可见物体.决定分辨力

#### FFT 和 Radon

## 成像

(但是这部分上面都写过了?这里不写了，不过要注意这部分是整个MRI体系中最为重要的地方，重在理解，公式不多，可以看《微雕》

### TR 和 TE

### 成像信号公式



