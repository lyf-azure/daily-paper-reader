---
title: "HALO: A Unified Vision-Language-Action Model for Embodied Multimodal Chain-of-Thought Reasoning"
title_zh: HALO：用于具身多模态链式推理的统一视觉-语言-动作模型
authors: "Quanxin Shou, Fangqi Zhu, Shuang Chen, Puxin Yan, Zhengyang Yan, Yikun Miao, Xiaoyi Pang, Zicong Hong, Ruikai Shi, Hao HUANG, Jie ZHANG, Song Guo"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f0a4b4b3d1775cb04d6e602c68bf3c4914033562.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 统一VLA模型与多模态链式推理
tldr: HALO提出了一种统一的视觉-语言-动作(VLA)模型，通过具身多模态链式推理(EM-CoT)将文本推理、细粒度视觉子目标预测和动作预测相结合。该模型解决了现有VLA在长视野和分布外场景中缺乏显式多模态推理机制的问题。实验表明，HALO在多种机器人操作任务上显著提升了成功率和泛化能力，尤其是在复杂任务中表现出更强的鲁棒性和可解释性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型缺乏显式的多模态推理机制，在长视野和分布外场景中性能不佳。
method: 提出HALO统一框架，集成文本推理、视觉子目标预测和EM-CoT增强的动作预测。
result: 在机器人操作任务上，HALO相比基线方法显著提升了成功率和泛化能力。
conclusion: 统一的多模态链式推理框架有效增强了VLA模型在复杂环境中的鲁棒性和可解释性。
---

## Abstract
Vision–Language–Action (VLA) models perform well in robotic manipulation, but often struggle in long-horizon or out-of-distribution scenarios due to the lack of explicit mechanisms for multimodal reasoning and anticipating how the world evolves under action.
Recent works introduce textual chain-of-thought or visual subgoal prediction within VLA models, but still fail to offer a unified human-like framework for joint textual reasoning, visual foresight, and action prediction.
To this end, we propose HALO, a unified VLA model that enables embodied multimodal chain-of-thought (EM-CoT) reasoning through textual reasoning, fine-grained visual subgoal prediction, and EM-CoT-augmented action prediction.
We instantiate HALO with a Mixture-of-Transformers (MoT) architecture that decouples semantic reasoning, visual foresight, and action prediction into specialized experts with seamless cross-expert collaboration.
To enable HALO learning at scale, we introduce an automated pipeline to synthesize EM-CoT training data along with a carefully crafted training recipe.
Extensive experiments demonstrate that: (1) HALO achieves superior performance in both simulated and real world, surpassing baseline policy $\pi_0$ by 25.9\% on the RoboTwin benchmark; (2) all proposed components of the training recipe and EM-CoT design help improve task success rate; and (3) HALO exhibits strong generalization under aggressive unseen environment randomization with our proposed EM-CoT reasoning.

---

## 论文详细总结（自动生成）

# HALO：用于具身多模态链式推理的统一视觉-语言-动作模型

## 1. 核心问题与整体含义

- **研究背景**：现有视觉-语言-动作（VLA）模型在机器人操作任务中表现良好，但在长视野（long-horizon）或分布外（out-of-distribution）场景中表现不佳，主要原因是缺乏显式的多模态推理机制来预测世界在动作下的演化。
- **研究动机**：近期工作虽在VLA中引入文本链式推理或视觉子目标预测，但未能提供统一的人类风格框架来同时进行文本推理、视觉预判和动作预测。
- **核心问题**：如何构建一个统一的VLA模型，使其具备具身多模态链式推理（EM-CoT）能力，从而提升在复杂和泛化场景中的鲁棒性和可解释性。

## 2. 方法论

- **核心思想**：提出HALO，一个统一的VLA模型，通过集成三种推理模块（文本推理、细粒度视觉子目标预测、EM-CoT增强的动作预测）来实现具身多模态链式推理。
- **关键技术细节**：
  - 使用**混合Transformer（Mixture-of-Transformers, MoT）**架构，将语义推理、视觉预判和动作预测解耦为专门专家，并实现跨专家的无缝协作。
  - 设计自动化的数据合成流程，生成EM-CoT训练数据，并配合精心设计的训练配方（training recipe）。
- **算法流程（文字说明）**：输入多模态数据（视觉、语言指令）→ 经过MoT架构，分别由语义推理专家进行文本CoT推理、视觉专家预测细粒度子目标、动作专家结合EM-CoT预测最终动作 → 输出动作序列。

## 3. 实验设计

- **数据集/场景**：实验在仿真环境和真实世界中进行，具体包括RoboTwin基准测试（常见机器人操纵任务）。
- **评估指标**：任务成功率（Success Rate）。
- **对比方法**：主要基线策略为$\pi_0$（基础VLA模型），以及可能包含的其他消融变体（文中未列出全部对比模型，但重点对比了$\pi_0$）。
- **实验内容**：
  - 在仿真和真实世界中进行性能测试。
  - 消融实验验证训练配方和EM-CoT各组件的有效性。
  - 在激进未见环境随机化下测试泛化能力。

## 4. 资源与算力

- 文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及“自动化数据合成流程”和“精心设计的训练配方”，但未给出具体计算资源消耗。

## 5. 实验数量与充分性

- **实验数量**：主要包括三个部分：主实验（RoboTwin上对比$\pi_0$）、消融实验（验证训练配方和EM-CoT组件）、泛化实验（环境随机化）。每组实验应有多次重复，但具体次数未提及。
- **充分性与公平性**：
  - 优势：在仿真和真实世界均进行了验证，消融实验覆盖了主要设计组件，泛化实验测试了分布外鲁棒性。
  - 不足：对比方法较少（仅提及$\pi_0$），可能未与近期其他SOTA VLA方法（如RT-2、Octo等）进行比较；未提供统计显著性检验或误差条；缺乏对失败案例的深入分析。

## 6. 主要结论与发现

- HALO在RoboTwin基准上比基线$\pi_0$**提升了25.9%的成功率**。
- 所有提出的训练配方组件和EM-CoT设计均有助于提高任务成功率。
- HALO在激进未见环境随机化下表现出强大的泛化能力，得益于EM-CoT推理。

## 7. 优点

- **方法创新**：首次提出统一的多模态链式推理框架（EM-CoT），同时融合文本推理、视觉子目标预测和动作预测，更接近人类决策过程。
- **架构设计**：采用Mixture-of-Transformers解耦不同推理专家，高效且可扩展。
- **数据合成**：自动化EM-CoT训练数据生成流程，降低人工标注成本。
- **实验验证**：涵盖仿真和真实场景，消融实验透明，泛化测试有力。

## 8. 不足与局限

- **对比基准有限**：仅与$\pi_0$比较，缺乏与RT-2、PaLM-E、EmbodiedGPT等更广泛VLA模型的对比，竞争力说服力不足。
- **算力信息缺失**：未提供训练资源消耗，不利于社区复现和评估效率。
- **实验细节不充分**：未报告任务具体种类、每个任务的样本数、重复次数、置信区间等，影响结果可靠性。
- **局限性讨论**：未提及长视野任务中的具体挑战（如累积误差）、模型在真实世界部署的安全性问题、对物体/场景类别偏见的讨论。
- **开源可能性**：未说明代码或模型是否开源，可能影响后续研究跟进。

（完）
