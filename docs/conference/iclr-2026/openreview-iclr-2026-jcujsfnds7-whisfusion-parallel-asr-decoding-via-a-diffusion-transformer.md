---
title: "Whisfusion: Parallel ASR Decoding via a Diffusion Transformer"
title_zh: "Whisfusion: 用扩散Transformer实现并行ASR解码"
authors: "Taeyoun Kwon, Junhyuk Ahn, Taegeun Yun, Heeju Jwa, Yoonchae Choi, Siwon Park, Nam-Joon Kim, Jongchan Kim, Hyun Gon Ryu, Hyuk-Jae Lee"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=JCujsFnDS7"
tags: ["query:speech-model"]
score: 6.0
evidence: 非自回归ASR解码加速识别，属于性能增强技术
tldr: 该论文针对Whisper式自回归ASR解码延迟随句长线性增长的问题，提出非自回归解码框架Whisfusion，将冻结的Whisper编码器与掩码扩散解码器结合，通过轻量交叉注意力适配实现并行令牌生成。实验表明Whisfusion在维持语音识别精度的同时大幅降低解码延迟，为实时转录应用提供了高效方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自回归ASR解码延迟随语音长度线性增加，成为实时应用的瓶颈。
method: 利用冻结Whisper编码器与掩码扩散Transformer，并行生成所有文本令牌。
result: 在各项ASR基准上解码速度大幅提升，且精度与自回归模型持平。
conclusion: 扩散模型可实现高质量非自回归ASR，为快速语音识别提供新范式。
---

## Abstract
Fast automatic speech recognition (ASR) is crucial for applications such as captioning and transcription. Although modern ASR encoders can process up to ~30 seconds of audio in a single pass, Whisper-style autoregressive (AR) decoders still generate tokens sequentially, making decoding latency grow linearly with utterance length. We propose Whisfusion, a non-autoregressive (NAR) ASR framework that fuses a frozen pre-trained Whisper encoder with a masked-diffusion text decoder. At each diffusion step, the decoder conditions on the full acoustic context and updates all tokens in parallel, mitigating the AR latency bottleneck while preserving Whisper-compatible generative structure. A lightweight cross-attention adapter trained via parameter-efficient fine-tuning bridges audio and text, and we introduce Parallel Diffusion Decoding (PDD), an ASR-tailored batch-parallel sampling scheme that improves the accuracy–latency trade-off in low-to-mid batch regimes. With 6.5k hours of training data, Whisfusion reaches 4.9\% WER on LibriSpeech test-clean, comparable to similarly sized Whisper model (Whisper-small at 5.0\%), while enabling much faster decoding. In particular, on 20–30s segments within Whisper’s 30s window, Whisfusion reduces decoding time from 674.7 ms to 80.7 ms (8.4× faster) at similar accuracy, demonstrating an efficient NAR operating point for Whisper-compatible ASR.

---

## 论文详细总结（自动生成）

## 1. 核心问题与研究背景
- **问题**：现代语音识别（ASR）编码器已能一次处理长达约30秒的音频，但主流Whisper模型的自回归（AR）解码器仍需逐个令牌串行生成，导致解码延迟随话语长度线性增加，成为实时字幕、转录等应用的主要瓶颈。
- **动机**：突破AR解码的顺序约束，实现并行化的非自回归（NAR）解码，同时保留Whisper编码器的强大语音表征能力，在不牺牲识别精度的前提下大幅降低延迟。

## 2. 方法论
- **整体框架**：提出Whisfusion，将冻结的预训练Whisper编码器与一个掩码扩散Transformer解码器相融合，以NAR方式一次性生成全部文本令牌。
- **核心组件**：
  - **冻结Whisper编码器**：直接复用预训练权重，保持音频特征质量并减少训练开销。
  - **掩码扩散解码器**：从带掩码噪声的令牌序列开始，在每一步扩散中，基于完整声学上下文并行更新所有令牌，逐步去噪直至生成最终文本。
  - **轻量交叉注意力适配器**：插入编码器与解码器之间，通过参数高效微调（PEFT）弥合音频-文本跨模态鸿沟，避免全量微调大量参数。
  - **并行扩散解码（PDD）**：提出一种面向ASR的批量并行采样策略，特别优化低到中批量场景下的准确率-延迟权衡，使得多个样本能在一个扩散批次内并行解码。
