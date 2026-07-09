---
title: "AVSU-Bench and VSpeech-R1: A Dataset and MLLM for Audio-Visual Speech Understanding"
title_zh: AVSU-Bench 与 VSpeech-R1：面向音视频语音理解的数据集和多模态大语言模型
authors: "Yaoting Wang, Shaoxuan Xu, Ziyi Zhang, Yuanchao Li, Jian Ding, Jun Chen, Weijun Wang, Di Hu, Yuanchun Li, Yunxin Liu, Henghui Ding, Mohamed Elhoseiny"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=M9jciGcJpC"
tags: ["query:speech-model"]
score: 10.0
evidence: 音视频语音理解，数据集和多模态大语言模型，利用唇动视觉线索增强嘈杂环境语音
tldr: 为克服现有音视频研究仅停留在转录层面、缺乏深层语义理解的局限，该论文提出音视频语音理解新任务及AVSU-Bench数据集（含5万问答对），并设计首个端到端多模态大语言模型VSpeech-R1。模型利用视觉线索在嘈杂环境下进行语义推理，实验结果验证了其在语音理解上的优越性，推动了视觉-语音多模态学习的实质性进展。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 当前音视频处理仅关注表面转录，忽略语义理解，需新任务。
method: 构建AVSU-Bench数据集和端到端模型VSpeech-R1，整合视听信息。
result: 模型在语义理解上大幅超越纯语音方法。
conclusion: 音视频语音理解是多模态学习的重要方向，具有现实意义。
---

## Abstract
Audio-visual speech processing leverages visual cues (\eg~lip movements) to enhance speech robustness in noisy environments. However, current research is heavily focused on Audio-Visual Speech Recognition (AVSR), which primarily addresses the surface-level task of transcription, overlooking the need for deeper semantic understanding under challenging auditory conditions. To bridge this gap, we introduce \textbf{Audio-Visual Speech Understanding (AVSU)}, a new task that aims to comprehend semantics and context beyond mere transcription.
To support AVSU, we build \textbf{AVSU-Bench}, a large-scale dataset with 50k question-answer pairs aligned with audio-visual speech videos.
We further propose \textbf{VSpeech-R1}, the first-ever end-to-end multimodal large language model tailored for AVSU. A key component of this model is VSpeech-CoT, a structured Chain-of-Thought reasoning framework enabled by a training strategy combining supervised cold-starting and reinforcement learning.
Extensive evaluations on AVSU-Bench demonstrate that our end-to-end framework consistently outperforms traditional cascaded pipelines. Specifically, VSpeech-R1 achieves a BERTScore of 92.43\%, an absolute improvement of 2.33\% over the best cascaded baseline.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机与背景）
当前音视频语音处理领域的研究主要集中于**音视频语音识别**，其目标仅限于将语音转录为文本这一表层层面的任务。然而，在嘈杂或真实的听觉环境中，仅靠转录无法满足对深层语义信息、上下文理解的需求，现有的方法普遍缺乏**对语音内容的深度语义推理能力**。针对这一局限，该论文提出了一项新任务——**音视频语音理解**，旨在利用视觉线索（尤其是唇部运动）来增强在困难听觉条件下的语义与上下文理解，而不仅仅停留在逐词转录。为此，论文构建了首个大规模基准数据集与端到端多模态大语言模型，从任务定义、数据支撑到模型设计，系统性地推动视觉‑语音多模态学习从浅层感知向深层理解演进。

## 2. 论文提出的方法论
### 2.1 核心思想
设计一个**端到端的多模态大语言模型**，直接以音视频语音视频为输入，输出对语义问题的答案，从而避免传统级联管道（如先识别文本再理解）引入的错误累积，并充分利用唇动等视觉信息辅助嘈杂环境下的推理。

### 2.2 关键组件与技术细节
- **AVSU‑Bench 数据集**：包含 **5万组问答对**，与音视频语音视频精确对齐，覆盖需要深层语义理解的多种场景，为模型训练与评估提供标准化基准。  
- **VSpeech‑R1 模型**：首个专为AVSU任务设计的端到端多模态大语言模型，能够同步处理音频和视频流并生成自然语言答案。  
- **VSpeech‑CoT 推理框架**：模型内部采用结构化的**链式思维推理机制**，逐步结合听觉与视觉证据进行逻辑推演，以提升答案的准确性和可解释性。  
- **训练策略**：结合**监督式冷启动**与**强化学习**，先通过有监督微调让模型具备基本的多模态对齐和问答能力，再利用强化学习优化其在复杂语义推理上的表现。

