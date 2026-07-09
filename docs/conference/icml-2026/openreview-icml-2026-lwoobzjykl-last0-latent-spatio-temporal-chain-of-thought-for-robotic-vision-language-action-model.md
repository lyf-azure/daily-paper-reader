---
title: "LaST$_{0}$: Latent Spatio-Temporal Chain-of-Thought for Robotic Vision-Language-Action Model"
title_zh: "LaST₀: 用于机器人视觉-语言-动作模型的隐式时空思维链"
authors: "Zhuoyang Liu, Jiaming Liu, Hao Chen, Jiale Yu, Ziyu Guo, Chengkai Hou, Chenyang Gu, Xiangju Mi, Renrui Zhang, Kun Wu, Zhengping Che, Jian Tang, Pheng-Ann Heng, Shanghang Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0e9ec532d1e01f801ca9bc49e258c05cf3a207f5.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 通过隐式时空推理提升VLA泛化能力
tldr: VLA模型在未知环境中的泛化受限于显式推理的延迟和语言瓶颈。LaST0提出隐式时空思维链（CoT）框架，在动作执行前进行高效的隐式推理，捕捉细粒度物理属性与机器人状态变化，无需像素级预测。实验表明该方法在多个操作任务上提升了泛化性能和推理效率。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 显式推理导致延迟且受限于语言空间，无法忠实捕捉难以描述的物理属性。
method: 提出LaST₀，在隐空间中进行时空CoT推理，捕获细粒度物理属性与机器人状态。
result: 在多个机器人操作任务上，LaST₀在保持低推理延迟的同时显著提升了泛化性能。
conclusion: 隐式时空推理是一种高效提升VLA泛化能力的方法。
---

## Abstract
Vision-Language-Action (VLA) models have recently shown strong generalization, with some approaches seeking to explicitly generate linguistic reasoning traces or predict future observations prior to execution. However, explicit reasoning typically incurs non-negligible inference latency, which constrains the temporal resolution required for robotic manipulation. Moreover, such reasoning is confined to the linguistic space, imposing a representational bottleneck that struggles to faithfully capture ineffable physical attributes. To mitigate these limitations, we propose LaST$_0$, a framework that enables efficient reasoning before acting through a Latent Spatio-Temporal Chain-of-Thought (CoT), capturing fine-grained physical and robotic dynamics that are often difficult to verbalize. Specifically, we introduce a token-efficient latent CoT space that models future visual dynamics, 3D structural information, and robot proprioceptive states, and further extends these representations across time to enable temporally consistent implicit reasoning trajectories. Furthermore, LaST$_0$ adopts a dual-system architecture implemented via a Mixture-of-Transformers design, where a reasoning expert conducts low-frequency latent inference and an acting expert generates high-frequency actions conditioned on robotics-oriented latent representations. To facilitate coordination, LaST$_0$ is trained with heterogeneous operation frequencies, enabling adaptive switching during deployment. Across 10 real-world tasks spanning tabletop, mobile, and dexterous hand manipulation, LaST$_0$ improves mean success rates by 13%, 14% and 14% over prior SOTA VLA methods, respectively.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文信息生成的详细中文总结。

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前的视觉-语言-动作（VLA）模型虽然在机器人操作任务中展现出较强的泛化能力，但大多依赖显式的推理过程（如生成语言推理链条或预测未来观察结果）。这种显式推理存在两大局限性：
  - **推理延迟高**：产生不可忽视的推理延迟，制约了机器人操作所需的高时间分辨率。
  - **语言表达瓶颈**：推理被限制在语言空间，难以忠实捕捉那些难以言述（ineffable）的细粒度物理属性（如摩擦力、物体软硬度、重心变化等）。
