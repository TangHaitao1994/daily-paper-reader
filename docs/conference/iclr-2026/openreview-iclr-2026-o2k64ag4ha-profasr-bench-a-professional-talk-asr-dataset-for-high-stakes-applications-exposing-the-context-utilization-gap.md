---
title: "ProfASR-Bench: A Professional-talk ASR Dataset for High-Stakes Applications Exposing the Context-Utilization Gap"
title_zh: "ProfASR-Bench: 面向高风险应用的专业对话ASR数据集，揭示上下文利用差距"
authors: Deepak Piskala
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=o2k64ag4HA"
tags: ["query:speech-model"]
score: 9.0
evidence: 专业领域ASR基准揭示语音识别上下文利用差距
tldr: 该工作构建了面向金融、医学、法律和科技等高风险场景的语音识别评测集ProfASR-Bench，旨在探测模型利用上下文信息的能力。数据集提供领域提示与实体丰富的语音对，支持传统及实体感知指标。测评揭示了现有模型在专业术语和关键实体上的显著短板，为提升ASR精准度提供了诊断工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有ASR评测低估了专业场景下的术语、语体和关键实体精度挑战。
method: 构建了含领域提示与实体标注的专业对话语音评测集，并设计实体感知指标。
result: 发现主流模型在专业实体上错误率极高，且未充分利用上下文提升识别。
conclusion: 需发展强上下文感知的ASR技术以应对高风险应用准确度要求。
---

## Abstract
Automatic Speech Recognition (ASR) in professional settings faces challenges that existing benchmarks underplay: dense domain terminology, formal register variation, and near-zero tolerance for critical entity errors. We present \textsc{ProfASR-Bench}, a \emph{professional-talk} evaluation suite for high-stakes applications across finance, medicine, legal, and technology. Each example pairs a natural-language \emph{prompt} (domain cue and/or speaker profile) with an entity-rich target utterance, enabling controlled measurement of \emph{context-conditioned} recognition. The corpus supports conventional metrics alongside \emph{entity-aware} scores and slice-wise reporting by accent and gender. Using representative families \emph{Whisper} (encoder–decoder ASR) and \emph{Qwen-Omni} (audio LM) under matched \emph{no-context}, \emph{profile}, \emph{domain+profile}, \emph{oracle}, and \emph{adversarial} conditions, we uncover a consistent pattern: lightweight textual context produces little to no change in average WER, even when providing the gold transcript as an oracle prompt, and adversarial prompts do not reliably degrade WER. We term this the \textbf{\emph{context-utilization gap(CUG)}}: current systems are nominally promptable yet underuse readily available side information. Entity-centric analyses reveal only modest, model-dependent gains on information-bearing tokens, underscoring the need for stronger fusion mechanisms and calibrated trust in prompts.\textsc{ProfASR-Bench} contributes (i) a standardized \emph{context ladder} with paired, within-utterance estimation; (ii) entity-aware and slice-aware reporting with confidence intervals; and (iii) a reproducible testbed to compare fusion strategies across model families. We release data and code to foster comparable, context-aware evaluation in high-stakes domains.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：自动语音识别（ASR）在专业场景（如金融、医学、法律、科技）中的应用面临严峻挑战，包括密集的领域术语、正式的语体变化、以及关键实体识别的零容忍要求。
- **核心问题**：现有的 ASR 评测基准低估了上述专业场景中的术语、语体和实体识别难度，尤其未能系统评估模型利用**上下文信息**（如领域提示、说话人背景）来提升识别精度的能力。
- **整体含义**：该工作旨在揭示 ASR 系统对可用上下文信息的**利用不足**现象（即“上下文利用差距”），推动高风险应用中更精准、更可靠的上下文感知 ASR 发展。

### 2. 论文提出的方法论

