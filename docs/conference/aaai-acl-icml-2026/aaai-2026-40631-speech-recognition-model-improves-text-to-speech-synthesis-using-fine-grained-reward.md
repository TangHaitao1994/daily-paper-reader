---
title: Speech Recognition Model Improves Text-to-Speech Synthesis Using Fine-Grained Reward
title_zh: 利用语音识别模型细粒度反馈提升文本到语音合成
authors: "Guansu Wang, Peijie Sun"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40631/44592"
tags: ["query:speech-model"]
score: 9.0
evidence: 利用ASR模型提供词级别不匹配检测，作为TTS训练的细粒度奖励
tldr: 当前TTS合成质量高，但评估方法仍依赖整段语音的MOS评分，难以定位局部错误。本文发现Whisper等ASR模型能精准捕获词级语音-文本不匹配，提出利用该能力生成细粒度奖励来优化TTS训练。实验证明该方法可有效提升合成语音的局部准确性和整体自然度，为TTS质量增强提供了新的技术思路。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有TTS评估技术滞后，传统MOS评分模型对整个语音段回归评分，无法捕捉孤立词级别的合成失败。
method: 发现Whisper等编码器-解码器ASR模型可在交叉注意力中精确捕获词级别语音与文本不匹配，据此提出细粒度奖励信号用于TTS训练优化。
result: 实验表明该方法能显著提升合成语音的自然度和准确性。
conclusion: 将ASR模型的细粒度错误检测能力用于TTS训练，为语音合成质量提升开辟了新途径。
---

## Abstract
Recent advancements in Text-to-Speech (TTS) technology have been remarkable, enabling current models to clone arbitrary unseen speakers and synthesize high-quality, natural-sounding speech. However, corresponding evaluation techniques appear to be lagging: existing Mean Opinion Score (MOS) estimation models typically perform regression-based scoring on entire speech segments, while a failed synthesized speech usually contains problematic elements in only a few isolated words rather than throughout the entire utterance. In this context, we present an intriguing finding: encoder-decoder ASR models, such as Whisper, leverage their extensive pre-training to precisely capture word-level mismatches between speech and text within their cross-attention mechanisms, thereby providing a fine-grained reward signal. Building upon this insight, we propose a novel TTS optimization method, which we term Word-level TTS Alignment by ASR-driven Attentive Reward (W3AR). Instead of relying on any explicit reward annotations, W3AR leverages the attention information within a pre-trained ASR model, enabling finer-grained alignment and optimization of the sequences predicted by the TTS model. Experimental results demonstrate that W3AR not only effectively improves the TTS generation quality of existing models but also further enhances zero-shot robustness based on both in-domain and out-of-domain prompt speakers. Additionally, our findings and proposed methodology offer a new insight for generative tasks: understanding models can potentially serve as evaluators, providing highly fine-grained and valuable feedback for generative optimization.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有文本到语音（TTS）评估技术滞后于合成能力的进步。主流评估（如MOS估计）只对整个语音段给出整体分数，而合成失败通常仅发生在个别单词上（如错读、重复、声学畸变），无法提供细粒度反馈以指导优化。
- **研究动机**：如何精确定位合成语音中的问题片段，进而为自回归TTS模型提供 **词级别、可解释的奖励信号**，从而实现更有针对性的后训练优化。
- **整体含义**：提出一种“理解模型可作为高细粒度评估器”的新范式。利用预训练语音识别（ASR）模型内部的跨注意力机制，从“倾听者”视角量化合成词质量，形成无需人工标注的奖励信号，驱动TTS策略提升。

### 2. 论文提出的方法论
- **核心思想**：将预训练好的编码器-解码器ASR模型（Whisper）作为一个客观的“评判者”。将合成语音输入ASR编码器，并用正确文本作为解码器输入（Teacher-forcing），提取交叉注意力图。通过分析注意力的分布特性，计算出每个目标词的两个质量指标：**注意力纯度**和**对齐单调性**，加权组合成词级奖励。然后在一个组相对策略优化框架下用该奖励更新TTS模型。
- **关键技术细节（公式与流程用文字说明）**：
  - **注意纯度 (Attention Purity)**：衡量合成某个词时，ASR解码器注意力在时间轴上的集中程度。对第t个文本token，找到注意力峰值所在的音频帧 j∗_t，计算以该峰值为中心、宽度为W的小窗口内所有权重之和。值越高代表发音越清晰、不模糊。
  - **对齐单调性 (Alignment Monotonicity)**：衡量相邻词的注意力峰值是否向前平稳移动。计算 j∗_t 相对于前一峰值的位移，并经过 tanh 缩放，惩罚停滞或后退（对应的韵律不自然或卡顿）。
  - **组合奖励**：`R(词) = λ_纯度 × 纯度奖励 + λ_单调性 × 单调性奖励`。
  - **策略优化**：基于组相对策略优化。对同一输入生成 N 个候选语音样本（N=8），为每个样本的每个词计算奖励，然后计算词级优势函数 `advantage = 该词奖励 - 组内平均奖励`。最终RL损失鼓励模型提升具有正优势的词所对应的声学序列概率，同时加入KL散度约束与冻结参考模型保持稳定。

