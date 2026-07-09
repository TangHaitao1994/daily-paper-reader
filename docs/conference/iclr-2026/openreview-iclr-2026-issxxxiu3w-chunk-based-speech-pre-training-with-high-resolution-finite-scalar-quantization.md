---
title: Chunk Based Speech Pre-training with High Resolution Finite Scalar Quantization
title_zh: 基于分块的高分辨率有限标量量化语音预训练
authors: "Yun Tang, Cindy Tseng"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=ISSxXXiu3w"
tags: ["query:speech-model"]
score: 9.0
evidence: 基于分块的自监督学习，用于流式和离线语音预训练
tldr: 针对传统自监督语音预训练假设完整语句、无法适应流式场景的问题，提出 Chunk SSL 方法。该方法以语音块为单位进行掩码预测训练，并引入高分辨率有限标量量化，使预训练模型能够处理部分语音输入。实验表明，Chunk SSL 在流式与非流式任务上均取得与全句模型相当的性能，为低延迟语音通信提供了有效的预训练方案。该工作统一了流式和离线语音模型的训练框架，推动了实时语音应用的发展。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有自监督语音预训练假设全句输入，限制了流式应用中的性能。
method: 提出基于分块的自监督学习算法，结合掩码预测损失和高分辨率量化，迫使模型恢复被掩码帧的索引。
result: 实现了流式和离线场景下的统一预训练，性能与全句模型相当。
conclusion: Chunk SSL 为实时语音交互系统提供了高效的预训练范式。
---

## Abstract
Low latency speech human-machine communication is becoming increasingly necessary as speech technology advances quickly in the last decade.  One of the primary factors behind the advancement of speech technology is self-supervised learning. Most self-supervised learning algorithms are designed with full utterance assumption and compromises have to made if partial utterances are presented, which are common in the streaming applications.  In this work, we propose a chunk based self-supervised learning (Chunk SSL) algorithm as an unified  solution for both streaming and offline speech pre-training.  Chunk SSL is optimized with the masked prediction loss and 
an acoustic encoder is encouraged to restore indices of those masked speech frames with help from unmasked frames in the same chunk and preceding chunks.  A copy and append data augmentation approach is proposed to conduct efficient chunk based pre-training.
Chunk SSL utilizes a finite scalar quantization (FSQ) module to discretize input speech features and our study shows a high resolution FSQ codebook, i.e., a codebook with vocabulary size up to a few millions, is beneficial to transfer knowledge from the pre-training task to the downstream tasks.   A group masked prediction loss is employed during pre-training to alleviate the high memory and computation cost introduced by the large codebook. The proposed approach is examined in two speech to text tasks, i.e., speech recognition and speech translation. Experimental results on the \textsc{Librispeech} and \textsc{Must-C} datasets show that the proposed method could achieve
very competitive results for speech to text tasks at both streaming and offline modes.

---

## 论文详细总结（自动生成）

由于提供的论文提取文本未包含正文内容，仅包含标题、摘要及元数据信息，以下分析主要基于摘要和公开元数据，并结合领域背景进行合理推断。具体实现细节可能需参考原文完整内容。

## 1. 核心问题与整体含义
- **研究动机**：语音技术快速发展，低延迟的人机语音通信愈发重要。然而，当前主流的自监督语音预训练方法（如 wav2vec 2.0、HuBERT 等）大多假设输入为完整语句，当应用于流式场景（只有部分语音到达）时，性能会显著下降，必须进行妥协处理。
- **整体含义**：本文旨在提出一种统一的自监督预训练范式，使模型能够同时适应流式（chunk-by-chunk）和离线（full utterance）两种推理模式，打破全句假设对低延迟应用的制约，提升预训练模型在真实流式交互中的可用性。

