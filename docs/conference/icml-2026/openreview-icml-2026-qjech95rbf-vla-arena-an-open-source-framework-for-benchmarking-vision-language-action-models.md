---
title: "VLA-Arena: An Open-Source Framework for Benchmarking Vision-Language-Action Models"
title_zh: VLA-Arena：面向视觉-语言-动作模型的开源基准框架
authors: "Borong Zhang, Jiahao Li, Jiachen Shen, Yuhao Zhang, Yishuai Cai, Yuanpei Chen, Juntao Dai, Jiaming Ji, Yaodong Yang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/ca813ba39da16d6e225b77217898ae47e975cb68.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 评估VLA模型泛化能力的结构化基准框架
tldr: VLA模型发展迅速但缺乏系统性的能力边界评估。本文提出VLA-Arena基准，通过任务结构、语言指令和视觉观测三个正交轴设计难度等级，包含11个任务套件覆盖安全、干扰、外推和长时域等维度。实验揭示现有VLA模型在不同难度下的失败模式，为后续模型改进提供了标准化评估平台和关键洞察。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 缺乏对VLA模型能力边界和失败模式的系统量化评估。
method: 提出VLA-Arena，基于三个正交轴设计结构化任务难度等级，包含11个任务套件。
result: 通过评估多个VLA模型，揭示了其在不同难度和场景下的性能边界和失败模式。
conclusion: VLA-Arena为VLA研究提供了标准化的比较平台和性能诊断工具。
---

## Abstract
While Vision-Language-Action models (VLAs) are rapidly advancing toward generalist robot policies, quantitatively characterizing their capability boundaries and failure modes remains challenging. To address this, we introduce **VLA-Arena**, a comprehensive benchmark. It features a novel structured task design framework to quantify difficulty across three orthogonal axes: **(1) Task Structure**, **(2) Language Command**, and **(3) Visual Observation**. This allows us to systematically design tasks with fine-grained difficulty levels, enabling a precise measurement of model capability frontiers. For task structure, VLA-Arena comprises 11 task suites organized into four dimensions: **Safety**, **Distractor**, **Extrapolation**, and **Long Horizon**, totaling 170 tasks. Each suite spans three difficulty levels (L0–L2), with fine-tuning restricted to L0 to rigorously assess generalization. Orthogonal to this, language (W0-W4) and visual (V0-V4) perturbations can be applied to any task as diagnostic probes to distinguish robust grounding from superficial pattern matching. Our extensive evaluation of state-of-the-art VLAs reveals critical limitations: memorization over generalization, superficial visual perception, and a neglect of safety constraints. Additionally, model rank reversals across L0–L2 validate that each level provides non-redundant insights. To foster research addressing these model limitations and ensure reproducibility, we provide the complete VLA-Arena framework, including an end-to-end toolchain from task definition to automated evaluation and the VLA-Arena-S/M/L datasets for fine-tuning. Our benchmark, datasets, models, and leaderboard are publicly available at https://vla-arena.github.io.

---

## 论文详细总结（自动生成）

# VLA-Arena 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：视觉-语言-动作模型（VLA）正快速向通用机器人策略发展，但缺乏对其能力边界和失败模式的系统性、量化评估手段。现有基准难以细粒度地刻画模型在不同难度和干扰下的表现。
- **核心问题**：如何设计一个结构化、可扩展的基准框架，精准测量VLA模型的泛化能力，并揭示其根本性局限（如记忆而非泛化、表面视觉感知、忽视安全约束等）。

## 2. 方法论

- **核心思想**：提出一种三维正交的任务难度设计框架，从 **任务结构（Task Structure）**、**语言指令（Language Command）**、**视觉观测（Visual Observation）** 三个轴独立定义难度等级，从而实现细粒度的能力边界测量。
- **关键技术细节**：
    - **任务结构轴**：包含11个任务套件，分为四个维度：**安全（Safety）**、**干扰（Distractor）**、**外推（Extrapolation）**、**长时域（Long Horizon）**，共170个任务。每个套件设置三个难度等级（L0–L2），其中仅L0允许微调，L1和L2用于严格评估泛化能力。
    - **语言指令轴**：对任何任务可施加W0–W4共5级语言扰动（如指令变体、歧义表达等），作为诊断探针，检验模型是否真正理解指令而非模式匹配。
    - **视觉观测轴**：对任何任务可施加V0–V4共5级视觉扰动（如遮挡、光照变化、视角改变等），测试模型视觉鲁棒性。
    - **评估流程**：任务定义 → 数据采集 → 微调（仅L0）→ 全难度测试 → 自动评分（成功率、安全违规率、因果归因指标等）。
