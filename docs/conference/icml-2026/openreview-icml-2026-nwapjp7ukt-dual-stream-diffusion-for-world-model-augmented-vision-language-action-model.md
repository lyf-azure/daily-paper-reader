---
title: Dual-Stream Diffusion for World-Model Augmented Vision-Language-Action Model
title_zh: 双流扩散：用于世界模型增强的视觉-语言-动作模型
authors: "John Won, Kyungmin Lee, Huiwon Jang, Dongyoung Kim, Jinwoo Shin"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d92e04b8c323fe4c7abd20c169f3ffac2113e0c0.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 多模态扩散Transformer实现VLA中的跨模态融合
tldr: 针对现有VLA模型融合世界模型时模态差异大、状态与动作联合预测难的问题，本文提出DUST框架，采用双流扩散Transformer分别处理视觉与语言模态，通过解耦流匹配损失学习跨模态因果关联，并引入异步采样策略提升推理效率。在机器人模拟基准上验证了该方法在策略学习中的有效性，为VLA模型架构设计提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型融合世界模型时面临模态差异导致的预测难题。
method: 提出DUST框架，包含多模态扩散Transformer，使用独立噪声扰动和解耦流匹配损失。
result: 在RoboCas等模拟基准上取得性能提升。
conclusion: 双流扩散方法有效实现了跨模态知识共享与异步推理。
---

## Abstract
Augmenting Vision-Language-Action models (VLAs) with world models is promising for robotic policy learning but faces challenges in jointly predicting states and actions due to the modality gap. To address this, we propose DUal-STream diffusion (DUST), a world-model augmented VLA framework featuring a multimodal diffusion transformer that maintains separate modality streams while enabling cross-modal knowledge sharing. In addition, DUST utilizes independent noise perturbations and a decoupled flow matching loss to learn cross-modal causal relationships. We further introduce an asynchronous sampling method for action and vision tokens that enhances performance through inference-time scaling. Experimental results on simulated benchmarks like RoboCasa and GR-1 show that DUST achieves up to 6\% gains over state-of-the-art VLA and world-modeling baselines, with inference-time scaling providing an additional 2–5\% improvement. In real-world tasks using the Franka Research 3, DUST outperforms baselines by 10\% in success rate. Finally, we demonstrate that DUST enables effective transfer learning through both pretraining on action-free videos and joint-training with heterogeneous robot and human datasets.

---

## 论文详细总结（自动生成）

# 论文总结：双流扩散：用于世界模型增强的视觉-语言-动作模型

## 1. 核心问题与整体含义
- **研究动机**：现有视觉-语言-动作模型（VLA）在融合世界模型时，面临两个关键挑战：一是视觉、语言和动作模态之间的**模态差异**（modality gap），导致联合预测状态与动作困难；二是缺乏有效的跨模态因果关联学习机制。
- **整体含义**：提出一种新型双流扩散框架（DUST），通过解耦模态处理与异步采样策略，增强VLA的世界模型预测能力，从而提升机器人策略学习的性能与泛化性。

## 2. 方法论
- **核心思想**：采用**双流扩散Transformer**架构，维护独立的视觉流和语言流，同时通过交叉注意力实现跨模态知识共享。
- **关键技术细节**：
  - **独立噪声扰动**：对视觉和语言token分别施加独立的噪声，避免互相干扰。
  - **解耦流匹配损失**（decoupled flow matching loss）：分别计算各模态的流匹配损失，学习跨模态因果关联。
  - **异步采样方法**：在推理阶段，对动作token和视觉token采用不同的采样步数（或时序策略），提升推理效率与性能。
- **公式/算法流程**（文字说明）：
  1. 输入：视觉观测、语言指令、历史动作。
  2. 双流编码：视觉流与语言流各自通过独立的Transformer层，其间进行交叉注意力交互。
  3. 扩散前向过程：对视觉和语言token分别加入独立的高斯噪声。
  4. 流匹配训练：使用解耦的流匹配损失，优化每个模态的去噪过程。
  5. 推理：采用异步采样（如动作token采样步数少于视觉token）生成预测动作。

## 3. 实验设计
- **数据集/场景**：模拟基准：RoboCasa、GR-1；真实世界任务：使用Franka Research 3机器人。
- **Benchmark**：与最先进的VLA及世界模型基线对比（如未列出具体基线名称，但提及“state-of-the-art VLA and world-modeling baselines”）。
- **对比方法**：包括标准VLA、经典世界模型方法等（具体未列出名称）。

## 4. 资源与算力
- 论文摘要中**未明确说明**使用的GPU型号、数量及训练时长。元数据中也无相关内容。因此无法总结。

## 5. 实验数量与充分性
- **实验数量**：至少包括三个主要实验：
  - RoboCasa模拟基准上的性能比较；
  - GR-1模拟基准上的性能比较；
  - 真实Franka Research 3机器人任务成功率测试。
- **消融实验**：包含异步采样方法的消融（inference-time scaling带来额外2-5%提升）。
- **迁移学习实验**：验证了通过无动作视频预训练以及异构机器人-人类数据集联合训练的有效性。
- **充分性评估**：实验覆盖模拟和真实环境，包含消融与迁移，设计较全面。但缺少对模型规模、不同噪声策略的进一步消融，以及与其他扩散策略的对比，客观性良好但并非极致充分。

## 6. 主要结论与发现
- 在RoboCasa和GR-1模拟基准上，DUST相比最先进的VLA与世界模型基线获得**最高6%**的性能提升。
- 推理时异步采样（inference-time scaling）额外带来**2-5%**的改进。
- 在真实Franka Research 3任务中，DUST成功率比基线**高出10%**。
- 支持有效的迁移学习：可从无动作视频预训练，以及联合异构数据集训练。

## 7. 优点
- **方法创新**：双流独立性设计简洁有效，解耦流匹配损失针对性地解决了模态差异问题。
- **异步采样策略**：在推理阶段通过计算资源分配提升性能，具有实用价值。
- **实验全面**：覆盖模拟与真实环境，包含消融与迁移，结论可靠。
- **迁移学习能力**：使VLA能利用大量无动作视频数据，降低标注成本。

## 8. 不足与局限
- **未披露算力信息**：无法评估方法的训练成本与复现难度。
- **实验细节缺失**：论文摘要未提供对比基线具体名称、每类实验的具体样本量、统计显著性等，可能影响可复现性。
- **应用限制**：目前仅在桌面机器人任务（Franka）上验证，未测试复杂移动操作或人机交互场景。
- **模态差异处理**：虽然双流缓解了部分问题，但语言模态与视觉模态的异步程度如何影响性能尚需探索。
- **偏差风险**：实验可能仅针对特定任务调优，跨领域泛化性未充分验证。

（完）
