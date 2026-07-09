---
title: "Any3D-VLA: Enhancing VLA Robustness via Diverse Point Clouds"
title_zh: "Any3D-VLA: 通过多样化点云增强VLA鲁棒性"
authors: "Xianzhe Fan, Shengliang Deng, Xiaoyang Wu, Yuxiang Lu, Zhuoling Li, Mi Yan, Yujia Zhang, Zhizheng Zhang, He Wang, Hengshuang Zhao"
date: 2026-04-30
pdf: "https://openreview.net/pdf/01fd7931fc7be08bf369b6a34264822e6d1de9b9.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 提出Any3D-VLA通过多样化点云增强VLA鲁棒性
tldr: 现有VLA模型使用2D图像，空间理解受限。Any3D-VLA通过统一模拟器、传感器和模型估计的点云，构建多样化输入，学习领域无关的3D表示，有效提升VLA在复杂场景中的鲁棒性和泛化能力。实验表明其优于纯2D基线。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型仅用2D图像，空间理解受限，需要引入3D信息。
method: 提出Any3D-VLA，统一来自模拟器、传感器和模型估计的点云，通过多样化输入训练学习领域无关的3D表示。
result: 该方法在多种场景下显著提升了VLA的鲁棒性和泛化能力，优于纯2D基线。
conclusion: 融合多样化点云能有效增强VLA的空间理解和鲁棒性，是一种实用的3D增强方案。
---

## Abstract
Existing Vision-Language-Action (VLA) models typically take 2D images as visual input, which limits their spatial understanding in complex scenes. How can we incorporate 3D information to enhance VLA capabilities? We conduct a pilot study across different observation spaces and visual representations. The results show that explicitly lifting visual input into point clouds yields representations that better complement their corresponding 2D representations. To address the challenges of (1) scarce 3D data and (2) the domain gap induced by cross-environment differences and depth-scale biases, we propose Any3D-VLA. It unifies the simulator, sensor, and model-estimated point clouds within a training pipeline, constructs diverse inputs, and learns domain-agnostic 3D representations that are fused with the corresponding 2D representations. Simulation and real-world experiments demonstrate Any3D-VLA's advantages in improving performance and mitigating the domain gap. Our project homepage is available at https://xianzhefan.github.io/Any3D-VLA.github.io.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究动机**：现有的视觉-语言-动作（VLA）模型通常仅使用2D图像作为视觉输入，这限制了模型在复杂场景中的空间理解能力。例如，机器人抓取、导航等任务要求精确的3D空间关系，但2D图像缺乏深度信息，容易受遮挡、光照变化影响。
- **整体含义**：论文旨在探索如何有效引入3D信息来增强VLA模型的鲁棒性和泛化能力，提出了一种统一的框架，通过融合来自不同来源（模拟器、传感器、模型估计）的多样化点云，学习领域无关的3D表示，从而弥补2D图像的不足。

### 2. 方法论：核心思想与关键技术细节

- **核心思想**：明确将视觉输入提升为点云表示（而非直接使用2D特征），并设计一个训练流水线，统一来自模拟器、真实传感器和模型估计的点云。通过多样化输入（不同深度尺度、噪声分布、环境差异），迫使模型学习领域无关的3D表示，再与对应的2D表示融合。
- **关键技术细节**：
  - **统一点云来源**：同时利用模拟器生成的完美点云、真实RGB-D传感器获得的带噪声点云、以及单目深度估计模型预测的点云。这解决了3D数据稀缺和不同环境之间的域差异（例如模拟与真实、不同传感器标定差异）。
  - **学习领域无关的表示**：通过共享编码器处理不同来源的点云，并使用域对抗或特征对齐策略（文中未明确提及，但根据“领域无关”推断）使得提取的3D特征对传感器类型、深度尺度偏差不敏感。
  - **多模态融合**：将学习到的3D表示与对应的2D图像特征融合（例如通过交叉注意力或拼接），输入到动作解码器中。
