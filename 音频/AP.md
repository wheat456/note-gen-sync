# 测量核心参数
| **指标名称**                            | **简单理解**       | **理想结果**                     |
| ----------------------------------- | -------------- | ---------------------------- |
| **THD+N (总谐波失真+噪声)**                | 放大后的声音“纯净度”。   | 数值越小越好（例如 < 0.1% 或 -80 dB）。  |
| **Frequency Response (频响)**         | 功放对高低音的放大是否公平。 | 曲线越平越好（20Hz - 20kHz 波动越小越好）。 |
| **Signal-to-Noise Ratio (SNR/信噪比)** | 音乐声比底噪大多少。     | 数值越大越好（通常要求 > 90 dB）。        |
| **Maximum Output Power (最大输出功率)**   | 功放在失真爆炸前能推多大声。 | 达到厂家标注的额定功率。                 |
# Level and Gain 

## Level  电平
- **Level = 信号电压大小，是绝对值。** 指某一点信号的实际电压有效值 Vrms，在仪器里通常用对数单位：**dBV** 
	- 1 Vrms = 0 dBV
	- 0.1 Vrms = −20 dBV
	- 0.01 Vrms = −40 dBV

- **Gen Level（信号源输出电平）**：AP 发给功放输入端的信号大小
- **Ana Level（分析仪输入电平）**：功放输出到 AP 采集口的信号大小

## Gain（增益）
- Gain = 放大倍数，相对值，输入和输出的比值。
- 功放把信号放大多少倍，音频行业习惯用 dB 表示增益
	- 电压放大 2 倍 → Gain ≈ +6 dB
	- 电压放大 10 倍 → Gain ≈ +20 dB
	- 输出等于输入 → Gain = 0 dB
	- 衰减一半 → Gain ≈ −6 dB

# THD+N
THD = 纯谐波失真；Noise = 电路底噪；THD+N = 谐波 + 噪声总和
## THD+N Ratio
- 总谐波失真加噪声比，单位：%/dB
- 在1$kHz$输入下，出现的其他频率的波形和噪声
$$THD+N_{all}=\frac{\sqrt{V_{THD}^2+V_{Noise}^2}}{V_{base}}$$

## THD+N Level
- 谐波和噪声电压的绝对值
$$THD+N_{all}=\sqrt{V_{THD}^2+V_{Noise}^2}$$
## THD Ratio
- 单位：%
$$THD=\frac{\sqrt{V_1^{2}+V_2^{2}+V_3^{2}+...}}{V_{base}}$$

## THD Level
$$V_{THD}=\sqrt{V_1^{2}+V_2^{2}+V_3^{2}+...}$$
## Noise Ratio
$$ Noise Ratio =\frac{V_{noise}}{V_{base}}$$
## Noise Level
- 无绝对值
## Distortion Product
- 互调失真产物
- AP 同时输出**两个不同频率正弦波**（标准常用 19kHz + 20kHz 双音）输入功放；功放非线性会生成全新的差频 / 和频杂波（本例：1kHz = 20k-19k），这些新产生的频率分量，就叫 **Distortion Product（失真产物）**；
### Ratio
$$DP Ratio=\frac{所有互调失真产物总电压}{双音信号中较低那一路的基波电压}$$
### Level
$$V_{DP}=\sqrt{V_{1k}^{2}+V_{2k}^{2}+V_{39k}^{2}+...}$$
# Frequency Response
### RMS Level
### Gain
### Relative Level
- 以**1kHz 频点的输出增益作为 0dB 参考基准**，全频段所有频率的增益都和 1kHz 做差值对比，单位 dB
### Deviation
- 全频段偏差曲线，计算每个频点和**整条扫频曲线平均值 / 平直理想 0dB 线**的偏差，或者理解为「整条曲线偏离完全平直的误差」。 括号内 20Hz–20kHz 代表计算区间是完整音频带宽

# Signal to Noise Ratio
- 功放关输入信号时，本身会有微弱电路噪声；有正常音乐信号时，有用信号越大、噪声越小，SNR 数值就越高，听感越干净、无杂音。 单位：dB，**数值越大越好**。

$$SNR(dB)=20log_{10}\frac{V_{signal}}{V_{noise}}$$
# Crosstalk
- 立体声功放分左 (L)、右 (R) 两个独立声道；
- **Crosstalk = 串音**：某一个声道的音频信号，通过电路、地线、PCB 漏到另一个无声通道里的干扰信号。

# interchannel Phase
- 左右两通道输出信号的相位差值，单位：°（度）
# CMRR
- Common Mode Rejection Ratio = 共模抑制比
- 专门针对**平衡差分输入功放**
1. **差模信号（Differential Mode / DM）** 真正有用的音频信号：平衡线 +、- 两端电压不一样，差值就是声音信号，功放需要**放大**这个信号。
2. **共模信号（Common Mode / CM）** 干扰噪声：+、- 两端**同时出现一模一样**的杂波（50Hz 工频、地线环路噪声、电源纹波、线缆辐射干扰），功放需要**抑制、过滤**掉这个信号。
$$CMRR(dB)=20log_{10}\frac{A_{差模增益}}{A_{共模增益}}$$
- 单位 dB，**数值越大越好**

