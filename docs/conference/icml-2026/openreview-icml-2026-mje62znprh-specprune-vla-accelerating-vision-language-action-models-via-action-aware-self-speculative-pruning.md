---
title: "SpecPrune-VLA: Accelerating Vision-Language-Action Models via Action-Aware Self-Speculative Pruning"
title_zh: "SpecPrune-VLA: 通过动作感知自推测剪枝加速视觉-语言-动作模型"
authors: "Hanzhen Wang, Jiaming Xu, Yushun Xiang, Jiayi Pan, Yongkang Zhou, Yong-Lu Li, Guohao Dai"
date: 2026-04-30
pdf: "https://openreview.net/pdf/7eec4eb2e35aaa2f83768225529a16f653c1cc5b.pdf"
tags: ["query:vlam"]
score: 7.0
evidence: 无需训练的剪枝加速VLA推理
tldr: 现有VLA加速方法仅关注当前步骤局部信息，导致成功率下降和加速有限。SpecPrune-VLA利用VLA任务中的时空一致性，提出无需训练的自推测剪枝方法，结合局部与全局上下文进行令牌选择。实验表明在保持甚至提升成功率的同时实现了显著加速。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有剪枝加速方法忽略全局上下文，导致成功率下降和加速效果有限。
method: 提出训练无关的SpecPrune-VLA，利用时空一致性结合局部与全局信息进行令牌选择。
result: 在多个VLA基准上，该方法在保持成功率的同时实现加速，且无需额外训练。
conclusion: 结合全局上下文的剪枝策略能有效加速VLA模型且不损失性能。
---

## Abstract
Pruning is a typical acceleration technique for compute-bound models by removing computation on unimportant values. Recently, it has been applied to accelerate Vision-Language-Action (VLA) model inference. However, existing acceleration methods focus on local information from the current action step and ignore the global context, leading to $>$20\% success rate drop and limited speedup in some scenarios. In this paper, we point out \textbf{spatial-temporal consistency} in VLA tasks: input images in consecutive steps exhibit high similarity, and propose the key insight that token selection should combine local information with global context of the model. Based on this, we propose ***SpecPrune-VLA***, a training-free, two-level pruning method with heuristic control.
**(1) Action-level static pruning.** We leverage global history and local attention to statically reduce visual tokens per action.
**(2) Layer-level dynamic pruning.** We prune tokens adaptively per layer based on layer-wise importance.
**(3) Lightweight action-aware controller:** We classify actions as coarse- or fine-grained by the speed of the end effector and adjust pruning aggressiveness accordingly. Extensive experiments show that SpecPrune-VLA achieves up to 1.57× speedup in LIBERO simulation and 1.70× on real-world tasks, with negligible success rate degradation.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- 视觉-语言-动作模型（VLA）在机器人操控等任务中推理延迟较高，需要加速。
- 现有加速方法（如剪枝）仅关注当前动作步骤的局部信息，忽略了全局上下文，导致成功率下降超过20%，且加速效果有限。
- 论文指出 VLA 任务中存在**时空一致性**（spatial-temporal consistency），即连续步骤的输入图像高度相似，因此令牌选择应结合局部信息与全局上下文。
- 基于此提出 **SpecPrune-VLA**，一种无需训练的自推测剪枝方法，在保持甚至提升成功率的同时实现显著加速。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用时空一致性，将历史全局信息与当前局部注意力相结合进行令牌选择，并通过动作感知控制器自适应调整剪枝力度。
- **关键技术细节**（两层剪枝 + 动作感知控制器）：
  - **(1) Action-level 静态剪枝**：利用全局历史信息和局部注意力，对每个动作步骤的视觉令牌进行静态减少。
  - **(2) Layer-level 动态剪枝**：根据层间重要性，在每一层自适应地剪枝令牌。
  - **(3) 轻量级动作感知控制器**：根据末端执行器速度将动作分类为粗粒度（coarse）或细粒度（fine-grained），并相应调整剪枝的激进程度。
- **算法流程**：无需训练，直接在预训练 VLA 模型上执行推断时的剪枝，通过上述两层剪枝和控制器实现加速。

## 3. 实验设计：使用的数据集/场景、Benchmark、对比方法
- **仿真场景**：LIBERO 仿真环境。
- **真实世界任务**：真实机器人操控任务。
- **Benchmark**：未明确列出具体基准名称，但提及 LIBERO 仿真和真实任务。
- **对比方法**：abstract 中提及“existing acceleration methods”（现有加速方法），但未具体列举方法名称。推测对比了其他剪枝/令牌减少方法。

## 4. 资源与算力
- 文中未明确说明使用了多少算力（GPU型号、数量、训练时长等）。
- 值得注意的是，该方法本身是“训练无关”的，因此可能仅需推理算力，无需额外训练资源，但具体硬件信息未提供。

## 5. 实验数量与充分性
- 在 LIBERO 仿真和真实世界任务上进行了“大量实验”（extensive experiments）。
- 提供了具体的加速比：LIBERO 仿真上最高 1.57×，真实任务上最高 1.70×，且成功率下降可忽略（negligible success rate degradation）。
- 可能包含消融实验（如 action-level 和 layer-level 剪枝各自的效果、动作感知控制器的贡献），但文中未明确列出实验组数。
- 总体而言，实验覆盖了仿真和真实场景，且结果量化明确，但缺乏与其他 SOTA 方法的详细对比，以及在不同 VLA 模型上的泛化性验证。

## 6. 论文的主要结论与发现
- SpecPrune-VLA 在不需要额外训练的情况下，通过结合全局上下文的自推测剪枝，能够有效加速 VLA 模型推理，同时保持甚至提高任务成功率。
- 证实了时空一致性在 VLA 令牌选择中的关键作用，以及动作感知的剪枝控制对性能的正面影响。

## 7. 优点：方法或实验设计上的亮点
- **训练无关**：无需微调或重新训练，可直接部署在预训练 VLA 模型上，实用性高。
- **双层剪枝架构**：action-level 静态减少 + layer-level 动态自适应，兼顾了计算效率与精度。
- **动作感知控制**：根据任务精细度调整剪枝强度，避免了过度剪枝导致失败。
- **实验真实性**：同时包含仿真和真实世界任务，验证了方法的实际落地能力。

## 8. 不足与局限
- **实验对比不充分**：未列出与最新加速方法（如 token 合并、稀疏注意力等）的详细对比，无法全面评估相对优势。
- **动作感知控制器依赖**：需要实时获取末端执行器速度信息，可能对某些低层感知接口有限制。
- **泛化性未验证**：仅在 LIBERO 和真实任务上测试，未涉及更复杂的多步骤 VLA 任务或不同视觉编码器/LLM架构。
- **计算开销未说明**：轻量级控制器本身的推理开销未量化，可能存在成为新瓶颈的风险。
- **偏差风险**：实验结果可能依赖于特定 VLA 模型和任务设置，在其他非连续或动态变化剧烈的环境中可能失效。

（完）
