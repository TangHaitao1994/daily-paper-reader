---
title: "UniTTS: Towards End-to-End Speech Synthesis with Joint Acoustic-Semantic Modeling"
title_zh: "UniTTS: 面向端到端语音合成的联合声学-语义建模"
authors: "Rui Wang, Qianguo Sun, Junlong Wu, Tianrong Chen, Zhiyun Zeng, Yiyan Qi"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=YsrswIqSZ9"
tags: ["query:speech-model"]
score: 9.0
evidence: 联合声学-语义建模克服LLM对齐差距提升TTS质量
tldr: 该工作针对基于大语言模型的语音合成中语义与声学信息难以对齐的问题，提出蒸馏多码本编解码器DistilCodec与联合建模框架UniTTS，在统一模型中融合多尺度声学与语义表示。实验表明该方法能有效缓解信息断层，生成更自然、高质量的语音。其贡献在于为端到端TTS提供了一种简洁的对齐方案，无需分立模块即可提升合成保真度。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 基于LLM的TTS中语义与声学信息无法充分对齐，限制了音频信息访问。
method: 提出DistilCodec蒸馏多码本编解码器，并与UniTTS框架联合建模声学与语义。
result: 实现了更优的声学-语义对齐，提升了合成语音的自然度与质量。
conclusion: 联合建模是提升LLM-based TTS性能的有效途径，简化了系统设计。
---

## Abstract
Recent advancements in multi-codebook neutral audio codecs, such as Residual Vector Quantization (RVQ) and Group Vector Quantization (GVQ), have significantly advanced text-to-speech (TTS) systems based on large language models (LLMs), whose exceptional capabilities in discrete token modeling have garnered significant attention within the speech processing community. However, since semantic and acoustic information cannot be fully aligned, a significant drawback of these methods when applied to LLM-based TTS is that large language models may have limited access to comprehensive audio information. To address this limitation, we propose DistilCodec and UniTTS, which collectively offer the following advantages: 1) DistilCodec distills a multi-codebook audio codec into a single-codebook codec with 32,768 codes, achieving near 100\% codebook utilization. 2) By avoiding semantic alignment constraints, DistilCodec enables the incorporation of extensive high-quality unlabeled audio—such as audiobooks with sound effects and musical segments—during training, thereby enhancing data diversity and general applicability. 3) Leveraging the comprehensive audio information modeling of DistilCodec, we integrated three key tasks into UniTTS's pre-training framework: audio modality autoregression, text modality autoregression, and speech-text cross-modal autoregression. This allows UniTTS to accept interleaved text and speech/audio prompts while substantially preserving LLM's text capabilities. 4) UniTTS employs a three-stage training process: Pre-Training, Supervised Fine-Tuning (SFT), and Alignment. Experiments demonstrate that DistilCodec effectively resolves codebook collapse in large, single-codebook settings. Building on this, UnitTTS demonstrates remarkable capabilities for zero-shot voice cloning with emotional expression.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：基于大型语言模型（LLM）的语音合成（TTS）系统常采用多码本神经音频编解码器（如残差向量量化 RVQ、分组向量量化 GVQ）将语音离散化，但由于语义信息与声学信息在码本中未能充分对齐，LLM 对完整音频信息的访问受到限制，导致合成语音的质量与自然度存在瓶颈。
- **整体含义**：该工作旨在打破 LLM-based TTS 中声学-语义信息断层，通过设计一个能高效融合多尺度信息的单一码本编解码器，并结合统一的端到端预训练框架，使大语言模型可以直接建模包含丰富声学细节的语音，从而无需分立的对齐模块，实现更简洁且更高质量的语音合成，特别是零样本语音克隆与情感表达。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将多码本的音频编解码器“蒸馏”成一个大容量（32,768 个码）的单码本编解码器，并利用该单码本提供的统一表示，让 LLM 在预训练中同时学习音频自回归、文本自回归以及语音-文本跨模态自回归，最终实现能处理交错文本与语音/音频提示的端到端 TTS 模型。
- **关键技术细节**：
  - **DistilCodec**：蒸馏多码本音频编解码器，得到单一码本（含 32,768 个离散码），并实现了接近 100% 的码本利用率，有效避免了大型单码本的码本坍缩问题。由于不施加语义对齐约束，训练时可使用大量无标注的高质量音频（如有声书中的音效和音乐片段），增强了数据多样性与通用性。
  - **UniTTS 框架**：利用 DistilCodec 提供的全面音频信息建模能力，将三项任务融入预训练框架：
    - 音频模态自回归
    - 文本模态自回归
    - 语音-文本跨模态自回归
  - 这种设计使得 UniTTS 能接受交错格式的文本和语音/音频提示，并较好地保留 LLM 的文本能力。
