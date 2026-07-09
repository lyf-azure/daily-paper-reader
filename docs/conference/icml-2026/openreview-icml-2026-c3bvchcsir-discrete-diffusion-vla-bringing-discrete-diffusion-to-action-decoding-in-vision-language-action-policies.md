---
title: "Discrete Diffusion VLA: Bringing Discrete Diffusion to Action Decoding in Vision-Language-Action Policies"
title_zh: "离散扩散VLA: 将离散扩散引入视觉-语言-动作策略的动作解码"
authors: "Zhixuan Liang, Yizhuo Li, Tianshuo Yang, Chengyue Wu, Sitong Mao, Liuao Pei, Tian Nian, Shunbo Zhou, Xiaokang Yang, Jiangmiao Pang, Yao Mu, Ping Luo"
date: 2026-04-30
pdf: "https://openreview.net/pdf/7c6c1101cef920f79b251ef422b6399d7e8f4ae1.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 在VLA骨干内使用离散扩散实现统一动作解码
tldr: 现有VLA要么使用固定顺序自回归解码（性能差），要么在骨干外附加扩散头（碎片化）。Discrete Diffusion VLA将动作块离散化并用离散扩散模式在统一transformer骨干内进行渐进式精炼，实现自适应解码顺序，避免信息碎片化，提升了动作质量和架构可扩展性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA动作解码方式存在性能差或架构碎片化问题。
method: 将动作离散化，在统一transformer骨干内用离散扩散模式进行渐进式精炼和自适应解码。
result: 在多个操作任务上，该方法在动作准确性和架构简洁性上优于自回归和外部扩散头方法。
conclusion: 离散扩散能有效统一VLA动作解码，提升性能和可扩展性。
---

## Abstract
Vision–Language–Action (VLA) models adapt large vision–language backbones to map images and instructions into robot actions. However, prevailing VLAs either generate actions autoregressively in a fixed left-to-right order with poor performance or attach separate diffusion heads outside the backbone that fragments information pathways and hinders unified, scalable architectures. Instead, we present Discrete Diffusion VLA that discretizes action chunks and models them with discrete diffusion pattern retaining progressive refinement inside the unified transformer backbone. Our method achieves an adaptive decoding order that resolves high-confidence action elements before harder ones and employs secondary re-masking to revisit uncertain predictions, enabling robust error correction. This design preserves pretrained vision-language priors, supports parallel decoding, and improves the efficiency. Discrete Diffusion VLA achieves 96.4% avg. success on LIBERO, 71.2% visual matching on SimplerEnv-Fractal, and 54.2% overall on SimplerEnv-Bridge. On out-of-distribution tests of LIBERO-Goal, our method exhibits only 0.8% language degradation versus 8.0% of parallel decoding, and 20.4% vision degradation versus 29.0% for continuous diffusion, demonstrating well retention of pretrained vision-language capabilities. We also conduct two real-robot evaluations on AgileX Cobot Magic platform to show the method's effectiveness.

---

## 论文详细总结（自动生成）

# 中文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有的视觉-语言-动作（VLA）模型在动作解码方面存在两种主流但均有缺陷的方式：一是采用固定从左到右顺序的自回归解码，性能差；二是在骨干网络外部附加独立的扩散头，导致信息通路碎片化，阻碍了统一、可扩展的架构。
- **整体含义**：本文旨在设计一种在统一Transformer骨干内部实现渐进式精炼的动作解码方法，以保留预训练的视觉-语言先验知识，提升动作质量、解码效率与架构简洁性。

## 2. 论文提出的方法论
### 核心思想
- 将连续动作块**离散化**，并采用**离散扩散模式**在统一的Transformer骨干网络内部进行渐进式精炼，替代外部扩散头或固定顺序自回归解码。

### 关键技术细节
- **自适应解码顺序**：先解析高置信度的动作元素，再处理困难的元素，避免固定顺序的局限性。
- **二次重掩码（secondary re-masking）**：对置信度低的预测进行重新掩码并再次精炼，实现鲁棒的错误纠正。
- **并行解码**：支持多个动作元素同时生成，提升解码效率。
- **保留预训练先验**：由于扩散过程在骨干内部进行，不破坏原有视觉-语言模型的权重和表示。

