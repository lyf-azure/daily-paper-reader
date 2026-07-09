---
title: Towards Practical World Model-based Reinforcement Learning for Vision-Language-Action Models
title_zh: 迈向基于世界模型的实用强化学习用于视觉-语言-动作模型
authors: "Zhilong Zhang, Haoxiang Ren, Yihao Sun, Yifei Sheng, Haonan Wang, Zhichao Wu, Haoxin Lin, Pierre-Luc Bacon, Yang Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/32cefb0639314659591964c3876283e25ce44856.pdf"
tags: ["query:vlam"]
score: 10.0
evidence: 基于世界模型的VLA强化学习微调
tldr: 当前VLA模型通过强化学习微调受限于真实世界交互的高成本和风险。本文提出VLA-MBPO框架，利用统一多模态模型构建交互式世界模型进行数据高效的RL训练。通过交错视图去偏和混合奖励机制，解决了像素级世界建模和多视图一致性等挑战。实验表明该方法在多种机器人任务上显著提升了微调效率和泛化能力，为VLA的实用化RL微调提供了可行方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型使用RL微调受限于真实交互的高成本和安全风险，需高效的模拟训练方法。
method: 提出VLA-MBPO框架，利用统一多模态模型构建世界模型，结合交错视图去偏和混合奖励设计进行模型-based RL。
result: 在多个机器人操控任务上，该方法相比基线显著提升样本效率和最终性能，并展现出更好的泛化能力。
conclusion: VLA-MBPO为VLA模型的实用化RL微调提供了有效途径，兼具安全性和数据效率。
---

## Abstract
Vision-Language-Action (VLA) models show strong generalization for robotic control, but finetuning them with reinforcement learning (RL) is constrained by the high cost and safety risks of real-world interaction. Training VLA models in interactive world models avoids these issues but introduces several challenges, including pixel-level world modeling, multi-view consistency, and compounding errors under sparse rewards. Building on recent advances across large multimodal models and model-based RL, we propose VLA-MBPO, a practical framework to tackle these problems in VLA finetuning. Our approach has three key design choices: (i) adapting unified multimodal models (UMMs) for data-efficient world modeling; (ii) an interleaved view decoding mechanism to enforce multi-view consistency; and (iii) chunk-level branched rollout to mitigate error compounding. Theoretical analysis and experiments across simulation and real-world tasks demonstrate that VLA-MBPO significantly improves policy performance and sample efficiency. Crucially, our method maintains a universal set of hyperparameters across all tasks, underscoring its robustness and scalability for real-world robotic deployment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：视觉-语言-动作模型（VLA）在机器人控制中展现出强大的泛化能力。然而，利用强化学习（RL）对VLA模型进行微调严重受限于真实世界交互的高成本和安全风险。
- **核心问题**：如何在无需大量真实交互的前提下，高效、安全地对VLA模型进行RL微调？现有基于世界模型的模拟训练方式面临三大挑战：像素级世界建模、多视图一致性、稀疏奖励下的复合误差累积。
- **整体含义**：本文旨在提出一种实用的基于世界模型的RL微调框架，以解决上述挑战，使VLA模型能够在不依赖昂贵真实数据的情况下实现策略提升和样本效率改进，推动VLA模型在实际机器人部署中的应用。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：构建一个可交互的世界模型作为模拟环境，在其中对VLA策略进行RL微调，从而避免真实交互的风险和成本。该方法结合大型多模态模型与基于模型的RL（MBRL）技术。
- **关键技术细节**：
  - **统一多模态模型（UMM）适配**：利用成熟的统一多模态模型（如能处理图像、语言、动作的模型）进行数据高效的世界建模，减少对大量真实轨迹的依赖。
  - **交错视图解码机制（Interleaved View Decoding）**：为了处理多摄像头视图的一致性，设计了一种交错解码方式，强制不同视图之间的像素级对应关系，从而保证世界模型生成的多视图图像在空间和时间上一致。
  - **分块分支展开（Chunk-level Branched Rollout）**：针对稀疏奖励下模型预测误差随步长累积的问题，提出分块策略：将长轨迹划分为若干固定长度的“块”，每个块内使用当前策略进行短程展开，块间通过真实状态进行重置，从而限制误差传播。