- **核心思想**：构建一个**具有可控上下文条件**的专业语音识别评测集，通过为每条语音样本配对自然语言提示（领域线索和/或说话人档案），测量模型在不同上下文条件下的识别表现，从而诊断其利用周边信息的能力。
- **关键技术细节**：
  - **上下文阶梯（Context Ladder）**：设计多种上下文条件，从“无上下文”到“提供说话人档案”、“提供领域+档案”、“提供真实转写（Oracle）”直至“对抗性提示”，形成层级化的上下文供给。
  - **实体感知报告**：除传统词错误率（WER）外，引入**实体感知指标**，专门评估信息承载实体（如金融术语、医学名词、法律条款等）的识别精度。
  - **切片报告**：支持按口音和性别等维度进行分组评估并报告置信区间，增强结果的细粒度与稳健性。
- **算法/公式**：虽未在元数据中详述公式，但整体流程为：给定语音 \( X \) 和上下文提示 \( C \)，模型输出转录 \( \hat{Y} \)，通过配对样本设计控制 \( C \) 的变化，计算传统 WER 及实体级错误率，对比不同 \( C \) 下的性能差异，量化“上下文利用差距”。

### 3. 实验设计

- **数据集/场景**：
  - **ProfASR-Bench 评测集**：覆盖金融、医学、法律、科技四大专业领域，样本为实体丰富的专业对话语音，每条语音配有自然语言提示及实体标注。
- **基准测试**：在自身数据集上定义基准任务，即在不同上下文条件下评估 ASR 系统的识别准确度。
- **对比方法**：
  - **模型家族**：代表性的**Whisper**（编码器-解码器 ASR 架构）与**Qwen-Omni**（音频语言模型）。
  - **上下文条件**：对比无上下文、档案提示、领域+档案提示、Oracle（真实转写）提示、对抗性提示等五种条件。

### 4. 资源与算力

- 元数据及提供的文本中**未明确说明**具体算力配置，如 GPU 型号、数量、训练时长或推理资源。由于该工作侧重于评测基准的构建与评估，可能不涉及大规模训练，但推理成本未知。

### 5. 实验数量与充分性

- **实验组数**：至少涉及两种模型家族 × 五种上下文条件，结合四大专业领域及按口音、性别的切片分析，可形成数十组以上的对比实验。
- **充分性与客观性**：
  - 通过设置统一的上下文阶梯和实体感知指标，实验设计具有系统性。
  - 在 Oracle 条件和对抗性条件上的对比能有效检验上下文线索的作用方向，设计客观。
  - 但元数据未提及错误分析深度、跨模型扩展性（仅两类模型）等因素，可能限制结论的泛化性；整体上实验覆盖了多维度，**较为充分**。

### 6. 论文的主要结论与发现

- **上下文利用差距（CUG）**：主流 ASR 系统对文本形式的上下文提示利用率极低——即使提供真实转写作为提示，平均 WER 也无明显变化；对抗性提示也未可靠地恶化 WER。
- **实体识别困境**：针对信息量大的专业实体，模型仅表现出微弱的、与模型相关的增益，说明现有融合机制不足以充分借助上下文线索。
- **诊断价值**：专业领域的 ASR 评测必须重视术语和关键实体的准确性，常规 WER 无法反映真实风险。

### 7. 优点

- **问题导向明确**：聚焦高风险专业场景的切实痛点，具有明确的应用驱动力。
- **评测设计创新**：提出“上下文阶梯”和实体感知指标，提供了比常规 WER 更细粒度的诊断工具。
- **可复现性与实用性**：公开数据与代码，且支持按社会属性（口音、性别）进行公平性切片分析，增强了基准的透明度和可操作性。
- **多维度对比**：覆盖两种不同范式的 ASR 模型，增强了发现的普遍性。

### 8. 不足与局限

- **覆盖模型有限**：仅测试了 Whisper 和 Qwen-Omni 两类模型，对更多种类的端到端模型或级联系统的泛化结论尚不明确。
- **上下文形式单一**：只探索了文本形式的提示，未涉及多模态上下文（如图表、病历图像）或动态上下文，实际应用场景可能更复杂。
- **语言及领域扩展**：当前聚焦英文四大领域，多语言、更多垂直领域（如制造业、航空）下的表现为未知。
- **评测本身不能解决融合**：基准揭示了“差距”，但未提出改进上下文利用的具体技术方案，留待后续研究。
- **偏差风险**：尽管报告了按口音和性别的切片，但数据集本身的偏差（如录音条件、领域覆盖均衡性）未在元数据中详细说明。

（完）
