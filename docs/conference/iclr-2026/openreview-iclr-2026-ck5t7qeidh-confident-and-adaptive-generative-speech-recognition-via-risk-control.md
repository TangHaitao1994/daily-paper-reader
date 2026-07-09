---
title: Confident and Adaptive Generative Speech Recognition via Risk Control
title_zh: 基于风险控制的自适应生成式语音识别
authors: "Amit Damri, Bracha Laufer-Goldshtein"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ck5T7QeiDh"
tags: ["query:speech-model"]
score: 9.0
evidence: 具有性能保证的自适应ASR错误纠正
tldr: 针对ASR后处理纠错，提出基于风险控制的自适应框架。通过利用置信度分数和Learn then test方法，动态确定每个输入的最佳假设数，在控制错误率降解的同时提升纠正效率。该方法为ASR误差校正提供了可靠且有保障的解决方案，有望提高语音识别的整体准确性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: ASR系统常因声学变化产生转录错误，需后处理校正；现有LLM纠错方法使用固定N-best假设数，缺乏保障。
method: 提出基于风险控制的自适应框架，利用置信度分数和Learn then test动态确定最优假设数。
result: 在控制预期词错误率降解的条件下，有效提升ASR纠错效率与可靠性。
conclusion: 该方法为ASR后处理提供了可解释的自适应纠错策略，有助于增强识别准确性。
---

## Abstract
Automatic Speech Recognition (ASR) systems frequently produce transcription errors due to acoustic variability, which require post-processing correction methods. Recent approaches leverage Large Language Models (LLMs) for generative ASR error correction using N-best hypotheses but rely on fixed set sizes regardless of input complexity and do not provide performance guarantees. We propose an adaptive framework that dynamically determines the optimal number of hypotheses for each input using risk control. This mechanism leverages ASR confidence scores and applies  Learn then test (LTT) to control the expected relative word error rate degradation compared to the best achievable performance for a given model and hypothesis set. Experimental results demonstrate that our approach provides theoretical guarantees with high-probability bounds while matching or exceeding fixed-size correction baselines and requiring fewer hypotheses on average, achieving substantial computational savings under diverse acoustic conditions.

---

## 论文详细总结（自动生成）

由于未能抓取到完整的论文原文（仅获取到 OpenReview 的验证页面），以下分析完全基于所提供的元数据与摘要信息。这些信息足以勾勒论文的核心框架，故按要点展开总结。

---

### 1. 论文的核心问题与整体含义

- **研究动机**：自动语音识别（ASR）系统因声学环境多变（如噪声、口音、语速等），常常产生转录错误，需要后处理纠错。近年来，利用大语言模型（LLMs）进行生成式纠错成为主流，典型做法是使用 ASR 输出的 N-best 假设列表作为上下文，让 LLM 生成更准确的转写。
- **核心问题**：现有方法通常采用**固定数量的 N-best 假设**（例如始终使用 top‑5 或 top‑10），未考虑每个样本的识别难度或不确定性，既缺乏效率，也无性能保障。在困难样本上，固定数量的假设可能不足以恢复正确文本；在简单样本上，过多的假设又会造成计算浪费。
- **整体含义**：论文提出一种**自适应**框架，能够为每个输入动态决定使用多少个假设，同时利用风险控制理论确保纠错后的性能退化被严格约束在可控范围内。该工作旨在实现**高效率、高可靠且具有理论保证**的 ASR 错误校正。

### 2. 论文提出的方法论

- **核心思想**：将确定最优假设数量的问题转化为一个**风险控制**任务。具体地，引入 ASR 系统自带的置信度分数（confidence scores）作为不确定性度量，然后借助 **Learn then Test (LTT)** 方法，在已知纠错模型和假设集的前提下，控制**预期的相对词错误率退化**（即与当前模型可能达到的最佳性能相比，性能损失不超过预设水平）。
- **关键技术细节**（据摘要推演）：
  - 输入：每个语音样本的 ASR 置信度分数（可能是整个假设的平均置信度或各 token 的置信度分布），以及 LLM 纠错模型。
  - 决策函数：基于置信度分数，为一个阈值参数族定义候选策略，每个策略对应一个假设个数（例如，置信度高于某阈值则用少量假设，低于另一阈值则用更多假设）。
  - 风险控制目标：控制期望的用户自定义损失函数（这里是相对词错误率退化），使其不超过允许的界限 α（例如 1%）。
  - LTT 过程：将一部分校准数据用于学习（挑选满足风险约束的策略），利用多重假设检验或保序回归等方法，保证在有限样本下以高概率获得一个有效策略，该策略在测试时能确保预期风险的上限。
  - 动态推断：测试时，对每个语音输入计算置信度分数，按学习到的策略动态选择假设个数，再用对应数量的 N-best 假设输入 LLM 进行纠错。

