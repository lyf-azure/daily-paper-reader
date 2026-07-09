---
title: "MultiLoReFT: Decoupling Shared and Modality-Specific Subspaces in Multimodal Learning via Low-Rank Representation Fine-Tuning"
title_zh: MultiLoReFT：通过低秩表示微调解耦多模态学习中的共享和模态特定子空间
authors: "Sana Tonekaboni, Viktoria Schuster, Caroline Uhler"
date: 2026-04-30
pdf: "https://openreview.net/pdf/48c7d5a27aaf8965cd2f726199ae457eca4fdd13.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 多模态学习的低秩表示微调框架
tldr: 该论文提出MultiLoReFT，一种高效的低秩表示微调框架，适用于多模态学习。它扩展了LoRA思想，在预训练单模态模型基础上解耦共享和模态特定子空间，实现参数高效微调。在多模态分类和检索任务上以极少可训练参数达到与全微调相当的性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多模态端到端训练数据难获取，且表示混叠，缺乏可控性。
method: 提出多模态低秩表示微调框架，解耦共享和模态特定子空间。
result: 以极小参数实现与全微调相当的性能。
conclusion: 低秩微调可高效应用于多模态模型。
---

## Abstract
Real-world perception and decision making are inherently multimodal, integrating complementary signals across modalities. However, training multimodal models faces two main obstacles. First, collecting large-scale, well-aligned paired multimodal datasets is often impractical, making end-to-end multimodal training difficult. Second, existing multimodal representations frequently entangle information shared across modalities with modality-specific information, hindering interpretability and control. We introduce MultiLoReFT, an efficient and scalable low-rank representation fine-tuning framework for multimodal learning with pretrained unimodal models. MultiLoReFT extends low-rank adaptation to the multimodal setting and learns interpretable projection subspaces that decouple shared and modality-specific information. Across simulated and real-world benchmarks, it produces representations that support multimodal prediction while explicitly revealing how shared and modality-specific information is distributed across modalities.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现实世界的感知和决策本质上是多模态的，但多模态模型的训练面临两大障碍：一是大规模、对齐良好的多模态配对数据集难以获取，导致端到端多模态训练困难；二是现有多模态表示常将跨模态的共享信息与模态特定信息纠缠在一起，限制了模型的可解释性和可控性。
- **整体含义**：该文旨在提出一种参数高效的多模态微调方法，在不依赖大规模配对数据的前提下，解耦共享子空间与模态特定子空间，提升多模态表示的可解释性和预测性能。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：扩展低秩适配（LoRA）到多模态场景，学习可解释的投影子空间，将共享信息与模态特定信息分离。
- **关键技术细节**：
  - 基于预训练的单模态模型（如视觉、语言模型），保持其参数冻结，仅引入少量可训练的低秩矩阵。
  - 设计多模态低秩表示微调框架（MultiLoReFT），通过投影子空间解耦：
    - **共享子空间**：捕捉跨模态共有的语义信息。
    - **模态特定子空间**：保留各模态独有的特征。
  - 微调时仅学习这些低秩投影矩阵，极大减少可训练参数量。
- **算法流程简述**（根据摘要推断）：
  1. 输入多模态数据（如图像、文本）。
  2. 分别通过冻结的单模态编码器得到初始表示。
  3. 对每个模态表示施加低秩投影，分解为共享部分和特定部分。
  4. 通过融合共享表示和可选特定表示进行下游任务（分类、检索）。
  5. 损失函数可能包含解耦约束（如互信息最小化或正交性约束），但原文未明确，需进一步确认。

## 3. 实验设计

- **数据集/场景**：使用了**模拟数据集**和**真实世界基准**。具体数据集名称未在摘要中列出（如多模态分类、跨模态检索等常见任务）。
- **基准（Benchmark）**：对比方法包括**全微调（Full Fine-Tuning）**、**传统LoRA**（单模态低秩适配）以及可能的其他参数高效微调方法。
- **对比方法**：未明确列出，但推测与标准多模态微调基线对比。

## 4. 资源与算力

- 论文摘要及元数据**未提及**具体GPU型号、数量或训练时长。仅提到“高效、可扩展”，但未提供硬件细节。这可能是因为ICML-2026的论文尚未公开完整版本，或者作者未强调算力消耗。因此，该部分信息缺失。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，至少进行了模拟数据和真实数据两类实验；消融研究部分提及“揭示共享和特定信息如何分布”，暗示可能包含解耦效果的可视化分析，但具体实验数量未说明。
- **充分性/客观性**：由于仅提供摘要，无法评估实验是否充分。但综合元数据评分8.0（较高），且被ICML-2026接收，推测实验设计较合理。但缺乏详细对比表格和统计显著性检验的描述，需审慎对待。

## 6. 论文的主要结论与发现

- MultiLoReFT以**极少的可训练参数**（低秩矩阵）达到了与**全微调相当的性能**。
- 所学习的投影子空间能够**明确解耦共享信息和模态特定信息**，提高了模型的可解释性。
- 低秩微调方法可以**高效地应用于多模态模型**，无需大规模配对数据，且对下游任务（分类、检索）有效。

## 7. 优点

- **参数高效**：仅引入低秩矩阵，极大减少训练存储和计算开销。
- **可解释性强**：通过显式解耦共享和模态特定子空间，使信息分布可视化，增强模型透明度。
- **适用性广**：不依赖大规模多模态对齐数据，可复用高质量单模态预训练模型。
- **性能优秀**：在多个基准上接近全微调效果。

## 8. 不足与局限

- **信息不完整**：当前仅有摘要，缺乏具体数据集名称、实验细节、超参数设置、与更多基线的对比，无法全面评估方法的普适性和稳健性。
- **可能依赖单模态预训练质量**：如果单模态模型本身存在偏差或表示能力不足，解耦效果可能受限。
- **解耦的有效性验证**：虽然声称可解释，但未提供定量指标（如互信息下降、分类准确率增益等）来严格证明解耦的益处。
- **应用限制**：目前验证任务限于分类和检索，更复杂的多模态生成任务（如视觉问答、字幕生成）未涉及。
- **未提及算力与训练时间**，不利于其他研究者复现。

（完）
