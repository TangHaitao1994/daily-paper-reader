---
title: "Speak, Edit, Repeat: High-Fidelity Voice Editing and Zero-Shot TTS with Cross-Attentive Mamba"
title_zh: 说、编辑、重复：用交叉注意力Mamba实现高保真语音编辑与零样本TTS
authors: "Baher Mohammad, Magauiya Zhussip, Stamatios Lefkimmiatis"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=yEKyIwJvvl"
tags: ["query:speech-model"]
score: 9.0
evidence: 基于交叉注意力Mamba的高保真语音编辑与零样本TTS
tldr: 该论文提出基于Mamba与交叉注意力的自回归架构MAVE，兼顾语音编辑与零样本语音合成。模型通过高效音频序列建模和精确文本-声学对齐，在真实音频上实现了保真度业界领先的语音编辑，同时在零样本TTS中超越现有扩散和自回归模型。其统一框架为高自然度语音交互应用提供了端到端解决方案，并具备强泛化能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音编辑与合成模型在保真度和零样本能力上仍有不足。
method: 设计交叉注意力Mamba架构，融合高效序列建模与文本-声学对齐。
result: 在语音编辑上达到最优自然度，零样本TTS指标也领先主流模型。
conclusion: Mamba架构为语音生成任务提供了高效且高质的统一建模途径。
---

## Abstract
We introduce $\textbf{MAVE}$ ($\textbf{M}$amba with Cross-$\textbf{A}$ttention for $\textbf{V}$oice $\textbf{E}$diting and Synthesis), a novel autoregressive architecture for text-conditioned voice editing and high-fidelity text-to-speech (TTS) synthesis, built on a cross-attentive Mamba backbone. MAVE achieves state-of-the-art performance in speech editing and very competitive results in zero-shot TTS, while not being explicitly trained on the latter task, outperforming leading autoregressive and diffusion models on diverse, real-world audio. By integrating Mamba for efficient audio sequence modeling with cross-attention for precise text-acoustic alignment, MAVE enables context-aware voice editing with exceptional naturalness and speaker consistency. In pairwise human evaluations on a random 40-sample subset of the RealEdit benchmark (400 judgments), 57.2\% of listeners rated MAVE-edited speech as perceptually equal to the original, while 24.8\% prefered the original and 18.0\% MAVE- demonstrating that in the majority of cases edits are indistinguishable from the source. MAVE compares favorably with VoiceCraft and FluentSpeech both on pairwise comparisons and standalone mean opinion score (MOS) evaluations. For zero-shot TTS, MAVE exceeds VoiceCraft in both speaker similarity and naturalness, without requiring multiple inference runs or post-processing. Remarkably, these quality gains come with a significantly lower memory cost and approximately the same latency: MAVE requires $\sim6\times$ less memory than VoiceCraft during inference on utterances from the RealEdit database (mean duration: 6.21s, A100, FP16, batch size 1). Our results demonstrate that MAVE establishes a new standard for flexible, high-fidelity voice editing and synthesis through the synergistic integration of structured state-space modeling and cross-modal attention.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机与背景）
- **核心问题**：如何在统一的生成框架下，同时实现高保真的语音编辑和高质量的零样本语音合成（TTS）。
- **研究动机**：
  - 现有的语音编辑模型在保真度（编辑后语音的自然度和与原始音频的一致性）上仍有不足。
  - 零样本TTS模型（如扩散模型和自回归模型）通常需要多步推理或后处理，且在真实场景下的泛化能力有限。
  - 需要一个更高效、更高质的模型来统一解决语音编辑和合成任务，并显著降低计算成本。
- **整体含义**：该工作旨在通过结构化状态空间模型（Mamba）与交叉模态注意力的结合，建立一个新范式，让语音编辑在大多数情况下人耳无法与原始录音区分，同时让模型在未曾专门训练的零样本TTS任务上取得优于专用模型的水平。

### 2. 论文提出的方法论
- **核心架构：MAVE (Mamba with Cross-Attention for Voice Editing and Synthesis)**
  - **基础框架**：自回归生成模型。
  - **骨干网络**：采用交叉注意力 Mamba 结构，将 Mamba 的高效长序列建模能力与交叉注意力机制相结合。
  - **关键技术思路**：
    - **Mamba 模块**：用于高效处理音频序列，凭借状态空间模型在长序列上保持线性或近线性复杂度，解决自回归模型中长距离依赖建模的高成本问题。
    - **交叉注意力机制**：实现文本条件与声学特征之间的精确对齐，使模型能够准确理解需要编辑或合成的文本内容，并在正确的时间位置生成对应语音。
  - **工作流程**：
    - 输入为待编辑的原始语音、编辑文本（或合成文本）和参考扬声器信息。
    - 通过交叉注意力 Mamba 层逐帧自回归预测声学特征，同时可对未编辑区域保持高保真上下文一致性。
  - **任务统一性**：模型在语音编辑任务上进行训练，但因其强大的文本-声学对齐和生成能力，能够直接泛化到零样本 TTS，无需微调、多次推理或后处理。

