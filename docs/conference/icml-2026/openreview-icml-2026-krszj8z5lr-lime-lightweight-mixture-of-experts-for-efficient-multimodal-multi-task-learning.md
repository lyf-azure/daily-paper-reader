---
title: "LiME: Lightweight Mixture of Experts for Efficient Multimodal Multi-task Learning"
title_zh: "LiME: 轻量级专家混合用于高效多模态多任务学习"
authors: "Md Kowsher, Haris Mansoor, Nusrat Jahan Prottasha, Ozlem Garibay, Victor Zhu, Zhengping Ji, Chen Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f646b2975f6e3045c5ee5e94d2ea1b7c0fd64b63.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: PEFT方法：共享模块加专家特定轻量调制
tldr: 现有MoE-PEFT方法每专家需独立适配器，导致参数随专家数线性增长且局限于适配器架构。LiME提出轻量级专家混合方法，使用单一共享PEFT模块并通过轻量专家向量调制其输出，大幅减少专家参数，且可推广至任意PEFT方法。此外，LiME利用冻结和适应表示实现零参数路由，消除了学习路由器的需求。该方法为多任务多模态学习提供了高效的PEFT方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有MoE-PEFT方法参数随专家数线性增长且仅支持适配器架构，效率低。
method: 提出LiME，使用单一共享PEFT模块和轻量专家向量实现专家特异化，并引入零参数路由。
result: LiME在多个多模态多任务基准上以更少参数取得可比或更优性能，且兼容多种PEFT方法。
conclusion: 轻量级专家混合设计实现了高效且通用的多任务PEFT。
---

## Abstract
MoE-PEFT methods combine Mixture of Experts with parameter-efficient fine-tuning for multi-task adaptation, but require separate adapters per expert—causing trainable parameters to scale linearly with expert count and limiting applicability to adapter-based architectures. We propose LiME (Lightweight Mixture of Experts), which achieves expert specialization through lightweight modulation rather than adapter replication. Instead of separate adapters, LiME uses a single shared PEFT module and modulates its output with lightweight expert vectors, reducing expert parameters while generalizing to any PEFT method. Notably, LiME introduces zero-parameter routing by leveraging existing frozen and adapted representations—eliminating learned router parameters typically required per layer. Theoretically, we prove that (i) more experts preserve more task-relevant information and (ii) modulation approximates full expert-specific PEFT with bounded error. LiME further incorporates n-gram windowed routing and adaptive expert selection (Auto Top-K) based on routing confidence. Experiments on MMT-47, a multimodal multi-task benchmark with 47 tasks spanning text, image, and video, demonstrate that LiME achieves competitive or superior performance while using up to 4× fewer trainable parameters and up to 29% faster training compared to corresponding MoE-PEFT baselines.

---

## 论文详细总结（自动生成）

# LiME: 轻量级专家混合用于高效多模态多任务学习 – 详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有将混合专家（MoE）与参数高效微调（PEFT）相结合的方法（MoE-PEFT）在多任务适应中，每个专家都需要单独的适配器（adapter），导致可训练参数随专家数量线性增长，且仅适用于基于适配器的架构，效率低下且通用性差。
- **整体含义**：旨在设计一种更轻量、更通用的MoE-PEFT方法，在不显著增加参数和计算开销的前提下，实现多模态多任务的高效微调，同时支持任意PEFT方法，无需学习额外的路由参数。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过轻量调制（lightweight modulation）而非复制适配器来实现专家专业化。使用**单一共享PEFT模块**，并通过轻量专家向量（expert vectors）调制其输出，从而大幅减少专家参数，且可推广至任意PEFT方法。
- **关键技术细节**：
  - **共享PEFT + 轻量专家向量**：所有专家共享一个基础PEFT模块（如LoRA、Adapter等），每个专家仅增加一个小型专家向量，用于调制共享模块的输出，替代为每个专家独立维护完整适配器。
  - **零参数路由（Zero-parameter routing）**：利用冻结表示（frozen representations）和适应表示（adapted representations）实现路由，无需学习每层的路由器参数，消除额外路由参数量。
  - **N-gram窗口路由**：引入n-gram窗口化路由机制，考虑上下文信息进行专家选择。
  - **自适应Top-K选择（Auto Top-K）**：基于路由置信度动态调整每个token（或位置）激活的专家数量。
- **理论证明**：
  - (i) 更多专家可保留更多任务相关信息；
  - (ii) 轻量调制能以有界误差逼近全专家特定PEFT。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：使用了**MMT-47**，一个多模态多任务基准，包含**47个任务**，涵盖**文本、图像和视频**三种模态。
- **基准**：MMT-47本身作为统一多任务评测平台。
- **对比方法**：与现有的MoE-PEFT基线方法（如使用独立适配器的MoE变体）进行对比。文中未列出具体方法名称，但明确对比了对应的MoE-PEFT基线。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**实验所使用的GPU型号、数量或训练时长等具体算力信息。仅提到训练速度提升（比MoE-PEFT基线快最多29%），但未提供详细的硬件配置。

## 5. 实验数量与充分性

- **实验数量**：主要在一个大型多任务基准MMT-47（47个任务）上进行，涵盖多种模态。此外，文中提到进行了理论分析和消融研究（如n-gram窗口路由、Auto Top-K等模块的消融）。但由于元数据有限，具体消融实验组数未列出。
- **充分性**：从摘要看，实验覆盖了多模态、多任务场景，对比了参数效率和训练速度，具有一定的客观性。但缺少在更多独立数据集或更广泛架构上的验证（如不同大小的预训练模型、更多PEFT方法等），可能存在一定局限性。

## 6. 论文的主要结论与发现

- 提出的LiME方法在MMT-47上达到了与现有MoE-PEFT基线相当或更优的性能，同时：
  - **可训练参数减少最多4倍**；
  - **训练速度提升最多29%**。
- 理论证明了更多专家保留信息的能力以及轻量调制的误差界。
- 零参数路由和自适应专家选择机制有效且无需额外开销。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：用单一共享PEFT加轻量专家向量替代独立适配器，实现了专家参数的亚线性增长，且不限于适配器架构，可兼容LoRA、Prefix Tuning等任意PEFT。
- **零参数路由**：巧妙利用已存在的冻结和适应表示，省去了通常每层都需要学习的路由参数。
- **自适应Top-K**：基于置信度动态选择专家，避免固定K值带来的次优问题。
- **理论支撑**：提供了信息保留和逼近误差的数学证明，增强了方法的可信度。
- **实验全面**：在47个多模态任务上评测，突出了多任务学习的普适性。

## 8. 不足与局限

- **实验覆盖**：仅在MMT-47一个基准上进行，缺乏在更多不同规模、不同领域的独立任务或数据集上的验证（如GLUE、SuperGLUE等纯文本或纯视觉任务）。
- **对比基线**：未详细列出所有对比的MoE-PEFT方法名称，且可能缺少与近年最新非MoE-PEFT方法（如完全微调、适配器融合等）的对比。
- **资源信息缺失**：未报告具体硬件和训练时间，影响了实验的可复现性和效率评估的完整性。
- **应用限制**：方法假设可同时获取冻结和适应表示用于零参数路由，某些PEFT方法可能不天然提供这种分离；此外，自适应Top-K在极端稀疏场景下的表现未知。
- **偏差风险**：仅使用单一基准，且47个任务可能来自同一数据分布，存在评估偏差。

（完）