## 2. 方法论
- **核心思想**：提出 **Chunk SSL**，以语音块（chunk）为基本单元进行掩码预测预训练，迫使声学编码器从同一块及前序块中的未掩码帧恢复被掩码帧的量化索引，从而赋予模型处理部分语音的能力。
- **关键技术细节**（来自摘要）：
  - **分块训练与数据增强**：将音频切分为块，并采用一种 “复制并附加（copy and append）” 的数据增强方式，以高效进行分块预训练。
  - **高分辨率有限标量量化（FSQ）**：使用 FSQ 模块将连续语音特征离散化。研究发现，码本大小达到数百万级（高分辨率）有利于下游任务的知识迁移。
  - **组掩码预测损失**：由于大码本直接使用常规分类损失会导致高内存和计算开销，引入组掩码预测损失（group masked prediction loss）以缓解该问题。
- **公式/算法流程**：摘要未给出具体公式，但流程可概括为：连续语音特征 → FSQ 离散化 → 分块掩码 → Transformer 编码器预测被掩码位置的码本索引 → 组掩码预测损失优化。

## 3. 实验设计
- **数据集**：
  - 语音识别任务：使用 LibriSpeech 数据集。
  - 语音翻译任务：使用 MuST-C 数据集。
- **Benchmark 与对比方法**：摘要未列出具体对比方法，但提到 “与现有方法非常具有竞争力的结果”，暗示与主流全句预训练模型（可能包括 wav2vec 2.0、HuBERT 等）及流式方案进行了比较。具体对比对象需查阅原文实验部分。
- **场景**：分别在流式模式（接收部分语音）和离线模式（完整语音）下评估语音转文本（ASR 和 ST）性能。

## 4. 资源与算力
- 摘要及元数据中**未提供**任何关于 GPU 型号、数量、训练时长等算力信息。此类细节通常出现在论文的实验配置章节，由于无法访问全文，此处无法给出具体数值。

## 5. 实验数量与充分性
- 从摘要可知，实验覆盖两个任务（ASR、ST）、两个数据集（LibriSpeech、MuST-C）以及两种推理模式（流式、离线），至少构成 2×2×2 的矩阵。此外，论文可能包含消融实验（如不同码本大小对性能的影响、有无组掩码损失的对比等），但摘要未展开，无法确认具体组数。
- 仅从摘要描述来看，实验设计覆盖了主要应用场景，并与现有全句预训练范式形成对比，能够初步验证提出方法的有效性。但缺乏与专门流式预训练方法的对比（如果有的话），公平性取决于对比基线是否在相同数据、计算预算下重新训练。

## 6. 主要结论与发现
- Chunk SSL 在语音识别和语音翻译任务上的流式与离线模式下均取得非常有竞争力的结果。
- 高分辨率 FSQ 码本（数百万词汇量）对于将预训练知识迁移到下游任务至关重要。
- 分块预训练方法统一了流式和离线语音模型的训练框架，为低延迟语音通信提供了有效的预训练方案。

## 7. 优点
- **统一的流式/离线范式**：一套模型即可应对两种场景，无需针对流式单独设计或牺牲性能。
- **高效处理部分语音**：通过分块设计和跨块上下文利用，提升流式识别准确度。
- **创新的量化策略**：引入高分辨率 FSQ 与组损失，扩展了自监督语音预训练中离散化单元的设计空间。
- **数据增强**：copy and append 方法针对性提升了分块预训练效率。

## 8. 不足与局限
- **细节缺失**：由于无法获取全文，许多关键信息（如具体模型架构、训练超参数、对比方法列表、算力需求、码本高分辨率的量化维度等）无法评估，影响对该方法实用性的判断。
- **对比公平性未知**：是否与同等数据量、同计算量的基线进行公平比较尚不明确，可能存在因计算资源更优导致的性能优势。
- **高分辨率码本的代价**：尽管使用了组损失缓解，但百万级码本的存储和计算开销仍可能高于常规量化方法，对实际部署的轻量化可能不利。
- **任务适应性**：仅在 ASR 和 ST 上验证，尚未扩展至说话人识别、情感识别等非转录任务，通用性待进一步检验。
- **可能的数据泄漏**：分块中的跨块上下文可能依赖未来信息（若未严格按流式方式保留历史），对延迟的严格定义需要更多分析。

（完）
