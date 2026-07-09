---
title: "WhisperDiari: A Whisper-Based Speaker Diarization Framework in Token Space Leveraging Semantic and Speaker Information for Better Text Adaptability"
title_zh: WhisperDiari：基于Whisper的在令牌空间融合语义与说话人信息的说话人日志框架
authors: "Yongkang Yin, Yuexian Zou"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40746/44707"
tags: ["query:speech-model"]
score: 8.0
evidence: 基于Whisper的统一说话人日志与语音识别框架，提升转录准确率
tldr: 为解决说话人日志中边界检测不准与语义分割错误影响ASR转录质量的问题，本文提出WhisperDiari，在令牌空间构建统一的说话人日志与语音识别框架，利用预训练Whisper模型的语义与说话人信息，提升了时间戳精度和最终转录准确率，有广阔实用价值。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 传统说话人日志边界不准、语义分割错误，影响ASR转录准确性。
method: 提出WhisperDiari，在离散令牌空间利用Whisper语义与说话人信息统一建模。
result: 提升了说话人边界检测和语义分割精度，改善了ASR输出质量。
conclusion: 统一的说话人日志与ASR框架提高了实用性，推动了语音转录技术发展。
---

## Abstract
Speaker diarization is a fundamental task in speech processing aims to determine 'who speaks when'. When combined with ASR, it enables speaker-labeled transcription with broad practical value. Most existing methods rely on frame-level classification, but the high cost of annotating mixed-speaker audio limits the availability of large-scale, accurately labeled datasets. As a result, even state-of-the-art models struggle with imprecise speaker boundary detection and semantic segmentation errors, which degrade timestamp accuracy and downstream ASR performance. To address these challenges, we propose WhisperDiari, a unified framework for speaker diarization and ASR. We first construct LibriDiari, a dataset derived from LibriSpeech, containing 2–4 speaker mixed audio annotated with transcripts and speaker labels. WhisperDiari builds on the Whisper model, incorporating speaker adapters and Speaker Similarity Matrix Supervision to enhance speaker representation. In addition, a dedicated speaker decoder fuses speaker embeddings with contextual semantics from Whisper's decoder, enabling token-level diarization. This design effectively resolves segmentation ambiguity, aligns diarization with semantic units, and jointly models 'who speaks what and when', producing accurate, timestamped transcripts. We train the model on LibriDiari and evaluate it on both LibriDiari and the real-world AMI corpus. Experimental results demonstrate that WhisperDiari consistently outperforms state-of-the-art open-source baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有说话人日志（Speaker Diarization）方法多基于帧级别分类，在与自动语音识别（ASR）结合时，容易出现**说话人边界检测不准**与**语义分割错误**，导致时间戳精度差、下游转录文本语义断裂。
- **背景**：高质量的多说话人标注数据稀缺，且传统独立管道（先日志后ASR）存在时序对齐困难。因此，该文旨在设计一个**统一框架**，在令牌空间联合建模“谁在何时说了什么”，提升转录的准确性与连贯性。

