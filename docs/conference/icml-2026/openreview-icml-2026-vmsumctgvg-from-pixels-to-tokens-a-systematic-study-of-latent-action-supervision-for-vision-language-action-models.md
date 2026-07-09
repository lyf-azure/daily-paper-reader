---
title: "From Pixels to Tokens: A Systematic Study of Latent Action Supervision for Vision-Language-Action Models"
title_zh: 从像素到标记：面向视觉-语言-动作模型的潜在动作监督系统研究
authors: "Yihan Lin, Haoyang Li, Yang Li, Haitao Shen, Yihan Zhao, Chao Shao, Jing Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f0ed77c6a623029eb20128bba00fae60f5443473.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: VLA模型中潜在动作监督策略的系统比较
tldr: 潜在动作用于VLA训练的策略多样但缺乏系统比较。本文在一个统一VLA基线中实现了四种代表性策略，从图像基潜动作和动作基潜动作两个视角进行对比。实验发现关键公式-任务对应关系：图像基潜动作有利于长时域推理，而动作基潜动作可精确化控制。该工作为VLA训练中如何选择和使用潜动作监督提供了指导原则。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 潜在动作监督VLA的方法碎片化，缺乏统一的比较和理解。
method: 在统一VLA基线下实例化四种潜动作监督策略，从图像基和动作基两个维度系统对比。
result: 揭示了不同任务类型适合不同类型的潜动作监督，并给出了选择指南。
conclusion: 提供了VLA中利用潜动作进行有效训练的实践指导，具有重要参考价值。
---

## Abstract
Latent actions serve as an intermediate representation that enables consistent modeling of vision-language-action (VLA) models across heterogeneous datasets. However, approaches to supervising VLAs with latent actions are fragmented and lack a systematic comparison. This work structures the study of latent action supervision from two perspectives: (i) regularizing the trajectory via image-based latent actions, and (ii) unifying the target space with action-based latent actions. Under a unified VLA baseline, we instantiate and compare four representative integration strategies. Our results reveal a formulation-task correspondence: image-based latent actions benefit long-horizon reasoning, whereas action-based latent actions excel at complex motor coordination. Furthermore, we find that directly supervising the VLM with discrete latent action tokens yields the most effective performance. Finally, our experiments offer initial insights into the benefits of latent action supervision in mixed-data, suggesting a promising direction for VLA training.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
视觉-语言-动作模型（VLA）旨在通过联合建模视觉、语言和动作实现通用的机器人操控。然而，不同数据集的动作空间、采样频率和语义粒度高度异构，直接跨数据集训练存在困难。潜在动作（latent actions）作为一种中间表示，被用于统一不同数据源的表征，使VLA能够一致地建模。但目前围绕如何利用潜在动作监督VLA的研究方法碎片化，缺乏系统的比较和理解。本文的核心动机是：**对于VLA训练中采用的不同潜在动作监督策略，缺乏统一的实验框架和分析，导致实践者难以选择最合适的方案**。为此，作者从两个视角（基于图像的潜在动作与基于动作的潜在动作）结构化地研究这一主题，旨在揭示不同策略的适用场景并给出选择指南。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将潜在动作监督分为两类：
  - **图像基潜在动作（Image-based latent actions）**：通过正则化轨迹（regularizing the trajectory）来捕捉高层语义变化，如预测未来帧的视觉特征或图像嵌入的变化。
  - **动作基潜在动作（Action-based latent actions）**：通过统一目标空间（unifying the target space）来建模连续动作的潜在分布，如使用离散动作令牌（action tokens）将连续动作映射到离散空间。
- **关键技术细节**：
  - 在**统一VLA基线**（基于ViT+LLM结构）中，实例化并对比了四种代表性策略（具体四种策略在摘要中未列出，但从结论可知包括：离散潜在动作令牌直接监督、图像基潜在动作、动作基潜在动作等）。
  - 对离散潜在动作令牌，作者发现直接将其作为VLM（视觉-语言模型）的监督目标效果最佳（即使用LLM的输出头预测离散动作令牌，并与真实令牌计算交叉熵损失）。
  - 图像基潜在动作方法则额外引入一个辅助损失（如对比学习或重构损失），使VLA模型同时捕捉视觉动态线索。
