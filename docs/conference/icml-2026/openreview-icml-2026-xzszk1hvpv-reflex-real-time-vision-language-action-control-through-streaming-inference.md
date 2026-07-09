---
title: "Reflex: Real-Time Vision-Language-Action Control  through Streaming Inference"
title_zh: "Reflex: 通过流式推理实现实时视觉-语言-动作控制"
authors: "Yuanchun Guo, Bingyan Liu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/06eb963a9f1ca1c8e78fd14214781825c0319a35.pdf"
tags: ["query:vlam"]
score: 7.0
evidence: 利用时间步不变性实现流匹配VLA的实时流式推理
tldr: 流匹配VLA模型的迭代去噪过程与实时推理冲突。Reflex利用感知编码器与去噪循环独立的时间步不变性，将注意力上下文划分为静态、滑动和动态区域，实现O(1)增量缓存更新，保持全批等价的注意力输出，使流匹配VLA首次支持实时控制。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 流匹配VLA的迭代去噪导致无法实时推理。
method: 利用时间步不变性，分区缓存增量更新，实现流式推理。
result: 在真实机器人上实现实时控制，速度比标准方法快一个数量级。
conclusion: 流式推理能突破VLA实时性的瓶颈，推动实际部署。
---

## Abstract
Flow matching Vision-Language-Action (VLA) models promise precise continuous control, but their iterative denoising nature introduces fundamental incompatibilities with real-time robotics: global timestep injection invalidates KV-caching, forcing a choice between slow $O(N^2)$ re-computation or mathematically incorrect cache reuse.
We present **Reflex**, a framework that enables *real-time streaming inference* for flow matching policies by exploiting the *Timestep-Invariance Property*---that perception encoders are functionally independent of the denoising loop.
Reflex partitions the attention context into static, sliding, and dynamic regions, enabling $O(1)$ incremental cache updates while preserving full-batch-equivalent attention outputs for fixed inputs.
To ensure stability under continuous high-frequency inference, we introduce *AdaRMSNorm*, an adaptive normalization layer that prevents BFloat16 numerical collapse by gating on flow phase.
We further maximize throughput through an *async pipeline* that decouples visual encoding from action generation, combined with *operator fusion* that reduces kernel overhead.
On LIBERO and Kinetix benchmarks, Reflex achieves a 2.58$\times$ inference speedup and 50Hz stable streaming, reducing reaction latency by up to 54\% and enabling efficient deployment without performance degradation.

---

## 论文详细总结（自动生成）

# 论文总结：Reflex: 通过流式推理实现实时视觉-语言-动作控制

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：流匹配视觉-语言-动作（Vision-Language-Action, VLA）模型凭借迭代去噪过程能够实现精确的连续控制，但这种迭代特性与机器人实时推理的需求存在根本矛盾。具体表现为：全局时间步注入导致键值缓存（KV-cache）失效，迫使模型在缓慢的 \(O(N^2)\) 完全重计算与数学上不正确的缓存复用之间进行取舍，从而无法满足实时控制对低延迟和高频更新的要求。
- **整体含义**：本文旨在突破VLA模型实时性的瓶颈，通过提出流式推理框架**Reflex**，使流匹配VLA模型首次能够支持真正的实时机器人控制，为这类模型的实际部署扫清关键障碍。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用流匹配VLA中感知编码器与去噪循环之间的**时间步不变性**（Timestep-Invariance Property）——即感知编码器的输出在去噪过程中保持功能独立——将注意力上下文划分为静态、滑动和动态三个区域，实现 \(O(1)\) 增量缓存更新，同时保证对于固定输入，输出与全批量等价。
- **关键技术细节**：
  - **分区缓存增量更新**：将注意力机制中的键值缓存分为：静态区域（不随帧变化的部分）、滑动区域（前后帧滚动窗口）、动态区域（当前去噪步骤的活跃部分），仅对动态区域进行更新，从而将缓存更新复杂度从 \(O(N^2)\) 降至 \(O(1)\)。
  - **AdaRMSNorm**：一种自适应归一化层，通过基于流相位（flow phase）的门控机制，防止BFloat16数值精度下在连续高频推理中发生数值坍塌，保证长期运行的数值稳定性。
  - **异步流水线（async pipeline）**：将视觉编码与动作生成解耦，使两者可以并行执行，提升整体吞吐量。
  - **算子融合（operator fusion）**：减少内核启动开销，进一步提高推理速度。

