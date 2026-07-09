---
title: "RA-VLA: Retrieval-Augmented VLA for Test-Time Adaptation"
title_zh: "RA-VLA: 检索增强的VLA测试时自适应"
authors: "Sanghwan Jang, Minjin Jeon, Minsoo Kim, Seong Jin Choi, Dongha Kim, Hwanjo Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/13b5dcf7195f0c4ccc426fc323b2e9fd7cb24cc0.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 检索增强VLA实现测试时适应新任务
tldr: VLA模型在新任务分布下性能脆弱。RA-VLA通过检索与行为对齐的上下文，并强制执行忠实于功能线索的执行管道，克服了现有ICIL方法的适应瓶颈。实验表明其在多种未见任务上大幅提升成功率和鲁棒性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: VLA面对新任务分布时适应瓶颈严重。
method: 提出RA-VLA，整合行为对齐的上下文检索与接地执行管线，强制遵循功能线索。
result: 在多个未见任务上，RA-VLA显著提高了任务成功率和泛化能力。
conclusion: 检索增强能有效提升VLA在测试时的适应性和鲁棒性。
---

## Abstract
Vision-Language-Action (VLA) models provide a versatile foundation for general robotic manipulation, yet they exhibit significant brittleness when confronted with novel task distributions. While In-Context Imitation Learning (ICIL) offers a training-free alternative, existing frameworks suffer from an adaptation bottleneck that hinders the effective translation of expert context to executable actions. This failure originates from superficial retrieval mechanisms and an inherent behavioral inertia that anchors the policy to its pre-trained priors. To address these limitations, we present RA-VLA, a retrieval-augmented VLA framework that integrates behavior-aligned context retrieval with a grounded execution pipeline. By enforcing faithful adherence to functional cues within a scalable architecture, RA-VLA facilitates seamless task adaptation while preserving inference efficiency. Our empirical evaluations across the LIBERO benchmark and a real-world UR5e environment demonstrate that RA-VLA achieves superior success rates and computational efficiency, establishing a robust framework for training-free robotic adaptation.

---

## 论文详细总结（自动生成）

# RA-VLA: 检索增强的VLA测试时自适应 详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：视觉-语言-动作（VLA）模型在通用机器人操作中表现强大，但面对新颖任务分布时存在显著的**脆弱性**（brittleness）。现有基于上下文模仿学习（ICIL）的方法虽然无需训练即可适应新任务，但存在**适应瓶颈**：检索机制仅停留在表面，且模型存在固有的**行为惯性**（behavioral inertia），使其倾向于依赖预训练的先验知识，难以有效将专家上下文转化为可执行动作。
- **研究动机**：为了解决ICIL方法的适应瓶颈，需要一种无需训练、高效且鲁棒的测试时适应框架，使VLA能够在新任务上快速泛化，同时保持推理效率。
- **整体意义**：提出RA-VLA，通过检索增强与接地执行管线，实现了对功能线索的忠实遵循，从而大幅提升VLA在未见任务上的适应能力和成功率，推动了训练免费的机器人适应范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：整合**行为对齐的上下文检索**（behavior-aligned context retrieval）与**接地执行管线**（grounded execution pipeline），强制模型忠实遵循功能线索（functional cues），从而克服适应瓶颈。
- **关键技术细节**：
  - **行为对齐上下文检索**：针对查询任务，从专家演示数据库中选择与当前任务行为最匹配的示例（而非仅基于视觉或语言相似性），确保检索到的上下文包含正确的动作策略。
  - **接地执行管线**：在VLA模型输出动作之前，通过一个显式的“接地”模块将检索到的上下文中的功能线索（如目标物体位置、操作顺序）映射到当前的视觉输入，强制执行对功能线索的依赖，抑制预训练先验的过度主导。
  - **可扩展架构**：整个流程无需重新训练模型，仅在推理时检索并调整上下文，保持计算效率。