- **工作流**：音频输入→冻结Whisper编码器→交叉注意力适配器→掩码扩散解码器（从全掩码开始，多步并行去噪）→输出文本令牌。

## 3. 实验设计
- **训练数据**：6.5k小时语音数据（具体数据集未在摘要中详述）。
- **主要基准**：
  - **LibriSpeech test-clean**：报告WER（词错误率）。
  - **解码延迟测试**：在20-30秒的长音频段上测量解码耗时。
- **对比方法**：
  - 与同等规模的Whisper小模型（Whisper-small）对比，该模型在test-clean上WER为5.0%。
  - 重点对比自回归与所提NAR方案的解码速度。
- **评估指标**：WER（精度）、解码时间（速度）、速度提升倍数。

## 4. 资源与算力
- 论文摘要中**未明确说明**使用的GPU型号、数量、训练时长及总计算量。仅提及使用了6.5k小时训练数据，但未给出资源消耗细节。

## 5. 实验数量与充分性
- **实验规模**：摘要中主要展示了LibriSpeech test-clean上的精度与20-30秒段的解码速度，并对所提PDD方案进行了准确率-延迟权衡分析。因摘要限制，无法获悉是否包含其他主流测试集（如其他LibriSpeech子集、多语种、噪声场景）以及完整的消融实验（如适配器设计、扩散步数影响等）。
- **公正性与充分性**：
  - 对比对象Whisper-small具有代表性，但仅与一款模型比较可能不够全面，未见与更小的Whisper变体或其他NAR ASR方法的横向对比。
  - 训练数据6.5k小时远小于Whisper原始预训练数据（数十万小时），但得到接近的WER，表明方法高效；然而实验的覆盖度、泛化性验证尚不充分，需更多实验支撑。

## 6. 主要结论与发现
- Whisfusion在LibriSpeech test-clean上达到4.9% WER，与自回归Whisper-small（5.0% WER）精度持平。
- 在20-30秒长音频上，解码时间从674.7 ms降至80.7 ms，实现**8.4倍加速**，解除了自回归解码的延迟与长度绑定。
- 扩散模型能够实现高质量的非自回归ASR，为Whisper兼容的快速语音识别提供了高效的新范式。

## 7. 优点
- **架构创新**：将冻结ASR编码器与扩散Transformer优雅结合，既复用强大语音知识又实现并行生成。
- **轻量适配**：通过交叉注意力适配器以参数高效方式完成跨模态对齐，训练成本相对较低。
- **并行解码突破**：彻底打破AR解码的顺序瓶颈，延迟不再随文本长度线性增长。
- **定制优化**：提出的PDD批量并行采样方案进一步提升了实际部署中的效率与精度平衡。

## 8. 不足与局限
- **实验覆盖面窄**：公开的摘要仅报告了LibriSpeech单一测试集的结果，缺乏在多语种、噪声、长尾词等复杂场景下的验证，泛化性存疑。
- **对比基线有限**：仅与Whisper-small进行对比，未比较其他NAR ASR框架（如CTC、非自回归Transformer等）或更大/更小的Whisper变体，难以全面评估竞争力。
- **扩散步数未明**：扩散模型通常需要多次前向推理，虽然并行但总计算量可能高于极简的CTC模型；文中未分析扩散步数对速度与精度的具体影响。
- **训练数据规模受限**：6.5k小时数据量可能无法展现模型在大规模预训练下的完整潜力，且与Whisper基础模型训练数据量存在量级差距，直接比较WER时可能存在不公平因素。
- **实时系统考量不足**：论文侧重解码延迟削减，但未讨论编码器端计算开销、流式处理适配、显存占用等全链路部署问题。

（完）