- **研究动机**：为了解决上述瓶颈，使VLA模型在保持低延迟的同时，能够感知和利用机器人-环境交互中关键的物理和运动学信息，从而提升在未知环境中的泛化能力。
- **整体含义**：本文提出一种**隐式时空思维链（Latent Spatio-Temporal Chain-of-Thought, CoT）**框架，在隐空间中高效地进行推理，而非依赖显式语言或像素级预测，从而兼顾泛化性能与实时性。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：在动作执行前，在隐空间中构建一个“推理专家”，通过隐式时空CoT对未来的视觉动态、3D结构信息和机器人本体感知状态进行建模，形成时间上一致的隐式推理轨迹，并将这些富含机器人学导向的隐式表征传递给“动作专家”生成高频动作。
- **关键技术细节**：
  - **双系统架构（Dual-System Architecture）**：采用混合变换器（Mixture-of-Transformers）设计，包含两个专家：
    - **推理专家（Reasoning Expert）**：以低频运行，执行隐式时空推理，捕捉细粒度物理属性和机器人状态变化。
    - **动作专家（Acting Expert）**：以高频运行，基于推理专家输出的隐式表征生成动作。
  - **隐式时空CoT空间（Latent Spatio-Temporal CoT Space）**：一种token高效的隐空间，用于建模未来视觉动态、3D结构和本体感知状态。该空间扩展了时间维度，使得隐式推理轨迹在时间上保持一致。
  - **异构操作频率训练（Heterogeneous Operation Frequencies）**：在训练过程中，推理专家和动作专家以不同的频率运行，从而在部署时能够自适应切换，保证推理的低延迟与动作的高频率。
  - **无需像素级预测**：与显式方法不同，LaST₀ 不生成中间图像或语言表述，直接在隐空间中完成推理，避免引入额外解码开销。

## 3. 实验设计

- **场景与基准（Benchmark）**：覆盖10个真实世界机器人操作任务，分为三大类：
  - 桌面操作（Tabletop）
  - 移动操作（Mobile）
  - 灵巧手操作（Dexterous Hand）
- **对比方法**：与先前的SOTA（State-of-the-Art）VLA方法进行了公平比较。
- **评价指标**：平均成功率（Mean Success Rate）。

## 4. 资源与算力

- **未明确说明**：论文提供的文本中没有提及使用的GPU型号、数量、训练时长等具体算力信息。

## 5. 实验数量与充分性

- **实验数量**：涵盖10个不同难度的真实世界场景，涉及三种不同类型的机器人操作任务（桌面、移动、灵巧手），对比了多个SOTA基线方法，实验规模较充分。
- **充分性与公平性**：
  - 覆盖任务类型多样，能够检验方法在不同环境下的泛化能力。
  - 对比的是“先前的SOTA VLA方法”，表明实验设计注重与强基线的比较。
  - 但文本未披露消融实验的具体细节（例如是否分别验证隐式CoT、双系统频率训练等组件的贡献），也未提及统计显著性检验或多次重复实验的方差。因此，从提供的信息看，实验在广度上较好，但细节充分性上可能有所欠缺。

## 6. 论文的主要结论与发现

- **主要结论**：LaST₀ 通过隐式时空思维链推理，在**三个大类的10个任务上**，相对于现有最佳VLA方法分别提升了**13%（桌面）、14%（移动）和14%（灵巧手）**的平均成功率。
- **关键发现**：
  - 隐式推理能够有效捕获那些难以用语言描述的物理动态（如摩擦力、刚度等），从而提升泛化性能。
  - 双系统架构结合异构频率训练，在不牺牲推理延迟的前提下实现了高效的隐式推理，克服了显式推理的速度瓶颈。
  - 隐式时空CoT是一种高效且有效的提升VLA泛化能力的方法。

## 7. 优点

- **方法亮点**：
  - **推理效率高**：隐式推理避免了显式语言生成或未来帧预测带来的延迟，适合高频实时操作。
  - **突破语言瓶颈**：在隐空间建模物理属性和状态变化，解决了语言无法表达细微物理量的固有限制。
  - **结构创新**：双系统设计（低频推理+高频动作）和混合变换器架构，展示了如何将复杂推理与快速执行解耦。
  - **无需像素级预测**：降低计算开销，且避免像素预测的模糊性。
- **实验亮点**：
  - 在真实世界场景（而不是模拟器）进行10个任务评估，落地意义强。
  - 覆盖桌面、移动、灵巧手三种典型操作形态，展现通用性。

## 8. 不足与局限

- **实验局限**：
  - 未公开具体算力消耗，读者难以复现或评估实际部署成本。
  - 缺少消融实验细节，无法准确判断每个组件（隐式CoT、异构频率、双系统等）的独立贡献。
  - 未报告成功率的标准差或置信区间，无法判断结果稳定性。
- **方法局限**：
  - 隐式推理的可解释性较弱，难以观察或诊断推理错误的原因。
  - 可能依赖于高质量的隐空间表征学习，若训练数据分布偏移，隐式表征可能失真。
  - 当前仅在成功率上报告提升，未涉及其他指标（如执行时间、鲁棒性、样本效率等）。
- **应用限制**：
  - 需要多模态数据（视觉、3D、本体感知）对齐训练，数据获取成本高。
  - 双系统架构可能引入额外的超参数（如推理频率），调优复杂度增加。

（完）
