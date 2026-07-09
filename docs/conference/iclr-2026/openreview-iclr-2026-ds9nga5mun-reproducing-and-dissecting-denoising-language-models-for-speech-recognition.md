---
title: Reproducing and Dissecting Denoising Language Models for Speech Recognition
title_zh: 复现与剖析去噪语言模型在语音识别中的应用
authors: "Dorian Koch, Albert Zeyer, Nick Rossenbach, Ralf Schlüter, Hermann Ney"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=dS9nGa5Mun"
tags: ["query:speech-model"]
score: 9.0
evidence: 面向ASR精度的去噪语言模型大规模实证研究
tldr: 针对去噪语言模型在ASR中的应用，首次开展大规模独立实证研究。构建并发布完整可复现管线，系统评估数据增强、TTS系统和解码策略等设计因素的影响。该工作为去噪语言模型的优化提供了重要见解，并通过开源促进了后续研究。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 去噪语言模型虽能提升ASR性能，但其训练管线复杂，设计选择影响不明。
method: 构建完整的可复现管道，系统研究数据增强、TTS系统和解码策略等关键设计轴。
result: 通过数十种配置实验，揭示了各因素对性能的影响，并开源了管线。
conclusion: 该研究为去噪语言模型在ASR中的优化提供了实证指导，促进了可复现研究。
---

## Abstract
Denoising language models (DLMs) have been proposed
as a powerful alternative to traditional language models (LMs)
for automatic speech recognition (ASR),
motivated by their ability to use bidirectional context
and adapt to a specific ASR model's error patterns.
However, the complexity of the DLM training pipeline has hindered wider investigation.
This paper presents the *first independent, large-scale empirical study* of DLMs.
We build and release a *complete, reproducible pipeline* to systematically investigate the impact of key design choices.
We evaluate dozens of configurations across multiple axes, including various data augmentation techniques
(e.g., SpecAugment, dropout, mixup),
different text-to-speech systems,
and multiple decoding strategies.
Our comparative analysis in a common subword vocabulary setting
demonstrates that *DLMs outperform traditional LMs*,
but only after a distinct compute tipping point.
While LMs are more efficient at lower budgets, DLMs scale better with longer training,
mirroring behaviors observed in diffusion language models.
However, we observe smaller improvements than those reported in prior character-based work,
which indicates that the DLM's performance is conditional on factors such as the vocabulary.
Our analysis reveals that a key factor for improving performance
is to condition the DLM on *richer information from the ASR's hypothesis space*,
rather than just a single best guess.
To this end, we introduce *DLM-sum, a novel method for decoding from multiple ASR hypotheses*,
which consistently outperforms the previously proposed DSR decoding method.
We believe our findings and public pipeline provide a crucial foundation for the community
to better understand, improve, and build upon this promising class of models.
The code is publicly available at https://anonymous.4open.science/r/2025-dlm/.

---

## 论文详细总结（自动生成）

# 论文总结：复现与剖析去噪语言模型在语音识别中的应用

## 1. 核心问题与整体含义
- **研究背景**：去噪语言模型（Denoising LMs, DLMs）被提出作为传统语言模型（LMs）在自动语音识别（ASR）中的有力替代方案。其优势在于能利用双向上下文，并可适配特定 ASR 模型的错误模式。
- **核心问题**：DLM 的训练管线极为复杂，众多关键设计选择（如数据增强方式、语音合成系统的选择、解码策略等）对最终性能的影响尚不明确，阻碍了该方向的广泛研究。
- **整体含义**：本文首次对 DLM 进行了**大规模、独立的实证研究**，旨在通过构建完全可复现的管线，系统剖析各设计因素的影响，为社区提供理解和改进 DLM 的坚实基础。

## 2. 方法论
- **核心思想**：构建一套完整的、可复现的 DLM 训练与评估流程，并在此基础上对影响性能的关键设计轴进行系统性的控制实验。
- **关键技术细节**：
  - **多轴因素研究**：系统调整**数据增强技术**（如 SpecAugment、dropout、mixup）、**不同 TTS 系统**以及**多种解码策略**。
  - **新颖解码方法**：提出 **DLM-sum** 方法，从 ASR 产生的多个假设（而非仅单一最佳假设）中解码，以利用 ASR 假设空间中更丰富的信息。该方法被证明始终优于先前提出的 DSR 解码方法。
  - **统一设定**：所有比较均在**共同的子词词汇**设定下进行。