### 算法流程（文字说明）
1. 输入图像和语言指令，通过预训练视觉-语言骨干提取多模态特征。
2. 将待预测的动作块（如连续动作序列）离散化为离散标记。
3. 在统一Transformer骨干内部，从完全掩码的离散动作标记开始，按照离散扩散的前向过程逐渐加入噪声，再通过反向过程（由Transformer的attention和FFN实现）逐步预测原始离散标记。
4. 利用自适应解码顺序（基于置信度）和二次重掩码机制，迭代修正错误，直到所有动作标记被确定。
5. 输出离散动作标记，解码回连续动作空间。

## 3. 实验设计
### 数据集与场景
- **LIBERO**：用于评估任务成功率（平均96.4%）。
- **SimplerEnv-Fractal**：视觉匹配任务（71.2%）。
- **SimplerEnv-Bridge**：总体成功率（54.2%）。
- **LIBERO-Goal**：用于分布外（OOD）测试，评估语言和视觉退化程度。

### 对比方法
- 自回归解码（固定顺序）
- 并行解码（其他论文方法）
- 连续扩散（在骨干外附加扩散头）

### 基准（Benchmark）
- 主要基准为上述三个环境的标准任务集，以及OOD鲁棒性测试。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中未提及使用的GPU型号、数量、训练时长等具体信息。因此无法总结算力细节。

## 5. 实验数量与充分性
### 实验数量
- 至少包含三类数据集（LIBERO、SimplerEnv-Fractal、SimplerEnv-Bridge）的主要结果，以及针对LIBERO-Goal的OOD鲁棒性对比。
- 对比了三种基线方法（自回归、并行解码、连续扩散）。
- 额外进行了两个真实机器人（AgileX Cobot Magic平台）的评估。

### 充分性与公平性
- 实验覆盖了模拟和真实场景，并专门测试了分布外泛化能力，说明对鲁棒性有较好评估。
- 对比基线选择合理，既有自回归也有扩散类方法。
- 但未提供消融实验细节（如单独验证自适应解码顺序、二次重掩码等组件），也未报告多次运行的标准差或置信区间。因此**充分性中等**——主要性能指标齐全，但消融和统计显著性尚不明确。

## 6. 论文的主要结论与发现
- 离散扩散VLA在LIBERO上平均成功率96.4%，SimplerEnv-Fractal视觉匹配71.2%，SimplerEnv-Bridge总体54.2%。
- 在LIBERO-Goal OOD测试中，语言退化仅0.8%（并行解码退化8.0%），视觉退化20.4%（连续扩散退化29.0%），表明方法能很好地保留预训练的视觉-语言能力。
- 真实机器人实验进一步验证了有效性。

## 7. 优点
- **架构简洁统一**：将扩散过程嵌入Transformer骨干，避免了外部扩散头带来的碎片化。
- **自适应解码与错误纠正**：通过置信度引导解码顺序和二次重掩码，提升动作质量。
- **高效的并行解码**：突破自回归串行限制。
- **强大的先验保留**：OOD测试中退化显著低于基线，证明了方法的鲁棒性。
- **实验全面**：包含模拟和真实机器人，以及分布外测试。

## 8. 不足与局限
- **缺乏计算资源描述**：无法评估方法的实际训练成本和可复现性。
- **未提供消融实验**：不清楚各组件（离散化、自适应顺序、二次重掩码）的贡献大小。
- **统计报告不完善**：仅有单点结果，无方差、置信区间等，可能难以判断随机性影响。
- **泛化范围有限**：仅在三个模拟环境和两种真实机器人设置下测试，更多复杂场景（如动态干扰、多任务组合）未涉及。
- **离散化精度**：动作离散化可能引入量化误差，文中未讨论对高精度任务的影响。
- **仅与有限基线对比**：未与最新的非扩散类VLA（如基于流匹配、transformer直接回归）对比。
- **应用限制**：对实时性要求极高的任务，扩散迭代次数可能带来延迟，文中未给出推理时间分析。

（完）
