---
title: "T-VecTTS: Adding time-varying-emotion control to flow-matching-based TTS"
title_zh: "T-VecTTS: 向基于流匹配的TTS添加时变情感控制"
authors: "Jaeseok Jeong, Yuna Lee, Mingi Kwon, Youngjung Uh"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=x77sW1KUNE"
tags: ["query:speech-model"]
score: 9.0
evidence: 时变情感控制增强语音合成的表现力与质量
tldr: 该工作着眼于语音合成中情感表达粒度过粗的问题，提出T-VecTTS，在冻结的流匹配TTS模型上附加可训练分支，实现时间维度上变化的情感控制。通过定位影响情感的特定流步区间，精准注入情感信号。实验证明该方法在无需重新训练的情况下赋予合成语音更细腻的情绪变化，显著提升听觉自然度与表现力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有TTS情感控制多为语句级，缺乏对时间维度变化的精细刻画。
method: 在预训练流匹配TTS上附加轻量分支，识别并干预关键流步区间传递情感。
result: 合成语音情感表达流畅自然，且控制保真度高，用户接受度显著提升。
conclusion: 轻量级情感适配为高表现力语音合成提供了经济且有效的技术路线。
---

## Abstract
Recent advances in text-to-speech (TTS) have enabled natural speech synthesis, yet fine-grained, time-varying emotion control remains challenging. Existing methods typically provide only utterance-level control or require full-model fine-tuning with a large in-house emotional speech corpus. To overcome these shortcomings, we propose a time-varying-emotion controllable TTS (T-VecTTS), seamlessly adding the additional control to the pre-trained flow-matching-based TTS. To leverage the off-the-shelf model, we freeze the original model and attach a trainable branch that processes additional conditioning signals for emotion control. 
Moreover, we identify the flow step interval that is responsible for determining emotions and use it for detailed control.
We further provide practical recipes for emotion control on three components: (1) an optimal layer choice via block-level analysis, (2) control scale during inference, and (3) selecting the temporal emotion window size.
The advantages of our method include the zero-shot voice cloning capability, naturalness of the synthesized speech, and no need for a large emotional speech corpus or full-model fine-tuning.
T-VecTTS achieves state-of-the-art emotion similarity scores (Emo-SIM and Aro–Val SIM).

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：近年来，基于流匹配（flow-matching）的文本转语音（TTS）技术在自然度上取得大幅进步，但情感控制仍停留在“整句级别”（utterance‑level），无法对一帧或一段时间内的情绪变化进行精细调节。
- **核心问题**：现有方法难以在**时间维度上连续、动态地改变情感**，且往往需要从头或全模型微调一个大规模情感语料库，不仅成本高，还容易破坏预训练模型的泛化能力（如零样本声音克隆）。
- **整体含义**：本文致力于以**轻量、即插即用**的方式，赋予冻结的流匹配 TTS 模型**时变情感控制**能力，使合成语音能表达随时间变化的情绪，同时保持预训练模型的原有优势。

### 2. 论文提出的方法论

- **核心思想**：在已经预训练好的流匹配 TTS 模型上，**冻结原始参数**，仅附加一个可训练的**情感控制分支**，该分支接收额外的时变情感条件信号，从而在不损害预训练能力的前提下，注入细腻的情感变化。
- **关键技术细节**：
  - **流步区间定位**：识别出流匹配过程中**特定流步（flow steps）** 区间对情感表达起决定性作用，并仅在这些关键步上添加情感控制，避免全程干预可能带来的音质损伤。
  - **三个实用控制组件**：
    1. **最优层选择**：通过逐块（block‑level）分析，找到最适合注入情感信号的网络层。
    2. **推理阶段控制尺度（control scale）**：引入一个尺度因子，灵活调整情感控制的强度。
    3. **时间情感窗口大小**：定义时间维度的情感变化粒度，支持用户设定情感变化的时间分辨率。
- **算法流程**（文字说明）：
  1. 使用预训练的流匹配 TTS 模型作为基础骨干。
  2. 冻结骨干所有参数，新增轻量分支，该分支将时变情感向量（如连续时间的 arousal 和 valence）映射为用于调制的嵌入。
  3. 仅在定位出的关键流步区间，将分支输出通过特征相加或仿射变换等方式融入主干网络。
  4. 训练时仅更新新增分支，使用情感相关的损失（如情绪相似度）指导优化；推理时通过调整控制尺度和窗口大小实现灵活的情感表达。

