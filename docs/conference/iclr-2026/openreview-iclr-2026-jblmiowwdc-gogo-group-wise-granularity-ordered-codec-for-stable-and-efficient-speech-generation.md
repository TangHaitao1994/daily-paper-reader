---
title: "Gogo: Group-wise granularity-ordered codec for stable and efficient speech generation"
title_zh: Gogo：面向稳定高效语音生成的分组粒度排序编解码器
authors: "Weidong Chen, Helen M. Meng, Xixin Wu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JbLmIoWwDC"
tags: ["query:speech-model"]
score: 9.0
evidence: 提出分组粒度排序编解码器，实现低令牌率的高效语音生成
tldr: 针对语音语言模型需要既能捕捉高层语义又能保留声学细节的离散表示，本文提出 Gogo，一种按粒度排序的分组式语音编解码器，将每组帧量化为从粗到细的令牌。基于此，构建了 GogoSpeech 两阶段生成模型，先以极低令牌率构建粗粒度语音骨架，再补充细节。实验证明，该方法在保证感知质量的同时，显著提高了语音生成的稳定性和效率。该工作为高效语音生成提供了新的编解码设计思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 当前语音编解码器难以同时满足高层语义建模和声学细节保留的需求，且生成效率待提升。
method: 设计分组粒度排序编解码器，将每组帧量化为从粗到细的排列，粗令牌编码高层信息，细令牌恢复细节。
result: 实现了极低令牌率的语音生成，模型稳定且高效，保持了高感知质量。
conclusion: Gogo 及其两阶段生成框架为语音语言模型提供了高效的离散化与生成方案。
---

## Abstract
Current speech language models require their core component, the speech codec, to discretize continuous speech signals into tokens that not only capture high-level cues for autoregressive modeling but also preserve sufficient acoustic details for perceptual quality. To address this need, we propose Gogo, a group-wise granularity-ordered codec that quantizes each group of frames into tokens arranged from coarse to fine, where coarse tokens encode high-level abstractions and fine tokens progressively recover low-level details. Building on the granularity-ordering property of Gogo, we introduce GogoSpeech, a two-stage speech language model that performs speech generation by first constructing a coarse speech backbone at an extremely low token rate and then enriching the backbone with fine-grained acoustic details. Considering the inherently non-uniform information distribution in speech signals, we further design a Group Relative Policy Optimization (GRPO)-trained token allocator that adaptively allocates token budgets to groups based on group-wise complexity. Experimental results demonstrate that Gogo delivers state-of-the-art reconstruction performance across most metrics at a token rate of 47. Moreover, evaluations on zero-shot text-to-speech tasks show that GogoSpeech enables efficient generation by adaptively reducing the average token rate, and attains state-of-the-art results in long-form speech generation.

---

## 论文详细总结（自动生成）

根据所提供的论文元数据，以下是针对论文《Gogo: Group-wise granularity-ordered codec for stable and efficient speech generation》的详细中文总结。由于仅获取到论文标题、摘要及简短核心要点，未包含正文细节，以下分析严格依据现有信息展开，缺失部分将如实说明。

### 1. 论文的核心问题与整体含义
- **核心挑战**：当前的语音语言模型依赖语音编解码器将连续语音离散化为令牌，但这要求编解码器同时具备两种能力：
  - **高层建模**：提取语义级抽象特征，用于自回归生成。
  - **细节保留**：保留足够的声学细节以确保高感知质量。
- **矛盾与不足**：现有方法难以完美平衡“高层语义捕捉”与“低级细节保留”，且生成效率（如令牌率）有待提升，限制了模型在长语音生成等场景中的稳定性与速度。
- **本文动机**：提出一种新型编解码器，从结构设计上解耦不同粒度的信息，从而让后续语言模型能以更低的成本、更高的稳定性完成高质量语音生成。

### 2. 论文提出的方法论
- **核心思想：分组粒度排序量化**
  - 将语音帧进行分组（Group），对每组内量化出一组**从粗到细排列的离散令牌**。
  - 粗粒度令牌（Coarse Tokens）编码高层抽象语义；细粒度令牌（Fine Tokens）逐步补充低级声学细节，形成粒度有序的表示。
