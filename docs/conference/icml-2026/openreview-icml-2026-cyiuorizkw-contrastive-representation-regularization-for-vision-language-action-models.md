---
title: Contrastive Representation Regularization for Vision-Language-Action Models
title_zh: 面向视觉-语言-动作模型的对比表示正则化
authors: "Taeyoung Kim, Jimin Lee, Myungkyu Koo, Dongyoung Kim, Kyungmin Lee, Changyeon Kim, Younggyo Seo, Jinwoo Shin"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0cc7ec26fa9f47ae531d226ac90d7d42c7e48b94.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 对比表示正则化桥接VLM和机器人信号以改进VLA
tldr: VLA模型从预训练VLM获取表示，但这些表示缺乏对机器人控制信号（如动作和本体感知）的敏感性。本文提出RS-CL损失，利用状态间相对距离作为软监督，将VLA表示与机器人本体状态对齐。该正则化项与原始动作预测损失互补。在多个模拟和真实机器人操控任务上，RS-CL显著提升任务成功率和泛化能力，且不增加推理开销。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型使用的VLM表示对机器人控制信号敏感度不足，限制了下游任务性能。
method: 提出Robot State-aware Contrastive Loss (RS-CL)，通过状态间距离软监督对齐表示与本体感知。
result: 在模拟和真实机器人操控任务上，RS-CL持续提升VLA模型的任务成功率和泛化性。
conclusion: RS-CL是一种简单有效的表示正则化方法，可即插即用于现有VLA模型。
---

## Abstract
Vision-Language-Action (VLA) models have shown strong capabilities in robot manipulation by leveraging rich representations from pre-trained Vision-Language Models (VLMs).
However, their representations arguably remain suboptimal, lacking sensitivity to robotic signals such as control actions and proprioceptive information. 
To address the issue, we introduce Robot State-aware Contrastive Loss (RS-CL), a simple and effective representation regularization for VLA models, designed to bridge the gap between VLM representations and robotic signals.
In particular, RS-CL aligns the representations more closely with the robot's proprioceptive states by using relative distances between the states as soft supervision.
Complementing the original action prediction objective, RS-CL enhances control-relevant representation learning, while being lightweight and fully compatible with standard VLA training pipelines.
Our empirical results demonstrate that RS-CL substantially improves the performance of state-of-the-art VLA models; it pushes the prior art to 69.7% achieving the state-of-the-art performance on the RoboCasa-Kitchen benchmark, and boosts success rates from 45.0% to 58.3% on challenging real-robot manipulation tasks.

---

## 论文详细总结（自动生成）

# 面向视觉-语言-动作模型的对比表示正则化

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有视觉-语言-动作（VLA）模型依赖预训练视觉-语言模型（VLM）提供的丰富表示，但这些表示对机器人控制信号（如动作指令、本体感知信息）的敏感性不足，导致下游操作任务性能受限。
- **整体含义**：旨在弥合VLM表示与机器人控制信号之间的鸿沟，通过一种轻量级正则化方法提升VLA模型在操控任务中的表现，使模型更好地利用本体状态信息，从而提高任务成功率和泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出**Robot State-aware Contrastive Loss（RS-CL）**，利用机器人状态之间的相对距离作为软监督，将VLA模型的表示与本体感知状态进行对齐，从而增强表示对控制信号的敏感性。
- **关键技术细节**：
  - RS-CL是一种对比学习损失，以状态（如关节位置、末端执行器位姿）间的相对距离作为正负样本划分的软标签。
  - 该损失与原始的动作预测损失（如行为克隆损失）互补，共同优化VLA模型。
  - RS-CL不改变模型推理结构，仅增加训练时的正则化项，因此可在现有VLA训练流程中即插即用，无额外推理开销。
- **公式流程说明**（文字）：
  - 输入：批量轨迹中的状态序列，每个状态对应一个机器人本体观测（如关节角度）。
  - 计算：对每个样本，根据其他样本的状态与本样本的相对距离（例如欧氏距离的归一化值）生成软对比权重，使表示空间中相近状态对应的嵌入更靠近，远离状态的嵌入更分离。
  - 损失形式：类似于基于距离的对比损失，但使用连续权值而非离散正负对。

## 3. 实验设计：数据集、场景及对比方法
- **数据集与场景**：
  - 模拟环境：RoboCasa-Kitchen基准（包含多种厨房操作任务）。
  - 真实机器人：挑战性的真实机器人操控任务（具体任务未详述，但成功率从45.0%提升至58.3%）。
- **对比方法**：
  - 基线为当前的先进VLA模型（prior art），未具体命名，但RS-CL将其在RoboCasa-Kitchen上的性能从基线水平（未明确）提升至69.7%的SOTA。
  - 在真实机器人上，对比了不加RS-CL的原始VLA模型（成功率45.0%）。
- **评价指标**：任务成功率（Success Rate）。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及RS-CL是轻量级的，但未给出具体资源消耗数据。

## 5. 实验数量与充分性
- 实验数量：
  - 一个模拟基准（RoboCasa-Kitchen）和一个真实机器人场景。
  - 可能包含消融实验（评估RS-CL的单独贡献），但摘要未明确列出。
- 充分性与客观性：
  - 模拟与真实环境相结合，验证了方法的泛化性。
  - 对比了基线和SOTA，结果提升明显，但仍需更多不同任务类型的数据集（如多种仿真环境、不同机器人形态）来证明其广泛适用性。
  - 缺乏对超参数敏感性和不同VLM骨干的分析，可能影响公平性的全面讨论。

## 6. 论文的主要结论与发现
- RS-CL能显著提升VLA模型在模拟与真实机器人操控任务上的成功率，在RoboCasa-Kitchen上达到69.7%的SOTA，在真实任务上从45.0%提升至58.3%。
- RS-CL是一种简单、有效且轻量的表示正则化方法，无需增加推理开销，可即插即用于现有VLA训练框架。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 创新性地将本体状态间的相对距离作为软监督用于表示对齐，避免了硬正负对划分的局限性。
  - 互补于原始动作预测损失，不破坏原有训练目标。
  - 零额外推理成本，实用性强。
- **实验亮点**：
  - 同时包含模拟和真实机器人实验，证明了方法的实际可行性。
  - 取得了新的SOTA，提升幅度可观（真实场景提升13.3%）。

## 8. 不足与局限
- **实验覆盖不足**：
  - 仅使用一个模拟基准和一个真实任务，缺乏更多样化的环境（如多种机械臂、不同任务难度的基准）验证。
  - 未报告在多种VLA架构（如RT-2、Octo等）上的结果，泛化性证据有限。
- **偏差风险**：
  - 距离度量选择（欧氏距离）可能不完全适应所有状态空间，未探讨其他距离度量。
  - 软对比权重的温度参数等超参数影响未在摘要中讨论。
- **应用限制**：
  - 依赖可获得的完整状态观测（如关节角度），若状态部分不可观测或噪声大，性能可能下降。
  - 未考虑动态环境或人类交互场景。
- **资源信息缺失**：缺少算力与训练开销的具体数据，难以评估实际部署成本。

（完）
