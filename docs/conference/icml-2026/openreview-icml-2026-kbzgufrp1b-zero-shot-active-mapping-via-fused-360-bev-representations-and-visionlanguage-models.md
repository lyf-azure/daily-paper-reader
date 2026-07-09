---
title: Zero-shot Active Mapping via Fused 360-BEV Representations and Vision–Language Models
title_zh: 基于融合360-BEV表示和视觉语言模型的零样本主动建图
authors: "Yuanze Wang, Dianxi Shi, Yuetian Wang, Shiming Song, Haikuo Peng, Chunping Qiu, Mengzhu Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/660b962b7ae442fb719f019905c65f1e4bbc6a74.pdf"
tags: ["query:vlam"]
score: 4.0
evidence: 使用VLM和BEV在未知环境中进行零样本主动建图和语言驱动导航
tldr: 主动建图在未知环境中的零样本泛化能力有限，且缺乏语言支持。本文提出一种基于VLM的零样本主动建图方法，通过360-BEV表示融合全景语义与几何结构，并利用VLM生成候选路径点并反投影为可执行动作。在仿真环境中，该方法在零样本条件下实现了高效建图和语言交互，为VLM驱动的开放世界导航提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有主动建图方法难以零样本泛化到大规模场景，且不支持语言指令。
method: 提出VLM驱动的零样本建图框架，使用360-BEV表示融合语义与几何，VLM选择路径点并映射为3D动作。
result: 在仿真环境中实现了零样本高效的建图和语言驱动探索。
conclusion: 该工作展示了VLM在自主建图任务中的零样本泛化潜力，可扩展至机器人导航。
---

## Abstract
Active mapping enables embodied agents to understand and interact in previously unseen environments. However, most methods struggle to achieve zero-shot generalization to large-scale scenes and lack support for language instructions. We propose a VLM-based active mapping method that achieves zero-shot mapping while facilitating language-driven human–agent interaction. First, we introduce a 360-BEV representation that integrates omnidirectional semantics with BEV-aligned geometric structure to enhance scene understanding. Second, we develop a candidate waypoint generation strategy that allows the VLM-driven agent to select informative 2D waypoints in image space and back-project them into executable metric actions in 3D space, enabling the VLM to plan in its strongest modality. Third, we design a VLM-based depth-first exploration agent that decomposes the scenes into explorable regions, selects informative waypoints within each region, and organizes them into a topological tree. The agent follows the depth-first exploration policy to achieve thorough coverage of large-scale scenes. Without task-specific training, our method outperforms the strongest baseline, improving coverage and AUC by approximately 13.25\% and 14.00\%, respectively, while enabling language-conditioned interaction.

---

## 论文详细总结（自动生成）

# 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：主动建图（Active Mapping）旨在让具身智能体在未知环境中自主探索并构建地图，从而实现环境理解和交互。然而，现有方法大多难以零样本（zero-shot）泛化至大规模场景，且缺乏对自然语言指令的支持，限制了人机交互的灵活性。
- **整体含义**：本文提出一种基于视觉-语言模型（VLM）的零样本主动建图方法，通过融合360°鸟瞰图（BEV）表示和VLM的推理能力，使智能体能够在无任务特定训练的情况下高效建图，并能接受语言指令进行探索，为开放世界中的语言驱动导航提供了新思路。

# 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

## 核心思想
- 利用VLM的零样本推理能力，将全景语义信息与BEV对齐的几何结构融合，并通过图像空间中的路径点规划与3D反投影，实现无需训练的语言驱动主动建图。

## 关键技术细节
- **360-BEV表示**：融合全方位（360°）语义信息与BEV对齐的几何结构，以增强对周围环境的场景理解。
- **候选路径点生成策略**：VLM驱动的智能体先在图像空间中选取信息量最大的2D路径点（waypoints），然后通过反投影映射为3D空间中可执行的动作度量，使VLM能在其最擅长的模态（图像）中进行规划。
- **基于VLM的深度优先探索代理**：
  - 将场景分解为若干可探索区域；
  - 在每个区域内选择信息量最大的路径点；
  - 将这些路径点组织成拓扑树结构；
  - 采用深度优先探索策略，指导智能体依次遍历各区域，实现对大规模场景的彻底覆盖。

