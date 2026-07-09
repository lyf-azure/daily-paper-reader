---
title: "STARE-VLA: Progressive Stage-Aware Reinforcement for Fine-Tuning Vision-Language-Action Models"
title_zh: STARE-VLA：面向视觉-语言-动作模型微调的渐进阶段感知强化学习
authors: "Feng Xu, Guangyao Zhai, Xin Kong, Tingzhong Fu, Yuanzhe Liu, Daniel F. N. Gordon, Xueli An, Benjamin Busam"
date: 2026-01-23
pdf: "https://openreview.net/pdf/81864f0ec4b5bd77e032c50a95befa894c71ae4f.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 基于强化学习的VLA阶段感知微调
tldr: 该论文发现VLA模型的长程动作序列具有因果链式阶段，现有轨迹级优化（如PPO）导致粗粒度信用分配。提出STARE-VLA，渐进式阶段感知强化学习微调方法，根据阶段难度自适应优化，在多个操作任务上训练更稳定且性能提升。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA长程动作存在因果阶段，轨迹级优化信用分配粗糙。
method: 提出渐进阶段感知的强化学习微调方法。
result: 训练稳定性提升，操作成功率显著提高。
conclusion: 阶段感知优化更适合VLA序列决策学习。
---

## Abstract
Recent advances in Vision–Language–Action (VLA) models, driven by large language models and reinforcement learning–based fine-tuning, have enabled promising progress in robotic manipulation. Existing methods often treat long-horizon actions as linguistic sequences and apply trajectory-level optimization methods such as Trajectory-wise Preference Optimization (TPO) or Proximal Policy Optimization (PPO), leading to coarse credit assignment and unstable training. However, unlike language, where a unified semantic meaning is preserved despite flexible sentence order, action trajectories progress through causally chained stages with different learning difficulties. This motivates progressive stage optimization. Accordingly, we introduce Stage-Aware Reinforcement (STARE), a module that decomposes a long-horizon action trajectory into semantically meaningful stages and provides dense, interpretable, and stage-aligned reinforcement signals. Integrating STARE into TPO and PPO, we develop Stage-Aware TPO (STA-TPO) and Stage-Aware PPO (STA-PPO) for offline stage-wise preference and online intra-stage interaction, respectively. Further building on supervised fine-tuning as initialization, we propose the Imitation→Preference→Interaction (IPI), a serial fine-tuning pipeline for improving action accuracy in VLA models. Experiments on SimplerEnv and ManiSkill3 tasks demonstrate substantial improvements, achieving state-of-the-art success rates of 98.0\% and 96.4\%, respectively, while exhibiting more stable and consistent training behavior. We further validate our approach on real-world robotic manipulation tasks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：Vision-Language-Action (VLA) 模型结合大语言模型和强化学习微调，在机器人操作任务中取得进展。现有方法通常将长程动作序列视为语言序列，并应用轨迹级优化方法（如 TPO、PPO）。
- **核心问题**：现有轨迹级优化导致粗粒度的信用分配和训练不稳定。与语言不同（语序灵活但语义一致），动作轨迹具有因果链式阶段，不同阶段学习难度各异。
- **整体含义**：需要一种阶段感知的优化方法，以实现更精细、更稳定的强化学习微调，从而提升 VLA 模型在长时间操作任务中的成功率。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **STARE**（Stage-Aware Reinforcement）模块，将长程动作轨迹分解为语义上有意义的阶段，并提供密集、可解释且与阶段对齐的强化信号。
- **关键技术细节**：
  - **阶段分解**：自动或基于先验将动作序列划分为因果链式阶段，每个阶段对应一个子目标或子步骤。
  - **阶段感知奖励**：在每一个阶段内部，提供细粒度的奖励（而非仅轨迹末端奖励），实现密集信号支撑。
  - **集成进主流算法**：
    - **STA-TPO**：将 STARE 集成到 Trajectory-wise Preference Optimization（TPO）中，用于离线阶段级偏好学习。
    - **STA-PPO**：将 STARE 集成到 Proximal Policy Optimization（PPO）中，用于在线阶段内交互优化。
  - **IPI 流水线**：基于监督微调（Imitation）作为初始化，依次经过偏好优化（Preference）和在线交互（Interaction），三个阶段串联，逐步提升动作精度。