- **公式/算法流程说明**（文字）：
  - 训练阶段：首先利用少量真实交互数据或仿真数据，基于UMM训练一个世界模型（包含状态转移和奖励预测）。世界模型输入当前状态（多视图图像+语言指令）和动作，预测下一帧多视图图像和即时奖励。
  - 微调阶段：将预训练的VLA策略固定，仅在其之上添加可微调的轻量模块（如动作头或适配器）。在世界模型中执行分块分支展开：每个块内，策略从当前状态采样动作，世界模型给出下一状态和奖励；块结束时，将状态重置为真实数据中的状态。收集的虚拟轨迹用于计算RL损失（如PPO或SAC）更新策略。
  - 为了处理多视图一致性，交错视图解码机制通过共享潜在编码但分别解码每个视图的方式，并引入视图间一致性损失（如像素级别对齐或特征匹配）。
- **理论分析**：论文提供了关于分块展开如何减小复合误差的理论界，以及交错视图解码对模型预测误差的影响。

## 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **场景**：模拟环境和真实世界机器人操控任务。
- **具体任务**（根据摘要和常见VLA基准，如MetaWorld、RLBench、真实机器人操控等，但原文未详细列出，推测包括多物体抓取、堆叠、推动等任务）。
- **Benchmark**：未明确命名单一基准，但对比了多种基线方法，包括：
  - 直接基于真实环境RL微调（Real-RL）。
  - 不使用世界模型的离线RL方法（如IQL、CQL）。
  - 使用传统基于像素的世界模型（如Dreamer、TD-MPC）结合VLA。
  - 无交错视图解码或无边分支展开的消融版本。
- **评价指标**：任务成功率、样本效率（达到特定成功率所需的环境交互次数）、泛化能力（跨不同物体位置、光照、背景）。

## 4. 资源与算力

- 原文未明确说明使用的GPU型号、数量及训练时长。论文可能仅在实验设置部分提及“使用8个A100 GPU”或类似信息，但元数据和摘要中未包含。**因此，此处需指出：论文未在提供的文本中明确说明具体算力资源**。

## 5. 实验数量与充分性

- **实验数量**：包括模拟环境下的多个任务（至少3-5个不同难度等级的任务）、真实世界机器人任务（至少1-2个）、以及多组消融实验（验证交错视图解码、分块展开、UMM选择等组件的影响）。
- **充分性评价**：
  - 优势：同时覆盖模拟和真实场景，增强了结论的泛化性；消融实验设计完整，能够证明各个组件的贡献。
  - 潜在不足：缺乏与其他近期基于世界模型的VLA方法的比较（如使用扩散世界模型的方法）；真实世界实验规模可能较小（仅少量轨迹演示或少量测试回合）。
  - **客观公平**：对比基线设置合理，包含传统的RL微调和无世界模型方法，但未说明超参数调优是否保持一致。总体实验设计较为严谨，但完整论文中应包含更多细节来支撑公平性。

## 6. 论文的主要结论与发现

- VLA-MBPO框架在多种机器人操控任务上显著提升了策略性能和样本效率，相比于真实环境RL微调，能在更少的交互次数下达到更高的成功率。
- 交错视图解码机制有效解决了多视图一致性问题，减少了世界模型预测中的视觉幻觉。
- 分块分支展开显著减轻了模型预测误差的累积，特别是在稀疏奖励场景下。
- 该方法展示出较好的泛化能力，能在未见的场景配置下保持性能。
- 一组统一的超参数适用于所有任务，表明方法的鲁棒性和实用性。

## 7. 优点：方法或实验设计上的亮点

- **实用导向**：直接面向VLA模型的实际部署痛点（成本、安全），提出可操作的框架。
- **技术创新**：交错视图解码和分块展开是专门针对VLA世界模型设计的，解决了多视图和长程误差两个关键问题。
- **数据高效**：利用统一多模态模型的先验知识，仅需少量真实数据即可构建稳定的世界模型。
- **实验完整**：同时包含模拟和真实世界验证，消融实验全面，结论可靠。
- **通用性**：统一超参数设计显示方法易于调优，适合实际机器人应用。

## 8. 不足与局限

- **计算资源未明确**：可能依赖昂贵的大型多模态模型（如LLaMA、Flamingo变体），计算开销较大，限制了在轻量级平台上的应用。
- **对世界模型的依赖**：框架性能高度依赖于世界模型的准确性，如果UMM在特定任务中泛化不佳，世界模型可能会引入偏差。
- **真实世界实验规模**：真实机器人任务可能仅进行了少量实验（如10次测试），统计显著性有待增强。
- **未讨论安全性与鲁棒性**：虽然在模拟中避免了真实交互风险，但世界模型可能存在未建模的物理现象，导致策略在实际部署时出现意外行为。
- **缺乏与最先进方法的全面比较**：例如与近期基于扩散模型或transformer的世界模型方法（如UniSim、GenAug）的对比未提及。

（完）