- **训练流程**（三阶段）：
  - 预训练（Pre-Training）：在大规模数据上进行上述三种自回归任务的联合预训练。
  - 监督微调（Supervised Fine-Tuning, SFT）：使用带标签的语音合成数据进行微调。
  - 对齐（Alignment）：进一步对齐模型输出与人耳偏好或特定任务需求。

### 3. 实验设计
- **数据集/场景**：摘要未明确列出具体数据集名称。仅提及使用了大量无标注音频（如带音效和音乐的有声书）训练 DistilCodec，以及用于 UniTTS 三阶段训练的数据。由于信息缺失，无法确定使用的公开基准数据集（如 LJSpeech、LibriTTS、VCTK 等）或场景。
- **Benchmark 与对比方法**：摘要中未提及任何具体的 baseline 或对比方法名称，仅说明实验证明了 DistilCodec 能解决大型单码本的码本坍缩，并且 UniTTS 展示了卓越的零样本语音克隆能力与情感表达能力。推断应与当前主流基于 LLM 的 TTS 方法（如 VALL-E、NaturalSpeech、Mega-TTS 等）进行比较，但无法确认。
- **评测指标**：摘要未列出主观或客观指标（如 MOS、WER、说话人相似度等），但提到“自然度”“质量”等定性描述。

### 4. 资源与算力
- 论文摘要中**未提供任何算力相关信息**，包括 GPU 型号、数量、训练时长或能源消耗等。无法根据现有内容评估其计算成本。

### 5. 实验数量与充分性
- **实验数量**：摘要仅概括性提到“实验证明”了 DistilCodec 的有效性和 UniTTS 的卓越性能，未给出具体实验组数或消融实验的细节。因此无法判断实验的总数量。
- **充分性与客观性**：由于摘要没有提供数据集、基准、消融设置或统计检验，无法评估实验是否充分、客观或公平。可能需要阅读完整论文以判断。

### 6. 论文的主要结论与发现
- DistilCodec 成功将多码本编解码器蒸馏为高利用率的单码本编解码器，解决了大容量单码本常见的码本坍缩问题。
- 去除语义对齐约束可以引入更多无标注音频数据，增强模型泛化能力。
- UniTTS 通过联合三种自回归任务，能在保留 LLM 文本能力的同时，实现包含情感表达的零样本语音克隆，显著提升了合成语音的自然度。

### 7. 优点：方法或实验设计上的亮点
- **码本设计创新**：蒸馏策略获得高利用率单码本，兼顾编解码效率与声学保真度。
- **数据利用灵活**：摆脱语义对齐依赖，可直接利用海量未标注音频，数据扩展性高。
- **统一建模范式**：将文本、音频自回归及跨模态自回归统一在一个 LLM 预训练中，支持交错输入，系统简洁且功能丰富。
- **训练流程清晰**：三阶段训练（预训练→SFT→对齐）遵循当前大模型训练的最佳实践。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **信息严重缺失**：由于仅提供摘要，关键技术细节（如蒸馏损失函数、UniTTS 具体架构）、实验设置（数据集、基准模型、评测指标）、超参数、资源消耗等均未知，无法评估其真实有效性。
- **实验覆盖可能不足**：未提及有无在多种语言、多说话人、噪声环境或低资源条件下的测试，泛化能力未知。
- **潜在偏差风险**：使用大量有声书数据可能引入特定领域（音效、朗读风格）的偏好，合成结果或偏向该类数据分布，在通用对话或专业场景下的表现存疑。
- **应用限制**：大容量单码本（32K）和解码器实时性未讨论，可能对推理延迟和计算资源有较高要求；模型保留了 LLM 文本能力，但交错输入的安全性、幻觉控制等未涉及。

（完）