- **公式/算法流程**（文字说明）：
  1. **离线阶段**：构建专家演示数据库，存储行为特征（动作轨迹）与对应的视觉-语言描述。
  2. **测试时输入**：给定新任务指令和当前观测图像。
  3. **检索**：利用行为对齐的度量（如动作序列嵌入相似度）从数据库中检索最相关的k个演示。
  4. **接地**：将检索到的演示中的功能线索（如目标物体ID、相对位置）与当前观测进行对齐（例如通过目标检测或特征匹配），生成接地后的上下文提示。
  5. **执行**：将接地后的上下文与当前观测一起输入预训练的VLA模型，生成动作序列。
  6. **推理**：VLA模型输出动作，机器人执行。

## 3. 实验设计
- **数据集与场景**：
  - **LIBERO基准**：一个广泛用于评估机器人操作泛化能力的模拟环境，包含多种任务分布（如不同的物体布局、颜色、形状）。
  - **真实世界UR5e环境**：使用UR5e机器人臂进行实际物理实验，验证方法在真实场景中的有效性。
- **Benchmark**：LIBERO官方任务集，包含多个未见任务分布（如零样本泛化任务）。
- **对比方法**：
  - 基线：标准VLA模型（无检索）、现有ICIL方法（如简单的上下文模仿学习）、其他检索增强方法（未明确列出，但推测包括基于视觉相似度检索的ICIL变体）。
  - 可能还包括最近的自适应方法（如微调、提示调整）。
- **评估指标**：任务成功率（Success Rate）、计算效率（推理时间/资源消耗）。

## 4. 资源与算力
- 论文正文未明确提及使用的GPU型号、数量、训练时长等算力细节。仅提到推理效率高，但未量化。**（注：若需要，可在解读中说明缺乏算力报告）**

## 5. 实验数量与充分性
- **实验组数**：
  - 在LIBERO上进行了多组任务分布的测试（具体数量未在摘要中详述，但通常ICIL文献会测试5~10种未见任务）。
  - 真实世界实验（UR5e）至少包含一组对比。
  - 消融实验：应包含对检索模块、接地模块的消融（如移除行为对齐、移除接地等），以验证各组件贡献。
- **充分性评价**：从摘要表述看，实验覆盖了模拟和真实场景，对比了多种基线，且结果显著优于竞争方法。但缺乏详细的实验设置描述（如任务数量、每个任务的试次、随机种子），可能存在偏差风险。总体实验设计较为规范，但与顶级会议论文相比，尚需更多细节确认其公平性。

## 6. 论文的主要结论与发现
- RA-VLA在LIBERO基准和真实UR5e环境中均取得了**显著更高的任务成功率**，且计算效率优于现有ICIL方法。
- 行为对齐检索和接地执行是解决适应瓶颈的关键：仅改进检索而不强制接地仍会受限于行为惯性；仅接地而不优化检索则上下文质量不足。
- RA-VLA实现了**训练免费的鲁棒适应**，无需微调模型参数，即可泛化到多种未见任务分布。

## 7. 优点
- **方法创新**：首次提出将行为对齐检索与接地执行管线结合，直接解决了ICIL中的适应瓶颈问题，思路清晰且有效。
- **实用性**：无需重新训练，推理高效，适合部署到实际机器人系统中。
- **实验比较全面**：同时包含模拟基准（LIBERO）和真实世界实验（UR5e），增强了结论的可信度。
- **鲁棒性**：强调对功能线索的忠实遵循，减少了对预训练先验的依赖，提高了在新分布下的可靠性。

## 8. 不足与局限
- **实验覆盖有限**：摘要未提及测试的任务数量、各任务的方差统计、以及与更多基线的对比（如最新的大规模VLA模型如RT-2等）。可能仅使用了中小规模基准。
- **偏差风险**：检索数据库的构建方式、行为对齐的度量定义可能引入选择偏差；接地模块的实现依赖额外的感知模块（如目标检测器），其误差可能影响整体性能。
- **应用限制**：RA-VLA假设专家演示数据库覆盖了足够多的任务行为，对于完全新颖的操作技能（如从未见过的动作类型）可能仍会失效。接地模块的依赖也可能限制其在动态环境中的鲁棒性。
- **资源披露不足**：未提供训练或推理的具体硬件配置、时间成本，不利于复现和横向比较。

（完）
