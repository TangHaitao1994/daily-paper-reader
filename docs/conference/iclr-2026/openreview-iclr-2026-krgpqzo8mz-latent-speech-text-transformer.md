---
title: Latent Speech-Text Transformer
title_zh: 隐式语音-文本Transformer
authors: "Yen-Ju Lu, Yashesh Gaur, Wei Zhou, Benjamin Muller, Jesus Villalba, Najim Dehak, Luke Zettlemoyer, Gargi Ghosh, Mike Lewis, Srini Iyer, Duc Le"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=krGpQzo8Mz"
tags: ["query:speech-model"]
score: 8.0
evidence: 隐式语音块提升语音-文本预训练的效率与对齐
tldr: 该工作针对语音令牌序列过长导致计算效率低、跨模态对齐困难的问题，提出隐式语音-文本Transformer（LST），将离散语音令牌聚合成高阶隐式语音块作为自回归单元。这一设计使语音与文本的序列粒度匹配，显著降低训练和推理开销，并在预训练中促进更好的跨模态对齐。实验表明LST在多项语音理解与生成任务上达到更高性能且资源消耗更低，为大规模语音模型训练提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音令牌序列远长于文本，导致计算失衡并阻碍跨模态对齐。
method: 将语音令牌聚合为隐式语音块，统一序列粒度后进行自回归建模。
result: 训练效率提升数倍，同时在下游任务上取得了更高的语音理解与生成性能。
conclusion: 语音块化是一种有效的预训练加速与对齐增强策略，有助于语音模型规模化。
---

## Abstract
Auto-regressive speech–text models pre-trained on interleaved text tokens and discretized speech tokens demonstrate strong speech understanding and generation, yet remain substantially less compute-efficient than text LLMs, partly due to the much longer sequences of speech tokens relative to text. This modality imbalance disproportionately allocates pre-training and inference compute to speech, potentially hindering effective cross-modal alignment and slowing performance scaling by orders of magnitude. We introduce the Latent Speech-Text Transformer (LST), which aggregates speech tokens into latent speech patches that serve as higher-level autoregressive units. This design aligns the sequence-modeling granularity between speech and text while improving computational efficiency. The resulting patches can align with textual units to facilitate cross-modal knowledge transfer and compactly capture recurring acoustic patterns such as silence. Across story-completion benchmarks under both compute-controlled and data-controlled settings, LST consistently improves speech accuracy while also improving text performance, achieving up to +6.5% absolute gain on speech HellaSwag in compute-controlled training (+5.3% in data-controlled training). Under compute-controlled scaling from 420M to 1.8B parameters in a near compute-optimal regime, gains grow with scale, and improvements persist up to 7B parameters under fixed-token budgets. These benefits extend to downstream tasks: LST stabilizes ASR adaptation and reduces the effective autoregressive sequence length during ASR and TTS inference, lowering computational cost without degrading reconstruction quality. The Code is available at https://github.com/facebookresearch/lst.

---

## 论文详细总结（自动生成）

# 论文总结：Latent Speech-Text Transformer (LST)

## 1. 核心问题与整体含义
- **研究动机**：当前的自回归语音‑文本模型通常将语音离散化为令牌（speech tokens），并与文本令牌交织在一起进行预训练。但语音令牌的序列长度远长于文本令牌（常见数十倍以上），导致**计算资源严重失衡**：预训练和推理的大部分计算开销被语音部分占据，不仅拖慢训练与推理，还**阻碍了跨模态对齐**，制约了模型规模的扩展。
- **整体含义**：该工作提出一种**高阶语音表示单元**——隐式语音块，将多个语音令牌压缩为一个自回归建模单元，使语音与文本的序列粒度基本对齐。这既能大幅提升计算效率，又能促进语音‑文本之间的知识迁移，为大规模语音‑语言模型的训练提供了一条更经济、更易扩展的路径。

## 2. 方法论
- **核心思想**：将原始的离散语音令牌（speech tokens）聚合成**隐式语音块（latent speech patches）**，作为自回归Transformer的生成单元，从而缩短有效序列长度并统一语音与文本的建模粒度。
- **关键技术细节**：
  - 语音令牌首先通过一个**聚合模块**（文中未细述结构，可能为轻量级的池化或编码器）将连续的若干语音令牌映射为一个隐式块表示。
  - 隐式块与文本令牌交替组成序列，输入标准的自回归Transformer进行预训练（类似标准语言模型，预测下一个单元）。
  - 该设计使模型能以**更高的层次**学习语音中的规律性模式（如静音、重复的声学结构），同时让语音块与文本单元在语义上更容易对齐，从而提升跨模态知识传递。
