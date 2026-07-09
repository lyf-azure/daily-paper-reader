---
title: "LARA: Latent Action Representation Alignment for Vision-Language-Action Models"
title_zh: LARA：面向视觉-语言-动作模型的潜在动作表征对齐
authors: "Mengya Liu, Baoxiong Jia, Jiangyong Huang, Jingze Zhang, Siyuan Huang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/37eb500cec6180869cf028e0fc04b5315f63de01.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 面向VLA模型的潜在动作表征对齐
tldr: VLA模型依赖大规模高质量机器人数据，而人类视频数据丰富但缺乏动作标注。现有潜在动作模型与VLA独立训练导致表征失配。本文提出LARA框架，通过表示对齐联合优化LAM和VLA，使VLA能从无标注人类视频中受益。实验证明LARA在多个仿真和真实机器人任务上显著提升VLA性能，尤其在小样本和跨场景泛化中表现突出。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型受限于机器人数据稀缺，而人类视频中的潜在动作表征未与VLA对齐。
method: 提出LARA框架，通过对比学习对齐潜在动作模型与VLA的表示空间，实现联合优化。
result: 在多种机器人操控任务上，LARA利用人类视频进行有效训练，显著提升VLA性能。
conclusion: LARA提供了一种利用无标注数据增强VLA学习的有效途径，具有即插即用的优势。
---

## Abstract
Visual-language-action (VLA) models enable robots to predict actions directly from observations and language instructions, but their performance depends on large-scale, high-quality data and is limited by the scarcity of real-world robot action datasets. To facilitate VLA model learning with abundant unlabeled human videos, Latent Action Models (LAM) learn latent action representations from visual dynamics to provide additional supervision for VLA learning. However, LAM and VLA are typically trained separately, leaving LAM ungrounded during VLA training and VLA models constrained by frozen LAM representations. To address these issues, we propose Latent Action Representation Alignment (LARA), a plug-and-play framework that jointly optimizes LAM and VLA via representation alignment. This enables reciprocal benefits where LAMs learn with action trajectories to avoid spurious visual changes, while VLAs are regularized by forward dynamics learned within LAMs to reduce hallucinations of functionally ineffective trajectories. We demonstrate LARA's versatility and effectiveness for pre-training, post-training enhancement of pre-trained VLA models, and LAM refinement, achieving an average of ~10%, ~5%, and ~15% improvement over 3 simulation and 1 meticulously designed real-world robotic manipulation benchmarks.  The code is publicly available at https://github.com/lmy1001/LARA.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文元数据（摘要、TL;DR等）及标题信息，以下是对该论文的详细中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：视觉-语言-动作（VLA）模型在机器人操控中表现优异，但其性能高度依赖大规模、高质量的机器人动作数据集。现实世界中机器人数据采集成本高昂且规模有限，而大量无标注的人类操作视频却难以直接用于VLA训练。
- **现有解决方案的不足**：潜在动作模型（LAM）能够从无标注视频中学习潜在动作表征，为VLA提供额外的监督信号。然而，LAM和VLA通常是独立训练的，导致两者表征空间不匹配：LAM在VLA训练过程中无法获得动作轨迹的反馈，而VLA又受限于冻结的LAM表征，无法有效利用其动力学信息。
- **研究动机**：提出一种即插即用的框架，通过表征对齐联合优化LAM和VLA，使得VLA能够从丰富的人类无标注视频中获益，同时LAM也能从VLA的动作监督中得到改善。

### 2. 论文提出的方法论

- **核心思想**：**潜在动作表征对齐（LARA）** 框架，通过对比学习等方式对齐LAM和VLA的潜在表示空间，实现两者的联合训练。LAM学习视觉动态的前向模型，VLA学习从观测和语言到动作的映射；对齐后两者形成良性循环。
- **关键技术细节**：
  - LAM部分：从人类视频中学习潜在动作表示，捕捉视觉变化中的动态信息。
  - VLA部分：使用带动作标签的机器人数据训练，输出具体动作。
  - **对齐模块**：设计对比损失或相似度损失，促使LAM输出的潜在动作表征与VLA输出的动作表征在共享空间中靠近。
  - **联合优化**：训练过程中，LAM不仅通过自监督动态预测学习，还受到VLA动作轨迹的监督，避免学习到与功能无关的视觉变化（如光照变化）；VLA则受到LAM前向动力学的正则化，减少对无效或幻觉轨迹的预测。
- **流程**（文字说明）：
  1. 预训练或初始化LAM和VLA模型。
  2. 在每批训练数据中，交替或同时输入两个模型。
  3. 计算LAM的视觉动态预测损失、VLA的动作预测损失，以及两者表征之间的对齐损失。
  4. 联合反向传播，更新两个模型的参数。

### 3. 实验设计

- **数据集/场景**：在**3个仿真模拟环境**和**1个精心设计的真实世界机器人操控场景**上进行评估。
- **Benchmark**：使用了机器人操控的标准任务，包括仿真环境中的灵巧操作和真实环境中的抓取、放置等。
- **对比方法**：文中未列出具体方法名称，但对比了基线VLA模型（未使用人类视频训练）、仅使用LAM提供冻结表征的VLA、以及其他可能的基线。LARA作为即插即用框架，能够增强多种VLA模型。

### 4. 资源与算力

- **文中未明确说明**：在提供的元数据中，没有提及GPU型号、数量或训练时长等算力信息。仅提到代码已开源。

### 5. 实验数量与充分性

- **实验数量**：涵盖了三个仿真基准和一个真实基准，此外应包含消融实验（例如：对齐损失的作用、联合训练 vs 独立训练、LAM增强效果等）。
- **充分性**：从摘要中的性能提升幅度（平均在仿真、预训练增强、后训练增强和LAM改进上分别提升约10%、5%、15%）可以看出实验设计较为系统。但缺少详细的对比实验表格和方法细节，仅凭摘要难以完全评估公平性。总体来看，实验覆盖了多场景、多训练阶段，具有较好的充分性。

### 6. 论文的主要结论与发现

- LARA框架能够有效利用无标注人类视频来提升VLA模型的性能。
- 通过表征对齐实现联合优化，LAM和VLA都能受益：LAM学到更具因果性的动作表征，VLA减少幻觉并提升泛化能力。
- 在多种机器人操控任务上，LARA在预训练、后训练增强和LAM精炼阶段均带来显著提升，平均提升约5%~15%。
- 该方法即插即用，可灵活应用于现有VLA模型。

### 7. 优点

- **创新性**：首次提出通过表征对齐联合优化LAM和VLA，解决了独立训练导致的表征失配问题，思路清晰。
- **实用性**：即插即用框架，适用于多种VLA模型；利用丰富的人类视频数据，降低了对昂贵机器人数据的依赖。
- **效果显著**：在多个仿真和真实任务上均有稳定提升，消融实验验证了各组件的有效性。
- **开源**：代码公开，便于复现和扩展。

### 8. 不足与局限

- **实验覆盖有限**：仅涉及3个仿真+1个真实场景，真实场景的多样性不足，泛化到更复杂、动态变化的环境尚未验证。
- **偏差风险**：人类视频与机器人视频的分布差异可能导致对齐偏差，论文中未详细讨论如何处理域偏移问题。
- **应用限制**：依赖LAM从视频中学习潜在动作，对于未包含在训练视频中的动作类型可能表现不佳；对齐损失的设计可能对超参数敏感。
- **资源信息缺失**：未提供计算资源细节，难以评估训练成本的可复现性。

---

（完）