## 算法流程（文字说明）
1. 智能体采集360°图像，生成360-BEV表示。
2. VLM基于该表示预测当前候选路径点（图像空间中的2D坐标）。
3. 将2D路径点反投影为3D空间中的目标位置及导航动作。
4. 智能体执行动作后更新拓扑树与区域划分。
5. 根据深度优先策略选择下一个待探索区域，重复步骤1-4直到覆盖所有区域。

# 3. 实验设计：数据集、基准、对比方法

- **数据集/场景**：仿真环境（未明确说明具体名称，可能是Habitat、Gibson等常见室内仿真平台，或自行构建的场景）。
- **基准（Benchmark）**：主动建图任务，以地图覆盖率（Coverage）和AUC（Area Under Curve，可能指标为覆盖率-时间曲线下面积）为主要评价指标。
- **对比方法**：最强基线（the strongest baseline，具体方法未在摘要中列出，推测为现有零样本或主动建图方法，如基于强化学习或启发式探索的方法）。本文方法在Coverage上提升约13.25%，在AUC上提升约14.00%。

# 4. 资源与算力

- **未明确说明**：摘要和元数据中未提及使用的GPU型号、数量、训练时长或推理资源。因此无法总结具体算力开销。仅可指出论文未提供此信息。

# 5. 实验数量与充分性

- **实验数量**：摘要仅报告了在一个仿真环境下的主要结果（Coverage和AUC的相对提升），未提及消融实验、不同场景下的泛化测试、或不同语言指令的变体实验。
- **充分性评估**：
  - **积极方面**：对比了最强基线，结果明确。
  - **局限性**：缺乏多场景、多基线的系统比较；缺少对各个模块（如360-BEV、深度优先策略、VLM选点等）的消融实验；未在真实机器人上验证；未展示语言指令交互的具体案例或成功/失败分析。整体实验覆盖不够全面，客观性和公平性有待更详细的描述支持。

# 6. 论文的主要结论与发现

- 提出的VLM驱动零样本主动建图方法在仿真环境中显著优于最强基线，实现了高效的场景覆盖和语言驱动探索。
- 该方法无需任务特定训练，展现了VLM在自主建图任务中的零样本泛化潜力。
- 360-BEV表示与VLM相结合，能够有效融合语义与几何信息，支持智能体在图像空间中规划并映射为可执行动作。

# 7. 优点：方法或实验设计上的亮点

- **零样本能力**：无需针对特定场景或任务进行训练，可直接部署，泛化性强。
- **语言交互**：支持自然语言指令驱动探索，提升了人机交互的自然性。
- **360-BEV表示**：融合全方位语义与BEV几何结构，提供更完整的场景理解，弥补了传统窄视场BEV的不足。
- **VLM优势发挥**：通过图像空间中的路径点选择与反投影，让VLM在其最擅长的模态（视觉+文本）中规划，避免了直接回归3D动作导致的域偏移。
- **深度优先拓扑树**：结构化的区域分解和策略，确保大规模场景的彻底覆盖。

# 8. 不足与局限

- **实验局限**：
  - 仅在仿真环境中验证，未在真实机器人或更复杂动态环境下测试，结果的可迁移性存疑。
  - 缺少消融实验，无法确认各模块的单独贡献。
  - 仅与一个“最强基线”对比，缺乏与多种基线（如经典探索方法、其他VLM方案）的全面比较。
- **方法局限**：
  - 依赖VLM的推理质量和速度，可能受限于模型响应延迟或幻觉。
  - 360-BEV的构建需要全景传感器或拼接处理，硬件要求较高。
  - 深度优先策略可能不适用于需要实时响应的动态场景。
- **偏差风险**：未报告失败案例或鲁棒性分析，可能对场景中罕见布局或遮挡情况敏感。
- **算力与效率**：未讨论计算开销，实际部署时VLM推理可能带来较大计算负担。

（完）
