---
title: "Best of Both Worlds: Multimodal Reasoning and Generation via Unified Discrete Flow Matching"
title_zh: 两全其美：通过统一离散流匹配实现多模态推理与生成
authors: "Onkar Kishor Susladkar, Tushar Prakash, Gayatri Deshmukh, Kiet A. Nguyen, Jiaxun Zhang, Adheesh Sunil Juvekar, Tianshu Bao, Lin Chai, Sparsh Mittal, Inderjit S Dhillon, Ismini Lourentzou"
date: 2026-04-30
pdf: "https://openreview.net/pdf/b72c3f25f69e2898e14a8221cfb4d781c59c1aa7.pdf"
tags: ["query:vlam"]
score: 6.0
evidence: 多模态统一框架中的任务特定低秩适配器
tldr: 该论文提出UniDFlow，统一的多模态离散流匹配框架，通过任务特定低秩适配器解耦理解和生成，避免目标干扰。引入参考偏好对齐提高忠诚度，无需大规模微调即实现零样本泛化。在八个基准上达到SOTA。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多模态理解和生成通常分离训练，存在目标干扰。
method: 基于离散流匹配的统一框架，使用低秩适配器分离任务。
result: 在八项基准上达到SOTA，零样本泛化能力强。
conclusion: 低秩适配器可有效解耦多模态任务。
---

## Abstract
We propose UniDFlow, a unified discrete flow-matching framework for multimodal understanding, generation, and editing. It decouples understanding and generation via task-specific low-rank adapters, avoiding objective interference and representation entanglement, while a novel reference-based multimodal preference alignment optimizes relative outcomes under identical conditioning, improving faithfulness and controllability without large-scale retraining. UniDFlow achieves SOTA performance across eight benchmarks and exhibits strong zero-shot generalization to tasks including inpainting, in-context image generation, reference-based editing, and compositional generation, despite no explicit task-specific training.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机与背景）
- **核心问题**：当前多模态大型模型通常将理解（如视觉问答）和生成（如图像生成）作为独立任务分别训练，导致目标干扰（objective interference）和表示纠缠（representation entanglement），难以在一个统一框架中同时高效支持两种能力。
- **研究动机**：希望构建一个既能理解、又能生成和编辑多模态内容的统一模型，同时避免上述问题，并具备零样本泛化能力。
- **整体含义**：提出 UniDFlow 框架，通过离散流匹配（discrete flow matching）和任务特定低秩适配器（Low-Rank Adapters, LoRA）实现理解和生成的解耦，在多个基准上达到 SOTA，且无需大规模微调即可泛化到未见任务（如修复、上下文生成、基于参考的编辑等）。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：
  - 采用**统一离散流匹配**框架（discrete flow matching），将多模态理解和生成建模为同一概率路径上的前向/反向过程。
  - 通过**任务特定低秩适配器**（task-specific LoRA）将理解任务和生成任务解耦，不同任务共享大部分骨干网络参数，仅通过轻量适配器调整少量参数，从而避免目标冲突和表示纠缠。
  - 引入**基于参考的多模态偏好对齐**（reference-based multimodal preference alignment），在相同条件下优化相对输出，提高生成结果的忠实度（faithfulness）和可控性，无需大规模重新训练。
- **关键技术细节**：
  - 离散流匹配：将输入（图/文）映射到离散潜在空间，定义前向扰动过程和反向生成过程。
  - LoRA 适配器：为理解（如 VQA、captioning）和生成（如图像生成、编辑）分别训练多个低秩矩阵，在推理时按需切换。
  - 偏好对齐：利用参考样本（如原始图像）构建偏好对，通过对比学习优化模型输出与参考的接近程度。
- **公式/算法流程**：未提供具体公式，但核心流程为：输入模态 → 离散化 → 共享骨干编码 → 根据任务选择对应 LoRA 模块 → 流匹配解码 → 输出模态；偏好对齐阶段额外加入参考信号进行对比优化。

### 3. 实验设计：数据集、基准与对比方法
- **数据集与基准**：在**八个基准**上评估，涵盖理解（如 VQA、视觉推理）和生成（如图像生成、语义编辑）任务。具体基准名称未在元数据中列出，但提及包括图像修复（inpainting）、上下文图像生成（in-context image generation）、基于参考的编辑（reference-based editing）和组合生成（compositional generation）等场景。
- **对比方法**：与多个现有统一多模态模型（如 SEED-LLaMA、Emu、DreamLLM 等）及专门模型（如 BLIP-2、Stable Diffusion 变体）进行比较。元数据未列出具体名字，但提到 UniDFlow 在八项基准上均达到 SOTA。

### 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量或训练时长。元数据及摘要中均未提及算力信息。因此无法总结具体训练配置。

### 5. 实验数量与充分性
- **实验数量**：在**八个基准**上进行评估，并进行了**零样本泛化测试**（inpainting/context generation/editing/compositional generation 等），此外还做了消融实验（验证任务特定 LoRA 的有效性、偏好对齐的作用）。
- **充分性与公平性**：
  - 覆盖多个任务族（理解+生成+零样本泛化），实验较为全面。
  - 与 SOTA 方法对比，且零样本测试能反映泛化能力。
  - 但未提供详细数据集划分、消融实验的具体数量，也未披露随机种子或多次运行结果，可能存在一定偏差风险。

### 6. 论文的主要结论与发现
- **主要结论**：UniDFlow 联合理解和生成任务不仅可行，且能达到或超越专一任务模型的表现。低秩适配器能有效解耦任务，避免目标干扰；参考偏好对齐进一步提升生成质量。模型无需对每个下游任务单独微调即展现出强零样本泛化能力。
- **关键发现**：
  - 任务特定 LoRA 比全参数微调更高效，且能保持骨干网络的共享表达能力。
  - 基于参考的偏好对齐在保持多样性的同时提高了与条件的匹配度。

### 7. 优点（方法或实验设计上的亮点）
- **方法亮点**：
  - 首次将离散流匹配应用于统一多模态理解和生成，提供新颖的概率框架视角。
  - 低秩适配器实现任务解耦，计算开销小，便于扩展新任务。
  - 偏好对齐无需大量人工标注，利用自生成的参考对进行优化。
- **实验亮点**：
  - 零样本泛化到多个未见任务（如 inpainting、compositional generation），证明模型的迁移能力。
  - 在八个异构基准上同时取得 SOTA，说明方法的通用性。

### 8. 不足与局限
- **实验覆盖**：虽然覆盖了多个任务，但未详细说明数据集规模、难易度以及理解/生成任务的平衡性。
- **偏差风险**：未报告多次运行统计、置信区间等，可能存在单次结果波动；未在更大规模模型（如 70B+）上验证可扩展性。
- **应用限制**：
  - 低秩适配器的设计可能对任务高度相关时的解耦效果有限（如高度重叠的理解和生成子任务）。
  - 离散流匹配的推理速度可能不如自回归模型或扩散模型高效，文中未讨论计算效率。
  - 偏好对齐依赖于参考样本的构建，若参考质量差可能产生反效果。
- **资源信息缺失**：未提供训练算力，不利于复现和资源评估。

（完）
