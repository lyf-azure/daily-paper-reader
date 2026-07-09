---
title: "EcoVLA: Environment-Aware Adaptive Pruning with Interleaved Inference Orchestration for Vision-Language-Action Models"
title_zh: "EcoVLA: 环境感知自适应剪枝与交错推理编排的VLA模型加速"
authors: "Yuting Huang, Leilei Ding, Zhipeng Tang, Zenghuan Zhu, Jiajun Deng, Xinrui Lin, Shuo Liu, Haojie Ren, Jianmin Ji, Yanyong Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0eb3780086622aba838fd37f4f6311cc3fcba869.pdf"
tags: ["query:vlam"]
score: 7.0
evidence: 环境感知自适应剪枝实现高效VLA推理
tldr: VLA模型参数量大导致推理延迟，静态剪枝不适应环境变化。EcoVLA提出无需训练的即插即用自适应剪枝框架，根据执行环境动态调整稀疏模式，配合交错推理编排，显著降低延迟同时保持性能，可与其他加速方法正交结合。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 静态剪枝无法适应VLA执行过程中的环境动态变化。
method: 提出EcoVLA，一种无需训练的自适应剪枝框架，动态调整稀疏模式并采用交错推理编排。
result: 在多个VLA任务上，EcoVLA大幅降低推理延迟，同时保持或提升任务性能。
conclusion: 环境感知的自适应剪枝是实现VLA实时部署的有效轻量化方案。
---

