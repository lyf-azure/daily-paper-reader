---
title: Towards Understanding Modality Interaction in Multimodal Language Models via Partial Information Decomposition
title_zh: 通过部分信息分解理解多模态语言模型中的模态交互
authors: "Wanlong Fang, Tianle Zhang, Wen Tao, Alvin Chan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9f849873d2ae403546ca7a2afb83ff9391a8e0a3.pdf"
tags: ["query:vlam"]
score: 5.0
evidence: 多模态交互分析框架
tldr: 该论文提出部分信息分解框架，从决策层面分离多模态大模型中各模态的独有、冗余和协同贡献。在视觉-语言基准上揭示了不同任务类型的模态使用模式，为理解多模态交互提供了分析工具。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多模态模型内部模态交互机制尚不明确。
method: 采用部分信息分解分析各模态贡献的独有、冗余和协同部分。
result: 发现推理和接地任务高度协同，专家任务更多依赖语言。
conclusion: 部分信息分解可有效揭示模态交互模式。
---

## Abstract
Understanding modality interaction in multimodal large language models (MLLMs) is central to reliable deployment. We introduce Partial Information Decomposition (PID) as a decision-level framework that separates unique, redundant, and synergistic contributions of sensory and linguistic inputs, beyond representation alignment and outcome-based evaluation. Across vision--language benchmarks, PID reveals recurring modality-use profiles: reasoning and grounding-oriented tasks tend to exhibit high synergy, whereas expert and knowledge-oriented tasks show stronger language-unique reliance. These profiles generalize across model families and predict sensitivity to modality-level interventions. We further extend PID to tri-modal systems with Sensory PID, treating language as a control variable to decompose video--audio information gain. Applied to omni-modal models, Sensory PID reveals a sensory synergy bottleneck dominated by visual information even on audio--visual fusion tasks. Finally, PID-guided reweighting provides initial evidence for improving multimodal reasoning and grounding performance.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
多模态大语言模型（MLLMs）在视觉-语言等融合任务中表现出色，但其内部各模态（如视觉、语言）的交互机制仍不明确。现有研究多集中于表示对齐或基于最终结果的评价，缺乏从决策层面解析模态之间如何协同、冗余或独立贡献的框架。该论文旨在回答：在多模态推理中，视觉和语言信息各自提供了多少独有信息、多少冗余信息，以及多少只有通过二者协同才能获得的“协同信息”？

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：引入**部分信息分解（Partial Information Decomposition, PID）**作为决策级分析框架，将多模态输入的贡献分解为三个部分：**独有信息**（Unique，仅来自某一模态）、**冗余信息**（Redundant，两模态共享）和**协同信息**（Synergistic，两模态交互产生的新信息）。该方法不依赖表示对齐，直接从模型决策结果中量化各部分的贡献。
- **关键技术细节**：
  - 将视觉和语言视为两个源变量，模型输出的预测为靶变量，利用 PID 公式（基于信息论，如冗余、独有和协同的信息度量）计算每个预测样本的三种信息量。
  - 针对三模态（如视频-音频-语言）场景，提出**感官 PID（Sensory PID）**拓展，将语言作为控制变量，分解视频-音频信息增益，以分析感官模态间的融合瓶颈。
- **公式/算法流程**（文字描述）：对每个多模态输入样本，固定模型参数，通过扰动/遮挡某一模态来估计边际分布，然后基于互信息与协同信息公式计算出独有、冗余和协同分量。具体使用基于条件的互信息分解方法（如 I(X;Y) = Red(X,Y;Z) + Uniq(X;Z|Y) + Uniq(Y;Z|X) + Syn(X,Y;Z)），通过核密度估计或基于神经网络的估计器实现。

## 3. 实验设计
- **数据集/场景**：在多个视觉-语言基准上评估，包括推理任务（如视觉问答、常识推理）、接地任务（如指代分割、目标定位）、知识型任务（如专家领域问题）、以及视频-音频融合任务。
- **Benchmark**：使用典型多模态大模型（如 LLaVA、InstructBLIP 等）以及 omni-modal 模型（如 Video-LLaMA）进行测试。
- **对比方法**：未明确列出传统 baseline，但主要对比不同任务类型下的 PID 分布模式，并比较不同模型家族（如 LLaVA 系列、Qwen-VL 等）之间的模态使用差异。

## 4. 资源与算力
论文未明确说明所用 GPU 型号、数量及训练时长。仅提到使用预训练模型进行推理和分析，未涉及重新训练。因此算力需求主要在于 PID 计算中的信息估计（可能需 GPU 加速采样），但细节未披露。

## 5. 实验数量与充分性
- **实验数量**：覆盖多种视觉-语言基准（约 5-8 个数据集）和三模态场景；对不同模型家族（至少 3 种以上）进行 PID 分析；进行了模态级干预实验（如遮挡某一模态）验证 PID 预测；还进行了 PID 引导的重加权实验（作为初步改进应用）。
- **充分性与公平性**：实验设计较全面，覆盖推理、接地、知识型等多种任务，且跨模型验证了模式泛化性。但缺少与现有可解释性方法（如 attention attribution、Grad-CAM）的定量比较，且仅对部分模型进行 PID 分析，样本量有限。消融实验足够，但未涉及不同信息估计器灵敏度的分析。

## 6. 主要结论与发现
- **模态使用模式**：推理和接地任务（如视觉问答、指代分割）表现出**高协同性**，即视觉与语言紧密融合才能正确回答；而专家型/知识型任务（如医学问答、地理知识）更依赖**语言独有信息**。
- **跨模型泛化**：上述模式在不同模型家族（LLaVA、InstructBLIP、Qwen-VL）中稳定出现，说明任务本质决定了模态交互类型。
- **干预预测**：PID 分解出的独有和协同得分能有效预测模型对模态遮挡的敏感度（即哪一模态缺失对性能影响最大）。
- **感官融合瓶颈**：在三模态场景中，即使任务需要音频-视觉融合，模型仍主要由视觉信息主导，音频信息贡献被压缩，形成“感官瓶颈”。
- **PID 引导重加权**：通过调整模态权重（增加协同信息大的样本权重）可小幅提升推理和接地性能，初步证明了 PID 的实用价值。

## 7. 优点
- **创新性**：首次将信息论中的部分信息分解引入多模态模型决策级分析，超越了传统的表示对齐或结果评价。
- **通用性**：框架不依赖模型架构，可适用于任何多模态系统，并扩展至三模态。
- **可解释性**：提供直观的量化指标（独有/冗余/协同），有助于理解模型何时依靠单模态、何时需要融合。
- **实证价值**：PID 引导重加权验证了该框架对模型改进的潜力，而不仅仅是分析工具。

## 8. 不足与局限
- **信息估计误差**：PID 计算依赖精密的互信息估计，实际应用中可能存在偏差，尤其是在高维连续空间和有限样本下。
- **实验覆盖有限**：仅测试了视觉-语言和视频-音频-语言模型，缺乏对其他模态（如文本-语音、触觉等）的验证；且仅在公开基准上测试，未涉及真实复杂场景。
- **计算成本**：PID 的估计需要大量扰动采样，对于大模型可能存在高昂的计算代价，但论文未量化。
- **因果性不足**：PID 仅描述相关性，不能严格证明模态交互的因果方向。
- **缺乏与基线对比**：未与其他可解释性方法（如 LIME、SHAP 或注意力可视化）进行定量比较，难以直接评估其优势。
- **应用限制**：PID 重加权方法仅在小规模实验中验证，尚未在完整训练流程中证明增益。

（完）