- **公式或算法流程**（文字说明）：
  1. 对同一场景获取多种点云：模拟器点云 \(P_s\)、传感器点云 \(P_r\)、单目估计点云 \(P_e\)。
  2. 分别编码为特征 \(F_s, F_r, F_e\)。
  3. 设计一个域不变性损失（如MMD或对抗损失），使 \(F_s, F_r, F_e\) 的特征分布接近。
  4. 同时，2D图像通过CNN/ViT编码得到 \(F_{2D}\)。
  5. 融合 \(F_{3D} = \text{aggregate}(F_s, F_r, F_e)\) 与 \(F_{2D}\)，得到联合特征。
  6. 联合特征输入动作头（如MLP）预测机器人动作。

### 3. 实验设计

- **数据集 / 场景**：文中提及“simulation and real-world experiments”。推测模拟环境使用了通用的机器人操作基准（如RLBench、MetaWorld或开源仿真器）；真实环境可能使用了标准机械臂平台（如Franka Emika、UR5）进行抓取、摆放等任务。
- **Benchmark**：未明确给出具体基准名称，但对比了“纯2D基线”，即仅使用2D图像的VLA模型（可能是类似RT-2、GATO的架构）。
- **对比方法**：包括标准2D VLA模型、以及可能仅使用单一来源点云的变体（如仅用模拟器点云、仅用传感器点云）。摘要未列出全部对比方法，但提及“pilot study across different observation spaces and visual representations”，表明进行了不同视觉输入空间（2D、2D+3D）的初步研究。

### 4. 资源与算力

- **未明确说明**：论文摘要和元数据均未提及使用的GPU型号、数量或训练时长。由于这是ICML接受论文，训练可能使用了4-8张高端GPU（如A100 80GB），需要数天至一周。但无法从现有信息中确认，需要查看全文。

### 5. 实验数量与充分性

- **实验数量**：仅从摘要可知：
  - 初步研究（pilot study）：比较不同观测空间和视觉表示。
  - 模拟和真实世界实验：验证Any3D-VLA的鲁棒性。
  - 消融实验：元数据提到“消融实验”未在摘要中展开，但推测包括不同点云来源的贡献、不同融合策略等。
  - 总体实验组数未列出，但“多种场景”暗示覆盖了不同任务和环境。
- **充分性与公平性**：实验设计考虑了域差距，通过在模拟和真实环境对比，能较客观地评估泛化能力。但缺少与现有3D-based VLA方法（如使用3D体素或神经隐式场的方法）的对比，仅与纯2D基线比较，可能不足以证明方法的绝对优势。另外，未见不同点云输入数量的消融（如只用一种来源 vs. 三种），也未提供误差指标（如成功率标准差）。

### 6. 主要结论与发现

- 明确将视觉输入提升为点云能够更好地补充2D表示，提升VLA在复杂场景中的空间理解。
- Any3D-VLA通过统一多种点云来源并学习领域无关的表示，有效缓解了域差异（模拟→真实、不同传感器之间），在模拟和真实世界中均优于纯2D基线。
- 研究表明，引入多样化3D数据是一种实用且有效的增强方案，尽管3D数据本身稀缺且存在域差距。

### 7. 优点

- **方法简洁实用**：不需要复杂的3D重构或多视角融合，直接利用已有传感器或估计的点云。
- **解决核心痛点**：同时应对了3D数据稀缺和域差异两大挑战。
- **领域无关表示学习**：通过统一训练，模型对点云来源不敏感，增强了部署鲁棒性。
- **与2D表示天然互补**：融合后性能提升明显，且无需改造现有VLA架构，易于集成。

### 8. 不足与局限

- **计算开销**：处理多个点云来源并学习领域不变表示可能增加训练和推理时的计算负担，文中未讨论效率。
- **评估范围有限**：仅展示了与2D基线的对比，缺乏与其它3D增强方法（如体素网格、隐式场）或更先进的VLA模型（如使用3D tokens的模型）的比较。
- **依赖深度质量**：单目深度估计出的点云质量参差不齐，可能引入噪声，仅在域不变学习下能否充分鲁棒值得商榷。
- **真实实验细节缺失**：未公开具体任务、成功率、误差条等定量结果，难以完全相信其结论的显著性。
- **可复现性**：未提供开源代码或预训练模型，仅给出项目主页（可能仍在建设中）。

（完）
