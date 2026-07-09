---
title: "Characterizing Vision-Language-Action Models across XPUs: Constraints and Acceleration for On-Robot Deployment"
title_zh: 跨XPU表征视觉-语言-动作模型：机器人部署的约束与加速
authors: "Kaijun Zhou, Qiwei Chen, Da Peng, Zhiyang Li, Xijun Li, Jinyu Gu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/5856b46ecb4060274a4380390d99a2540574862a.pdf"
tags: ["query:vlam"]
score: 7.0
evidence: 跨XPU的VLA部署系统研究
tldr: VLA模型在机器人上部署受限于实时性和成本能耗。本文提出跨XPU的模型-硬件协同表征框架，建立CET（成本、能量、时间）排行榜，发现合适规模的边缘设备在满足控制速率约束下比旗舰GPU更具成本/能效。该工作为VLA在真实机器人上的低功耗部署提供了实用指南。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA部署评估多基于桌面GPU，忽略了边缘异构加速器的权衡与机会。
method: 构建跨加速器排行榜，利用CET指标评估模型-硬件配对，并分析细粒度性能瓶颈。
result: 指出边缘设备可在满足控制率约束下实现更低成本和能耗，为VLA部署提供指导。
conclusion: 通过模型-硬件协同优化，可实现VLA在低成本边缘设备上的高效部署。
---

## Abstract
Vision-Language-Action (VLA) models are promising for generalist robot control, but on-robot deployment is bottlenecked by real-time inference under tight cost and energy budgets.
Most prior evaluations rely on desktop-grade GPUs, obscuring the trade-offs and opportunities offered by heterogeneous edge accelerators (GPUs/XPUs/NPUs).
We present a systematic framework for low-cost VLA deployment via model--hardware co-characterization.
First, we build a cross-accelerator leaderboard and evaluate model--hardware pairs under \textbf{CET} (Cost, Energy, Time), showing that ``right-sized'' edge devices can be more cost-/energy-efficient than flagship GPUs while meeting control-rate constraints.
Second, using fine-grained SM tracing and Roofline analysis, we uncover a consistent two-phase inference pattern: a compute-bound VLM backbone followed by a memory-bound Action Expert, which induces phase-dependent underutilization and hardware inefficiency.
Finally, guided by these insights, we propose \textbf{DP-Cache} and \textbf{V-AEFusion} to reduce diffusion redundancy and enable asynchronous pipeline parallelism, achieving up to \(2.9\times\) speedup on GPUs and \(6\times\) on edge NPUs with only marginal success degradation.
The example leaderboard website is: \url{https://vla-leaderboard-01.vercel.app/}.

---

## 论文详细总结（自动生成）

# 论文中文详细总结

## 1. 核心问题与整体含义
- **研究动机**：视觉-语言-动作（VLA）模型有望实现通用机器人控制，但在机器人上部署受限于实时推理，且需满足严格成本和能耗预算。
- **背景问题**：现有评估大多基于桌面级GPU，忽略了异构边缘加速器（GPU/XPU/NPU）带来的权衡与机会，缺乏跨平台系统的性能、成本和能耗对比。

## 2. 方法论：核心思想与关键技术细节
- **核心思想**：通过模型-硬件协同表征（model–hardware co-characterization）实现低成本VLA部署。
- **关键技术**：
  - 建立**跨加速器排行榜（Cross-Accelerator Leaderboard）**，使用**CET指标（Cost, Energy, Time）**评估模型-硬件配对。
  - 利用**细粒度SM追踪**和**Roofline分析**揭示两阶段推理模式：计算密集型VLM骨干 → 内存密集型Action Expert，导致阶段依赖的低利用率和硬件低效。
  - 提出两种优化方法：
    - **DP-Cache**：减少扩散步骤中的冗余。
    - **V-AEFusion**：实现异步流水线并行。
- **公式/算法流程**：以文字描述为主，无显式公式。核心流程为：先评估基准性能 → 分析瓶颈 → 提出优化策略。

## 3. 实验设计
- **数据集/场景**：论文未明确列出具体数据集名称，但提及VLA模型（如RT-2等）用于机器人控制，实际评估可能基于标准机器人任务。
- **Benchmark**：跨加速器排行榜，覆盖多种XPU（GPU、NPU等），对比旗舰GPU与边缘设备。
- **对比方法**：未列出传统方法对比，主要对比不同硬件-模型配对的CET指标，以及优化前后的速度与成功率。

## 4. 资源与算力
- **未明确说明**：文中未列出具体GPU型号、数量、训练时长或实验环境配置。仅提及在“旗舰GPU”、“边缘设备”、“边缘NPU”上进行评估，但未提供详细规格。
- **隐含信息**：使用了多种XPU（包括NPU），但缺乏算力统计，无法量化总体资源消耗。

## 5. 实验数量与充分性
- **实验数量**：主要包含三部分实验：① 跨加速器CET排行榜对比；② 细粒度SM追踪/Roofline分析；③ 优化方法（DP-Cache和V-AEFusion）的加速效果验证。
- **充分性判断**：
  - 实验覆盖多种硬件，展示了边缘设备在控制速率约束下的成本/能效优势。
  - 优化方法在GPU和NPU上分别达到2.9倍和6倍加速，且仅轻微降低成功率，结果具有说服力。
  - 但未提供多次重复实验的标准差或统计显著性检验，实验设计尚可更严谨。
- **公平性**：对比不同硬件时考虑成本和能耗，较为客观；但未说明是否在相同软件栈下比较。

## 6. 主要结论与发现
- “合适尺寸”的边缘设备可在满足控制速率约束的前提下，比旗舰GPU更具成本效益和能效。
- VLA推理存在一致的两阶段模式（计算密集型→内存密集型），导致硬件利用不足。
- 提出的DP-Cache和V-AEFusion能有效加速推理，最高达到GPU 2.9倍、边缘NPU 6倍加速，成功率损失很小。

## 7. 优点
- **系统性框架**：首次提出跨XPU的VLA部署系统，涵盖模型-硬件协同表征。
- **实际指导意义**：为低成本、低功耗机器人部署提供了实用指南和排行榜。
- **新颖优化**：针对VLA两阶段特性设计专用优化，加速效果显著。
- **公开资源**：提供示例排行榜网站，便于社区复现和扩展。

## 8. 不足与局限
- **实验覆盖**：仅评估有限硬件和模型，未涵盖所有主流VLA变体或更多边缘设备。
- **偏差风险**：排行榜可能偏向特定架构，且未考虑实际机器人部署中的通信延迟、传感器融合等细节。
- **应用限制**：优化方法（如DP-Cache）可能对特定模型架构（如扩散动作解码器）依赖性强，通用性待验证。
- **算力信息缺失**：未提供硬件型号和训练/推理能耗实测数据，影响可复现性。

（完）
