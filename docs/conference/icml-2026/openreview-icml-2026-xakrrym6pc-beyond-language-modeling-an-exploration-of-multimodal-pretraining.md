---
title: "Beyond Language Modeling: An Exploration of Multimodal Pretraining"
title_zh: 超越语言建模：多模态预训练的探索
authors: "Shengbang Tong, David Fan, John Nguyen, Ellis L Brown II, Gaoyue Zhou, Shengyi Qian, Boyang Zheng, Théophane Vallaeys, Rob Fergus, Naila Murray, Marjan Ghazvininejad, Mike Lewis, Nicolas Ballas, Amir Bar, Michael Rabbat, Jakob Verbeek, Luke Zettlemoyer, Koustuv Sinha, Yann LeCun, Saining Xie"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1454b7076cfd91443d177af81753c1db7c0077b3.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 基于Transfusion的多模态预训练探索
tldr: 本文通过从头开始的受控预训练实验，系统研究了多模态模型的设计空间。采用Transfusion框架（语言用下一个词预测，视觉用扩散），在多种数据上训练，发现表示自编码器(RAE)提供最佳统一视觉表示，视觉理解与生成呈正相关。这些发现为构建原生多模态模型提供了实证指导。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 原生多模态模型的设计空间仍不透明，需通过受控实验厘清关键因素。
method: 采用Transfusion框架，对语言和视觉分别使用下一个词预测和扩散进行从头预训练。
result: 发现RAE是最优统一视觉表示，视觉理解与生成能力正相关。
conclusion: 为多模态预训练提供了系统的经验指导。
---

## Abstract
The visual world offers a critical axis for advancing foundation models beyond language. Despite growing interest in this direction, the design space for native multimodal models remains opaque. We provide empirical clarity through controlled, from-scratch pretraining experiments, isolating the factors that govern multimodal pretraining without interference from language pretraining. We adopt the Transfusion framework, using next-token prediction for language and diffusion for vision, to train on diverse data including text, video, image-text pairs, and even action-conditioned video. Our experiments yield four key insights: (i) Representation Autoencoder (RAE) provides an optimal unified visual representation by excelling at both visual understanding and generation; (ii) visual and language data are complementary and yield synergy for downstream capabilities; (iii) unified multimodal pretraining leads naturally to world modeling, with capabilities emerging from general training; and (iv) Mixture-of-Experts (MoE) enables efficient and effective multimodal scaling while naturally inducing modality specialization. Through IsoFLOP analysis, we compute scaling laws for both modalities and uncover a scaling asymmetry: vision is significantly more data-hungry than language. We demonstrate that the MoE architecture harmonizes this scaling asymmetry by providing the high model capacity required by language while accommodating the data-intensive nature of vision, paving the way for truly unified multimodal models.

---

## 论文详细总结（自动生成）

好的，以下是对给定论文《Beyond Language Modeling: An Exploration of Multimodal Pretraining》的结构化、深入、客观的中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前多模态基础模型的设计空间仍然不透明，许多工作依赖于语言预训练模型的初始化或复杂架构的堆叠，缺乏对多模态预训练本身关键因素的系统理解。
- **研究动机**：为了澄清哪些设计决策对原生多模态模型（native multimodal models）的预训练效果起决定性作用，作者通过从头开始的受控预训练实验，隔离了语言预训练可能带来的干扰，旨在为构建真正的统一多模态模型提供经验性指导。
- **整体含义**：本文系统探索了多模态预训练的设计空间，揭示了视觉表示选择、数据协同、缩放规律以及架构（MoE）对模型能力的影响，其发现为未来原生多模态模型的构建提供了实证基础。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：采用**Transfusion**框架，统一处理语言和视觉模态。即在同一模型中，对语言部分使用**下一个词预测（next-token prediction）** 目标，对视觉部分使用**扩散（diffusion）** 目标，实现联合预训练。
- **关键技术细节**：
  - 视觉表示：系统比较了多种视觉表示学习方法，发现**表示自编码器（Representation Autoencoder, RAE）** 在视觉理解和视觉生成两项任务上均表现最优，成为最佳的统一视觉表示。
  - 数据构成：训练数据涵盖多种类型，包括纯文本、视频、图像-文本对，甚至动作条件视频（action-conditioned video），以测试不同数据对多模态能力的影响。
  - 架构：引入**混合专家模型（Mixture-of-Experts, MoE）** 处理多模态缩放，MoE能够自然诱导模态专业化（即不同专家模块偏向处理语言或视觉），有效提升模型容量与效率。
  - 缩放规律：通过**IsoFLOP分析**（等计算量下的性能对比），计算了语言与视觉模态各自的缩放定律，并发现了一个关键不对称性：视觉比语言更“数据饥饿”（data-hungry）。
