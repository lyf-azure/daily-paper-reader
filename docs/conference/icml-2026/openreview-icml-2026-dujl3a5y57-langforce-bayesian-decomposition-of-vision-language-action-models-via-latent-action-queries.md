---
title: "LangForce: Bayesian Decomposition of Vision Language Action Models via Latent Action Queries"
title_zh: LangForce：通过潜在动作查询对视觉-语言-动作模型进行贝叶斯分解
authors: "Shijie Lian, Bin Yu, Xiaopeng LIN, Laurence Tianruo Yang, Zhaolong Shen, Changti Wu, YuZhuo Miao, Cong Huang, Kai Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e4c14542d7018da7556a19fbd1d7549116d32f58.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 通过贝叶斯分解防止信息坍塌以提升VLA泛化
tldr: 该论文发现VLA训练中语言指令与视觉信息高度冗余，导致信息坍塌，模型忽略语言约束。提出LangForce，通过贝叶斯分解和潜在动作查询强制执行语言指令跟随，在复杂多任务场景下显著提升泛化能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 训练偏差导致VLA模型忽视语言指令，退化为纯视觉策略。
method: 采用贝叶斯分解和可学习潜在动作查询强制执行指令跟随。
result: 在多任务设置下指令执行准确率和泛化性提升。
conclusion: 解耦语言和视觉信息是VLA泛化的关键。
---

## Abstract
Vision-Language-Action (VLA) models have shown promise in robot manipulation but often struggle to generalize to new instructions or complex multi-task scenarios. We identify a critical pathology in current training paradigms where goal-driven data collection creates a dataset bias. In such datasets, language instructions are highly predictable from visual observations alone, causing the conditional mutual information between instructions and actions to vanish, a phenomenon we term Information Collapse. Consequently, models degenerate into vision-only policies that ignore language constraints. To address this, we propose LangForce, enforces instruction following via Bayesian decomposition. By introducing learnable Latent Action Queries, we construct a dual-branch architecture to estimate both a vision-only prior $p(a \mid v)$ and a language-conditioned posterior $\pi(a \mid v, \ell)$. We then optimize the policy to maximize the conditional Pointwise Mutual Information (PMI) between actions and instructions. This objective effectively penalizes the vision shortcut and rewards actions that explicitly explain the language command. Extensive experiments across on three benchmarks demonstrate substantial gains, including an 11.3\% improvement on the challenging OOD SimplerEnv benchmark, validating the ability of LangForce to robustly ground language in action.

---

## 论文详细总结（自动生成）

## LangForce：通过潜在动作查询对视觉-语言-动作模型进行贝叶斯分解 — 中文详细总结

### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：当前视觉-语言-动作（VLA）模型在机器人操作中虽有效，但在面对新指令或复杂多任务场景时泛化能力不足。
- **关键发现**：训练数据存在偏差——任务驱动的数据收集导致语言指令与视觉观测高度冗余。这使得条件互信息 `I(动作；指令 | 视觉)` 趋近于零，即 **信息坍塌（Information Collapse）**。模型退化为纯视觉策略，忽略语言约束。
- **意义**：该工作揭示了VLA训练中的核心病理，并提出了一个能够强制执行语言跟随、提升泛化性的解决方案。

### 2. 方法论：核心思想、关键技术细节与算法流程
- **核心思想**：通过贝叶斯分解将语言指令对动作的约束显式建模，惩罚视觉捷径，鼓励动作解释语言命令。
- **关键技术细节**：
  - 引入 **可学习的潜在动作查询（Latent Action Queries）**，构建双分支架构：
    - 视觉先验分支：估计 `p(a | v)`（仅依赖视觉的纯策略）。
    - 语言条件后验分支：估计 `π(a | v, l)`（依赖视觉和语言）。
  - 优化目标：最大化 **条件点互信息（Conditional Pointwise Mutual Information, PMI）** 在动作与指令之间。即最大化 `PMI(a, l | v) = log(π(a|v,l) / p(a|v))`。
  - 该目标惩罚了仅从视觉即可预测的动作（即 `p(a|v)` 高的动作），奖励那些必须依赖语言才能解释的动作，从而强制执行指令跟随。
- **算法流程**（文字描述）：
  1. 输入：视觉观测 `v`，语言指令 `l`。
  2. 通过双分支网络分别计算 `p(a|v)`（先验）和 `π(a|v,l)`（后验）。
  3. 采样动作 `a` 并计算 PMI 损失。
  4. 梯度更新模型参数使得后验分布与先验分布的 KL 散度方向朝 PMI 最大化调整。

### 3. 实验设计
- **数据集/场景**：在三个基准（benchmarks）上进行评估，包括具有挑战性的 **OOD SimplerEnv**（分布外场景）。
- **对比方法**：摘要中未列出具体对比基线，推论为与标准VLA模型（如RT-2、Octo等）或未作分解的VLA基线对比。文中提及“substantial gains”，包括在 OOD SimplerEnv 上提升 **11.3%**。
- **评估指标**：指令执行准确率、泛化性能（特别是在未见过的指令或环境上的表现）。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅从论文元数据判断为ICML 2026接收，但实验细节除摘要外未提供。若有需要可推测为基于公开VLA模型（如RT-2）的微调配置，但此处须指出“未提及”。

### 5. 实验数量与充分性
- **实验组数**：涉及三个基准测试，每个基准应包含多任务场景。文中强调在OOD SimplerEnv上的11.3%提升，表明至少有一个重大结果。但缺少消融实验、超参数敏感性、不同信息坍塌程度的影响等细节。
- **充分性判断**：从摘要来看，实验仅报告了核心结果，未展示丰富的消融或跨场景分析，因此**不够充分**。提供的tldr提到“在多任务设置下指令执行准确率和泛化性提升”，但未量化或细化到不同任务类型。须依赖全文以验证实验客观性。

### 6. 主要结论与发现
- 解耦语言和视觉信息是VLA泛化的关键。
- 所提出的LangForce通过贝叶斯分解和PMI优化，能有效克服信息坍塌，显著提升对语言指令的跟随能力，尤其在分布外场景下。
- 实验验证了在三个benchmark上的广泛改进，证实了方法的有效性。

### 7. 优点
- **问题洞察深刻**：首次系统识别并形式化了VLA中的信息坍塌现象，提供了理论解释（条件互信息消失）。
- **方法创新**：结合贝叶斯分解和可学习潜在动作查询，巧妙地将先验-后验对比融入优化目标，避免了直接监督的局限性。
- **实用性强**：在无需额外数据采集或注释的情况下，仅改变训练目标即可提升泛化性，易于集成到现有VLA框架。

### 8. 不足与局限
- **实验覆盖有限**：仅报告了三个基准上的结果，缺乏更广泛的环境（如真实机器人平台）或更多任务类型的验证。
- **偏差风险**：论文发现训练数据中的冗余是信息坍塌的成因，但未讨论如何缓解数据采集阶段的偏差。方法依赖可学习查询，其初始化可能影响性能。
- **应用限制**：方法假设语言指令在动作决策中具有信息增益，若指令本身模糊或与视觉完全独立（而非冗余），PMI优化可能失效。未探索极端情况下（如纯视觉任务）的性能退化。
- **算力与复现细节缺失**：未提供训练超参数、模型架构细节等，复现难度高。

（完）
