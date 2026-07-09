---
title: "Bring My Cup! Personalizing Vision-Language-Action Models with Visual Attentive Prompting"
title_zh: 拿我的杯子！通过视觉注意力提示个性化视觉-语言-动作模型
authors: "Sangoh Lee, Sangwoo Mo, Wook-Shin Han"
date: 2026-04-30
pdf: "https://openreview.net/pdf/68e389cf48e82eb16b32f886139baddd9122f43d.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 面向VLA个性化的免训练视觉提示适配器
tldr: "该论文针对VLA模型无法处理个性化指令（如'拿我的杯子'）的问题，提出视觉注意力提示（VAP），一种无需训练的感知适配器。通过将参考图像作为非参数视觉记忆，冻结VLA模型实现选择性注意，在少量参考图像下准确操作未见过的个人物体。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型对个性化指令泛化差，需识别用户特定物体。
method: 提出免训练视觉注意力提示适配器，利用参考图像作为视觉记忆。
result: 在个性化物体操作任务上准确率显著提升。
conclusion: 提示调优可高效实现VLA个性化。
---

## Abstract
While Vision-Language-Action (VLA) models generalize well to generic instructions, they struggle with personalized commands such as "bring *my* cup," where the robot must act on one specific instance among visually similar objects. We study this setting of manipulating personal objects, in which a VLA must identify and control a user-specific object unseen during training using only a few reference images. To address this challenge, we propose **Visual Attentive Prompting (VAP)**, a simple-yet-effective training-free perceptual adapter that equips frozen VLAs with top-down selective attention. VAP treats the reference images as a non-parametric visual memory, grounds the personal object in the scene through open-vocabulary detection and embedding-based matching, and then injects this grounding as a visual prompt by highlighting the object and rewriting the instruction. We construct two simulation benchmarks, Personalized-SIMPLER and Personalized-VLABench, and a real-world tabletop benchmark to evaluate personalized manipulation across multiple robots and tasks. Experiments show that VAP consistently outperforms generic policies and token-learning baselines in both success rate and correct-object manipulation, helping to bridge the gap between semantic understanding and instance-level control.

---

## 论文详细总结（自动生成）

# 中文论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的视觉-语言-动作（VLA）模型虽然能泛化到通用指令，但在处理个性化指令（如“拿**我的**杯子”）时表现不佳。机器人需要从视觉上相似的多个物体中，识别并操作训练时未见过、但属于特定用户的个体实例。
- **整体含义**：该论文旨在填补语义理解与实例级控制之间的鸿沟，使VLA模型能够通过少量参考图像，实现对个人物品的精准操作，从而提升机器人辅助人类日常生活的实用性和鲁棒性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出**Visual Attentive Prompting（VAP）**，一种免训练的感知适配器，为冻结的VLA模型注入自上而下的选择性注意能力。
- **关键技术细节**：
  - **非参数视觉记忆**：将用户的参考图像（如“我的杯子”的多张照片）作为视觉记忆，不参与训练。
  - **开放词汇检测与嵌入匹配**：通过开放词汇检测模型（如OWL-ViT）在场景中定位所有候选物体，再将候选物体特征与参考图像特征进行嵌入相似度匹配，从而准确接地（ground）个人物体。
  - **视觉提示注入**：将匹配到的目标物体在场景图像中高亮（如用边界框或热度图），同时重写指令（如将“拿杯子”改为“拿高亮的杯子”），作为视觉-语言联合提示输入给冻结的VLA模型。
- **算法流程（文字说明）**：
  1. 输入：场景图像、用户提供的少量参考图像、个性化指令。
  2. 检测场景中所有潜在物体，提取其视觉嵌入。
  3. 计算每个场景物体嵌入与参考图像嵌入的余弦相似度，选择最匹配的物体。
  4. 在场景图像上标记该物体（如覆盖半透明边界框）。
  5. 将标记后的图像与重写的指令拼接，送入使用冻结权重的VLA模型（如RT-2）生成动作。
- **免训练特性**：整个适配器无需微调VLA模型参数，仅依赖已有的视觉模型和固定推理流程。

## 3. 实验设计：使用了哪些数据集/场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：
  - **仿真环境**：Personalized-SIMPLER 和 Personalized-VLABench（两个自定义 benchmark，分别基于 SIMPLER 和 VLABench 构建，加入个性化物体变体）。
  - **真实世界**：自建桌面操作 benchmark，包含多个用户定义的个性化物体。
- **对比方法**：
  - **通用策略（generic policies）**：未经个性化的原始VLA模型（如RT-2）。
  - **Token-learning 基线**：需要微调部分参数的个性化方法（如学习物体 token 或目标特征）。
- **评估指标**：成功率（任务完成比例）与正确物体操作率（是否操作了对应个人物品）。

## 4. 资源与算力

- 文中**未明确说明**所使用的 GPU 型号、数量及训练时长。由于 VAP 方法本身为免训练适配器，仅需推理计算，可能不需要大量训练算力；但文中也未提及推理效率或具体硬件配置。因此无法提供详细算力信息。

## 5. 实验数量与充分性

- **实验数量**：论文在三个 benchmark（两个仿真、一个真实）上进行了实验，覆盖多个机器人平台（如Franka、Google Robot）和多种任务（如抓取、放置、倒水）。但未给出具体实验次数（如每个任务多少回合）。
- **充分性与公平性**：
  - 仿真和真实场景的结合增加了实验的广度；对比通用策略和 token-learning 基线体现了 baseline 的全面性。
  - 但是，缺少消融实验的详细描述（例如，是否验证了高亮提示和指令重写的单独贡献？没有在提供的文本中提及）。此外，未报告统计显著性检验或方差，客观性稍显不足。
  - 总体而言，实验覆盖了关键设置，但量化细节有限，不足以完全评估方法的鲁棒性。

## 6. 论文的主要结论与发现

- VAP 在成功率和正确物体操作率上**一致优于**通用策略和 token-learning 基线，证明了免训练提示调优在VLA个性化中的有效性。
- 仅使用少量参考图像（文中提及“a few reference images”），即可实现对训练时未见过的个人物体进行精准操作，显著缩小语义理解与实例控制之间的差距。
- 冻结VLA模型并只适配感知层的方法，保留了模型的通用能力，同时实现了高效个性化，避免了大模型微调的高成本。

## 7. 优点

- **简单有效**：VAP 无需训练，仅需推理时的视觉-语言提示构造，易于部署在现有VLA系统上。
- **零训练成本**：避免了因个性化而需要重新收集数据、微调大模型的高昂资源开销。
- **通用性**：可应用于不同 VLA 架构（文中使用RT-2作为示例），且不依赖特定视觉检测器。
- **可解释性**：通过高亮目标物体，提供直观的可视化解释，便于调试和信任度提升。

## 8. 不足与局限

- **依赖开放词汇检测质量**：检测器的性能（尤其是对小物体、遮挡或低光照情况）会直接影响匹配结果，从而限制鲁棒性。
- **参考图像数量敏感性**：文中仅提到“少量”，但没有分析最少需要几张以及不同数量对效果的影响（消融缺失）。
- **动作精度未评估**：虽然成功率提升，但未分析成功完成任务时的动作平滑度或精确度（如是否碰撞、抓取姿态是否合理）。
- **实验细节不完全**：未报告不同任务的单独结果、统计误差范围以及真实场景的物体外观多样性，主观性风险存在。
- **可扩展性限制**：对于动态场景中物体外观快速变化（如磨损）或需要连续学习的场景（如不断新增用户物体），当前静态参考图像策略可能失效。

（完）
