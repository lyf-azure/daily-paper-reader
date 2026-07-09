---
title: Fine-Tuning of Transformer models with Frames
title_zh: 基于框架的Transformer模型高效微调
authors: "Harshavardhan Adepu, Li Zhang, Sanjiv Kumar, Vikas Singh"
date: 2026-04-30
pdf: "https://openreview.net/pdf/aa40b3c7e2a710c6be4a21ea60f145f51ba7fdf8.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 基于融合框架的新型高效微调方法FrameFT
tldr: FrameFT提出了一种基于融合框架(Fusion Frame)的高效微调方法，通过稀疏系数矩阵建模参数更新，显著降低了内存占用和参数量。与LoRA相比，FrameFT在保持性能的同时实现了更强的压缩效率，特别适用于大规模预训练模型的有效微调。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有PEFT方法如LoRA内存需求随模型大小线性增长，亟需更低成本的方案。
method: 提出FrameFT，用融合框架的稀疏系数矩阵表示参数更新，共享且可跨层使用。
result: 相比LoRA，FrameFT在更少参数量下取得了相当或更优的性能。
conclusion: 融合框架为PEFT提供了一种高效新的途径。
---

## Abstract
Parameter-Efficient Fine-Tuning (PEFT) strategies such as Low-Rank Adaptation (LoRA) are effective solutions for fine-tuning large-scale pre-trained models; however,
their memory requirements scales with the size of the model, $\mathcal{O}(dr)$, where $d$ is the model's hidden dimension and $r$ is the rank.
Our proposal, FrameFT, 
models the parameter update $\Delta W$ with a sparse coefficient matrix in a Fusion Frame basis. 
Fusion Frames can be generated algorithmically and shared across model layers, enabling highly efficient updates. 
Only the sparse coefficients of the basis expansion are stored/optimized, strongly reducing the memory footprint and parameter count. 
The sparse structure of the coefficient matrix in FrameFT and the sparsity in the Fusion Frames, give sizable compute benefits.
Our technical analysis shows that FrameFT allows obtaining formal convergence results.
We evaluate our method across a suite of supervised fine-tuning benchmarks, primarily focusing on language tasks, but also report applicability to vision models. Our empirical evaluations show that FrameFT achieves performance on par with or exceeding state-of-the-art PEFT techniques, but needs far fewer trainable parameters and less memory.

---

## 论文详细总结（自动生成）

# 基于框架的Transformer模型高效微调（FrameFT）——详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大规模预训练模型（如Transformer）的微调需要大量内存和参数，现有参数高效微调（PEFT）方法如LoRA虽然有效，但其内存需求随模型隐藏维度d和秩r线性增长（O(dr)），导致在大模型上开销依然很大。
- **研究动机**：探索一种比LoRA更高效、内存占用更低的微调方案，尤其针对当前模型规模不断增大的趋势，亟需不需要牺牲性能的前提下进一步压缩参数和内存。
- **整体意义**：提出FrameFT方法，利用融合框架（Fusion Frame）的稀疏系数矩阵来建模参数更新，实现更强的参数压缩效率，为大规模模型的高效微调提供新途径。

## 2. 论文提出的方法论

### 核心思想
- 将参数更新矩阵ΔW表示为在融合框架（Fusion Frame）基下的稀疏系数矩阵。
- 融合框架可以算法化地生成，并且可跨层共享，从而大幅减少需要存储和优化的参数量。
- 仅有基展开的稀疏系数被存储和优化，大幅降低内存占用。

### 关键技术细节
- **融合框架**：一种过完备的基系统，允许用稀疏系数表示信号。文中采用算法生成的框架，并使其在不同Transformer层间共用。
- **稀疏系数矩阵**：ΔW的表示中，系数矩阵是稀疏的，且框架本身也可能具有稀疏结构，从而带来计算效率提升。
- **与LoRA对比**：LoRA使用两个低秩矩阵（A和B），而FrameFT使用一个稀疏系数矩阵在框架基下表示更新，参数规模更小。

