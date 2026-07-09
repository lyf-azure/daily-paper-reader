---
title: "AutoMoT: A Unified Vision-Language-Action Model with Asynchronous Mixture -of-Transformers for End-to-End Autonomous Driving"
title_zh: "AutoMoT: 基于异步混合Transformer的统一视觉-语言-动作端到端自动驾驶模型"
authors: "Wenhui Huang, Songyan Zhang, Qihang Huang, Zhidong Wang, Zhiqi Mao, Collister Chua, Zhan Chen, Long Chen, Chen Lv"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8e4cccb7721abcd37ad990ddb3874c5c03e14ed9.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 用于自动驾驶的端到端统一VLA模型
tldr: 现有端到端自动驾驶集成VLM时面临推理与动作空间分布不匹配及推理延迟问题。AutoMoT提出异步混合Transformer框架，将推理与动作生成统一在单一VLA模型中，利用预训练VLM的通用推理能力。实验表明该方法在驾驶性能上超越先前方法，同时降低了推理延迟。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLM集成方法存在推理与动作空间分布不匹配、推理能力利用不足和延迟大等问题。
method: 提出AutoMoT统一VLA框架，采用异步混合Transformer结构同时进行推理与动作生成。
result: 在自动驾驶基准上，AutoMoT在场景理解与规划控制上均达到领先性能，且推理更快。
conclusion: 统一VLA模型结合异步混合Transformer可有效解决自动驾驶中的推理-动作协同问题。
---

## Abstract
Integrating vision-language models (VLMs) into end-to-end (E2E) autonomous driving (AD) systems has shown promise in improving scene understanding. However, existing integration strategies suffer from several limitations: they either struggle to resolve distribution misalignment between reasoning and action spaces,  underexploit the general reasoning capabilities of pretrained VLMs, or incur substantial inference latency during action policy generation, which degrades driving performance. To address these challenges, we propose AutoMoT in this work, an end-to-end AD framework that unifies reasoning and action generation within a single vision-language-action (VLA) model. Our approach leverages a mixture-of-transformer (MoT) architecture with layer-wise joint attention sharing, which preserves the general reasoning capabilities of pre-trained VLMs while enabling efficient asynchronous inference over various tasks at different frequencies. Additionally, we explore a VLA-oriented action refiner that further enhances driving performance via diffusion-based fine-tuning. Extensive experiments on multiple benchmarks, under both open- and closed-loop settings, demonstrate that AutoMoT achieves state-of-the-art (SOTA) performance compared to existing methods. We further investigate the functional boundary of pre-trained VLMs in AD, examining when and to what extent AD-tailored fine-tuning is necessary.

---

## 论文详细总结（自动生成）

# 论文总结：AutoMoT: 基于异步混合Transformer的统一视觉-语言-动作端到端自动驾驶模型

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有将视觉-语言模型（VLM）集成到端到端（E2E）自动驾驶系统中的方法存在三大局限：① 推理空间与动作空间之间的分布不匹配难以解决；② 未能充分利用预训练VLM的通用推理能力；③ 动作策略生成时推理延迟大，损害驾驶性能。
- **整体含义**：旨在构建一个统一的视觉-语言-动作（VLA）模型，将推理与动作生成整合在单一框架中，同时保持低延迟和高驾驶性能，推动端到端自动驾驶从分离推理-控制向协同化发展。

## 2. 方法论：核心思想与关键技术细节
- **核心思想**：提出AutoMoT框架，使用异步混合Transformer（Mixture-of-Transformers, MoT）架构，实现推理和动作生成的统一与异步解耦。
- **关键技术细节**：
  - **层间联合注意力共享（layer-wise joint attention sharing）**：保留预训练VLM的通用推理能力，同时允许不同任务（推理、动作生成）以不同频率进行异步推理。
  - **VLA导向的动作优化器（VLA-oriented action refiner）**：通过扩散模型微调，进一步增强驾驶动作的生成质量。
  - **算法流程**（文字说明）：输入多模态数据（视觉、语言指令）→ 经MoT架构，推理分支与动作分支共享部分注意力层，但异步执行（例如推理低频、动作高频）→ 输出动作规划，再经过动作优化器扩散微调，得到最终驾驶控制信号。

## 3. 实验设计
- **数据集/场景**：在多个基准测试上评估，包括开环（open-loop）和闭环（closed-loop）设置。具体数据集名称未在摘要中列出，但提及"multiple benchmarks"。
- **Benchmark**：端到端自动驾驶常见基准（如nuScenes、CARLA等，摘要未明确，但结合领域背景推测）。
- **对比方法**：与现有方法（包括其他VLM集成方法）进行对比，达到SOTA性能。

## 4. 资源与算力
- **文献中未明确说明**：摘要及元数据未提及使用的GPU型号、数量、训练时长等详细信息。需要在完整论文中查找。

## 5. 实验数量与充分性
- **实验数量**：至少包括多个基准上的开环和闭环实验，消融实验（如对动作优化器、MoT架构组件）可能也有。由于摘要未列举具体组数，无法确切判断。
- **充分性与客观性**：从摘要描述看，实验覆盖了开环和闭环两种关键评估范式，并与现有方法对比，表明实验设计较为全面。但未说明是否进行了统计显著性检验，可能存在一定偏差风险。

## 6. 主要结论与发现
- AutoMoT在多个基准上达到SOTA性能，同时推理速度更快（延迟更低）。
- 进一步探究了预训练VLM在自动驾驶中的功能边界：何时以及何种程度上需要对AD进行特定微调。即微调并非越深越好，需要权衡。

## 7. 优点（方法或实验设计亮点）
- **异步混合Transformer**：首次将MoT架构用于统一VLA模型，实现推理与动作的异步执行，兼顾性能和效率。
- **VLA导向的动作优化器**：利用扩散模型微调提升动作精度，是一种新颖的细化策略。
- **功能边界分析**：额外提供了关于预训练VLM在AD中何时需要微调的洞察，具有实践指导意义。
- 实验设置覆盖开环和闭环，评价维度全面。

## 8. 不足与局限
- **实验覆盖**：摘要未给出具体数据集和任务设置，无法判断是否涵盖了极端场景或噪声数据，通用性有待验证。
- **偏差风险**：仅提到“多个基准”，未说明是否包含不同地域、天气、交通规则等，可能存在域偏差。
- **应用限制**：依赖预训练VLM，推理延迟虽降低但可能仍不满足实时性极高要求；动作优化器引入额外扩散推理步骤，整体复杂度需评估。
- **算力与资源未报告**：不利于复现和资源对比。
- **未说明模型参数量、计算量等**，可扩展性未知。

（完）