### 3. 实验设计
- **数据集 / 场景**：
  - **基础TTS模型**：CoSyVoice（主实验），训练于约17万小时多语者音频。
  - **优化数据集**：文本来自 LibriTTS；提示音频从 LibriTTS 训练集抽取 2500 条短片段（3–6秒），2000 条用于优化，500 条用于域内评估。
  - **域外评估**：额外从 Emilia 和 GigaSpeech 各取 2500 条样本构建域外说话人库。
- **对比方法（Benchmark）**：
  - **基线**：未优化的 CoSyVoice。
  - **其他TTS优化方法**：SpeechAlign（话语级偏好对比）、UNO（话语级，忽略不确定性系数）、FPO（首个考虑细粒度问题的选择性训练方法）。
  - **跨模型验证**：VoiceCraft、MaskGCT。
- **评估指标**：
  - **客观指标**：WER（用 Whisper-medium 转写）、说话人相似度（SECS）、预测自然度（UTMOS）、差例率（BC）。
  - **主观指标**：自然度 MOS-N、相似度 MOS-S（5分制，20名英语母语者）；AB偏好测试（100对样本，含平局选项）。

### 4. 资源与算力
- **计算硬件**：使用 **2 块 NVIDIA A100 GPU** 进行训练。
- **优化配置**：AdamW 优化器，学习率 2e-5，余弦衰减，预热 2000 步；组大小 N=8；RL 损失权重 γ=0.1。论文未明确给出完整训练时长或总迭代轮次。

### 5. 实验数量与充分性
- **实验组数**：包含三张主要结果表格和一份主观 AB 测试图。
  - 表1：主结果（域内 vs 域外，客观+主观）。
  - 表2：消融实验（去掉纯度/单调性/组相对优化）+ 与其他优化方法对比（SpeechAlign、UNO、FPO）。
  - 表3：跨模型泛化（VoiceCraft、MaskGCT）。
  - 图2：人工 AB 偏好测试。
- **充分性与公平性评估**：
  - 实验覆盖了域内、域外两种分布，兼顾客观与主观指标，并对多个竞争性基线进行了复现和比较，整体设计较全面。
  - 消融实验直接验证了各组件（纯度、单调性、组优化）的必要性。
  - 跨模型实验证明了方法的模型无关性，不是只针对单一架构特化。
  - 所有优化方法统一基于 CoSyVoice 的基础上进行，比较条件相对公平，且注意排除了不开放源代码方法中可能影响性能的模块（如 UNO 的不确定性系数）。

### 6. 论文的主要结论与发现
- **W3AR 显著提升合成质量**：在域内和域外测试中均大幅降低 WER（域外从 8.92 降至 4.54），并提高自然度和说话人相似度。
- **域外鲁棒性突出**：W3AR 对未见说话人的改善尤为明显，将域外自然度 MOS 提升至与域内基线相当的水平。
- **组件必要性**：注意纯度和单调性互补，缺一不可；组相对优化策略对稳定训练、保证泛化至关重要。
- **模型无关性**：W3AR 可成功应用于架构不同的 VoiceCraft 和 MaskGCT，是一种通用的后训练优化层。
- **范式启示**：“理解模型”的内部感知可被蒸馏为细粒度、可解释的奖励，用于增强另一个生成模型的质量。

### 7. 优点：方法或实验设计上的亮点
- **细粒度奖励无监督**：利用 ASR 内部注意力，无需任何额外的人工标注或配对比较数据，即可获得词级反馈。
- **捕捉歧义而非简单错误**：奖励不是仅基于 WER 的二元判断，而是通过注意力分散和单调性变化，捕捉模糊发音或韵律不畅等更细微的缺陷。
- **优化框架稳定**：采用组内相对优势去均值作为基线，有效降低奖励方差，避免了早期强化学习中的不稳定问题。
- **泛化验证严谨**：不只在单一模型和域内数据上有效，还通过域外说话人和两种不同架构的模型展示了强泛化性。
- **实验对比全面**：同时与话语级和最新的细粒度 TTS 优化方法（如 FPO）进行比较，突出自身优势。

### 8. 不足与局限
- **计算开销**：优化过程中需要对每个输入生成 N 个候选样本并分别送入 ASR 计算奖励，显著增加了训练时的算力和时间成本。
- **ASR模型的依赖与偏置**：奖励信号完全取决于所选 ASR 模型（Whisper-large-v2）的表现和内在偏置，若 ASR 对某些口音、噪声或语音特征不鲁棒，奖励可能引入系统性偏差。
- **超参数敏感性**：窗口大小 W、单调性缩放系数 β、权重 λ 等可能需针对不同 TTS 模型或语言环境重新调整，文中未提供参数鲁棒性分析。
- **任务与语言局限**：实验均在英文数据集（LibriTTS, Emilia, GigaSpeech）上开展，未涉及多语言或低资源语音场景的验证。
- **未与人类反馈（RLHF）直接对比**：虽然优于部分自动化基线，但没有与基于真实人类偏好的RLHF方法进行直接比较，难以判断其与人类评价标准的一致性上限。
- **仅优化自回归模块**：方法聚焦于自回归阶段的声学token生成，对后续的流匹配或非自回归重建模块未作考量，可能遗漏部分质量改进空间。

（完）
