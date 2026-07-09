---
title: Towards real-time BCI for speech with Whisper-based decoding of neural activity
title_zh: 基于Whisper解码神经活动的实时脑机接口语音
authors: "Tommaso Boccato, Michał Olak, Matteo Ferrante"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=gcwzqA8iWa"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用预训练Whisper ASR模型实现高精度神经语音解码
tldr: 提出Whisper-BCI，首次将高分辨率多电极阵列记录与大型预训练ASR模型Whisper集成，用于神经活动语音解码。利用Whisper编码器层学到的音素选择性表示，实现更准确、更泛化的解码性能。该方法为脑机接口语音恢复提供了新范式，有望改善瘫痪患者的沟通能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有神经语音解码器依赖循环模型，数据需求大、泛化差，难以跨会话或个体迁移。
method: 首次将高精度MEA记录与预训练ASR模型Whisper集成，利用其音素选择性表示进行解码。
result: 在脑机接口语音解码任务上取得高效且可泛化的性能。
conclusion: 该工作展示了预训练ASR模型在神经解码中的潜力，为失语患者提供了新的通信恢复途径。
---

## Abstract
Decoding continuous speech from neural activity is a central challenge for brain–computer interfaces (BCIs), with major implications for restoring communication in individuals with paralysis. While recent work has achieved impressive performance using recurrent neural decoders trained on multi-electrode array (MEA) recordings, these models remain brittle, data-hungry, and struggle to generalize across sessions or participants. In this work, we introduce Whisper-BCI, the first neural speech decoder to integrate high-resolution MEA recordings with a large pretrained automatic speech recognition (ASR) model. Our approach leverages interpretability findings showing that Whisper’s encoder layers learn phoneme-selective representations with localized attention. Building on this insight, we adapt Whisper to predict phoneme embeddings from neural signals into the third layer of Whisper's encoder and fine-tune the model end-to-end with a hybrid objective combining CTC loss on phoneme alignments and cross-entropy loss on word tokens. We further introduce domain-informed modifications including windowed self-attention to capture articulatory continuity, day-specific low-rank projections to address non-stationarity and reduce computational complexity, and subject-specific input embedders for cross-subject training. Evaluated on Card et al. and Brain-to-Text '25 data, Whisper-BCI performs on par with or outperforms baselines relative to prior MEA decoders, and achieves cross-subject generalization, opening the door to robust decoding with limited resources. Post-processing with rescoring and grammar-guided correction yields an additional relative improvement, and the use of windowed attention has the potential to significantly reduce latency, enabling near-real-time online decoding. Our results demonstrate that pretrained ASR models can serve as effective language backbones for neural decoding and suggest a scalable path toward foundation models for speech BCIs.

---

## 论文详细总结（自动生成）

# 论文总结：Towards real-time BCI for speech with Whisper-based decoding of neural activity

## 1. 论文的核心问题与整体含义
- **核心问题**：如何从高分辨率神经活动（多电极阵列，MEA）中稳定、泛化地解码连续语音，以服务于脑机接口（BCI）恢复瘫痪患者的沟通能力。
- **研究背景与动机**：
  - 现有基于循环神经网络（RNN）的神经语音解码器存在“脆弱、数据饥渴、难以跨会话或跨被试泛化”的问题。
  - 大型预训练自动语音识别（ASR）模型具有强大的语言表征能力，但尚未被深入整合到高精度神经解码中。
- **整体含义**：本文提出 Whiper-BCI，首次将高分辨率 MEA 记录与预训练 ASR 模型 Whisper 集成，利用其编码器层学到的音素选择性表示，构建更准确、泛化更强且具有近实时潜力的神经语音解码范式，为脑机接口的语音恢复开辟可扩展路径。

## 2. 论文提出的方法论
- **核心思想**：将神经信号映射到 Whisper 编码器已学得的音素嵌入空间，借助预训练语言知识提升解码精度与泛化性。
- **关键技术细节与流程**：
  - **信号注入点**：将神经信号经变换后送入 Whisper 编码器的**第三层**（该层在可解释性研究中被发现具有音素选择性表示与局部化注意力）。
  - **训练目标**：混合损失函数。
    - 连接时序分类（CTC）损失，基于音素对齐。
    - 交叉熵损失，基于词元（word tokens）。
  - **领域适应性修改**（三项关键设计）：
    1. **窗口化自注意力**：将全局自注意力限制在窗口内，以捕捉发音运动的连续性，并**显著降低延迟**，为近实时在线解码提供可能。
    2. **天特异性低秩投影**：针对神经活动随时间的非平稳性，引入按天（session）可调整的低秩投影层，在降低计算复杂度的同时保持适应性。
    3. **被试特异性输入嵌入器**：为每个被试学习独立的输入映射模块，支持**跨被试训练**。
