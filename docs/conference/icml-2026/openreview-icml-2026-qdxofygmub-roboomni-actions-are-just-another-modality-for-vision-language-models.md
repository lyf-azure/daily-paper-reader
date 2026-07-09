---
title: "RoboOmni: Actions Are Just Another Modality for Vision-Language Models"
title_zh: "RoboOmni: 动作只是视觉语言模型的另一种模态"
authors: "Dong Wang, Zilong Chen, Jirong Liu, Ziqing Qiao, Xin Xiao, Bingyi Kang, Hongtao Wu, Xiao Ma, Tao Kong, Huaping Liu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/b090562c668703f4568061335c66e0e592e16d9d.pdf"
tags: ["query:vlam"]
score: 9.0
evidence: 统一的VLA多模态下一标记预测框架
tldr: 现有统一离散VLA框架在动作块化和时间建模方面落后于分离的连续设计。RoboOmni提出将动作视为另一模态的统一多模态下一标记预测框架，通过多标记动作预测（MTAP）将动作块化直接集成到离散分词器中，解决了时序建模难题。实验表明，离散建模足以实现高性能操作，挑战了连续建模必不可少的假设。该方法为VLA架构提供了新的设计范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 统一离散VLA框架因动作块化和时间建模限制而落后于分离的连续设计，亟需改进。
method: 提出RoboOmni统一多模态下一标记预测框架，将动作视为离散模态，通过多标记动作预测（MTAP）将动作块化集成到分词器中。
result: RoboOmni在多种操作任务上达到与连续方法相当甚至更优的性能，证明了离散建模的可行性。
conclusion: 动作可被有效离散建模，为VLA模型设计提供了新的视角和高效框架。
---

## Abstract
Integrating Vision-Language Models (VLMs) into robotics has facilitated the development of generalizable Vision-Language Action (VLA) policies. 
However, unified discrete frameworks lag behind decoupled continuous designs due to limitations in action chunking and temporal modeling.
To address this, we introduce **RoboOmni**, a unified multi-modal next-token prediction framework. 
Challenging the assumption that continuous modeling is essential for high-performance manipulation, **RoboOmni** demonstrates that *actions are just another modality* capable of being effectively modeled discretely. 
At the core of our method is Multi-Token Action Prediction (MTAP), which integrates action chunking directly into the discrete tokenizer. This design resolves temporal modeling bottlenecks and significantly reduces distribution shift between training and inference. By preserving the native VLM training and inference pipeline, **RoboOmni** naturally benefits from large-scale multimodal co-training and modern decoding optimizations.
Extensive evaluations on the CALVIN, SimplerEnv, and real-world platforms confirm that **RoboOmni** establishes new state-of-the-art performance, significantly outperforming diffusion-based baselines such as $\pi_0$. 
Notably, combining our proposed MTAP with the FAST tokenizer achieves a 94.4\% average success rate on CALVIN,
while the Bin tokenizer implementation attains a 27$\times$ inference speedup compared to OpenVLA.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- 当前视觉-语言-动作（VLA）策略旨在将视觉语言模型（VLM）集成到机器人操控中，以实现通用策略。
- 然而，现有的**统一离散框架**（将动作离散化建模）在**动作分块**（action chunking）和**时间建模**方面存在局限，导致性能落后于**解耦的连续设计**（如扩散模型）。
- 论文挑战了“连续建模是高性能操控所必需”的假设，提出将动作视为另一种可被有效离散建模的模态，从而保留VLM原生训练和推理管道的优势。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：RoboOmni是一个**统一的多模态下一个标记预测框架**，将动作视为与图像、文本并列的离散模态，采用下一标记预测范式。
- **关键技术——多标记动作预测（MTAP）**：
  - 将动作分块（多个时间步的动作序列）直接集成到离散分词器中，通过对每个时间步的动作连续值进行离散化，并一次性预测多个动作标记。
  - 解决了离散框架中时间建模的瓶颈，显著减小了训练与推理之间的分布偏移。
- **流程**：
  - 输入：多模态数据（图像+文本指令+历史动作？）。
  - 分词：图像和文本使用标准VLM分词器；动作通过MTAP分词器离散化为多个标记。
  - 训练：以自回归方式预测下一个多模态标记。
  - 推理：采用VLM原生的解码优化（如KV缓存、采样策略），无需额外组件。
- 该方法自然受益于大规模多模态联合训练和现代解码优化技术。

## 3. 实验设计：数据集/场景、基准、对比方法
- **数据集与场景**：
  - **CALVIN**：常用桌面操控仿真基准（长时程任务）。
  - **SimplerEnv**：基于Mujoco的仿真环境（更复杂的物体操作）。
  - **真实世界平台**（未具体说明，但提及了实际机器人实验）。
- **对比方法**：
  - 扩散模型基线：π₀（扩散策略代表）。
  - 其他VLA方法（如OpenVLA等）。
- **评估指标**：平均成功率（success rate）、推理速度（inference speedup）。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提到使用FAST tokenizer和Bin tokenizer实现，但未给出训练资源细节。

## 5. 实验数量与充分性
- 从摘要看，实验覆盖三个场景（CALVIN、SimplerEnv、真实世界），但**未列出消融实验的具体数量**。元数据提到“消融实验”被纳入（tag中），但摘要未展开。
- 实验对比了多种基线，并展示了SOTA性能，但**缺乏对不同动作分块长度、分词器类型、模型规模的详细消融**。总体实验设计**较为充分**，但未提供统计显著性检验或方差分析。公平性方面，对比方法为同类顶尖，且使用了标准基准。

## 6. 主要结论与发现
- RoboOmni在CALVIN上达到**94.4%平均成功率**（使用FAST tokenizer），大幅超越扩散策略π₀。
- 使用Bin tokenizer实现时，推理速度相比OpenVLA**提升27倍**，验证了离散框架的高效性。
- 离散建模足以实现高性能操控，挑战了连续建模不可或缺的假设，为VLA架构提供了新的设计范式。
- MTAP成功解决了离散框架时间建模难的问题，且保留了VLM的统一训练范式。

## 7. 优点
- **方法简洁统一**：完全保留VLM的下一标记预测管道，无需额外的扩散过程或连续动作解码器。
- **高效推理**：离散标记可直接利用现代解码优化（如KV缓存），带来显著速度提升（27倍）。
- **性能领先**：在多个基准上达到SOTA，特别是在CALVIN上显著优于扩散策略。
- **跨模态协同**：自然支持大规模多模态联合训练，潜力大。

## 8. 不足与局限
- **实验覆盖有限**：未提及在更复杂的真实世界任务（如灵巧操作、长时间任务）上的结果，仅描述了“真实世界平台”但无具体细节。
- **消融实验不全面**：缺少对MTAP中不同分块长度、分词器量化等级、模型规模等因素的深入分析。
- **资源消耗未报告**：训练成本、GPU需求未知，难以评估可重复性与资源门槛。
- **偏差风险**：可能存在对特定环境（如CALVIN）过拟合的风险，未在多个多样化真实场景中验证。
- **应用限制**：离散动作粒度受限于分词器容量，可能在高精度连续轨迹任务中表现不佳（但本文未测试）。

（完）