### 2. 论文提出的方法论
- **整体架构**：基于 Whisper 编码器-解码器结构，扩展出 **说话人适配器 (Speaker Adapter)** 和 **说话人解码器 (Speaker Decoder)**，实现令牌级说话人日志。
- **关键组件**：
  - **说话人适配器**：从 Whisper 编码器中间层提取隐藏状态，经 LayerNorm 和瓶颈结构（Linear→ReLU→Linear）生成更具鉴别力的说话人表示 \(H_{\text{spk}}\)。
  - **说话人相似度矩阵监督 (Speaker Similarity Matrix Supervision)**：使用预训练说话人嵌入模型（CAM++）提取参考嵌入，计算相似度矩阵 \(M'\)，与适配器输出的 \(M\) 计算 MSE 损失 \(L_{\text{sim}}\)，提升说话人表示的鲁棒性。
  - **说话人解码器**：以语义解码器输出的完整令牌序列 \(Y\) 和 \(H_{\text{spk}}\) 为输入，通过交叉注意力并行预测每个令牌的说话人标签 \(Y_{\text{spk}}\)，实现令牌空间中的日志。
- **优化目标**：总损失 \(L = L_{\text{spk}} + \alpha L_{\text{sim}}\)，其中 \(L_{\text{spk}}\) 为说话人令牌交叉熵损失。
- **数据构造**：提出 LibriDiari 数据集，基于 LibriSpeech 通过随机采样不同说话人话语、拼接并控制总时长（≤30秒）来合成多说话人音频。

### 3. 实验设计
- **数据集**：
  - **LibriDiari**（模拟）：由 LibriSpeech 构造，包含2、3、4说话人及混合人数（nspk）的30秒片段，带对应转录和说话人标签。
  - **AMI Meeting Corpus**（真实）：含4人会议的对话录音，预处理时尽量降低重叠语音。
- **基线方法**：Pyannote‑audio 3.1、3D‑Speaker Diarization、DiariZen、DiaPer（均为开源先进方法）。
- **评价指标**：DER（日志错误率）、tDER（令牌级日志错误率）、WER（词错误率）。所有基线统一使用 Whisper‑medium 作为 ASR 后端，通过时间戳对齐得到令牌级说话人标签用于 tDER 计算。
- **对比方式**：在相同 ASR 输出下比较 tDER 和 WER；同时比较时序对齐管道（先日志后 ASR）与顺序管道（分段 ASR）下的效果。

### 4. 资源与算力
- 文中**未明确说明**使用的 GPU 型号、数量以及训练时长。
- 仅提及使用预训练 Whisper‑medium 模型，优化器为 Adam（学习率 \(10^{-4}\)），损失权重 \(\alpha=0.5\)。

### 5. 实验数量与充分性
- **实验组数**：约 4组对比实验（不同说话人数、真实数据）+ 1组消融实验（去除适配器、不同相似度步长、去除监督）。
- **充分性评价**：
  - 覆盖了模拟和真实场景，对比了多种主流方法，消融实验验证了各模块有效性。
  - 所有基线共享相同 ASR 后端，公平性较好。
  - 但缺少对噪声、多通道场景及更多真实数据集的测试，数量上不算庞大，实验深度适中。

### 6. 论文的主要结论与发现
- WhisperDiari 在 tDER 指标上**显著优于**所有开源基线，说明令牌级联合建模大幅改善了说话人标签与语义令牌的对齐。
- 在 WER 上优于顺序管道方法，证明统一框架能减少语义碎片，提升转录连贯性。
- DER 与最佳多阶段系统（3D‑Speaker）可比，但由于解码器生成的时间戳精度较低，DER 略逊；当提前知道说话人数目时，tDER 表现更加稳定。
- 推理速度快于各基线管道。

### 7. 优点
- **令牌级统一建模**：直接将日志与 ASR 深度融合，避免了管道式方法中的对齐误差和语义断裂。
- **说话人表示增强**：适配器与相似度矩阵监督有效利用预训练知识，提升了说话人鉴别力（通过 PCA 可视化验证）。
- **新评价指标 tDER**：提出了更适合令牌级日志的评价方式，便于与 ASR 后端联合评估。
- **数据构造方案可推广**：LibriDiari 构造方法可扩展到其他 ASR 数据集，有助于后续研究。
- **强实用性**：基于流行的 Whisper 架构，易于部署和复现。

### 8. 不足与局限
- **DER 精度受限**：令牌级时间戳受解码器生成频率影响，在严格帧级对齐的 DER 上不及部分多阶段系统。
- **依赖说话人数目**：在未知说话人数目时性能有所下降，对开放场景的适应性有待提升。
- **实验覆盖有限**：仅在模拟 LibriDiari 和 AMI 两个数据集上评测，未测试噪声、混响、远场等复杂声学环境，泛化性待验证。
- **计算资源未披露**：训练算力需求不明，不利于后续工作复现和资源评估。
- **处理重叠语音能力未深入探讨**：采用预处理减少重叠，可能削弱了对真实会议中重叠语音的适应性。

（完）
