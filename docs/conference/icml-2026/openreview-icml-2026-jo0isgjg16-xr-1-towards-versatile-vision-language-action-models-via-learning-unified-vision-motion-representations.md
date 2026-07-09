---
title: "XR-1: Towards Versatile Vision-Language-Action Models via Learning Unified Vision-Motion Representations"
title_zh: XR-1：通过统一视觉-运动表示实现通用视觉-语言-动作模型
authors: "Shichao Fan, Kun Wu, Zhengping Che, Xinhua Wang, Di Wu, Fei Liao, Ning Liu, Yixue Zhang, Zhen Zhao, Zhiyuan Xu, Meng Li, Qingjie Liu, Shanghang Zhang, Min Wan, Jian Tang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/181715f87df4dd5677ebf2619dcb456e071c95dd.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 新颖VLA框架与统一视觉-运动表示
tldr: XR-1提出了一种新颖的VLA框架，通过学习统一的视觉-运动表示来解决异构机器人数据中的精确动作生成和跨域泛化问题。该框架有效融合了来自不同机器人本体和人类演示的多模态知识，在多个仿真和真实机器人任务上取得了领先性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型难以从高维观测生成精确动作，且跨异构数据源存在域差距。
method: 提出统一视觉-运动表示学习框架，充分挖掘大规模异构数据集中的互补多模态知识。
result: 在多个机器人任务上，XR-1实现了精确动作生成和跨域泛化。
conclusion: 统一视觉-运动表示是构建通用VLA模型的有效途径。
---