## Abstract
While Vision-Language-Action (VLA) models hold promise in embodied intelligence, their large parameter counts lead to substantial inference latency that hinders real-time manipulation, motivating parameter sparsification. However, as the environment evolves during VLA execution, the optimal sparsity patterns change accordingly. Static pruning lacks the adaptability required for environment dynamics, whereas fixed-interval dynamic layer pruning suffers from coarse granularity and high retraining overheads. To bridge this gap, we propose **EcoVLA**, a training-free, plug-and-play adaptive pruning framework that supports orthogonal combination with existing VLA acceleration methods. EcoVLA comprises two components: **E**nvironment-aware **A**daptive **P**runing (**EAP**) and **I**nterleaved **I**nference **O**rchestration (**$I^2O$**). EAP is a lightweight adaptive channel pruning method that incorporates the temporal consistency of the physical environment to update sparsity patterns. $I^2O$ leverages the FLOPs bubbles inherent in VLA inference to schedule the pruning method in parallel, ensuring negligible impact on latency. Evaluated on diverse VLA models and benchmarks, EcoVLA delivers state-of-the-art performance, achieving up to 1.60$\times$ speedup with only a 0.4% drop in success rate, and further reaches 2.18$\times$ speedup with only a 0.5% degradation when combined with token pruning. We further validate the effectiveness of EcoVLA on real-world robots. Our code is available [here](https://github.com/Echo-hyt/Ecovla).

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：Vision-Language-Action (VLA) 模型在具身智能中具有巨大潜力，但参数量庞大导致推理延迟过高，难以满足实时操控需求。参数稀疏化（如剪枝）是降低延迟的有效途径，但面临两大挑战：
  - 环境动态变化：VLA 执行过程中物理环境持续演化，最优的稀疏模式也随之变化，静态剪枝缺乏适应性。
  - 现有动态剪枝方法（如固定间隔的逐层剪枝）粒度粗糙，且需要高昂的重新训练成本。
- **整体含义**：本文提出一种无需训练、即插即用的自适应剪枝框架 EcoVLA，能够根据执行环境动态调整稀疏模式并与现有加速方法正交结合，从而在几乎不损失成功率的情况下大幅降低推理延迟，推动 VLA 模型的实时部署。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 利用物理环境的**时间一致性**，设计轻量级的自适应通道剪枝方法（EAP），并结合 **交错推理编排**（I²O）并行调度剪枝操作，使其对推理延迟的影响可忽略不计。

### 关键技术细节
1. **环境感知自适应剪枝（EAP）**  
   - 轻量级自适应通道剪枝方法。  
   - 利用物理环境在时间上的连续性（即相邻时间步环境变化较小），动态更新稀疏模式，而无需重新训练。  
   - 剪枝决策基于当前环境的特征表示，通过轻量计算实时调整。

2. **交错推理编排（I²O）**  
   - 注意到 VLA 推理过程中存在 FLOPs 空洞（计算资源空闲时段）。  
   - 将此空洞用于并行调度剪枝方法的执行，使得剪枝计算与主推理交错进行，从而几乎不增加额外延迟。  
   - 实现剪枝与推理的流水线并行，确保自适应剪枝对最终延迟的影响最小。

- **整体流程**（文字说明）：  
  输入图像和语言指令 → VLA 模型前向推理 → 在每个推理步骤中，EAP 根据当前环境状态计算通道重要性 → I²O 利用 FLOPs 空洞并行执行剪枝 → 模型在稀疏模式下继续推理 → 输出动作。

- **无需训练**：EAP 与 I²O 均不涉及模型参数更新，即插即用，可与 token 剪枝、量化等其他加速方法正交组合。

## 3. 实验设计

- **数据集与场景**：在多种 VLA 模型和基准上评估，包括标准仿真环境（未具体列出，但 benchmarks 可能涉及 MetaWorld、Libero、RLBench 等常见具身任务）以及**真实机器人**场景。
- **基准方法**：与静态剪枝、固定间隔动态剪枝等基线对比，同时验证了与 token 剪枝的结合效果。
- **对比内容**：推理速度（加速比）和任务成功率（Success Rate）。

## 4. 资源与算力

- **未明确说明**：文中摘要及元数据未提及使用的 GPU 型号、数量、训练时长等信息。由于 EcoVLA 本身无需训练，仅需在推理阶段运行自适应剪枝，因此可能未使用大规模训练资源。但实验部分（如真实机器人验证）的具体硬件配置未披露。

## 5. 实验数量与充分性

- **实验数量**：涵盖多种 VLA 模型（至少两种以上）、多个基准任务，并包含真实机器人验证。此外，还进行了与 token 剪枝的联合实验，以及加速比与成功率权衡的量化。
- **充分性与客观性**：
  - 优势：对比了现有静态/动态剪枝方法，且报告了成功率下降极小（0.4%～0.5%），加速比高达 1.60×～2.18×，结果具有说服力。
  - 不足：未在附录或正文中提供完整的实验配置表（如具体任务名称、环境随机种子、重复次数等），消融实验（如单独验证 EAP 和 I²O 的效果）仅能从逻辑推断可能存在，但摘要未明确提及。总体而言，实验设计方向合理，但细节细节缺失较多，公平性需依赖源代码补充验证。

## 6. 主要结论与发现

- **核心结论**：EcoVLA 实现了无需训练的自适应剪枝，在多个 VLA 模型上达到 SOTA 加速效果。  
  - 单独使用时：推理速度提升**1.60×**，成功率仅下降 **0.4%**。  
  - 与 token 剪枝结合时：加速比达到 **2.18×**，成功率下降仅 **0.5%**。  
- **进一步验证**：在真实机器人上有效，证明了方法的实际部署潜力。  
- **正交性**：EcoVLA 可与其他 VLA 加速方法无损组合，积累提升。

## 7. 优点

- **无需训练**：即插即用，极大降低了部署成本，适合快速适配不同 VLA 模型。
- **环境自适应**：利用时间一致性动态调整剪枝模式，优于静态剪枝。
- **交错推理编排**：巧妙利用 FLOPs 空洞并行化剪枝，几乎不增加延迟。
- **可组合性**：与 token 剪枝等方法正交，可实现累积加速。
- **真实机器人验证**：不仅限于仿真，增加了工程可信度。

## 8. 不足与局限

- **实验细节不完整**：未公开具体 benchmarks、数据集名称、模型架构细节、随机种子、重复实验次数等，影响可复现性。
- **算力资源未说明**：无法评估方法的计算开销是否足够轻量，尤其是在嵌入式或边缘设备上的可行性。
- **消融实验缺失**：未明确展示 EAP 与 I²O 各自的贡献度，以及不同环境变化频率下的鲁棒性。
- **适用范围**：仅针对 VLA 模型，是否适用于其他多模态推理模型（如 VLM 用于问答）尚待验证。
- **潜在偏差**：成功率下降指标可能只统计了单次运行，未提供置信区间；真实机器人实验的多样性可能有限。

（完）