### 3. 实验设计

- **数据集与场景**：摘要仅提及“多样的声学条件”（diverse acoustic conditions），未给出具体数据集名称（如 LibriSpeech、Common Voice、CHiME 等）。推测实验覆盖了人工加噪或真实噪声场景，以体现自适应方法的鲁棒性。
- **基准与对比方法**：
  - **固定大小基线**：使用固定数量（如 5、10、20 等）N-best 假设的 LLM 纠错方法，可能是现有主流做法。
  - **最佳可达性能**：作为理论上界，可能指针对每个样本若能提前知道最佳假设数量所取得的效果（oracle），或使用全部假设的最优重打分结果。
  - 可能还包含无纠错（原始 1‑best）的基线。
- **评价指标**：词错误率（WER）及其相对退化；平均使用的假设数量（计算开销）。

### 4. 资源与算力

- **文中提及情况**：提供的元数据和摘要**未透露任何计算资源信息**（如 GPU 型号、数量、训练时长等）。由于论文本身聚焦风险控制框架而非训练大规模模型，其计算开销可能主要体现在 LLM 推理上，但具体资源使用量需查阅全文。

### 5. 实验数量与充分性

- **实验组数**：摘要未列出具体实验数量。但考虑到需要验证理论保障、对比固定基线、展示不同声学条件下的节省效果，预计至少包括：① 主要结果（风险控制有效性，各基线对比）；② 分析置信度分数的影响；③ 不同风险参数 α 的敏感性分析；④ 不同噪声类型或信噪比下的表现等。
- **充分性与客观性**：摘要宣称实验显示出“理论保证的高概率界限”，且方法在匹配或超越固定基线的同时使用了更少的假设。这表明实验验证了核心声明。然而，由于缺少数据集、模型规模、随机种子等细节，无法评估实验的**可复现性**与**公平性**。固定基线是否调优、是否用相同 LLM 等关键因素尚不明确。

### 6. 论文的主要结论与发现

- 所提自适应框架能够为 ASR 错误校正提供**可证明的风险控制**，即预期的相对词错误率退化被严格限定在预设水平内（高概率保证）。
- 在满足风险约束的前提下，自适应策略使用的平均假设数量**显著少于**固定大小基线，实现了**实质性的计算节省**。
- 在多种声学条件下，该方法的纠错性能**匹配或优于**固定数量的 N‑best 纠错，同时避免了在简单样本上的冗余计算。
- 该工作展示了将统计风险控制理论融入生成式 ASR 后处理的可行性，赋予系统可解释的可靠性保障。

### 7. 优点

- **理论创新**：首次将风险控制（特别是 LTT）引入 ASR 纠错的假设数量决策，提供性能保证，突破了以往仅凭经验固定集合大小的做法。
- **自适应计算**：根据输入难度动态调整计算量，尤其适合计算资源受限或延迟敏感的场景。
- **可解释的可靠性**：输出的纠错结果伴随一个可控的错误率退化上界，有助于部署在关键任务（如医疗听写、法律速记）中。
- **简洁的切入点**：仅依赖 ASR 置信度分数和校准集，无需修改底层 LLM，易于与现有纠错流水线集成。

### 8. 不足与局限

- **信息缺失导致的评估局限**：基于摘要无法判断具体数据集的大小、多样性及领域覆盖。若仅在小型或单一领域测试，泛化能力存疑。
- **置信度依赖性**：方法高度依赖 ASR 置信度分数的校准质量。若置信度过于乐观或未良好校准，风险控制的有效性可能下降。
- **理论假设**：风险控制框架通常要求数据独立同分布（i.i.d.）或满足交换性，现实中的语音流（如对话）可能存在分布漂移，实际风险可能超过理论界。
- **LLM 模型固定**：框架给出的保证是相对于“给定模型和假设集”的最佳性能。若底层 LLM 能力不足（如无法利用额外假设），则最佳可达性能本身就低，风险控制虽保证不退化太多，但绝对 WER 仍可能不佳。论文未探讨更换更强 LLM 的影响。
- **实验细节未知**：对比的固定基线是否已针对每个数据集单独调优假设数量？若不公平比较，则“匹配或超越”的结论可能偏高。

（完）
