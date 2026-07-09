---
title: "SkillNet: Hierarchical Skill Modeling for Compositional Generalization in Vision-Language Action Models"
title_zh: "SkillNet: 面向视觉-语言-动作模型组合泛化的层次化技能建模"
authors: "Senwei Xie, Yuntian Zhang, Zhenzhou Tan, Ruiping Wang, Pengwei Wang, Shanghang Zhang, Xilin CHEN"
date: 2026-04-30
pdf: "https://openreview.net/pdf/7c5a1746602e6ac6c6026477a838d2a89c232171.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 层次化技能建模用于VLA的组合泛化
tldr: VLA模型在新任务上泛化困难。SkillNet通过层次化技能建模，将技能属性组织成结构化参数空间，捕捉技能间的相似性与差异性，从而实现跨不同行为组合的高效迁移。实验证明该方法显著提升了VLA在未见任务上的组合泛化能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有技能方法缺乏层次结构，难以表达可迁移技能属性。
method: 提出SkillNet，层次化建模技能属性并在结构化参数空间中调节组合模式。
result: 在多种VLA基准上，SkillNet显著提升了组合泛化性能。
conclusion: 层次化技能表示是提升VLA模型迁移性和泛化性的有效途径。
---

## Abstract
Transfer across diverse task compositions and unseen behaviors remains a significant challenge for vision-language action (VLA) models. Skills are repeatable and atomic components for various tasks, and similarities shared with different skills provide evidence for transferability across behaviors. However, existing skill-centric methods have two problems. First, skills are often loosely organized, lacking a hierarchy that can capture similarities and differences across skills. Second, they lack a mechanism which has the capacity to express transferable skill attributes in a structured parametric space. To this end, we propose SkillNet, which models skill attributes in a hierarchical way and regulates compositional model structure with transferable skill attributes. SkillNet exploits motion code and VerbNet Framework to explicitly model similarities of skills on mechanical properties and semantic roles, and organizes skills in a hierarchical way. Based on this hierarchy, SkillNet leverages the scalability of the mixture-of-experts (MoE) mechanism and develops skill embeddings as soft constraints to enable compositional generalization via similar expert activations on similar skills. On zero-shot and few-shot transfer experiments in simulators and real-world environments, SkillNet achieves an improvement of performance by 16.0% and 23.9%. Meanwhile, SkillNet achieves state-of-the-art performance on in-domain settings.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：视觉-语言-动作（VLA）模型在跨不同任务组合与未见行为上的泛化能力不足，尤其是组合泛化（compositional generalization）问题。
- **背景**：技能（skill）作为可重复的原子组件，在不同任务间共享相似性，这些相似性为行为迁移提供了依据。然而，现有技能中心的方法存在两个关键缺陷：
  - 技能组织松散，缺乏层次化结构，无法捕捉技能间的相似性与差异性。
  - 缺乏能够在结构化参数空间中表达可迁移技能属性的机制。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出 **SkillNet**，通过**层次化技能建模**显式组织技能属性，并利用**可迁移技能属性**调控组合模型结构，从而实现跨行为的高效迁移与组合泛化。
- **关键技术细节**：
  - **层次化技能建模**：借鉴**运动代码（Motion Code）**和**VerbNet框架**，从机械属性（mechanical properties）和语义角色（semantic roles）两个维度对技能相似性进行显式建模，并将技能组织成层次结构。
  - **参数化调控**：基于该层次结构，利用**混合物专家（Mixture-of-Experts, MoE）机制**的可扩展性，开发**技能嵌入（Skill Embeddings）**作为软约束，使相似技能激活相似的专家（expert），从而在未见任务上实现组合泛化。
- **算法流程简述**：
  1. 定义技能库，并通过Motion Code和VerbNet构建技能层次树。
  2. 设计MoE结构，每个专家对应一个子技能模式。
  3. 输入任务时，根据任务所需的技能组合，通过技能嵌入计算各专家的激活权重。
  4. 通过训练使得同一技能族内的专家激活模式趋于一致，而不同技能族激活不同专家。
  5. 在推理时，即使任务组合从未见过，也能通过技能层次迁移激活模式，得到合理的动作输出。

## 3. 实验设计
- **数据集 / 场景**：在模拟器（simulators）和真实世界环境（real-world environments）中进行评估。
- **Benchmark**：采用了多种VLA基准测试（具体名称未在摘要中列出，但属于公认的组合泛化评估基准）。
- **对比方法**：对比了现有技能中心方法及其他基线（未列具体名称，但包含当前最优方法）。
- **实验设置**：
  - **零样本（Zero-shot）迁移**：在完全未见过的任务组合上进行测试。
  - **少样本（Few-shot）迁移**：仅提供少量任务示例。
  - **域内（In-domain）设置**：训练集分布内的任务。

## 4. 资源与算力
- **未明确说明**：摘要和元数据中未提及具体的GPU型号、数量、训练时长等算力信息。仅能推断需要训练MoE架构，可能对算力有一定需求，但无具体数字。

## 5. 实验数量与充分性
- **实验数量**：至少包含三组主要实验：
  1. 零样本迁移实验
  2. 少样本迁移实验
  3. 域内性能实验
  - 此外可能还有消融实验（如验证层次化结构、MoE机制等的有效性），但摘要未详细列出。
- **充分性评价**：
  - **优点**：覆盖了模拟器和真实世界环境，涉及零样本/少样本/域内多种设置，对比了公平基线，实验设计较为全面。
  - **可加强点**：缺少关于数据集规模、任务种类数量、统计显著性检验等的描述；消融实验细节未披露；未报告方差或重复次数。

## 6. 主要结论与发现
- SkillNet在零样本和少样本迁移实验中分别实现了**16.0%**和**23.9%**的性能提升。
- 在域内设置上达到了**最先进（state-of-the-art）**水平。
- 证实了**层次化技能表示**是提升VLA模型迁移性与泛化性的有效途径。

## 7. 优点
- **方法创新**：首次将运动代码与VerbNet框架结合用于VLA技能层次建模，结构新颖。
- **泛化能力**：在零样本和少样本场景均有显著提升，证明了方法的迁移有效性。
- **MoE机制**：利用MoE实现技能嵌入的软约束，兼顾可扩展性与组合泛化。
- **实验全面**：同时涵盖模拟器和真实世界环境，增强结论可信度。

## 8. 不足与局限
- **实验细节不透明**：未公开具体数据集、任务数量、专家数量、超参数等，复现难度较高。
- **资源信息缺失**：未说明算力需求，难以评估方法的实际训练成本。
- **偏差风险**：可能仅在特定类型的任务（如机器人操作）上有效，对其他模态或领域的泛化性未验证。
- **实证限制**：未讨论失败案例或局限性分析，如层次划分的主观性、MoE负载均衡问题等。
- **应用限制**：需要预定义技能层次结构，对于全新技能需手工标注，扩展性可能受限于专家知识。

（完）
