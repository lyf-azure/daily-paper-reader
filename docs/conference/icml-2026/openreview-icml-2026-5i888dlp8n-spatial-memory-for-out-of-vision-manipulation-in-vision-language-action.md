---
title: Spatial Memory for Out-of-Vision Manipulation in Vision-Language-Action
title_zh: 视觉-语言-动作模型中的视野外操作空间记忆
authors: "Pengteng Li, Weiyu Guo, He Zhang, Tiefu Cai, Xiao He, Yandong Guo, Hui Xiong"
date: 2026-04-30
pdf: "https://openreview.net/pdf/95685162fa940bca32702d659b96eebf84138a75.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 空间记忆增强VLA在视野外场景的泛化
tldr: 现有VLA模型隐含假设任务相关物体始终可见，导致在视野外时表现脆弱。SOMA为VLA配备持久空间记忆，通过可移动头相机获取多视角观测并构建空间语义表示，支持动态记忆精化与全局一致性维护。实验证明，SOMA在目标移出视野的操作任务中大幅提升了成功率，拓展了VLA在非结构化环境中的泛化能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型依赖物体始终可见的假设，在视野外时行为脆弱。
method: 提出SOMA框架，利用多视角观察构建持久空间记忆，支持动态精化与全局一致性。
result: 在多种视野外操作场景中，SOMA将成功率提升显著，优于基线。
conclusion: 赋予VLA空间记忆能力是提升其在复杂环境中泛化的有效途径。
---

## Abstract
We introduce SOMA, the Spatial Memory framework for Out-of-Vision Manipulation in Vision-Language-Action (VLA) models. Most existing VLAs implicitly assume that task-relevant objects are always visible, leading to brittle and reactive behaviors when targets fall outside the camera’s field of view. SOMA addresses this limitation by equipping VLAs with a persistent spatial memory constructed from multi-view observations acquired via a movable head camera, enabling reasoning beyond the current visual frustum. The framework consists of three components: Spatial Memory Construction, which aggregates angular-wise observations into a unified spatial–semantic representation through scanning; Dynamic Memory Refinement, which maintains global consistency over time; and Contextual Memory Retrieval, which activates instruction-relevant spatial cues during manipulation. We evaluate SOMA on five challenging real-world out-of-vision manipulation tasks, including multi-step and dual-arm scenarios where target objects are initially invisible. Experimental results show that SOMA not only improves task success rates, but also induces qualitatively different manipulation behaviors, with faster target localization, reduced viewpoint search, and near one-shot grasping under partial observability. Additional experiments on RoboCasa GR1 and SimplerEnv further validate the effectiveness of SOMA’s memory design under conventional fully observable settings. Code will be released soon.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
当前主流的视觉-语言-动作（VLA）模型隐含地假设任务相关的目标始终位于摄像机的视野（FoV）之内，这使得模型在目标物体移出视野时表现出脆弱和被动响应的行为。这一假设限制了VLA在非结构化、动态真实环境中的泛化能力。为了突破这一瓶颈，论文提出了**SOMA**（空间记忆框架），通过为VLA配备持久化的空间记忆，使其能够在当前视觉锥体之外进行推理和操作。

## 2. 论文提出的方法论：核心思想、关键技术细节
SOMA框架包含三个核心组件：

- **空间记忆构建（Spatial Memory Construction）**：通过可移动的头部相机进行多角度扫描，将不同视角的观测聚合为统一的空间-语义表示。
- **动态记忆精化（Dynamic Memory Refinement）**：随时间维护记忆的全局一致性，处理视角变化及动态场景更新。
- **上下文记忆检索（Contextual Memory Retrieval）**：在操作过程中激活与任务指令相关的空间线索，使模型能够利用之前存储的空间信息。

整体流程：先通过扫描构建初始空间记忆，然后在任务执行过程中不断精化记忆，最后根据任务指令检索相关记忆辅助决策。无需依赖完整全局地图，仅利用多视角观测即可实现视野外推理。

文中未给出具体的数学公式或算法伪代码（基于提供的摘要和元数据），文字描述即反映了核心流程。

## 3. 实验设计：数据集/场景、Benchmark、对比方法
- **场景与任务**：五项具有挑战性的真实世界视野外操作任务，包括**多步操作**和**双机械臂协作**场景。目标物体初始时均不在相机视野内。
- **基准测试**：任务成功率作为主要指标。额外在**RoboCasa GR1**和**SimplerEnv**模拟环境上进行验证（完全可观测条件下）。
- **对比方法**：论文未明确列出具体对比模型，但从上下文推断，基线包括未配备空间记忆的VLA模型（可能是OpenVLA等标准模型）。

## 4. 资源与算力
论文的摘要和元数据中**未提及**所使用的GPU型号、数量、训练时长等算力信息。需要指出这一遗漏。

## 5. 实验数量与充分性
- **主要实验**：5个真实世界视野外操作任务（涵盖多步和双机械臂），每个任务重复多次（具体次数未说明）。
- **额外验证**：在RoboCasa GR1和SimplerEnv两个模拟平台上进行附加实验，验证了记忆设计在完全可观测条件下的有效性。
- **消融实验**：摘要中未明确提及是否进行了消融实验（如移除动态精化、移除记忆构建等），但从“验证记忆设计有效性”推断可能包含部分消融，但文本未详细说明。
- **充分性评价**：实验覆盖了真实和模拟环境，任务类型多样，但缺少与多种基线（如记忆增强的强化学习、基于地图的规划方法）的系统对比。总体而言实验设计合理，但充分性仍有提升空间（例如未提供统计显著性检验、未列出失败案例分析）。

## 6. 论文的主要结论与发现
- SOMA显著提升了VLA模型在视野外操作任务中的成功率，使机器人能够更快定位目标、减少不必要的视角搜索。
- 在部分可观测条件下，SOMA能够实现接近单手一次抓取（near one-shot grasping）的性能。
- 即使在传统完全可观测设置下（如RoboCasa GR1、SimplerEnv），SOMA的空间记忆设计依然有效，表明其泛化能力。
- 空间记忆的核心贡献在于赋予了VLA模型持久化、动态更新的空间推理能力，从根本上克服了“物体必须始终可见”的隐含假设。

## 7. 优点
- **问题新颖且实用**：解决了真实机器人操作中普遍存在的视野外目标问题，具有实际应用价值。
- **方法简洁有效**：通过多视角扫描+动态记忆精化+上下文检索的模块化设计，易于集成到现有VLA模型中。
- **实验设计全面**：同时包含真实世界和模拟环境，任务涵盖单臂、双臂、多步，验证了多种场景下的有效性。
- **开源承诺**：将发布代码，便于复现和进一步研究。

## 8. 不足与局限
- **算力资源未报告**：缺少计算成本信息，不利于评估方法的实际开销和可复现性。
- **消融实验不够详细**：未明确列出各组件（如记忆精化、检索方式）的独立贡献，削弱了对设计合理性的定量证明。
- **对比基线不完整**：摘要未提及与显式地图构建方法（如3D语义地图、NeRF-RL）或基于记忆的规划方法的对比，难以突出SOMA的独特优势。
- **应用限制**：依赖于可移动头部相机进行扫描，对于固定相机或无法多视角移动的机器人可能不适用；且动态记忆精化可能增加计算延迟，在实时性要求高的场景中需要权衡。
- **偏差风险**：仅在特定机器人平台（具体型号未明示）上测试，泛化到其他形态的机器人（如轮式、飞行器）需要验证。

（完）