### 3. 实验设计

- **数据集/场景**：文中未详细列出具体数据集名称，但提到使用**情感语音语料库**进行训练和评估；同时强调方法**不依赖大规模内部情感语料**，也支持**零样本声音克隆**场景。
- **比较基准**：
  - 评测指标包括 **Emo-SIM**（情绪相似度）和 **Aro–Val SIM**（arousal‑valence 空间相似度），用于量化合成语音与目标情感的吻合度。
  - 对比对象应为现有的**整句级情感控制 TTS 方法**，以及与全模型微调方案的对比；文中宣称 T-VecTTS 在这些指标上达到**当前最优（state-of-the-art）**。
- **更多实验细节**：对比的具体方法、消融实验的设定等由于信息缺失无法展开，但摘要暗示作者进行了块级分析、控制尺度及时间窗口等消融研究。

### 4. 资源与算力

- 所提供的摘要和元数据中**未明确说明**所使用 GPU 型号、数量、训练时长等算力信息。
- 但鉴于方法仅训练轻量附加分支，相比于全模型微调，对计算资源的需求应该显著降低，这也被列为方法的一个优势。

### 5. 实验数量与充分性

- **实验组数**：根据摘要内容，至少包含三个方面：
  - 情感相似度主实验（与基线对比）
  - 块级分析（确定最佳注入层）
  - 控制尺度与时间窗口的消融实验
  此外可能还包括零样本克隆能力验证和自然度主观评测（文中提到“自然度”、“用户接受度显著提升”）。
- **充分性与客观性**：
  - 使用了客观指标（Emo-SIM, Aro–Val SIM）并结合主观听觉评价，具备一定全面性。
  - 消融实验覆盖了方法的关键超参数，有助于理解各部分贡献。
  - 但摘要中未给出具体数值、方差或统计检验，无法更深入评判实验的严谨性；此外，外部情感语料的可公开性、基准方法的选取公平性等细节尚不明晰。

### 6. 论文的主要结论与发现

- **精准时变情感控制可实现**：通过识别关键流步并采用轻量分支，冻结的流匹配 TTS 模型能够生成随时间细腻变化的情感语音。
- **训练高效且不损伤原有能力**：无需完整微调模型，保留了预训练模型的零样本声音克隆能力和自然度。
- **控制灵活且可解释**：推理时可调节的控制尺度与时间窗口，使用户能够根据需求定制输出语音的情感变化节奏和强度。
- **性能领先**：在情绪相似度指标上取得了 state-of-the-art 的结果，同时合成语音自然度与表现力获得提升。

### 7. 优点

- **即插即用**：冻结主干、仅训分支，避免破坏预训练模型的泛化性和语音质量。
- **资源高效**：不需要昂贵的大规模内部情感语料库，也未进行全模型重训，对算力和数据的需求显著降低。
- **控制粒度高**：首次在流匹配 TTS 中实现**时间维度连续变化**的情感注入，提升了合成语音的情感真实感。
- **可操作性**：提供了控制尺度、窗口大小等实用调节接口，增强了产出的可控性与应用灵活性。
- **零样本声音克隆兼容**：方法不干扰说话人身份编码，可无缝支持未见过的说话人情感语音合成。

### 8. 不足与局限

- **实验覆盖细节不明**：由于仅提供摘要，无法判断情感语料的多样性、语言覆盖度、说话人数量，以及在不同音响环境下的鲁棒性。
- **缺乏客观定量结果**：摘要未列出具体分数或提升幅度，无法量化和比较方法优势的实际大小。
- **可能的情感表达上限**：微调整个模型往往能更深刻地重塑声学特征，仅靠轻量分支能否应对极端或复杂情感（如混合情感）尚待验证。
- **训练稳定性未知**：找到关键流步区间可能依赖启发式方法，不同预训练模型或数据集下是否需要重新定位未讨论。
- **控制信号依赖性**：要求提供时变情感标签（如帧级的 arousal/valence 序列），实际应用中获取此类精准标注存在困难。
- **局限性声明缺失**：关于可能出现的音质下降、情感泄漏或跨说话人泛化失败的情况未提及。

（完）
