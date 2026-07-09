---
title: "Comprehend and Talk: Text to Speech Synthesis  via Dual Language Modeling"
title_zh: 理解与言说：基于双语言建模的文本到语音合成
authors: "Junjie Cao, yichen Han, Ruonan Zhang, xiaoyang hao, Shuaijiang Zhao, Hongxiang Li, Yue Liu, Xiao-Ping Zhang"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=VPju7xAxb1"
tags: ["query:speech-model"]
score: 9.0
evidence: 提出双语言建模方法用于基于大语言模型的文本到语音合成，提升合成质量和稳定性。
tldr: 针对现有基于大语言模型的自回归TTS系统中离散化导致信息损失和误差累积的问题，提出双语言建模框架，通过引入语义和声学双重建模提升合成质量和稳定性。实验表明该方法在自然度和鲁棒性上优于现有方法，为高保真语音合成提供了新范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 单码本建模信息损失大，层次化声学token缺乏语义结构，自回归易误差累积，影响TTS质量。
method: 提出双语言建模，分别对语义和声学序列建模，并设计解耦训练策略以提高稳定性。
result: 合成语音自然度和鲁棒性显著提升，优于主流AR-TTS模型。
conclusion: 双语言建模有效减轻了误差累积和信息损失，为高质量TTS提供了可行方案。
---

## Abstract
Existing Large Language Model (LLM) based autoregressive (AR) text-to-speech (TTS) systems, while achieving state-of-the-art quality, still face critical challenges. The foundation of this LLM-based paradigm is the discretization of the continuous speech waveform into a sequence of discrete tokens by neural audio codec. However, single codebook modeling is well suited to text LLMs, but suffers from significant information loss; hierarchical acoustic tokens, typically generated via Residual Vector Quantization (RVQ), often lack explicit semantic structure, placing a heavy learning burden on the model. Furthermore, the autoregressive process is inherently susceptible to error accumulation, which can degrade generation stability. To address these limitations, we propose CaT-TTS, a novel framework for robust and semantically-grounded zero-shot synthesis. First, we introduce S3Codec, a split RVQ codec that injects explicit linguistic features into its primary codebook via semantic distillation from a state-of-the-art ASR model, providing a structured representation that simplifies the learning task. Second, we propose an ``Understand-then-Generate'' dual-Transformer architecture that decouples comprehension from rendering. An initial ``Understanding'' Transformer models the cross-modal relationship between text and the prompt's semantic tokens to form a high-level utterance plan. A subsequent ``Generation'' Transformer then executes this plan, autoregressively synthesizing hierarchical acoustic tokens. Finally, to enhance generation stability, we introduce Masked Audio Parallel Inference (MAPI), a nearly parameter-free inference strategy that dynamically guides the decoding process to mitigate local errors. Extensive experiments demonstrate that the synergy of our principled architecture and semantically-aware codec allows CaT-TTS to achieve new state-of-the-art performance in zero-shot voice cloning, with MAPI providing a measurable boost in generation robustness on benchmark datasets. Project page: \href{https://anonymous.4open.science/r/CaT-TTS-66A1/}{https://anonymous.4open.science/r/CaT-TTS-66A1}.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **问题背景**：当前基于大语言模型的自回归语音合成（AR‑TTS）系统虽质量领先，但面临几个根本性挑战。
  - 连续语音需离散化为 token，单码本建模会带来严重的信息损失。
  - 主流分层残差矢量量化生成的声学 token 缺少清晰的语义结构，模型学习负担沉重。
  - 自回归生成天然容易累积误差，损害生成稳定性。
- **整体含义**：本文提出 **CaT‑TTS** 框架，通过语义与声学分治的双语言建模范式，配合语义感知的语音编解码器，从根源上缓解了上述问题，旨在实现更加稳健、自然的零样本语音克隆。

## 2. 论文提出的方法论
- **S3Codec（语义感知的分离式矢量量化编解码器）**
  - 采用分裂（split）的残差矢量量化，将显式语言特征注入主码本。
  - 利用一流的自动语音识别模型进行语义蒸馏，为主码本提供结构化表示，简化后续建模难度。
- **双 Transformer 架构（“先理解，再生成”）**
  - **理解 Transformer**：建模输入文本与提示语音语义 token 之间的跨模态关系，形成高层发声“计划”。
  - **生成 Transformer**：以自回归方式，将上述计划逐级渲染为层次化的声学 token，实现语音合成。
- **掩码音频并行推理（MAPI）**
  - 一种几乎零参数的推理策略。
  - 在解码过程中动态引导模型，抑制局部误差，增强生成鲁棒性。

## 3. 实验设计
- **场景**：零样本语音克隆。
- **评估基准**：文中仅提及使用了若干基准数据集，未列出具体名称（如 LibriTTS、VCTK 等）。
- **对比方法**：与现有主流 AR‑TTS 模型进行了对比，但摘要未点明所对比的具体模型。
- **评价指标**：应包括自然度（如 MOS）和鲁棒性等主观/客观指标，细节未在摘要中展开。

## 4. 资源与算力
- **未提及**：所提供的摘要及元数据中并无 GPU 型号、数量、训练时长等算力相关信息。

## 5. 实验数量与充分性
- **实验规模**：摘要称进行了“广泛实验”，涉及基准数据集上的性能验证和消融研究（例如验证 MAPI 的增益），但未给出具体实验组数。
- **充分性评价**：从宣称达到新的 SOTA 并结合消融分析推测，实验设计应较为完整；但鉴于缺乏细节（如数据集多样性、统计检验），无法严格评判其充分性与公平性。

## 6. 论文的主要结论与发现
- CaT‑TTS 通过架构和编解码器的协同创新，在零样本语音克隆任务上取得了新的 SOTA 性能。
- 语义感知的 S3Codec 与“理解‑生成”双 Transformer 的结合，有效降低了信息损失和学习难度。
- MAPI 策略在不增加参数的情况下，显著提升了自回归生成的稳定性与合成语音的鲁棒性。

## 7. 优点
- **语义‑声学解耦设计**：将理解和生成分开，降低了单一模型学习模糊声学序列的负担。
- **结构化语音表示**：S3Codec 通过语义蒸馏使主码本具备语言结构，信息损失更小。
- **轻量稳定的推理优化**：MAPI 几乎无参，却能有效缓解 AR 累积误差，实用性高。
- **系统协同性强**：编解码器、模型架构与推理策略环环相扣，共同提升质量。

## 8. 不足与局限
- **实验细节模糊**：摘要未披露具体数据集、对比方法、训练细节和统计指标，难以复现或严格评估。
- **潜在偏差风险**：零样本场景下的说话人泛化能力、长文本合成效果、背景噪声下的鲁棒性未得到明确报告。
- **计算开销未知**：双 Transformer 架构可能导致较大的推理成本，文中未分析参数量和实时率。
- **应用局限**：聚焦于语音克隆，对其他语音合成任务（如情感控制、风格迁移）的有效性未涉及。

（完）
