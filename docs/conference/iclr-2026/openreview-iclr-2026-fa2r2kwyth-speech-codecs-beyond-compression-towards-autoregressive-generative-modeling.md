---
title: "Speech Codecs Beyond Compression: Towards Autoregressive Generative Modeling"
title_zh: 超越压缩的语音编解码器：迈向自回归生成建模
authors: "Yazheng Yang, Yao Qiu, Hui Su, Qi Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=FA2R2KwyTH"
tags: ["query:speech-model"]
score: 8.0
evidence: 提出自回归兼容约束改进语音编解码器分词，提升生成建模
tldr: 该论文指出，现有语音编解码器主要面向压缩设计，未考虑自回归语言模型的训练特性，导致序列建模性能次优。为此，提出将自回归兼容约束引入编解码器训练，鼓励生成反映时序一致性的标记序列。实验表明，该方法能生成更适配自回归训练的表示，显著提升语音语言模型的序列建模效果。该工作为语音生成模型提供了更优的离散化方案，推动了语音大模型的训练效率与效果。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有语音编解码器主要面向压缩设计，未考虑自回归语言模型训练特性，导致序列建模性能次优。
method: 提出将自回归兼容约束引入编解码器训练，鼓励生成反映时序一致性的标记序列。
result: 生成的标记序列更好地适配自回归训练，提升了语音语言模型的序列建模效果。
conclusion: 通过自回归感知的离散化，显著改善了语音语言模型的训练与生成性能。
---

## Abstract
Recent advances in speech language models have leveraged discrete speech representations from pretrained codecs to enable scalable training and generation. However, existing codecs are primarily designed for compression, without accounting for the autoregressive nature of language model training. This mismatch leads to suboptimal performance when using compressed speech tokens for sequence modeling. In this work, we revisit speech discretization from the perspective of generative modeling and propose a novel framework to align tokenization with the autoregressive training paradigm. Specifically, we introduce autoregressive-compatible constraints into the codec training process, encouraging token sequences that better reflect the temporal consistency and predictability expected by language models. In addition, we propose using heterogeneous sampling strategy for different layers of audio tokens (semantic versus acoustic) to enhance the alignment between semantic tokens and the speech's textual content. Experiments across multiple benchmarks demonstrate that our approach bridges the gap between audio compression and generative modeling, enabling more effective continued pretraining of existing large language models on audio data. Consistent performance gains across multiple codecs further validate the generalizability of our proposed method.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：当前主流的语音语言模型依赖预训练语音编解码器（codec）输出的离散标记进行可扩展的训练与生成。这类编解码器的设计初衷是音频压缩，重点关注重建质量，其训练目标与语言模型的自回归训练范式并不一致。
- **核心问题**：压缩导向的标记化过程未考虑自回归序列建模所需的时序一致性和可预测性，导致生成的离散序列在语言模型训练中性能次优——即“压缩”与“生成”之间存在目标错位（mismatch）。
- **整体含义**：本文从生成建模的视角重新审视语音离散化问题，旨在使语音标记化与自回归训练范式对齐，从而弥合音频压缩与生成模型之间的鸿沟，并为在音频数据上继续预训练大语言模型提供更有效的表示。

### 2. 论文提出的方法论
- **核心思想**：将**自回归兼容约束**（autoregressive-compatible constraints）集成到语音编解码器的训练过程中，使产生的标记序列本身更符合语言模型训练时对时序依赖的预期。
- **关键技术细节**：
  - **自回归兼容约束**：在编解码器训练时引入额外目标，鼓励输出标记序列具备更好的时序连贯性，从而提高其被预测的难易度和一致性（即生成更适合自回归建模的标记）。
  - **异质采样策略**：针对语音的不同抽象层级采用不同采样方式。具体地，对语义标记（semantic tokens）和声学标记（acoustic tokens）分别设计采样机制，以增强语义标记与语音底层文本内容之间的对齐度，从而提升标记中语言信息的密度。
- **算法流程**（文字描述）：整体上是在传统的编码-量化-解码结构上，于训练阶段叠加自回归兼容损失项，并结合分层/异质采样模块，对编码后的潜在表示进行按层级差异化的量化与约束，最终输出更有利于自回归语言模型训练的离散标记序列。

### 3. 实验设计
- **数据集与场景**：原文摘要未列出具体数据集名称，仅提及在“多个基准”上进行了评估。
- **对比方法**：以多种现有预训练语音编解码器为基线，验证所提方法的通用性与有效性。实验对比了同一语言模型在使用原始编解码器输出标记与使用本文方法调整后标记时的性能。
- **评估基准**：即在标准的语音语言模型生成或评估指标上（如序列建模困惑度、生成质量等），衡量加入自回归兼容约束及异质采样后的改进幅度。

### 4. 资源与算力
- 摘要及提供的元数据中**未明确说明**所使用的GPU型号、数量、训练时长或具体的算力消耗。原文可能包含详细的实验设置部分，但根据现有材料无法给出具体数值。

### 5. 实验数量与充分性
- **实验数量**：从摘要推断，至少进行了以下几类实验：
  - 在**多个基准（benchmarks）** 上的主实验。
  - 在**多种不同编解码器**上的泛化性验证（“across multiple codecs”）。
  - 可能包含消融研究以分离自回归兼容约束和异质采样策略的贡献，但摘要中未具体说明。
- **充分性与公平性**：通过在多个基准和多种编解码器上验证，显示出较好的泛化性，评估维度较为综合。由于未透露具体对比细节与指标数值，无法深入判断实验覆盖的全面性，但设计框架在逻辑上是自洽且公平的。

### 6. 论文的主要结论与发现
- 通过引入自回归感知的离散化方法，显著提升了语音语言模型的训练效果与生成质量。
- 提出的框架能够有效弥合音频压缩与自回归生成建模之间的鸿沟，使基于现有大语言模型的音频持续预训练更加高效。
- 方法在多种编解码器上均取得了一致的性能增益，证明了其良好的通用性和鲁棒性。

### 7. 优点
- **问题洞察深刻**：精准抓住了语音离散化在压缩与生成两种目标间的错位，切入点新颖。
- **方法设计巧妙**：以约束的形式“软化”传统编解码器训练，无需推翻原有架构即可实现与自回归训练的兼容。
- **分层解耦思想**：提出的异质采样策略考虑到了语音中语义与声学信息的不同特性，增强了标记的语义承载能力。
- **通用性强**：不依赖于特定编解码器结构，可扩展至多种预训练模型，实验也印证了这一点。

### 8. 不足与局限
- **细节缺失风险**：由于仅依据摘要和元数据，实际论文中对计算开销、量化误差、超参数敏感性等细节的讨论未知。方法若引入额外约束，可能增加训练复杂度和收敛难度。
- **实验覆盖未知**：缺少具体的数据集、对比基线及指标数值，无法评估其在低资源、多语言或噪声环境下的鲁棒性。
- **应用限制**：增强自回归兼容性可能使表示更加“平滑”或“可预测”，这是否会在一定程度上牺牲重建保真度或保留某些非语言信息（如副语言特征）仍有待权衡，摘要未涉及此取舍。

（完）
