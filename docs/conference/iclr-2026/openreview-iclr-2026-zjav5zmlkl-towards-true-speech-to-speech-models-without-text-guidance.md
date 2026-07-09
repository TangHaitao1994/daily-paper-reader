---
title: Towards True Speech-to-Speech Models Without Text Guidance
title_zh: 迈向无需文本引导的真正语音到语音模型
authors: "Xingjian Zhao, Zhe Xu, Luozhijie Jin, Yang Wang, Hanfu Chen, Yaozhou Jiang, Ke Chen, Ruixiao Li, Mingshu Chen, Ruiming Wang, Wenbo Zhang, Qinyuan Cheng, Zhaoye Fei, Shimin Li, Xipeng Qiu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=zjaV5zmlkl"
tags: ["query:speech-model"]
score: 9.0
evidence: 无需文本中介的真正语音到语音模型，提升语音交互质量
tldr: 提出一种无需文本引导的真正语音到语音大语言模型，通过模态层分割和冻结预训练策略，直接理解并生成语音，保留副语言信息。实验表明，该模型在口语问答任务上达到领先水平，同时保持了预训练文本LLM的推理能力，为自然高表现力的语音交互提供了新方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 传统口语对话系统依赖文本中介，丢失副语言信息，限制表达力。
method: 提出基于模态层分割架构和冻结预训练策略的真正语音到语音大语言模型。
result: 在口语问答等任务上达到最先进性能，并保留了文本LLM的推理能力。
conclusion: 该模型为高表现力语音交互开辟了新途径，消除了文本瓶颈。
---

## Abstract
Spoken dialogue systems often rely on cascaded pipelines that transcribe, process, and resynthesize speech. While effective, this design discards paralinguistic cues and limits expressivity. Recent end-to-end methods reduce latency and better preserve these cues, yet still rely on text intermediates, creating a fundamental bottleneck. We present a true speech-to-speech large language model that directly understands and generates speech without relying on text guidance. Our approach combines a modality-based layer-splitting architecture with a frozen pre-training strategy, preserving the reasoning and knowledge of pretrained text LLMs while adding native speech capabilities. Experiments show that our model achieves state-of-the-art results in spoken question answering and delivers comparable speech-to-speech performance relative to existing text-guided systems, while still maintaining competitive text performance. By narrowing the gap between text-guided and direct speech generation, our work establishes a new paradigm for expressive and efficient end-to-end speech interaction. We will release our code and models to support further research in true speech-to-speech foundation models.

---

## 论文详细总结（自动生成）

# 论文总结：Towards True Speech-to-Speech Models Without Text Guidance

## 1. 论文的核心问题与整体含义
- **研究动机**：传统的口语对话系统多采用级联式（ASR→NLU→TTS）流水线，虽能工作，但会丢失语速、语调、情感等副语言信息（paralinguistic cues），制约了表达的自然度和丰富性。
- **核心问题**：现有端到端方法虽然减少了延迟并更好地保留了上述线索，但**仍然依赖文本作为中间表示**（text intermediates），这形成了一个根本性的瓶颈——语音理解与生成未能完全脱离文本模态，限制了模型对语音原生特征（如韵律、停顿、情绪）的直接建模能力。
- **整体含义**：本文提出一种**真正的语音到语音大语言模型（True Speech-to-Speech LLM）**，无需任何文本引导，直接以语音作为输入和输出，旨在消除文本中间层带来的信息损失，将大语言模型的推理能力直接拓展到语音域，从而建立更高表现力和更高效的端到端语音交互新范式。

## 2. 论文提出的方法论
- **核心思想**：在保留现有成熟文本大语言模型（Text LLM）强大的推理与知识的基础上，为其“添加”原生语音理解与生成能力，同时避免损害文本LLM已有的能力。
- **关键技术细节**（依据摘要提炼）：
  - **模态基的层分割架构（Modality-based Layer-splitting Architecture）**：将模型的不同层划分给不同模态处理，如一部分层负责语音特征的编码/解码，另一部分层保留文本语义推理，实现模块化的多模态融合。
  - **冻结预训练策略（Frozen Pre-training Strategy）**：在训练语音能力时，保持预训练文本LLM的参数冻结（或部分冻结），只训练新引入的语音处理模块，从而保证原LLM的推理和知识不退化，同时让语音模块学会与冻结的文本骨干对齐。