### 3. 实验设计
- **数据集与基准**：
  - **语音编辑**：使用 **RealEdit** 基准测试中的 40 个随机样本子集（真实世界音频），衡量编辑后语音与原语音的感知差异。
  - **零样本 TTS**：直接在零样本设定下评测，无需使用该任务的特定训练数据。
- **对比方法**：
  - 语音编辑任务：与 **VoiceCraft** 和 **FluentSpeech** 进行成对比较（pairwise comparisons）及独立的平均意见分（MOS）评估。
  - 零样本 TTS：主要与 **VoiceCraft** 对比，评估说话人相似度和自然度。
- **评估指标**：
  - 人类主观评测：成对偏好判断（原始语音 vs. MAVE编辑语音），共收集 **400 个判断**（40 样本 × 10 人左右）。
  - 客观/半客观指标：MOS、说话人相似度。
  - 推理效率：内存占用与延迟（在 A100 GPU、FP16、batch size=1 下测试，平均音频时长 6.21 秒）。

### 4. 资源与算力
- **推理阶段**：论文明确给出了推理时的计算资源——NVIDIA A100 GPU，FP16 精度，批次大小为 1，并对比了内存占用（MAVE 所需显存约为 VoiceCraft 的 1/6）。
- **训练阶段**：摘要中**未提及**训练所用的 GPU 型号、数量、训练时长或总计算量。无法得知为了达到所述性能，需要消耗多少预训练或微调算力。

### 5. 实验数量与充分性
- **实验组数量**：
  - 提交了定量的人类评估结果（400 次判断），覆盖与原始录音的对比、与其他模型的 MOS 对比。
  - 提供了零样本 TTS 的主客观结果，与当前领先模型 VoiceCraft 对标。
  - 进行了推理效率的对比（内存和延迟）。
- **充分性分析**：
  - **优点**：结合了主观（成对偏好、MOS）与客观（说话人相似度）评估，评估维度较全面；使用了真实场景数据集 RealEdit，增强了可信度。
  - **潜在不足**：摘要未提及**消融实验**（如去掉交叉注意力或替换 Mamba 的效果），也未说明在更多样化数据集（如不同噪声、多语种、多说话人）上的鲁棒性测试。因此，基于现有信息难以判断实验是否完全充分，但核心性能对比的铺垫是客观、公平的。

### 6. 论文的主要结论与发现
- **语音编辑性能卓越**：在 RealEdit 随机子集上，**57.2%** 的听者认为 MAVE 编辑后的语音与原录音无法区分（感知相等），仅 24.8% 偏好原录音，证明了编辑的高保真性。
- **零样本 TTS 具有竞争力**：在没有专门训练的情况下，MAVE 在说话人相似度和自然度上均超越了 VoiceCraft，且**无需多次推理或后处理**。
- **极高的内存效率**：在推理同等长度的语音时，MAVE 的内存需求仅为 VoiceCraft 的约 **1/6**，同时保持相近的延迟。
- **统一架构的优越性**：交叉注意力 Mamba 能够有效融合结构化状态空间建模与跨模态对齐，为高保真语音编辑和合成设立了新标准。

### 7. 优点
- **方法论亮点**：
  - 创新性地将 Mamba 模型与交叉注意力结合，用于语音生成，在长序列建模效率和文本-语音对齐精度之间取得平衡。
  - 一个模型解决两个任务，且零样本 TTS 能力是“涌现”的，无需针对性优化。
- **性能与效率**：
  - 在主观保真度评估中达到极高标杆（超过半数编辑样本与人耳不可区分）。
  - 相比强大的基线模型 VoiceCraft，以约 6 倍更低的内存消耗取得了更优或持平的性能，实用价值高。
- **实验设计亮点**：
  - 采用真实世界音频基准 RealEdit，而非仅使用干净朗读数据，评估更贴近实际应用。
  - 成对比较（编辑后语音 vs. 原始录音）的设计直接验证了编辑的无痕性，实验结论很有说服力。

### 8. 不足与局限
- **实验覆盖与详实性**：
  - 摘要未提及消融研究，难以评估 Mamba 模块、交叉注意力等各组件分别的贡献。
  - 未提及其他对比模型（如基于扩散的 TTS 模型）的更多细节，零样本 TTS 对比可能不够全面。
  - 训练算力与数据规模未披露，无法判断模型的训练效率和数据依赖性。
- **潜在偏差与风险**：
  - 人类评估仅使用 40 个样本，尽管判断次数较多，但样本量仍可能引入选择偏差。
  - 编辑评估基于 RealEdit 子集，未知其语言、口音、录音环境等的多样性；零样本 TTS 的泛化性也缺乏多语种、多风格验证。
- **应用限制**：
  - 基于自回归生成，可能存在长文本或长音频时的误差累积风险，摘要未讨论。
  - 推理延迟虽与 VoiceCraft 相近，但能否满足实时应用（如流式编辑）仍待验证。

（完）
