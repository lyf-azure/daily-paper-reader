---
title: "TokenDrop: Token-Level Importance-Aware Backward Propagation Skipping for Efficient LLM Fine-Tuning"
title_zh: TokenDrop：面向高效LLM微调的Token级重要性感知反向传播跳过
authors: "Beomseok Kim, Sol Namkung, Dongsuk Jeon"
date: 2026-04-30
pdf: "https://openreview.net/pdf/7ed2097a6ad49dbc72628296b85d006e1970ac9d.pdf"
tags: ["query:vlam"]
score: 8.0
evidence: 基于token重要性的高效PEFT反向传播跳过方法
tldr: 大语言模型微调即使使用PEFT方法仍受限于显存和计算瓶颈。本文提出TokenDrop，通过在前向传播中评估token重要性，在反向传播中跳过不重要token的计算，从而减少激活内存和加速微调。采用累积token选择保持梯度连续性。实验表明在多个模型和任务上显著降低训练成本和显存占用，且不损失精度，为PEFT提供了正交优化方向。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有PEFT虽减少可训练参数，但激活内存和计算开销仍是瓶颈。
method: 提出TokenDrop，根据残差更新幅度评估token重要性，跳过不重要token的反向传播，同时引入累积选择策略保证梯度连续性。
result: 在多个LLM和下游任务上，TokenDrop降低训练显存和加速比明显，且性能无损。
conclusion: TokenDrop是一种轻量级、即插即用的训练加速方法，可与现有PEFT方法互补。
---

## Abstract
Despite the success of parameter-efficient fine-tuning (PEFT) methods in reducing parameter-related overhead, fine-tuning large language models (LLMs) is still bottlenecked by significant memory and computational demands. In this paper, we propose **TokenDrop**, a token-level importance-aware backpropagation skipping method that reduces activation memory and accelerates LLM fine-tuning by skipping backward computations for less informative tokens. TokenDrop evaluates token importance based on the magnitude of residual updates during the forward pass, enabling lightweight, gradient-free importance estimation. Furthermore, we introduce cumulative token selection to preserve gradient continuity across layers and lazy selection scheduling that defers token selection to facilitate globally informed importance scoring under memory constraints. Across a range of experiments, TokenDrop achieves up to **42.9**\% reduction in memory usage and up to **1.50**$\times$ training speedup, while preserving accuracy and outperforming existing backpropagation-skipping baselines. The code is available at https://github.com/kimbss470/tokendrop_official.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：尽管参数高效微调（PEFT）方法大幅减少了可训练参数，但大语言模型（LLM）微调仍面临显著的内存和计算瓶颈，主要源于**激活值（activation）的存储和反向传播计算**。现有PEFT侧重参数侧优化，忽略了激活侧的开销。
- **核心问题**：如何在不影响微调精度的前提下，进一步降低激活内存消耗并加速训练。
- **整体含义**：提出一种**token级别的重要性感知反向传播跳过方法**（TokenDrop），通过在前向传播中判断哪些token贡献较低，在反向传播中跳过其梯度计算，从而直接减少激活存储和计算量。该方法与PEFT正交，可进一步拓展高效微调的性能边界。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用前向传播时**残差更新幅度（magnitude of residual updates）** 作为token重要性的轻量级、无梯度的估计指标。重要性低的token在反向传播中被跳过，只计算重要token的梯度。
- **关键技术细节**：
  - **基于残差更新的重要性评估**：在每一层的前向传播中，计算该层输出相对于输入的残差（即 \( \Delta = \text{output} - \text{input} \)），取范数作为token重要性分数。该方法无需额外梯度计算，开销极低。
  - **累积token选择（Cumulative Token Selection）**：为了保持跨层的梯度连续性，将多层的重要性分数累积起来，在最后统一进行选择，避免单层突变导致梯度失真。
  - **懒惰选择调度（Lazy Selection Scheduling）**：由于内存限制，将token选择推迟到深层（如Transformer后几层），利用更全局的信息进行重要性评分，提高选择准确性。