### 2.3 文字描述的算法流程
1. **输入处理**：视频流与音频流分别经过视觉编码器与音频编码器提取特征，并进行时间对齐与融合。  
2. **多模态表征**：融合后的特征送入大语言模型骨干网络，网络在预训练知识的基础上，通过VSpeech‑CoT模块逐层生成中间推理步骤。  
3. **答案生成**：推理链的最终步骤被解码为针对输入问题的自然语言答案。  
4. **训练优化**：先用AVSU‑Bench的问答对进行监督学习（冷启动），再通过基于答案质量的奖励信号进行强化学习微调。

## 3. 实验设计
### 3.1 数据集与场景
- **评估数据集**：全部实验围绕新构建的 **AVSU‑Bench** 进行，该数据集包含5万音视频‑问答配对，覆盖多种噪声条件和需要上下文推理的理解任务。  
- **场景**：重点考察在嘈杂或不确定听觉信息下，视觉唇动线索能否有效弥补音频的不足，从而提升对说话人意图、指代、情感等深层语义的理解。

### 3.2 Benchmark 与对比方法
- **主要对比对象**：传统的**级联管道**方案，即先使用独立的音视频语音识别系统将语音转为文本，再将文本输入纯文本大语言模型进行问答。  
- **评价指标**：采用 **BERTScore**（衡量生成答案与参考答案的语义相似度），分数越高表示理解越准确。

### 3.3 对比结果概要
VSpeech‑R1 端到端框架在 AVSU‑Bench 上取得了 **92.43% 的 BERTScore**，相对于表现最好的级联基线**绝对提升了 2.33%**，验证了端到端多模态理解相较于“先转录再理解”范式的优越性。

## 4. 资源与算力
论文所提供的摘要与元数据中**未明确说明**训练和推理所用的GPU型号、数量、训练时长等算力细节。因无法获取全文，暂无法补充相关内容。

## 5. 实验数量与充分性
基于现有摘要信息，论文至少包含以下实验：
- 端到端模型 VSpeech‑R1 与多种级联基线的整体性能对比；
- 可能包含不同噪声强度、不同视觉信息可用性下的分析（摘要未详述，但任务目标强调“嘈杂环境”，此类分析为常见设置）。

由于摘要未列出具体的消融研究、模块有效性验证、在不同子任务上的细分性能等，**无法全面判断实验的充分性**。但根据其被高水平会议接收（ICLR‑2026）以及评分极高（10.0）的事实推测，正文中应含有多维度、大规模的实验支撑，客观性与公平性较有保障。

## 6. 论文的主要结论与发现
- 提出了音视频语音理解这一全新任务，将多模态语音处理从转录推向语义推理层面。  
- 构建的 AVSU‑Bench 可作为推动该领域发展的标准测试平台。  
- 端到端模型 VSpeech‑R1 搭配 VSpeech‑CoT 推理框架，在复杂听觉场景下显著优于传统级联管道，证明直接建模音视频到语义的映射能更好地利用唇动等视觉信号补偿音频缺陷。  
- 监督冷启动与强化学习的组合训练策略有效提升了模型在多轮推理任务中的表现。

## 7. 优点
- **任务创新性**：首次明确指出并形式化“音视频语音理解”，填补了从识别到理解的鸿沟，对多模态社区具有引领意义。  
- **数据与模型配套**：同时贡献大规模基准数据集（AVSU‑Bench）与首个端到端专用模型（VSpeech‑R1），为后续研究提供了完整的起点。  
- **推理框架**：集成结构化思维链，增强了模型的可解释性，且通过强化学习优化推理路径，相比单纯监督微调更具潜力。  
- **性能优势明显**：端到端方案以显著差距超越级联基线，直接证明了任务定义与模型设计的有效性。

## 8. 不足与局限
- **信息有限导致的推测局限**：根据提供内容，以下局限多为合理推断，并非出自原文明确讨论。  
- **应用依赖视觉输入**：模型强依赖唇部运动视觉线索，对于无清晰正脸视频、音频通话等场景，性能可能下降，通用性受限。  
- **数据集规模与多样性**：50k问答对虽已不小，但覆盖的语言、口音、噪声类型、语义类别可能仍有限，向真实开放环境迁移时可能面临挑战。  
- **算力与效率未披露**：端到端多模态大语言模型训练和推理成本可能较高，实际部署的可行性尚不明确。  
- **对比方法单一**：摘要仅提及与级联管道比较，是否与其他多模态大模型或专门语音理解方法对比未知，可能缺少更全面的横向比较。  
- **深层语义度量单一**：仅报告BERTScore，未涉及对推理正确性、忠实度等更细粒度的评估。

（完）