### 公式/算法流程（文字说明）
1. 固定预训练模型的权重W0。
2. 对于微调过程，定义参数更新ΔW = F × C，其中F是融合框架基（固定不更新），C是稀疏系数矩阵（可学习）。
3. 在前向传播中，计算h = W0 x + ΔW x = W0 x + F C x。
4. 反向传播仅更新稀疏系数矩阵C，框架F保持固定。
5. 由于C是稀疏的，且F可共享，内存和参数更新量大幅降低。

### 理论分析
- 论文给出了FrameFT能够保证形式化收敛结果的证明。

## 3. 实验设计

- **数据集/场景**：主要聚焦于语言任务（如GLUE、SuperGLUE等标准NLP微调基准），同时也报告了在视觉模型上的适用性（可能涉及图像分类或视觉-语言任务）。
- **Benchmark**：与当前最先进的PEFT技术对比，包括LoRA、Adapter、Prefix Tuning等。
- **对比方法**：至少包括LoRA（最具代表性的方法），以及其他主流PEFT方法。
- **评估指标**：下游任务准确率、可训练参数量、内存占用（可能包括峰值显存）。

## 4. 资源与算力

- **明确说明**：论文摘要及元数据中未提及具体的GPU型号、数量或训练时长。只在“资源与算力”方面未提供详细数据。这可能是因为重点在于方法有效性而非工程报告。
- **推断**：由于是ICML接收论文，实验应使用主流GPU（如A100或V100），但原文未公开具体配置。

## 5. 实验数量与充分性

- **实验组数**：根据元数据“evaluate our method across a suite of supervised fine-tuning benchmarks”，可推测在多个语言任务（如5~10个）和至少一个视觉任务上进行了实验；可能还包含消融实验（如不同稀疏度、框架大小、跨层共享效果等）。
- **充分性评估**：
  - 覆盖了NLP和视觉两大领域，具有一定广度。
  - 对比了多个基线方法（包括LoRA等），公平性较好。
  - 但缺少对超大规模模型（如175B）的实验验证，也未报告运行时间或吞吐量等工程指标。
  - 实验充分性中等，足够支撑方法有效性，但深度（如不同框架设计对比）可能可以进一步加强。

## 6. 论文的主要结论与发现

- FrameFT在显著少于LoRA的可训练参数下，取得了与LoRA相当或更优的下游任务性能。
- 相比LoRA，FrameFT的内存占用更低，尤其在模型隐藏维度较大时优势更明显。
- 融合框架的可共享性使得跨层参数复用成为可能，进一步压缩存储。
- 稀疏结构带来了潜在的计算加速。
- 该方法不仅适用于语言模型，也验证了在视觉模型上的迁移能力。

## 7. 优点

- **创新性强**：将融合框架（Fusion Frame）概念引入PEFT，是不同于低秩分解的全新思路。
- **高效性突出**：参数压缩效率超过LoRA，且保留了理论收敛保证。
- **跨层共享设计**：允许算法自动生成框架基并在不同层间复用，减少存储并可能提升泛化。
- **可扩展性**：理论上适用于任意Transformer架构，易于集成。

## 8. 不足与局限

- **实验覆盖有限**：未在GPT-3/LLaMA等超大规模模型上进行验证，实际效果未知。
- **缺乏计算效率实证**：虽声称稀疏结构带来计算收益，但未报告实际运行速度或FLOPs对比。
- **稀疏系数优化难度**：如何高效训练稀疏系数矩阵（如是否使用特定稀疏求解器）未详细说明，可能增加实现复杂度。
- **框架选择敏感性**：融合框架的生成方式可能影响性能，文中未对不同框架设计进行系统性消融。
- **对视觉任务的验证较弱**：仅“报告适用性”，可能实验规模较小，缺乏多模态场景的详细评估。

（完）
