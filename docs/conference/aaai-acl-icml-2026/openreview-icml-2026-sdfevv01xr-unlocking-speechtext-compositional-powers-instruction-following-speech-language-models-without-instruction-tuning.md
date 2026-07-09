---
title: "Unlocking Speech–Text Compositional Powers: Instruction-Following Speech Language Models without Instruction Tuning"
title_zh: 解锁语音-文本组合能力：无需指令微调的遵循指令语音语言模型
authors: "Congrui Du, Yang Zhang, Kaizhi Qian, Shiyu Chang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3a4b668fb12bcf010b55515b4e3ad9bb17cdfb17.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 无需指令微调，仅通过语音预训练获得遵循指令的语音语言模型
tldr: 针对语音语言模型指令微调成本高、难以扩展的问题，提出SpeechCombine，无需指令微调，仅通过一轮语音预训练便赋予模型遵循指令和语音-文本组合的能力，大幅简化训练流程。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音语言模型需要大规模合成语音指令微调数据集，由于语音序列长，难以扩展。
method: 提出SpeechCombine，通过精心设计的语音预训练策略，使模型在单一预训练阶段学习语音-文本组合能力，无需额外指令微调。
result: SpeechCombine在多个指令遵循和语音-文本任务上表现出色，媲美经指令微调的模型。
conclusion: 该工作证明了仅靠预训练即可解锁语音模型的指令遵循能力，为高效语音大模型训练开辟新路径。
---

## Abstract
Instruction tuning for speech language models (SLMs) is substantially more challenging than for text-based large language models (LLMs), as it requires learning a new modality and a wide range of speech-specific instructions in addition to those supported by text LLMs. Existing SLM training approaches largely replicate the text LLM training paradigm by synthesizing large-scale speech pre-training and instruction-tuning datasets. However, this strategy is difficult to scale, since speech sequences are significantly longer than text sequences.
In this paper, we propose SpeechCombine, an instruction-following speech language model trained **without any instruction tuning**, using only a single round of speech pre-training on as little as 30k hours of speech data. Starting from a text LLM base model, we perform continuous pre-training on speech utterances to obtain a speech-adapted model, and then directly combine its weights with the weight difference between the instruction-tuned and base versions of the text LLM.
Our results show that this simple combination strategy not only preserves the knowledge and capabilities of the original text LLM, but also effectively transfers them to the speech domain. These findings suggest a new direction for SLM training that avoids reliance on massive volumes of speech data.

---

## 论文详细总结（自动生成）

好的，我将基于提供的论文元数据和摘要，生成详细的中文总结。

### 1. 研究动机与核心问题
- **核心问题**：现有语音语言模型（SLM）的训练范式高度依赖大规模、多样化的合成语音指令微调数据集，该模式难以扩展。
- **根本原因**：
  - 语音序列显著长于文本序列，导致合成语音指令数据的存储与计算开销巨大。
  - 语音指令微调需要同时学习新模态（语音）和大量语音专用指令，训练成本远高于纯文本指令微调。
- **研究目标**：探索一种无需任何指令微调，仅通过一轮语音预训练即可赋予模型遵循指令和语音-文本组合能力的方法，从根本上绕开大规模语音指令数据合成与微调的瓶颈。

### 2. 方法论
- **核心思想**：将“语音理解能力”与“遵循指令的能力”解耦。前者通过语音预训练获得，后者则复用文本大语言模型（LLM）已有的指令跟随能力，利用**权重空间内的算术组合**进行能力融合。
- **技术流程（SpeechCombine）**：
  1. **基座准备**：选定一个文本LLM（基座版）及其对应的指令微调版文本LLM。
  2. **计算指令向量**：通过减法得到文本LLM的“指令遵循能力向量”：`ΔW_inst = W_instructed_text - W_base_text`。
  3. **语音适应训练**：在纯语音-文本数据（如自动语音识别数据）上对基座文本LLM进行连续预训练，得到一个已适应语音模态的模型 `W_speech_adapted`。该模型初步具备语音理解能力，但丧失了部分文本指令跟随能力。
  4. **权重直接组合**：将语音适应模型的权重与指令向量直接相加，得到最终模型：`W_SpeechCombine = W_speech_adapted + ΔW_inst`。
- **关键特性**：整个过程**零指令微调**，仅需单轮语音预训练（最少仅3万小时语音数据），无需合成复杂的语音指令对话数据。

### 3. 实验设计
- **训练数据**：3万小时语音数据，用于连续预训练阶段（具体数据来源与构成未在摘要中详述）。
- **评估任务**：涵盖多个**指令遵循任务**和**语音-文本组合任务**（摘要未列出具体基准名称，可能包括语音问答、语音翻译、语音内容总结等需要跨模态指令理解的场景）。
- **对比方法**：与传统的、需要大规模语音指令微调的语音语言模型进行对标，结果表明SpeechCombine能够媲美甚至匹配经指令微调的模型。

### 4. 资源与算力
- **未明确提及**：提供的摘要与元数据中，完全没有提及训练所用的GPU型号、数量、批次大小或具体训练时长。
- 上述关键工程细节需查阅论文全文方可获知。

### 5. 实验数量与充分性
- **实验组数**：摘要未给出具体实验数目。仅从“在多个任务上表现出色”以及核心算法涉及的消融要素（如语音适应程度、指令向量来源等）推断，论文至少应包含了主结果对比、消融实验、不同数据量（如3万小时）的缩放分析等若干组实验。
- **充分性评价（基于现有信息）**：由于缺乏基准名称、消融项和统计指标等细节，无法对实验的完全充分性与客观性做出精准判断。但验证“无需指令微调”这一核心主张并对比传统范式，实验设计在逻辑上是聚焦且公平的。

### 6. 主要结论与发现
- **核心发现**：证明仅靠语音预训练，结合文本LLM指令向量的简便权重算术，就能有效解锁语音模型的指令遵循能力。
- **能力迁移**：此权重组合策略不仅能保留原始文本LLM的绝大部分知识和能力（如常识推理），还能有效将其迁移至语音域，实现零指令微调的语音-文本组合智能。
- **范式启示**：为高效语音大模型训练开辟了新方向，证明可避免对大体积语音指令微调数据集的依赖。

### 7. 优点
- **训练效率极高**：完全移除昂贵的语音指令微调环节，将两阶段训练简化为单轮预训练，极大节省数据合成和计算开销。
- **巧妙的能力复用**：将“指令遵循”显式建模为权重空间中的低秩或可加性向量，巧妙地复用了现有文本LLM的成熟能力，避免了在语音模态从头学习复杂指令。
- **极简实现**：SpeechCombine思想直观、实现简单，无需复杂的数据混合、课程学习或多任务优化，仅涉及模型权重加减，便于其他研究者复现与迁移。

### 8. 不足与局限
- **信息缺失下的评估风险**：由于仅基于摘要，无法评估实验细节是否存在偏差（如对比方法是否公平调优、基准是否全面等）。真实局限需阅读全文。
- **对基座模型的依赖**：该方法高度依赖已存在的高质量指令微调版文本LLM。若没有合适的文本指令模型，则无法获得指令向量。
- **组合策略的鲁棒性**：简单的权重加法可能对语音适应阶段的偏离程度敏感，若语音预训练导致模型权重发生剧烈变化，直接相加的有效性可能下降，论文或未覆盖此类极端情况。
- **任务覆盖上限未知**：未经指令微调，模型在处理高度复杂、需要紧密语音-文本交互的指令（如长篇多轮语音对话、带有副语言指令的任务）时，性能是否始终媲美完全微调的模型仍待验证。

（完）
