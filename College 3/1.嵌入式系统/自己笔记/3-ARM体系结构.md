# 第三章、ARM 体系结构

## Chapter1、ARM 体系结构主要特点

1. Load/Store 结构
2. 3 地址的数据处理指令
3. 强大的多寄存器 Load/Store 指令  

### 固定的32 位指令  


## Chapter2、ARM 体系结构的各种版本

- V4（目前商用最老）：开始支持 Thumb 指令
- V4T：增加了 16 位 Thumb 指令集
- V5TE：增强 Thumb 体系，更好平衡代码空间和性能；在 ARM ISA 上扩展了增强的 DSP 指令集；（命名中的 E 表示）
- V6：多项改进，尤其在视频音频处理方面；
- V7：最新的版本

Thumb 指令在处理器中仍为 32 位，但用户采用 16 位，获得更高代码密度

## Chapter3、ARM 处理器系列

**按体系结构分**：
- V5TE：ARM7、ARM9
- V6：ARM9、ARM10
- V7：ARM11、ARM Cortex

**按应用特征**：
- 应用处理器、实时控制处理器、微控制器（性能上递减）
- Cortex-A、Cortex-R、Cortex-M

目前最快的嵌入式处理器之一：Cortex-A9；最便宜的之一：Cortex-M3

---