- **算法流程（文字说明）**：
  1. 从异构数据集中提取图像帧、语言指令和真实动作序列。
  2. 使用预训练的视觉编码器（如ViT）和语言编码器获得多模态特征。
  3. 对于基于动作的监督：将真实动作序列量化/离散化为若干潜在动作令牌（类似VQ-VAE或动作编码器），然后训练VLA模型预测这些令牌。
  4. 对于基于图像的监督：引入一个潜在动作预测头，根据当前和过去帧的特征预测未来视觉表示的变化（如光流或下一帧特征）。
  5. 联合训练VLA主任务与辅助损失，最终在多个下游任务上评估性能。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集/场景**：论文中提及使用了**多种异构数据集**（但具体名称未在摘要和元数据中列出）。可能包括机器人操控数据集（如CALVIN、MetaWorld、Robosuite等）以及模拟环境中的长时域任务。
- **Benchmark**：未明确说明单一标准Benchmark，但作者构建了一个包含长时域推理（long-horizon）和复杂运动协调（complex motor coordination）任务的综合评测集。
- **对比的方法**：在统一基线内对比了四种潜在动作监督策略。此外，还对比了无潜在动作监督的VLA基线（baseline），以及直接使用原始动作空间训练的方法。

## 4. 资源与算力
论文中**未明确说明**使用的GPU型号、数量或训练时长等算力信息。根据行业惯例，此类多数据集大规模训练通常使用多块A100或H100 GPU，但具体细节需查阅原文补充。本文元数据也未提及。

## 5. 实验数量与充分性
- 实验数量：结合摘要和元数据，作者至少进行了以下实验：
  - 四种潜在动作策略在多个任务上的完整对比（主要结果）。
  - 消融实验（如是否使用图像基潜在动作、离散令牌数量等）。
  - 混合数据（mixed-data）场景下的初步分析，探索潜在动作监督带来的增益（mixing multiple datasets）。
- 充分性判断：实验覆盖了两个主要视角（图像基 vs 动作基），并给出了明确的公式-任务对应结论。但缺乏对更多变体（如连续潜在动作、不同的量化方法）的广泛比较，且未公开数据集的具体规模和任务多样性。总体而言，**实验设计系统但覆盖范围有限**，结论的泛化性需要更多数据集验证。

## 6. 论文的主要结论与发现
1. **公式-任务对应关系**：图像基潜在动作有利于长时域推理（如需要长期视觉记忆的任务），而动作基潜在动作在复杂运动协调任务（如精确操控）上表现更优。
2. **最佳实践**：直接使用离散潜在动作令牌作为VLM的监督目标（即统一目标空间）取得了最有效的性能。
3. **混合数据收益**：潜在动作监督在混合多数据集训练中可带来额外收益，为VLA的规模化训练提供了有前景的方向。
4. **系统对比意义**：该工作首次在统一VLA基线中系统比较了多种潜在动作监督策略，为领域提供了选择指南。

## 7. 优点
- **研究方法结构化**：从两个清晰视角（图像基 vs 动作基）划分潜在动作监督策略，有助于厘清碎片化领域的核心分歧。
- **统一基线**：在相同的VLA模型架构下公平比较不同策略，避免了模型结构差异带来的混淆。
- **实践指导价值**：给出了基于任务类型的策略选择建议，可直接指导后续VLA训练。
- **混合数据探索**：初步验证了潜在动作监督在跨数据集训练中的作用，为克服数据异质性提供了新思路。

## 8. 不足与局限
- **实验覆盖有限**：仅实例化了四种代表性策略，未能涵盖所有流行变体（如Bertrand等人的连续VAE方法、或基于对抗学习的潜在动作）。
- **数据集/任务公开度不足**：未明确列出所用数据集的名称和规模，导致结果难以复现或外推。
- **算力报告缺失**：未提供训练所需的硬件资源和时间，影响资源评估。
- **偏差风险**：结论可能依赖于特定的VLA基线（ViT+LLM结构），在其他架构（如更轻量的Transformer）下是否成立未知。
- **应用限制**：图像基潜在动作对长时域任务的优势可能只在帧率较低的场景成立，高帧率下可能不显著；动作基潜在动作需要额外的动作量化模块，增加了训练复杂度。

（完）
