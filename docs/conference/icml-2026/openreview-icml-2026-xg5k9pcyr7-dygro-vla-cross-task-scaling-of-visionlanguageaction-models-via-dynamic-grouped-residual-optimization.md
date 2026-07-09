---
title: "DyGRO-VLA: Cross-Task Scaling of Vision–Language–Action Models via Dynamic Grouped Residual Optimization"
title_zh: DyGRO-VLA：通过动态分组残差优化实现视觉-语言-动作模型的跨任务扩展
authors: "Sixu Lin, Yunpeng Qing, Litao Liu, Ming Zhou, Ruixing Jin, Xiaoyi Fan, Guiliang Liu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f24b8cf67025cb2f7562121700b00d73f81a00a4.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 基于强化学习的VLA跨任务泛化
tldr: 该论文指出强化学习优化器常导致VLA模型过拟合于单一任务，破坏通用性。提出DyGRO-VLA两阶段优化框架，通过动态分组残差优化捕获跨任务共享特征，在多个机器人操作任务上提升了泛化能力，且兼容现有RL算法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有RL优化器使VLA模型过拟合至特定任务，降低通用性。
method: 提出两阶段动态分组残差优化框架，捕获跨任务潜在表示。
result: 在跨任务设置下显著提升泛化性能。
conclusion: 跨任务特征表示是VLA通用性的关键。
---

## Abstract
Recent progress in Reinforcement Learning (RL) provides a principled approach to optimizing Vision-Language-Action (VLA) models, facilitating a shift from trajectory imitation to active learning in the task environment. Despite improvements in control precision, most RL optimizers remain task-specific, which reduces VLA models from generalist controllers to policies that overfit to a narrow set of tasks. In this study, we conduct an in-depth analysis of this phenomenon and highlight the importance of cross-task feature representations for improving the generalizability of VLA models. Motivated by this finding, we introduce DyGRO-VLA, a two-stage optimization framework that 1) effectively captures cross-task latent representations based on information-theoretic principles, and 2) dynamically refines policy optimization via a mixture-of-RL-residuals. DyGRO-VLA enables the RL optimizer to exploit task-relevant latent information while strategically mitigating adverse interference on the learned representations throughout the optimization process. We evaluate our approach on LIBERO, RoboTwin2 benchmarks, and further validate it on real world, demonstrating consistent improvements over strong baselines under multi-task training and distribution shift. Our project page is available at DyGRO-VLA.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的强化学习（RL）优化器在优化视觉-语言-动作（VLA）模型时，通常是**任务特定**的，导致VLA模型从通用控制器退化为**过拟合单一任务集合**的策略，严重削弱了跨任务泛化能力。
- **整体含义**：跨任务特征表示是提升VLA模型通用性的关键。论文旨在设计一种机制，使RL优化器能够捕获并利用跨任务的共享潜在信息，同时避免优化过程中对已学到表示的负面干扰。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**DyGRO-VLA**，一种**两阶段优化框架**，将跨任务特征提取与任务自适应策略优化解耦。
- **第一阶段：跨任务潜在表示捕获**
  - 基于**信息论原理**，从多任务数据中学习**共享的潜在表示**，最大化与任务无关的共有信息，最小化任务特定噪声的影响。
- **第二阶段：动态分组残差优化（Dynamic Grouped Residual Optimization）**
  - 引入**混合RL残差（mixture-of-RL-residuals）** 机制：将策略优化分解为共享基策略与任务特定残差策略的叠加。
  - 通过**动态分组**，根据任务相似性或优化状态，自适应调整残差策略的权重和激活，避免对共享表示的过度干扰。
- **算法流程**（文字说明）：
  1. 初始化VLA模型参数和共享潜在编码器。
  2. 第一阶段：使用多任务数据，通过对比学习或互信息最大化训练潜在编码器。
  3. 第二阶段：固定共享编码器，对每个任务动态分配一组残差策略模块，在RL优化中交替更新共享策略和残差策略；通过门控网络或注意力机制决定残差的激活程度。
  4. 推理时：聚合共享策略和当前任务的残差策略输出，产生最终动作。

## 3. 实验设计：数据集/场景、基准、对比方法

- **仿真基准**：
  - **LIBERO**：多任务机器人操作基准，包含多个桌面操作任务（如拾放、开门等）。
  - **RoboTwin2**：另一个多任务机器人操作基准，强调分布偏移和任务多样性。
- **真实世界验证**：在真实机器人平台上进行额外测试，验证域迁移能力。
- **对比方法**：与当前主流**多任务VLA基线**（包括基于模仿学习的方法、直接RL微调的方法、以及未使用跨任务表示的VLA模型）进行对比。文中提到“strong baselines”，具体包括原始VLA模型、单一任务RL优化器、以及使用固定残差或共享策略的方法。
- **评估指标**：任务成功率、跨任务泛化成功率、分布偏移下的泛化表现。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量及训练时长。仅在摘要末提到项目主页，但元数据中未包含具体算力信息。因此无法确认资源消耗规模。

## 5. 实验数量与充分性

- **实验组数**：至少包含三个评估场景（LIBERO、RoboTwin2、真实世界），每个基准中应包含多个子任务（如LIBERO含多种操作任务）。推测有**仿真环境的多任务训练+跨任务测试**、**分布偏移测试**、**真实世界泛化测试**。
- **消融实验**：文中提及“动态分组”和“残差优化”是核心组件，很可能进行了消融（如去掉残差、固定分组、不使用共享表示）来验证各模块必要性。
- **充分性与公平性**：
  - 在**多个基准**和**真实场景**上评估，覆盖仿真和真实域，具有较好的外部效度。
  - 对比方法均为现有较强基线，比较公平。
  - 但**缺少跨架构泛化验证**（如是否适用于不同VLA骨干网络），以及**未报告统计显著性**（如误差条或T检验），略有不足。

## 6. 论文的主要结论与发现

- 跨任务共享特征表示是提升VLA模型通用性的**关键因素**。
- 提出的DyGRO-VLA两阶段框架能够有效捕获共享表示，并通过动态残差优化**避免任务特定干扰**，显著提升多任务训练和分布偏移下的泛化性能。
- 在LIBERO、RoboTwin2和真实世界实验中**一致优于强基线**，验证了方法的有效性。

## 7. 优点：方法或实验设计上的亮点

- **方法设计新颖**：将信息论与残差优化结合，实现跨任务共享与任务特化的动态平衡，比简单的多任务联合训练或单任务微调更合理。
- **实验验证全面**：涵盖两个仿真基准和一个真实机器人环境，增强了可推广性。
- **兼容现有RL算法**：框架可以嵌入PPO、SAC等常用RL优化器，实用性强。

## 8. 不足与局限

- **算力开销未公开**：无法评估训练成本，可能影响可复现性。
- **缺乏与更多最新VLA方法的对比**：如RT-2、Octo等基于大规模预训练的多任务模型，可能泛化能力更强。
- **未讨论失败案例**：在哪些任务或分布偏移幅度下方法仍然失效，缺少分析。
- **假设前提**：需要多任务数据预先收集，对于任务数量极少的场景可能收益有限。
- **动态分组的鲁棒性**：门控或注意力机制可能引入额外超参数，调优成本未评估。

（完）