- **算法流程**（推测）：输入语音 → 语音编码器（可训练） → 模态层分割的LLM骨干（冻结文本层与语音适配层交叉） → 语音解码器（可训练） → 输出语音。整个过程无文本转录或文本中间向量。

## 3. 实验设计
- **所用数据集与基准**：摘要明确指出**在口语问答（Spoken Question Answering）任务上取得了最先进的结果**，但未给出具体的数据集名称（如 SQuAD 语音版、NMSQA 等）。同时提到与现有文本引导系统（text-guided systems）进行了语音到语音性能的对比。具体对比方法和基准名称在摘要中未展开。
- **对比方法**：论文将自身与“现有文本引导系统”对比，暗示至少比较了基于 ASR+LLM+TTS 的级联方案或端到端但中间使用文本的模型。由于仅有摘要，无法确定是否与其它无文本语音模型（如 SpeechGPT、AudioPaLM 等）进行了全面对比。

## 4. 资源与算力
- 所提供的摘要文本**未明确说明**训练所用 GPU 型号、数量、训练时长、浮点运算量等算力信息。因此无法从现有材料中总结资源消耗情况。

## 5. 实验数量与充分性
- **实验幅度推测**：摘要仅提及两类结果——口语问答 SOTA 和与文本引导系统的可比语音到语音表现——暗示至少包含这两种核心评测实验。可能还包含消融实验（如冻结策略的有效性对比、模态层分割设计的选择），但摘要未着墨。
- **充分性评价**：由于信息极其有限，**无法评判实验的充分性、客观性和公平性**。缺少对数据集规模、具体指标、多轮对话、鲁棒性测试等细节，难以确认结论的可靠性。若要客观评价，需阅读完整论文。

## 6. 论文的主要结论与发现
- 该方法成功构建了一个无需文本引导的真正语音到语音大语言模型，在**口语问答任务上达到最先进水平**。
- 该模型能够生成与现有文本引导系统性能相当的语音，同时**保持了预训练文本 LLM 的竞争性文本表现**，说明没有因为添加语音能力而牺牲文本推理能力。
- 通过缩小文本引导与直接语音生成之间的差距，为**高表现力、高效率的端到端语音交互开辟了新范式**，证明去除文本瓶颈是可行的。

## 7. 优点（方法或实验设计亮点）
- **概念创新**：首次明确定义并实现了“真正”的语音到语音模型，彻底绕开文本中间表征，为保留副语言信息提供了架构基础。
- **实用设计**：采用冻结策略保护了昂贵预训练文本 LLM 的知识，使得模型能够快速继承已有大模型的推理能力，降低了纯语音训练的门槛。
- **模态层分割**：通过将模型分层服务于不同模态，提供了一种模块化扩展多模态能力的新思路，具有可解释性和可扩展性。
- **性能均衡**：在获得语音 SOTA 的同时仍保持文本性能，展示了多模态融合的稳定性。

## 8. 不足与局限
- **信息不完整**：目前只能依据摘要，无法评估实验覆盖的广度（如噪音环境、多语言、情感感知任务等）和深度（如长序列生成、对话一致性等）。
- **实验偏差风险**：口语问答可能针对特定口音或清晰语音，实际场景中的副语言信息利用效果未知；对比对象仅提及“现有文本引导系统”，可能未与最新无文本语音模型充分比较，存在选择性偏差。
- **应用限制**：模型可能对计算资源要求较高，且缺乏对实时性、低资源场景的讨论；冻结策略虽保护文本能力，但也可能限制语音模块与文本交互的深度，影响复杂推理场景下的协同效果。
- **可解释性缺失**：未说明如何确保模型真正理解了副语言信息而非浅层统计，缺乏对语音模态内部表征的分析。

（完）