- **关键技术细节**（基于摘要描述，无公式原文）：
  - **Gogo编解码器**：实现上述分组量化机制，使每组输出具有天然的多尺度结构。
  - **GogoSpeech两阶段生成框架**：
    1. **粗骨架构建**：先以极低的令牌率生成粗粒度的语音骨干，仅使用高层信息。
    2. **细节丰富**：在骨干基础上补充细粒度细节，保证感知质量。
  - **自适应令牌分配器**：
    - 考虑到语音信号的信息分布不均匀，设计了一个基于 **Group Relative Policy Optimization（GRPO）** 训练的令牌分配器。
    - 该分配器根据每组帧的复杂度，**动态分配不同的令牌预算**，进一步提升整体效率。

### 3. 实验设计
- **对实验细节的信息说明**：本文提供的摘要和核心描述中**未列出具体数据集名称、基准（Benchmark）标准及对比的具体方法**，仅提供了结论性描述。
- **根据摘要判断涉及的实验场景**：
  - **语音重建任务**：在令牌率为 47 的条件下评估重建性能，声称在大多数指标上达到最先进水平。
  - **生成任务**：在零样本文本转语音（Zero-shot TTS）任务上验证，突出**长语音生成（Long-form Speech Generation）**场景下的效果。
- **公平性判断**：由于缺少对比方法列表，无法判断实验是否完全客观公平，但摘要声称达到了“最先进”结果，暗示与当时主流方法进行了比较。

### 4. 资源与算力
- **文中未提供相关信息**。提供的论文摘要及元数据中，没有提及使用的GPU型号、GPU数量或训练时长等算力消耗细节。

### 5. 实验数量与充分性
- **实验数量**：从摘要推断，实验至少包含**语音重建曲线**、**零样本TTS传统性能**及**长语音生成专项**三组评估；此外，设计了自适应令牌分配器（GRPO训练）可能伴随消融实验（未明确列出）。但具体的实验组数、图表数量、用户研究等无法得知。
- **充分性评估**：摘要强调达到了最先进性能，并提到了自适应降低平均令牌率以提高效率，推测实验设计覆盖了主要的应用维度（重建、生成、效率）。但鉴于缺乏细节，难以判定是否全面覆盖了鲁棒性、多语言、不同条件等场景，因此其充分性只能持保留意见。

### 6. 论文的主要结论与发现
- **Gogo编解码器**能够在令牌率仅为 47 的情况下，实现卓越的重建性能（多数指标达 SOTA）。
- 基于Gogo的**GogoSpeech两阶段模型**实现了稳定且高效的生成：能自适应降低平均令牌率，在保持高感知质量的同时显著提升生成效率。
- 在**长语音生成**这一挑战性任务上，该方法取得了最先进的成果，证明了粒度排序设计对生成稳定性的帮助。

### 7. 优点（亮点总结）
- **创新的编解码结构**：按组实现从粗到细的粒度排序，从设计上解耦了语义与细节，适合后续分级建模。
- **高效生成范式**：两阶段生成策略（先骨架后细节）以极低令牌率完成核心架构，极大提升了生成效率与长序列生成的稳定性。
- **自适应资源分配**：引入GRPO训练的令牌分配器，可根据语音复杂度动态调整预算，避免浪费，展现了非均匀建模的智能性。
- **结论优秀**：在长语音生成等关键任务中获得SOTA，证明该方法兼顾了质量与效率。

### 8. 不足与局限
- **信息缺失导致的评估局限**：
  - 由于未提供正文，**无法评估其方法的具体局限**，例如分组边界处的处理、对噪声数据的鲁棒性、客观评测之外的自然度主观评分等。
  - **实验覆盖未知**：未提及多语言、多说话人、极端声学环境下的性能，无法判断其通用性。
- **潜在局限性（基于常识推测，非原文明确写出的缺陷）**：
  - 粒度排序的量化可能增加编解码器设计的复杂度，并导致训练上的挑战。
  - 自适应令牌分配虽然高效，但GRPO训练本身可能带来额外的策略网络开销与训练不稳定性。
- **文献缺失**：未给出与哪些最新编解码器（如SoundStream、EnCodec、SpeechTokenizer等）的详细对比，无法准确判断优势幅度。

（完）