- **算法流程（文字说明）**：
  1. 首先使用模仿学习（监督微调）初始化 VLA 模型参数。
  2. 然后使用 STA-TPO 进行离线阶段级偏好学习，基于轨迹的阶段分解生成偏好对，优化策略朝向更好阶段表现。
  3. 最后使用 STA-PPO 进行在线阶段内交互，让模型在环境中实时调整阶段内动作，获得阶段级别奖励信号，进一步优化策略。
- **公式**：摘要中未给出具体公式，但核心是阶段分解和阶段对齐奖励的设计。

## 3. 实验设计：数据集/场景、基准、对比方法

- **实验场景**：
  - **SimplerEnv**：一个模拟机器人操作环境。
  - **ManiSkill3**：另一个机器人操作模拟基准。
- **评估指标**：任务成功率（Success Rate）。
- **对比方法**：
  - 基线包括使用轨迹级优化的原始 TPO 和 PPO。
  - 可能还比较了其他 VLA 微调方法（具体名称未在摘要中列出，但摘要中强调 SOTA）。
  - 此外，在实际机器人操作任务上进行了验证（真实世界操作）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及 GPU 型号、数量、训练时长等具体算力信息。仅能推断使用了标准的深度学习训练资源（如多块 GPU 集群）。

## 5. 实验数量与充分性

- **实验组数**：至少在两个模拟环境（SimplerEnv、ManiSkill3）上进行了完整实验，并额外有真实世界验证。元数据中未列出消融实验数量，但方法中包含 STARE 模块和 IPI 流水线，应当有相应的消融：
  - 单独使用 STARE 对 TPO 和 PPO 的提升。
  - 对比有/无阶段感知。
  - IPI 不同阶段的贡献。
- **充分性评价**：实验覆盖了模拟和真实场景，且报告了 SOTA 结果（98.0%、96.4%），相对充分。但由于已知信息有限，无法判断是否在多种任务或不同难度上进行了公平对比。通常需要更多细节。

## 6. 主要结论与发现

- 阶段感知强化学习（STARE）显著优于轨迹级优化方法，训练更稳定，成功率大幅提升。
- 在 SimplerEnv 上达到 98.0% 成功率，在 ManiSkill3 上达到 96.4%，均为 SOTA。
- IPI 流水线能够逐步提高动作精度，从模仿到偏好再到交互，效果优于直接用单一方法。
- 真实机器人操作实验也验证了方法的有效性。

## 7. 优点

- **方法论创新**：首次将动作轨迹的因果链式阶段显式建模，解决了 VLA 长程任务中信用分配粗粒度的问题。
- **兼容性强**：STARE 可插入 TPO、PPO 等主流算法，无需修改底层框架。
- **训练稳定性**：阶段密集奖励信号降低了训练方差，避免了无效探索。
- **实验结果突出**：在两个标准基准上均取得极高成功率，且优于现有方法。

## 8. 不足与局限

- **阶段分解依赖先验**：如何自动且通用地分解阶段未在摘要中详细说明，可能需对任务有预定义子目标，限制了迁移性。
- **算力细节缺失**：未报告训练资源消耗，难以评估实际部署成本。
- **实验覆盖有限**：仅两个模拟环境加少量真实场景，可能未涵盖多种复杂操作（如非刚体、精确装配等）。
- **偏差风险**：阶段分解可能导致模型过度依赖固定子目标，在动态环境中泛化能力待验证。
- **应用限制**：仅针对 VLA 模型，对于纯 RL 或非语言驱动策略是否适用未讨论。

（完）