## Abstract
Recent progress in large-scale robotic datasets and vision-language models (VLMs) has advanced research on vision-language-action (VLA) models.  However, existing VLA models still face two fundamental challenges: (\textit{i}) producing precise low-level actions from high-dimensional observations, (\textit{ii}) bridging domain gaps across heterogeneous data sources, including diverse robot embodiments and human demonstrations. Existing methods often encode latent variables from either visual dynamics or robotic actions to guide policy learning, but they fail to fully exploit the complementary multi-modal knowledge present in large-scale, heterogeneous datasets. In this work, we present \textbf{XR-1}, a novel framework for versatile and scalable VLA learning across diverse robots, tasks, and environments.
At its core, XR-1 introduces the \emph{Unified Vision-Motion Codes (UVMC)}, a discrete latent representation learned via a dual-branch VQ-VAE that jointly encodes visual dynamics and robotic motion.  UVMC addresses these challenges by (\textit{i}) serving as an intermediate representation between the observations and actions, and  (\textit{ii}) aligning multimodal dynamic information from heterogeneous data sources to capture complementary knowledge. To effectively exploit UVMC, we propose a \emph{three-stage training paradigm}: (\textit{i}) self-supervised UVMC learning, (\textit{ii}) UVMC-guided pretraining on large-scale cross-embodiment robotic datasets, and (\textit{iii}) task-specific post-training.  We validate XR-1 through extensive real-world experiments with more than 12,000 rollouts on six different robot embodiments, spanning over 120 diverse manipulation tasks. XR-1 consistently outperforms state-of-the-art baselines such as $\pi_0$ and GR00T-N1.5 while demonstrating strong generalization to novel objects, background variations, distractors, and illumination changes. Our project is at \href{https://xr-1-vla.github.io/}{https://xr-1-vla.github.io/}.

---

## 论文详细总结（自动生成）

# 论文总结：XR-1：通过统一视觉-运动表示实现通用视觉-语言-动作模型

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有视觉-语言-动作（VLA）模型在处理大规模异构机器人数据时面临两个根本性挑战：
  1. **精确的低层动作生成困难**：从高维观测（如摄像头图像）直接映射到精细的机器人动作（如关节角度）精度不足。
  2. **跨异构数据源的域差距**：不同机器人本体、人类演示数据之间存在形态、执行方式和环境差异，导致模型难以泛化。
- **核心问题**：如何充分挖掘大规模异构数据集中的互补多模态知识（视觉动态与机器人运动），构建一个通用、可扩展的VLA框架，实现对多种机器人、任务和环境的统一建模。
- **整体含义**：本文提出的XR-1框架通过引入统一的离散潜在表示（UVMC）作为观察与动作之间的中介，并设计三阶段训练范式，有效解决了上述挑战，在多个机器人操控任务上超越了现有方法，为构建通用VLA模型提供了新途径。

## 2. 方法论：核心思想、关键技术细节与算法流程
- **核心思想**：学习一个**统一的视觉-运动隐编码（Unified Vision-Motion Codes, UVMC）**，作为连接高维观测和低层动作的中间表示，同时对齐来自不同源（机器人、人类）的多模态动态信息。
- **关键技术细节**：
  - **UVMC学习**：采用**双分支VQ-VAE（Vector Quantized Variational Autoencoder）**，其中一个分支编码视觉动力学（如视频帧序列），另一个分支编码机器人运动（如关节轨迹或末端执行器位姿）。两支路共享同一离散码本，通过联合训练使得视觉和运动信息映射到统一的隐空间。
  - **三阶段训练范式**：
    1. **自监督UVMC学习**：在大规模未标注的机器人视频-动作配对数据上预训练双分支VQ-VAE，学习离散的码本表示，无需任务标签。
    2. **UVMC引导的跨本体预训练**：利用学到的UVMC作为中间监督，在大规模跨本体机器人数据集上进行预训练，使模型能够理解不同形态机器人的运动模式与视觉动态的关联。
    3. **任务特定后训练**：在目标任务的小规模标注数据上进行微调，将预训练知识迁移到具体操控任务上。
- **算法流程（文字描述）**：
  1. 输入：多视角视频帧序列（观测）和对应的机器人动作序列。
  2. 分别通过视觉编码器和运动编码器提取特征。
  3. 经VQ-VAE矢量量化得到离散的UVMC代码。
  4. 利用UVMC代码作为条件，通过动作解码器生成精确的低层动作。
  5. 在推理时，仅需视觉输入，通过视觉编码器预测UVMC代码，再解码为动作。

## 3. 实验设计
- **数据集与场景**：
  - 使用大规模跨本体机器人数据集（包括多种机器人、人类演示数据），具体来源未在摘要中详列，但提及“超过12,000次真实世界rollout”。
  - 场景涵盖**6种不同的机器人本体**，包括不同形态的机械臂、移动操作平台等。
  - 任务数量：**超过120种不同操控任务**，如抓取、放置、组装、操作工具等。
- **Benchmark**：与两个强基线方法对比：
  - **π0**（一个代表性的VLA模型）。
  - **GR00T-N1.5**（另一个先进的VLA基准）。
- **对比结果**：XR-1在所有任务上一致优于这些基线，并展现出对**新物体、背景变化、干扰物、光照变化**的强大泛化能力。

## 4. 资源与算力
- **论文明确说明**：文中未提及具体的GPU型号、数量或训练时长。仅提到训练数据规模和真实世界rollout数量，未公开计算资源消耗。因此，无法得知算力需求的具体细节。

## 5. 实验数量与充分性
- **实验数量**：
  - 真实世界实验：**超过12,000次rollout**（可理解为每次任务完成一次尝试），分布在6种机器人、120多种任务上。
  - 消融实验：论文未在摘要中详细提及消融实验的具体数量，但一般VLA论文会包含对UVMC有效性、训练阶段、码本大小等的分析，此处无法确认具体组数。
- **充分性与客观性**：
  - 实验覆盖了多种机器人本体和大量任务，跨域泛化测试（新物体、背景、干扰、光照）增强了结论的普适性。
  - 与多个SOTA方法进行公平比较（在同一环境、相同任务设置下）。
  - 但未提供仿真实验对比或统计显著性检验信息，且缺少对失败案例的分析，因此充分性方面有一定局限。

## 6. 主要结论与发现
- XR-1框架通过学习统一的视觉-运动表示（UVMC），能够有效弥合不同机器人本体和人类演示之间的域差距。
- 三阶段训练范式（自监督+跨本体预训练+任务微调）使得模型能够充分利用大规模异构数据中的互补知识。
- 在真实世界多个机器人操控任务上，XR-1显著优于现有VLA模型（π0和GR00T-N1.5），且对新环境的泛化能力更强。
- 统一视觉-运动表示是构建通用、可扩展VLA模型的有效路径。

## 7. 优点
- **方法创新性**：首次提出双分支VQ-VAE学习统一的离散潜在表示（UVMC），作为视觉与动作之间的桥梁，避免了直接高维映射的困难。
- **数据利用高效**：三阶段训练范式充分利用了大规模未标注的跨本体数据，减少了对手工标注的依赖。
- **跨域泛化能力强**：在物体、背景、光照、干扰物等变化下表现鲁棒。
- **实验量大且真实**：超过12,000次真实机器人rollout，覆盖多种本体和任务，结果可信度高。
- **开源项目**：提供了项目网站，便于复现和扩展。

## 8. 不足与局限
- **算力开销未公开**：缺乏训练所需的GPU型号、数量和时长，无法评估资源门槛，可能限制实际应用。
- **消融实验细节不详**：摘要中未提供UVMC码本大小、VQ-VAE结构选择、各阶段贡献的分析，削弱了方法组件的可解释性。
- **未涉及仿真环境**：实验完全基于真实世界，缺少标准仿真平台（如RLBench、Metaworld）的对比，不利于与学界其他工作直接比较。
- **潜在偏差风险**：120种任务可能集中在某一类操控技能（如夹爪型抓取），对不同任务类型（如移动、灵巧手操作）的覆盖可能不均。
- **应用限制**：依赖多视角视频输入和精密机械系统，在低成本机器人或低光照场景下性能可能下降；且未提及对实时性的要求。
- **统计显著性未报告**：未提供多次实验的方差或置信区间，单次rollout的结果可能存在随机性。

（完）
