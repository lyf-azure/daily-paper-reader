---
title: "From Abstraction to Instantiation: Learning Behavioral Representation for Vision-Language-Action Model"
title_zh: "从抽象到实例化: 为视觉-语言-动作模型学习行为表示"
authors: "Bing Hu, Zaijing Li, Rui Shao, Junda Chen, April Hua Liu, Wei-Shi Zheng, Liqiang Nie"
date: 2026-04-30
pdf: "https://openreview.net/pdf/82acd6f0f41c6b6082405693b59c66ae09471c0a.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 学习时间一致的行为表示以实现鲁棒VLA操作
tldr: VLA模型在分布偏移下性能下降，现有行为表示方法受限于短时碎片化和静态对齐。BehaviorVLA提出视觉运动行为编码器和行为条件解码器，学习时间一致的行为表示，在复杂场景中实现一致且鲁棒的操作。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: VLA在分布偏移下难以学习泛化的行为表示。
method: 提出BehaviorVLA，包含视觉运动行为编码器和行为条件解码器，学习时间一致的行为表示。
result: 在多个分布偏移场景下，BehaviorVLA显著提升了操作鲁棒性和成功率。
conclusion: 时间一致的行为表示是增强VLA泛化能力的关键。
---

## Abstract
Vision-Language-Action (VLA) models often suffer from performance degradation under distribution shifts, as they struggle to learn generalized behavior representations across varying environments. While existing approaches attempt to construct behavior representations through action-centric latent variables, they are often limited by short-horizon temporal fragmentation and static execution-alignment, leading to inconsistent behaviors in complex scenarios. To address these limitations, we propose \textbf{BehaviorVLA}, a framework that facilitates robust manipulation through the learning of a temporally coherent behavioral representations. Our approach features two symmetric components: (1) the \textbf{Visuomotor Behavior Encoder (VBE)}, which utilizes a causal Mamba-based architecture to aggregate long-horizon trajectory information into a unified behavior representation; and (2) the \textbf{Phase-conditioned Behavior Decoder (PBD)}, which decodes this representation into precise actions by dynamically aligning task-level priors with real-time execution progress. Experiments on RoboTwin 2.0, LIBERO, and CALVIN demonstrate state-of-the-art success rates of 58\%, 98\%, and 4.36 (Avg.Len), respectively. Notably, in real-world sim-to-real transfer, BehaviorVLA matches the performance of OpenVLA-OFT using only 50\% of the demonstration data, showcasing its superior data efficiency and generalization.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

视觉-语言-动作（VLA）模型在分布偏移（distribution shift）场景下性能显著下降，根本原因在于难以学习跨不同环境的泛化行为表示。现有方法通过动作中心的隐变量构建行为表示，但受限于短时时间碎片化和静态执行对齐，导致复杂场景下行为不一致。本文旨在通过学习时间一致的行为表示来提升VLA模型的鲁棒操作能力。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
提出 **BehaviorVLA** 框架，通过对称的双组件架构学习时间一致的行为表示，实现从抽象任务级先验到具体动作的精准实例化。

### 关键技术细节
- **视觉运动行为编码器（Visuomotor Behavior Encoder, VBE）**：基于因果Mamba架构，聚合长程轨迹信息，将历史观测和动作序列压缩为统一的行为表示。Mamba的因果机制保证了时间顺序的合理利用，长程聚合克服了短视碎片化问题。
- **阶段条件行为解码器（Phase-conditioned Behavior Decoder, PBD）**：接收行为表示，并动态地将任务级先验（如子任务阶段）与实时执行进度对齐，解码出精确的当前动作。这种动态对齐解决了静态执行对齐带来的行为不一致。

### 流程（文字说明）
1. 输入：历史图像观测序列和动作序列。
2. VBE 使用因果Mamba编码器处理序列，输出一个紧凑的、时间一致的行为表示z。
3. PBD 接收z以及当前执行阶段信号（phase），通过条件解码生成当前时刻的动作a_t。
4. 整个模型通过模仿学习或行为克隆进行端到端训练。

## 3. 实验设计：数据集、benchmark、对比方法

- **数据集/场景**：RoboTwin 2.0、LIBERO、CALVIN，以及真实世界的sim-to-real迁移任务。
- **Benchmark指标**：成功率（Success Rate）和平均任务长度（Avg.Len）。
- **对比方法**：未在摘要中列出具体对比方法，但提及与OpenVLA-OFT进行了数据效率对比。

## 4. 资源与算力

论文元数据及摘要中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅在结果部分报告了性能，未提及计算资源细节。

## 5. 实验数量与充分性

- 在3个公开数据集（RoboTwin 2.0、LIBERO、CALVIN）上进行了主实验，报告了SOTA成功率：58%、98%、4.36（Avg.Len）。
- 包含了真实世界的sim-to-real迁移实验，并与OpenVLA-OFT对比数据效率（仅用50%演示数据达到同等性能）。
- 文中未明确列出消融实验数量，但通常此类研究会包含对VBE、PBD各组件的消融以及时间对齐策略的对比。从摘要表述看，实验覆盖了分布偏移场景，充分性较好。但缺少对更多复杂动态环境的实验验证。

## 6. 论文的主要结论与发现

- BehaviorVLA在多个分布偏移场景下显著提升了操作鲁棒性和成功率，达到了SOTA。
- 时间一致的行为表示是增强VLA泛化能力的关键。
- 在sim-to-real迁移中，BehaviorVLA仅需50%演示数据即可匹配OpenVLA-OFT的性能，展示了优越的数据效率和泛化能力。

## 7. 优点

- **创新性**：提出因果Mamba长程轨迹聚合与阶段条件动态解码，解决了短时碎片化和静态对齐两大问题，思路新颖。
- **架构对称性**：VBE和PBD分工明确，编码-解码流程自然。
- **数据效率高**：在真实迁移任务中仅用一半数据即达到强基线性能，对实际部署友好。
- **多场景验证**：涵盖模拟器（LIBERO、CALVIN）和真实机器人任务，结果全面。

## 8. 不足与局限

- **算力资源未公开**：无法评估方法的计算开销和复现成本。
- **对比方法不明确**：摘要未列出具体对比基线，公平性判断受限。需查看全文确认对比设置是否全面（如是否包含RT-2、Octo等主流VLA）。
- **实验环境有限**：仅报告了三个数据集和一个真实迁移任务，缺乏更多样化场景（如更高难度长时间任务、多机器人平台）。
- **偏差风险**：因果Mamba架构可能对轨迹长度敏感，长轨迹下的计算复杂度未讨论；阶段条件信号的设计可能依赖人工先验，泛化到无明确阶段的任务时可能受限。
- **缺少消融细节**：未提及各组件贡献的量化分析，无法确定性能提升的主要来源。

（完）