- **算法/流程说明**（无公式）：
    - 对每个任务，先定义基本版本（L0、W0、V0），然后正交扩展难度。
    - 模型在L0数据上微调后，在L1、L2及不同语言/视觉扰动下测试。
    - 通过成功率变化、安全违规率等指标，量化模型对每个轴的敏感性。

## 3. 实验设计

- **数据集/场景**：使用VLA-Arena本身包含的170个任务，覆盖模拟机器人操作环境（具体环境未在摘要中说明，但框架是开源的）。同时提供VLA-Arena-S/M/L三个规模的微调数据集。
- **Benchmark**：VLA-Arena作为一个完整的基准测试框架，自身即 benchmark。
- **对比方法**：评估了多个当前最先进的VLA模型（具体模型名未在此摘要中列出，但通常包括RT-2、Octo、π0等系列），比较了它们在L0/L1/L2以及语言/视觉扰动下的表现，并分析了模型排名反转现象。

## 4. 资源与算力

- **文中未明确说明**所使用的GPU型号、数量、训练时长等具体算力信息。仅提到提供了完整的端到端工具链（从任务定义到自动评估）以及微调数据集，但未披露训练VLA模型或运行基准测试所消耗的硬件资源。

## 5. 实验数量与充分性

- **实验数量**：评估覆盖了11个任务套件×3个难度等级×多个模型，以及可选的语言/视觉扰动组合（W0-W4、V0-V4）。虽未给出确切实验次数，但170个任务、多扰动设置意味着数百至数千次测试。
- **充分性与公平性**：
    - **充分性**：通过正交轴设计，系统地探索了多个泛化维度（安全、干扰、外推、长时域），并引入跨难度等级（L0→L2）的评估，能够揭示模型在不同复杂度下的性能变化。语言和视觉扰动作为探针，进一步检验了鲁棒性。
    - **客观公平**：统一使用L0数据微调，保证不同模型在相同训练条件下比较；任务难度分级独立于模型能力，避免设计偏差；自动评估减少主观因素。但未提及数据泄露或任务相关性控制，可能仍需进一步验证。

## 6. 主要结论与发现

- 现有SOTA VLA模型存在关键局限：
    - **记忆而非泛化**：模型在L0任务上表现良好，但难度提升（L1/L2）后成功率大幅下降，表明模型依赖训练数据的具体模式而非真正理解任务本质。
    - **浅层视觉感知**：视觉扰动（如遮挡、光照变化）导致性能显著下降，说明模型未学习到鲁棒的视觉特征。
    - **忽视安全约束**：在安全相关任务中，模型常违反安全指令（如碰撞障碍物），暴露了对安全优先级的缺失。
- **模型排名反转**：不同模型在L0、L1、L2上的相对表现发生变化，验证了每个难度等级提供非冗余的洞察，简单基准不足以评估真实能力。

## 7. 优点

- **结构化难度设计**：三维正交轴（任务结构、语言、视觉）允许独立剖析模型在不同维度的失败模式，解释性强。
- **多难度层级**：通过L0-L2区分训练与泛化，避免测试集污染，能精确测量泛化边界。
- **任务多样性**：11个套件覆盖安全、干扰、外推、长时域四个关键维度，全面性高。
- **诊断工具链**：支持语言和视觉扰动作为探针，可深入分析模型是真正理解还是表面匹配。
- **开源可复现**：提供完整框架、数据集、模型和排行榜，便于社区使用和扩展。

## 8. 不足与局限

- **实验覆盖**：
    - 未提及是否包含真实机器人环境，可能仅在模拟器中验证，缺乏真实世界噪声和硬件约束的考验。
    - 语言和视觉扰动虽分级，但可能未充分覆盖自然语言歧义或真实视觉退化（如运动模糊、传感器噪声）。
- **偏差风险**：
    - 仅评估了部分SOTA模型，未涵盖所有最新VLA方法（尤其2026年后新模型），结论是否具有普适性待验证。
    - 微调仅限L0，但L0任务的设计可能隐含了某种视角或动作空间偏好，影响公平性。
- **应用限制**：
    - 任务套件可能偏向特定类型操作（如桌面抓取），对于复杂灵巧操作或移动操作未充分覆盖。
    - 安全维度定义可能过于简化，实际安全风险需要物理约束和伦理考量。
    - 缺乏对模型规模与性能关系的系统分析（未提及不同参数量模型对比）。

（完）
