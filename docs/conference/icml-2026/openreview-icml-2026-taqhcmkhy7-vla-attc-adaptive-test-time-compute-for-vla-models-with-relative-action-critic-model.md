---
title: "VLA-ATTC: Adaptive Test-Time Compute for VLA Models with Relative Action Critic Model"
title_zh: "VLA-ATTC: 基于相对动作评论家模型的VLA自适应测试时计算"
authors: "Wenhao Li, Xiu Su, Yichao Cao, Hongyan Xu, Xiaobo Xia, Shan You, Yi Chen, Chang Xu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1cf5cb66514ba358738b16085b29802bfb108855.pdf"
tags: ["query:vlam"]
score: 7.0
evidence: 具有自适应测试时计算的VLA模型
tldr: VLA模型决策缺乏深思熟虑，在复杂场景下易产生次优或灾难性动作。VLA-ATTC引入自适应测试时计算（TTC），通过基于不确定性的认知离合器在反射执行与TTC深思阶段之间动态切换，并提出相对动作评论家（RAC）模型在TTC阶段选择最优动作。实验表明该方法在多种具身操作任务上提升了任务成功率与鲁棒性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型依赖快速本能决策，在复杂场景下容易产生次优动作。
method: 提出VLA-ATTC框架，利用不确定性认知离合器动态切换至TTC深思阶段，并由RAC模型选择最优动作。
result: 在多个机器人操作基准上，VLA-ATTC显著提升了成功率和鲁棒性。
conclusion: 自适应测试时计算能有效增强VLA模型的决策质量。
---

## Abstract
Vision-Language-Action (VLA) models have demonstrated remarkable capabilities and generalization in embodied manipulation. However, their decision-making relies on a fast, instinctive process that lacks deliberation. This strategy often leads to suboptimal or catastrophic actions when facing complex or ambiguous scenarios that require greater consideration. In this paper, we introduce \textbf{VLA-ATTC}, a framework that endows VLA models with adaptive test-time compute (TTC).  VLA-ATTC employs an uncertainty-based ``cognitive clutch'' to dynamically transition from reflexive execution to a TTC deliberation phase when necessary. During TTC phase, a novel \textbf{Relative Action Critic} (RAC) model identifies the optimal action from generated candidates via pairwise comparisons. This relative mechanism replaces unstable absolute value estimation, significantly simplifying the learning objective. Furthermore, we introduce an efficient sampling strategy to amortize computational costs and an automated data pipeline that curates preference pairs without manual annotation. On the LIBERO-LONG benchmark, VLA-ATTC reduces the failure rate of the SOTA model PI0.5 by over 50\%.

---

## 论文详细总结（自动生成）

好的，以下是基于您提供的论文元数据及摘要信息生成的中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：视觉-语言-动作（VLA）模型在具身操作任务中表现出强大的泛化能力，但其决策过程模仿了人类快速、本能的“系统1”思维，缺乏深思熟虑（deliberation）。在复杂或模糊场景下，这种快速本能决策常导致次优甚至灾难性的动作。
- **研究动机**：为VLA模型引入**自适应测试时计算（Test-Time Compute, TTC）**，使其在必要时能够切换到更具推理能力的深思模式，从而提升决策质量和鲁棒性。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：VLA-ATTC框架通过一个**不确定性认知离合器**动态判断当前决策是否需要深思，若需要则进入TTC阶段；在TTC阶段使用**相对动作评论家（Relative Action Critic, RAC）**模型通过成对比较从候选动作中选出最优动作。
- **关键技术细节**：
  - **不确定性认知离合器**：基于模型对当前输入的不确定性估计，动态决定是继续使用默认的快速执行（reflexive execution）还是切换到TTC深思阶段。
  - **相对动作评论家（RAC）模型**：在TTC阶段，生成多个候选动作，RAC模型通过两两比较（pairwise comparisons）选出最优动作，避免传统方法中不稳定、难学习的绝对价值估计。通过相对比较简化了学习目标。
  - **高效采样策略**：用于摊销计算成本，使TTC阶段不会带来过大的额外计算开销。
  - **自动化数据管道**：无需人工标注，自动生成用于训练的偏好对数据，降低了数据构建成本。

### 3. 实验设计
- **基准与数据集**：论文在 **LIBERO-LONG** benchmark 上进行了主要评估。这是一个用于长期任务规划的机器人操作基准。
- **对比方法**：与当前SOTA模型 **PI0.5** 进行了直接对比。
- **主要指标**：任务成功率（或失败率降低）。

### 4. 资源与算力
- 论文元数据及摘要中**未明确说明**具体的GPU型号、数量及训练时长等算力细节。因此无法总结资源消耗情况。

### 5. 实验数量与充分性
- 从已有信息看，论文在LIBERO-LONG一个基准上进行了验证，并报告了与一个SOTA模型的比较结果。实验数量相对有限，但结果显著（失败率降低>50%）。
- 元数据未提及消融实验或其他数据集（如CALVIN、RLBench等）的验证，因此**实验覆盖的充分性有待进一步了解完整论文内容**。仅从摘要推测，实验设计可能较为聚焦，但未必覆盖所有常见具身环境，其客观性和公平性尚需查看完整论文中的超参数、随机种子等细节。

### 6. 论文的主要结论与发现
- **主要结论**：VLA-ATTC通过引入自适应测试时计算，显著提升了VLA模型在复杂具身操作任务中的决策质量和鲁棒性。在LIBERO-LONG上，将SOTA模型PI0.5的失败率降低超过50%。
- **发现**：相对动作评论家模型（RAC）通过成对比较替代绝对价值估计，能有效简化学习目标，并选出更优的动作。不确定性驱动的认知离合器能实现动态、高效的TTC切换。

### 7. 优点
- **方法创新性**：将“自适应测试时计算”引入VLA模型，结合不确定性认知离合器，实现了从本能模式到深思模式的智能切换，思路新颖。
- **技术亮点**：相对动作评论家（RAC）避免了绝对价值回归的不稳定性，简化了训练；自动数据管道去除了人工标注依赖，提升了实用性。
- **结果突出**：在具有挑战性的长程任务基准上取得了超过50%的失败率降低，性能提升显著。

### 8. 不足与局限
- **实验覆盖有限**：仅报告了在LIBERO-LONG上的结果，缺少其他常见机器人操作基准（如CALVIN、MetaWorld等）的对比，泛化性需进一步验证。
- **资源细节缺失**：未提供训练和推理所需的计算资源与时间，不利于复现和成本评估。
- **消融与分析不足**：摘要中未提及对离合器、RAC模型、采样策略等各组件有效性的消融实验，无法全面评估各模块的贡献。
- **应用限制**：目前仅在仿真环境中验证，真实机器人环境中的效果、延迟和稳定性尚未讨论。
- **公平性风险**：仅与PI0.5一个模型对比，缺少与更多基线模型（如RT-2、Octo等）的比较，可能夸大相对优势。

（完）