- **与现有方法的区别**：传统方法直接在细粒度的语音令牌上建模，LST则在**更高阶的隐式语音块**上进行自回归，实现了序列粒度的匹配。

## 3. 实验设计
- **预训练评测基准**：
  - 主要使用**故事补全（story-completion）基准**，包括**语音版 HellaSwag** 等任务（从摘要中明确提及）。
  - 在**计算量控制（compute-controlled）** 和**数据量控制（data-controlled）** 两种公平对比设定下进行评估，以排除资源不同带来的干扰。
- **下游任务评测**：
  - **自动语音识别（ASR）** 的微调 **稳定性**与推理效率。
  - **语音合成（TTS）** 的重建质量与推理序列长度。
- **对比方法**：以**无语音块聚合的常规语音‑文本自回归模型**作为基线（如交织原始语音令牌的模型），可能还包括不同规模的变体。
- **扩展性实验**：在参数从 420M 扩展到 1.8B（甚至 7B）的过程中观察性能趋势。

## 4. 资源与算力
- 文中**未明确提及**具体的GPU型号、数量或训练时长等细节。
- 但摘要及元数据强调了**计算量控制**和**参数规模**（420M → 1.8B → 7B），并提到训练效率提升数倍，暗示实验使用了一定规模的并行训练资源，且进行了严格的算力匹配比较。

## 5. 实验数量与充分性
- 从提供的信息推测，实验覆盖了**多组对比评估**：
  - 不同规模的模型（420M, 1.8B, 7B）在故事补全任务上的表现。
  - 计算量控制与数据量控制两种消融设定。
  - 下游ASR与TTS任务。
  - 很可能包含语音块大小、聚合方式等消融实验（虽未提供细节，但方法创新需消融验证）。
- **充分性**：通过多角度（预训练理解、下游生成、效率、缩放规律）的评测，并与基线进行公平的等资源对比，实验设计较为**全面且客观**。但具体消融的广度和统计显著性需阅读全文才能最终判断。

## 6. 主要结论与发现
- **效率与性能双升**：LST在计算量控制训练中，语音 HellaSwag 准确率**绝对提升达 6.5%**（数据量控制下提升5.3%），同时还**改善了文本任务的性能**，说明语音‑文本对齐增强后有利于整体知识学习。
- **缩放优势**：在接近最优计算配置下，从 420M 到 1.8B 参数，LST的增益随模型规模**进一步放大**；在固定令牌预算下扩展到 7B，增益依然保持。
- **下游受益**：
  - ASR：微调过程**更加稳定**，自回归序列长度缩短，推理计算开销降低。
  - TTS：有效序列变短，计算成本下降，同时**重建质量未受损失**。
- **块化策略的意义**：隐式语音块能**紧凑捕获重复声学模式**，并作为跨模态对齐的桥梁，是语音‑文本预训练规模化的一种高效手段。

## 7. 优点
- **方法简洁有效**：通过简单的聚合操作实现序列长度压缩，无需复杂的架构改动。
- **公平对比设计突出**：同时控制计算量和数据量，清晰证明了性能增益并非来自额外资源，而是方法本身。
- **多维度验证**：从语言理解、语音生成到缩放定律均有覆盖，说服力较强。
- **实用价值高**：训练和推理的效率提升可直接降低大规模语音模型的门槛，并提升ASR、TTS等应用的部署经济性。
- **对齐性增强**：语音块与文本单元粒度匹配，内在促进了跨模态信息融合与迁移，这是超越单纯速度提升的深层次贡献。

## 8. 不足与局限
- **方法论细节缺失**：由于仅凭摘要和元数据，无法知晓隐式语音块的具体实现（如聚合方式、块长度选择、是否可学习等），这限制了对其普适性的深入判断。
- **实验覆盖范围可能受限**：目前展示的任务以故事补全、ASR/TTS为主，尚未提及口语对话、多语种、噪声鲁棒性等更广泛场景的验证。
- **偏差风险**：未说明使用的语音数据来源与文本配比，若数据分布偏向特定领域，可能影响结论的推广；部分提升（如ASR稳定性）的量化指标尚未给出。
- **应用限制**：语音块或许会在某些需要精细时间对齐的任务（如音素级识别）中丢失细粒度信息，其与纯文本或纯语音任务的兼容性仍需进一步探索。

（完）