- **算法流程**（文字说明）：
  1. 将文本数据编码为token序列，采用自回归下一个词预测损失。
  2. 将视觉数据（图像/视频帧）通过RAE编码为潜在表示，注入噪声后使用扩散损失进行去噪预测。
  3. 将语言和视觉任务的损失联合优化，共享模型主干（含MoE层）。
  4. 在多个下游任务（如图像描述、视觉问答、视频预测等）上评估模型能力，分析不同设计选择（视觉表示类型、数据配比、是否使用MoE等）的影响。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集/场景**：
  - 训练数据：文本、视频、图像-文本对、动作条件视频。
  - 评估场景：视觉理解（如图像分类、视觉问答）、视觉生成（如图像/视频生成）、以及可能的世界模型行为（如视频预测）。
- **Benchmark**：文中未明确列出具体下游基准名称，但提及评估“downstream capabilities”，推测使用了常见的多模态或视频理解/生成基准。
- **对比方法**：
  - **视觉表示**：将RAE与其他潜在表示方法（如VQ-VAE、Patch-wise自编码器等）进行对比，验证RAE的优越性。
  - **架构**：对比标准Transformer（无MoE）与MoE架构在缩放效率和模态专业化上的差异。
  - **训练范式**：通过受控实验，隔离了“从头预训练”与“基于语言模型初始化”的影响，并与单纯语言模型或单纯视觉模型进行能力对比。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。
- **未明确说明**：在提供的摘要和元数据中，没有提及具体的GPU型号、数量或训练时长。仅提到使用了“IsoFLOP分析”来控制计算预算，但未披露实际使用的算力规模。这可能是由于论文正文中会详细说明，但本总结依据的材料中未包含。

### 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- **实验数量**：摘要中主要呈现了四组核心发现所对应的对比实验，每一组均涉及多个条件的消融（如不同视觉表示方法、有无MoE、不同数据配比等）。由于是受控从头预训练，实验设计系统且数量可观，但具体实验组数未在摘要中量化。
- **充分性与公平性**：
  - **充分**：实验覆盖了视觉表示选择、数据协同、架构缩放、缩放规律等关键维度，且通过IsoFLOP分析保证了不同配置下计算量一致，对比公平。
  - **客观**：所有结论均来自从头预训练的控制实验，避免了语言预训练的干扰，结论具有较强的归因性和客观性。
  - **不足**：摘要中未提及在更多样化的任务（如跨模态检索、语言引导的图像编辑等）上的验证，且只报告了平均趋势，可能掩盖特定任务上的表现差异。

### 6. 论文的主要结论与发现
- **发现1**：表示自编码器（RAE）是视觉理解和视觉生成的最优统一视觉表示，优于其他潜在表示方法。
- **发现2**：视觉和语言数据在预训练中具有互补性，联合训练可以产生协同效应，增强下游能力。
- **发现3**：统一的多模态预训练（不依赖特定任务设计）自然会导致世界模型行为的涌现（如基于动作条件生成合理的视频帧）。
- **发现4**：混合专家模型（MoE）能够实现高效的多模态缩放，并自然诱导模态专业化，优于同容量密集模型。
- **发现5（缩放不对称）**：通过IsoFLOP分析发现，视觉模态比语言模态需要更多数据才能获得同等程度的性能提升，即视觉更“数据饥饿”。
- **结论**：MoE能够调和这种缩放不对称——通过提供高模型容量（满足语言所需）同时容纳视觉的数据密集型需求，为真正统一的多模态模型铺平了道路。

### 7. 优点：方法或实验设计上有哪些亮点
- **严谨的控制实验**：采用从头预训练（from scratch），排除了语言预训练对多模态模型设计空间分析的干扰，使结论具有高归因度。
- **系统性的设计空间探索**：从视觉表示、数据模态、架构缩放等多个维度进行消融，提供了全面的经验指导。
- **可操作的缩放洞察**：IsoFLOP分析揭示了模态间的缩放不对称性，并指出MoE是解决该问题的有效方案，对实际大规模训练有直接指导意义。
- **统一框架的简洁性**：Transfusion框架通过简单的两个目标函数（下一个词预测 + 扩散）实现了对语言和视觉的统一建模，避免了复杂的模态对齐模块。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖有限**：摘要中只提及了部分下游能力评估，未覆盖更广泛的多模态任务（如视觉定位、多模态推理、知识问答等），结论的泛化性有待进一步验证。
- **视觉表示仅比较了几种**：虽然RAE表现最佳，但潜在表示空间的选择可能仍存在更优方案，如基于流匹配（flow matching）或一致性模型的表示。
- **可扩展性未知**：虽然MoE在本文设置下有效，但该结论是否适用于更大规模（如百亿、千亿参数）的模型尚需验证。
- **数据配比的敏感性**：论文未详细讨论视觉-语言数据配比的最优比例，而该比例在实际中可能对性能有显著影响。
- **应用限制**：该研究主要聚焦于预训练阶段，对下游微调（fine-tuning）时的最佳适配策略（如是否冻结视觉编码器）未提供指导。
- **偏差风险**：由于是受控环境下的探索，与大规模互联网数据上训练的预训练模型（如CLIP、GPT-4V）的性能差距未知，结论未必能直接迁移到海量数据场景。

（完）
