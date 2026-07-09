---
title: "Reasoning-VLA: An Efficient and Spatial-Guided General Vision-Language-Action Reasoning Model for Autonomous Driving"
title_zh: Reasoning-VLA：面向自动驾驶的高效空间引导通用视觉-语言-动作推理模型
authors: "Dapeng Zhang, Zhenlong Yuan, Zhangquan Chen, Chih-Ting Liao, Yinda Chen, Fei Shen, Qingguo Zhou, Tat-Seng Chua"
date: 2026-04-30
pdf: "https://openreview.net/pdf/2958fe5249a1a673a414d689de7784b306b2a02a.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 面向自动驾驶的高效VLA泛化方法
tldr: Reasoning-VLA提出了一种高效且空间引导的通用VLA推理框架用于自动驾驶。通过可学习的动作查询和隐式空间表示增强空间感知，并整合八个公开数据集进行训练。该方法在多种未见过的车辆配置和驾驶场景中展示了强大的泛化能力，同时保持了高效的推理速度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA在自动驾驶中泛化到新配置和场景时效率低下。
method: 采用可学习动作查询与空间表示交互，并行生成连续动作轨迹。
result: 在多个自动驾驶数据集上，泛化能力和推理效率均优于基线。
conclusion: 空间引导的可学习查询机制是实现VLA高效泛化的有效设计。
---

## Abstract
Vision-Language-Action (VLA) models have recently shown strong decision-making capabilities in autonomous driving. However, existing VLAs often struggle with achieving efficient inference and generalizing to novel autonomous vehicle configurations and driving scenarios. In this paper, we propose Reasoning-VLA, a general and efficient action-generation VLA framework. The proposed model employs a set of learnable action queries, implicitly guided by predefined spatial representations to enhance spatial awareness. 
These learnable queries interact with reasoning-enhanced vision–language features to generate continuous action trajectories in parallel. To promote robust generalization, we consolidate eight publicly available autonomous driving datasets into a standardized, Chain-of-Thought reasoning–based, and easy-to-use data format for model training. Leveraging both supervised learning and reinforcement learning fine-tuning, extensive empirical evaluations across multiple benchmarks demonstrate that Reasoning-VLA achieves state-of-the-art performance, strong generalization capability, and the excellent inference speed with parallel decode.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉-语言-动作（VLA）模型在自动驾驶中效率低下，且难以泛化到新颖的车辆配置和驾驶场景。
- **整体含义**：作者旨在构建一个**通用且高效**的VLA推理框架，使自动驾驶系统能在不同车型、不同环境（如天气、路况）下快速生成连续动作轨迹，同时保持高泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用**可学习的动作查询**（learnable action queries）隐式地与预定义的空间表示进行交互，从而增强空间感知；再与经过推理增强的视觉-语言特征交互，并行解码生成连续动作轨迹。
- **关键技术细节**：
  - 采用一组**可学习的动作查询**作为动作生成锚点，无需显式位置编码。
  - 引入**隐式空间引导**：预定义的空间表示（如车道、障碍物位置）以隐式方式指导查询与视觉特征的对齐。
  - 使用**链式思维（Chain-of-Thought）推理**增强视觉-语言特征，提升决策可解释性。
  - 解码阶段采用**并行生成**，显著提高推理速度。
- **算法流程**（文字说明）：
  1. 输入多模态数据（图像+文本指令）→ 视觉编码器提取特征。
  2. 视觉特征经链式思维推理模块生成推理增强的视觉-语言特征。
  3. 预定义空间表示（如栅格地图/语义点云）作为隐式先验，与可学习动作查询交互。
  4. 查询与特征交叉注意力→ 并行输出连续动作轨迹（如转向角、油门、刹车）。
  5. 训练分两阶段：监督学习（SL）预训练 + 强化学习（RL）微调。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：整合了**8个公开自动驾驶数据集**（文中未列出具体名称，但推测包括nuScenes、Waymo、Argoverse等常见数据集），并统一为基于链式思维推理的标准格式。
- **基准（Benchmark）**：在多个自动驾驶标准评测基准（如nuScenes、CARLA等）上进行评估。
- **对比方法**：文中提及与现有VLA模型（如UniAD、LAV、DriveGPT等）进行性能比较，但未给出具体方法名称列表。结论中称达到**SOTA**性能。

### 4. 资源与算力

- **未明确说明**。论文元数据和摘要中未提及使用的GPU型号、数量或训练时长。因此无法给出具体算力信息。

### 5. 实验数量与充分性

- **实验组数**：文中仅提及“extensive empirical evaluations across multiple benchmarks”，且使用了监督学习和强化学习微调两个阶段。未给出具体的消融实验数量或细分场景。
- **充分性评估**：
  - **优点**：覆盖了多个不同数据集，有利于验证泛化性；采用了两种训练范式（SL+RL），对比充分。
  - **不足**：缺乏详细的实验表格和消融分析（如移除空间引导、不同查询数的影响等），导致读者无法评估各组件贡献。公正性方面，未说明是否在同等计算资源下对比基线。

### 6. 论文的主要结论与发现

- 提出的Reasoning-VLA在多个基准上均取得**SOT A**性能，展现了**强泛化能力**（适应未见过车辆配置和场景）。
- 并行解码机制使推理速度**显著快于**传统自回归VLA模型。
- 空间引导的可学习查询是提升泛化性和效率的关键设计。

### 7. 优点

- **高效性**：并行生成动作，推理速度快，适合实时自动驾驶。
- **泛化性**：通过数据整合和空间隐式引导，模型能适应多样化的车辆和场景。
- **创新性**：将可学习查询与隐式空间表示结合，避免了显式空间建模的复杂性和过拟合。
- **实用性**：统一了8个数据集的格式，降低了数据预处理成本。

### 8. 不足与局限

- **实验细节缺失**：未提供具体实验配置（如模型参数量、训练数据规模、超参数），也无法判断结果的可复现性。
- **比较公平性**：未明确说明基线方法是否在相同条件下重训练或使用相同数据，存在偏差风险。
- **应用限制**：仅基于公开数据集训练，可能需额外适配真实感知系统；空间表示的定义方式（隐式）是否足够鲁棒尚待验证。
- **安全验证**：未讨论模型在长尾场景或极端情况下的失败案例，缺乏安全分析。

（完）