- **算法流程（文字说明）**：
  1. 前向传播：逐层计算每层的残差更新范数，并累积得到每个token的全局重要性分数。
  2. 根据设定的丢弃率（如 \( r \)），保留分数最高的 \( (1-r) \) 比例 token，其余标记为“跳过”。
  3. 反向传播：只对保留的token计算梯度，跳过token的梯度置零，不更新相关激活缓存。
  4. 更新参数（仅对PEFT中的可训练参数进行梯度累积），重复训练步骤。

### 3. 实验设计：使用的数据集/场景、benchmark、对比方法

- **数据集/场景**：论文摘要未列出具体数据集名称，仅提及“在多个LLM和下游任务上”进行实验。结合领域常识，可能涵盖文本分类、问答、摘要等常见NLU/NLG任务。由于信息有限，无法明确其覆盖范围。
- **Benchmark**：未明确定义统一基准，但实验对比了**现有的反向传播跳过方法**（backpropagation-skipping baselines）。
- **对比方法**：摘要直接对比了已有反向传播跳过方法（如随机丢弃、基于梯度范数的选择性反向传播等），但未给出具体名称。另外，TokenDrop本身也兼容各种PEFT方法（如LoRA、Adapter等），文中可能对比了有无TokenDrop的PEFT性能。

### 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等信息。因此无法获知具体算力消耗，仅能从加速效果（1.50×）和显存减少（42.9%）推测其效率。

### 5. 实验数量与充分性

- **实验数量**：根据摘要描述，进行了“一系列实验”（a range of experiments），但未列出具体组数（如不同模型、不同任务、消融实验等）。元数据中仅概括为“在多个模型和任务上”。
- **充分性评估**：由于缺乏详细实验列表（如模型规模、任务类别、超参数变化等），难以判断实验是否覆盖足够场景。但从结果描述看，在显存和速度上均优于基线，且性能无损，说明方法有效。但未提供消融分析（如不同丢弃率的影响、累积选择与单层选择的对比等）和扩展到更大模型的验证，因此**实验充分性有限，存在信息缺口**。

### 6. 论文的主要结论与发现

- TokenDrop能有效降低LLM微调的激活内存占用（最高 **42.9%**）并加速训练（最高 **1.50×**），同时保持与完整反向传播相当的精度。
- 相比现有的反向传播跳过基线方法，TokenDrop在内存和速度上均表现更优。
- 该方法是轻量级、即插即用的，可与任何PEFT方法结合使用，进一步突破微调效率瓶颈。

### 7. 优点：方法或实验设计上的亮点

- **方法轻量无梯度**：利用前向传播中的残差更新幅度作为重要性指标，无需额外计算梯度，开销可忽略。
- **累积与懒惰选择设计**：通过跨层累积和延迟选择，在内存受限条件下也能获得全局重要性信息，提升选择准确性并保持梯度连续性。
- **即插即用**：与现有PEFT方法互补，易于集成到现有训练框架中，无需大幅修改模型结构。
- **实验指标清晰**：提供了显存减少百分比和训练加速比等具体量化结果，直观展示收益。

### 8. 不足与局限

- **实验细节不完整**：未公开数据集列表、模型规模（如7B、13B、70B等）、丢弃率范围等重要超参数，读者难以复现或判断泛化性。
- **缺乏消融与敏感性分析**：未说明不同丢弃率下精度-效率权衡曲线，也未分析累积选择与懒惰调度的独立贡献。
- **可能存在的偏差风险**：重要性估计基于残差更新幅值，可能对长尾、稀有token不友好，导致潜在精度损失在特定任务（如关键实体识别）中被放大，但论文未讨论。
- **应用限制**：跳过不重要token的反向传播会破坏梯度的完整性，尽管累积选择缓解了梯度连续性问题，但在极小批大小或极短序列上可能效果不佳；方法依赖于Transformer结构的残差性质，对其他架构（如RNN）不适用。
- **资源算力未披露**：无法评估方法本身的额外计算开销是否可忽略，也无法与其他方法的实际能耗对比。

（完）