- **整体算法流程**：被试特异性嵌入 → 天特异性低秩投影 → 映射至 Whisper 第三层嵌入 → Whisper 编码器（窗口注意力） → CTC/词元解码 → 后处理重打分与语法纠错。

## 3. 实验设计
- **数据集**：
  - Card et al. 数据集（MEA 录音）
  - Brain-to-Text ’25 数据集
- **基准（Benchmark）**：与先前基于 MEA 的记录训练的解码器基线进行比较（文中未详列具体模型名，但指出为“先前的 MEA 解码器”）。
- **对比方法**：除基线外，包含自身变体（如是否使用窗口注意力、低秩投影等）、跨被试训练方案、以及加入后处理（重打分、语法引导校正）的版本。
- **评估角度**：解码性能、跨被试泛化能力、实时性潜力（窗口注意力带来的延迟降低）。

## 4. 资源与算力
- 论文提供的摘要与元数据中**完全没有提及**所用 GPU 型号、数量、训练时长等算力信息。无法得知具体的硬件开销与训练效率。

## 5. 实验数量与充分性
- **实验组数推断**：基于两个独立数据集、与基线对比、跨被试泛化实验、多种后处理对比、以及窗口注意力的延迟评估，可以推测作者开展了多组实验。此外，方法设计了三种域适应组件，通常暗含着对应的消融研究（例如移除低秩投影等），但摘要中并未明确列出消融实验的表格与数量。
- **充分性与公平性评价**：
  - 在数据集覆盖上较为合理（使用两个外部数据集），并考虑了跨被试泛化这一关键挑战。
  - 对比对象为先前 MEA 解码器，但未在摘要中提供具体基线名称与精度数字，因此难以从现有信息评判对比是否严格公平。
  - 若文中如摘要所言提供了完整的消融实验和性能图表，则实验具有充分性；但基于当前文本无法最终断定。

## 6. 论文的主要结论与发现
- Whisper-BCI 在解码性能上与先前专用 MEA 解码器相当或更优。
- 实现了跨被试的泛化能力，说明预训练语言表征可有效迁移至神经解码任务。
- 窗口化注意力能够显著降低延迟，为近实时在线解码奠定基础。
- 经重打分和语法引导校正可进一步提升准确率。
- **核心结论**：大型预训练 ASR 模型可以充当神经解码的强大语言“骨干”，该工作为通向语音脑机接口的基础模型提供了一条可扩展的路线。

## 7. 优点
- **新颖的集成**：首次将高精度 MEA 信号与大规模预训练 ASR 模型（Whisper）融合，开创了基于大模型的神经语音解码范式。
- **利用可解释性**：基于 Whisper 编码器层的音素选择性这一解释性发现来设计信号接入点，方法具有科学依据。
- **跨被试与泛化设计**：被试特异性嵌入器和天特异性投影考虑了实际应用中的个体差异与信号漂移，增强了方法的实用性。
- **混合训练目标**：结合 CTC（强制对齐）与交叉熵，兼顾了序列对齐与词级语义。
- **实时性考虑**：通过窗口注意力优化延迟，展现出在线系统的潜力。

## 8. 不足与局限
- **算力与部署细节缺失**：未说明模型训练和推理的实际计算开销，限制了对资源受限场景可行性的判断。
- **校准依赖性**：天特异性低秩投影仍需每日校准数据，无法做到完全免校准的稳定解码。
- **实时评估不完整**：文中讨论的是窗口注意力“可能”显著降低延迟，但未展示完整的在线实时系统实现和端到端延迟测量。
- **主体与长期性未知**：仅跨被试泛化未见长期多次会话无校准性能的评估，非平稳性处理的实际边界不清楚。
- **对基线描述的匮乏**：摘要未提供基线方法的具体名称和量化指标，难以判别声称的提升是否一致且稳健。
- **模型依赖**：强依赖 Whisper 的语言分布和特性，对其他语言或非典型语音的泛化能力未探讨。

（完）