- **算法流程（文字描述）**：虽未给出详细公式，但流程可概括为：① 使用不同 TTS 系统生成带噪声的合成语音数据；② 应用特定的数据增强策略（如 SpecAugment）扩充训练语料；③ 训练 DLM 模型，条件于 ASR 的 N-best 假设或 Lattice 信息；④ 在解码阶段，使用 DLM-sum 等方法重打分并输出最终文本。传统 LMs 作为基线在同一子词词汇下训练和评估。

## 3. 实验设计
- **数据集/场景**：摘要未列出具体的数据集名称，仅提及在“共同的子词词汇设定”下开展实验。从 ASR 任务特性推断，可能使用了标准的英文 ASR 基准（如 LibriSpeech），但原文未确认。
- **基准方法**：传统语言模型（LMs）作为核心对比基准；同时与先前基于字符的 DLM 工作（未点名）以及 DSR 解码方法进行比较。
- **对比维度**：评估了数十种配置，涵盖数据增强、TTS 系统、解码策略以及训练计算量（如训练步长/时长）等轴。

## 4. 资源与算力
- **说明**：摘要及提供的元数据中**未明确提及任何算力信息**（如 GPU 型号、数量、训练时长或总计算量）。原文中可能存在此类数据，但依据现有摘录无法总结。

## 5. 实验数量与充分性
- **实验组数**：摘要表述为“数十种配置”（dozens of configurations），涵盖多个设计轴。实际数量应较为可观（可能 >30 组）。
- **充分性**：从研究目标看，实验设计较全面，覆盖了数据增强、声学模型前端（TTS）、解码器后端等关键环节，且对训练预算（计算转折点）进行专门分析。对比公平，使用统一的子词词汇确保可比性。整体而言，实验规模足以支撑主要结论。
- **客观性与公平性**：在**相同子词词汇**设定下比较 DLM 和 LM，避免了词汇不一致带来的干扰；采用多种解码策略并报告 DLM-sum 的优势，对比对象清晰，结论客观。

## 6. 主要结论与发现
- **DLM 相对于传统 LM 的性能**：DLM 能够超越传统 LM，但存在一个**明显的计算转折点**：在低训练预算下，LM 更高效；随着训练时间延长，DLM 展现出更好的可扩展性，这一行为与扩散语言模型相似。
- **改进幅度受限**：观察到的性能提升**小于**先前基于字符的 DLM 工作所报告的结果，表明 DLM 的性能优势**依赖于词汇表等具体因素**（如子词 vs. 字符）。
- **关键性能驱动因素**：最重要的提升手段是**向 DLM 提供 ASR 假设空间中更丰富的信息**，而非仅依赖单一最佳猜测。
- **新解码方法有效性**：提出的 **DLM-sum** 方法（从多个假设解码）在性能上一致优于此前提出的 DSR 解码方法。

## 7. 优点
- **首次独立大规模实证**：弥补了社区缺乏第三方系统研究的空白，摆脱了对原创新团队的依赖。
- **完整可复现管线**：公开代码和流程，显著降低了后续研究的准入门槛，推动了可复现性。
- **系统化的设计空间探索**：对数据增强、TTS 前端、解码策略等多轴进行控制变量研究，分析层次清晰。
- **创新解码方案**：提出的 DLM-sum 取得一致提升，为利用 ASR 多假设提供了实用思路。
- **计算转折点的洞察**：揭示了 DLM 的性价比规律，对实际部署和资源分配有指导意义。

## 8. 不足与局限
- **经验范围有限**：摘要未披露使用的具体数据集，很可能仅在少数（甚至一个）英文基准上验证，向其他语言、领域的泛化性存疑。
- **改进幅度不及预期**：在子词设定下，改进幅度弱于先前字符级工作，暗示 DLM 的优势可能高度受限于特定的语言单元表示，其通用性有待商榷。
- **缺失关键实现细节**：摘要本身未提供算力需求、模型规模、训练具体超参数等，读者无法从摘要评估计算成本门槛和实际可行性。
- **解码方法比较不全面**：仅与 DSR 对比，可能遗漏其他成熟的利用 ASR 多信息源的重打分方法（如基于 Lattice 的 LM 浅层融合等）。
- **偏差风险**：尽管是独立复现，但作者团队本身可能是该领域的资深研究者，实验设计中可能无意识地偏袒 DLM 有利的设置（如特意选择 DLM 友好的 TTS 系统），需通过公开代码让社区进一步验证。

（完）