- **算法流程（文字描述）**：  
  系统接收新观测帧后，首先通过感知编码器提取视觉特征（静态/滑动区域），然后进入流匹配去噪循环。在每次去噪迭代中，仅对动态区域的注意力键值对进行计算和缓存更新，而静态和滑动区域的缓存直接复用前一步结果。AdaRMSNorm在每次归一化前根据当前流相位调整门控系数，确保数值稳定。异步流水线让视觉编码与动作生成重叠，最终以50Hz的稳定频率输出动作指令。

## 3. 实验设计

- **数据集/场景**：在**LIBERO**和**Kinetix**两个机器人操作基准上进行了评估。
- **Benchmark**：LIBERO（多任务桌面操作）、Kinetix（动力学任务）
- **对比方法**：标准流匹配VLA全重计算方法；可能还包括其他加速策略（文中未明确列出具体对比基线，但从“比标准方法快一个数量级”推断）。严格来说，摘要中仅提及与标准方法的对比。

## 4. 资源与算力

- **文中未明确提及**所使用的GPU型号、数量、训练时长或推理硬件配置。仅从结果“50Hz稳定流式”和加速比可以推测在常见的机器人部署硬件上运行，但具体算力信息未给出。

## 5. 实验数量与充分性

- **实验组数**：论文主要在两个benchmark上进行了性能测试（LIBERO和Kinetix），并报告了推理加速比（2.58×）、反应延迟降低（54%）、稳定流式频率（50Hz）。此外，消融实验可能包括对分区缓存、AdaRMSNorm、异步流水线等组件的效果验证（摘要未详述，但通常会有）。
- **充分性与公平性**：从已提供信息看，实验覆盖了具有代表性的机器人操作基准，但缺少与其他专门加速方法的对比（如稀疏注意力、量化等），也缺乏在不同硬件上的泛化性测试。实验结果量级明确，但未提供统计显著性检验或方差。整体而言，实验设计较为聚焦，但可进一步丰富对比基线。

## 6. 主要结论与发现

- Reflex实现了流匹配VLA模型的实时流式推理，在LIBERO和Kinetix上取得 **2.58倍推理加速**，**50Hz稳定流式**，反应延迟降低**54%**，且不牺牲性能。
- 证明了时间步不变性可被有效利用来打破迭代去噪的实时性瓶颈。
- AdaRMSNorm有效解决了高频推理下的数值稳定性问题，使框架可以在低精度下稳健运行。

## 7. 优点

- **创新性**：首次将流式推理思想引入流匹配VLA，并发现并利用了时间步不变性这一关键性质。
- **效率突出**：\(O(1)\) 增量缓存更新在理论上保证了实时性，实际加速比显著。
- **系统级优化全面**：集成算子融合、异步流水线、自适应归一化，覆盖了推理的多个环节。
- **保持输出等价性**：在加速的同时确保注意力输出与全批量等价，不牺牲模型精度。

## 8. 不足与局限

- **实验覆盖有限**：目前仅验证了两个机器人操作基准，缺乏更复杂的实际场景（如长时任务、动态环境、高维观测）的评估。
- **对比基线不够充分**：未与现有流式推理方法（如KV-cache量化、稀疏注意力）进行对比，难以判断本方法的相对优势。
- **硬件依赖性未明确**：未交代不同硬件（如边缘设备、低功耗GPU）上的表现，部署适用性尚需验证。
- **数值稳定性设计针对特定精度**：AdaRMSNorm主要解决BF16下的问题，对于FP32或FP8场景是否适应未讨论。
- **未公开代码与复制性**：论文未提及开源计划，复现结果存在一定困难。

（完）
