---
title: "SpeechJudge: Towards Human-Level Judgment for Speech Naturalness"
title_zh: SpeechJudge：迈向语音自然度的人类水平评判
authors: "Xueyao Zhang, Chaoren Wang, Huan Liao, Ziniu Li, Yuancheng Wang, Li Wang, Dongya Jia, Yuanzhe Chen, Xiulin LI, Zhuo Chen, Zhizheng Wu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=I9ED9VWZq6"
tags: ["query:speech-model"]
score: 9.0
evidence: 语音自然度奖励模型和数据集，用于对齐语音合成与人类偏好
tldr: 语音合成模型与人类感知对齐受限于缺乏大规模自然度偏好数据。本文发布SpeechJudge-Data（9.9万对语音人类反馈）并训练奖励模型，能够准确评判合成语音的自然度。该工作为语音合成领域建立了人类感知对齐的基础设施，有望显著提升TTS模型质量。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 缺乏大规模语音自然度人类偏好数据，阻碍语音合成模型与人类感知对齐。
method: 构建大规模人类反馈数据集SpeechJudge-Data，并训练奖励模型来评判语音自然度。
result: 奖励模型能有效预测人类偏好，提升TTS模型对齐质量。
conclusion: 大规模自然度评判数据和模型是语音合成质量提升的关键。
---

## Abstract
Aligning large generative models with human feedback is a critical challenge. In speech synthesis, this is particularly pronounced due to the lack of a large-scale human preference dataset, which hinders the development of models that truly align with human perception. To address this, we introduce ***SpeechJudge***, a comprehensive suite comprising a dataset, a benchmark, and a reward model centered on naturalness—one of the most fundamental subjective metrics for speech synthesis. First, we present ***SpeechJudge-Data***, a large-scale human feedback corpus of 99k speech pairs. The dataset is constructed using a diverse set of advanced zero-shot text-to-speech (TTS) models across diverse speech styles and multiple languages, with human annotations for both intelligibility and naturalness preference. From this, we establish ***SpeechJudge-Eval***, a challenging benchmark for speech naturalness judgment. Our evaluation reveals that existing metrics and AudioLLMs struggle with this task; the best-performing model, Gemini-2.5-Flash, achieves less than 70% agreement with human judgment, highlighting a significant gap for improvement. To bridge this gap, we develop ***SpeechJudge-GRM***, a generative reward model (GRM) based on Qwen2.5-Omni-7B. It is trained on SpeechJudge-Data via a two-stage post-training process: Supervised Fine-Tuning (SFT) with Chain-of-Thought rationales followed by Reinforcement Learning (RL) with GRPO on challenging cases. On the SpeechJudge-Eval benchmark, the proposed SpeechJudge-GRM demonstrates superior performance, achieving 77.2% accuracy (and 79.4% after inference-time scaling @10) compared to a classic Bradley-Terry reward model (72.7%). Furthermore, SpeechJudge-GRM can be also employed as a reward function during the post-training of speech generation models to facilitate their alignment with human preferences.

---

## 论文详细总结（自动生成）

# 论文总结：SpeechJudge: Towards Human-Level Judgment for Speech Naturalness

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前语音合成模型（TTS）在自然度上难以与人类感知对齐，主要瓶颈在于缺乏大规模、高质量的人类偏好数据，导致奖励信号不足，难以训练出能准确判断语音自然度的模型。
- **研究动机**：现有语音质量评估指标（如MOS预测模型）和通用音频语言模型（AudioLLMs）在自然度评判任务上与人类判断的一致性不足，难以作为可靠的自动评估手段或奖励函数来指导TTS模型的优化。因此，构建大规模、覆盖多样风格和语种的人反馈数据集，并训练专门的奖励模型，成为推动语音合成对齐人类感知的关键。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：以语音自然度为核心主观指标，构建大规模人类反馈数据集（SpeechJudge-Data），并在此基础上训练一个生成式奖励模型（SpeechJudge-GRM），使其能够像人类一样评判语音自然度，甚至可作为TTS后训练阶段的奖励函数。
- **关键技术细节**：
  - **SpeechJudge-Data**：包含99,000对语音样本，由多种先进的零样本TTS模型生成，覆盖不同语音风格和多种语言；每条样本附带人类标注，包括可懂度和自然度偏好。
  - **SpeechJudge-Eval**：从上述数据中构建的挑战性基准，用于评测模型对语音自然度判断的能力。
  - **SpeechJudge-GRM**：基于Qwen2.5-Omni-7B的生成式奖励模型，采用两阶段后训练策略：
    - **第一阶段（监督微调, SFT）**：使用带思维链（Chain-of-Thought）解释的标注数据，让模型学会生成判断理由。
    - **第二阶段（强化学习, RL）**：采用GRPO算法，在标注困难样本上进一步优化模型，提升对人类偏好的拟合能力。
- **公式或算法流程**（文字描述）：
  - 第一阶段SFT：模型输入语音对，输出自然度比较判断及理由，通过监督学习优化生成概率。
  - 第二阶段RL：基于比较判断的准确率信号，使用GRPO对模型进行策略优化，尤其关注易错/困难样本。
  - 推理时还支持test-time scaling，通过多次采样/投票提升最终准确率。

## 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法
- **数据集**：SpeechJudge-Data（99k对语音）及其派生的SpeechJudge-Eval基准。
- **评测场景**：语音自然度偏好判断（二选一比较）。
- **对比方法**：
  - 传统的客观指标（如可能包括MOSNet等，但具体名称未给出）。
  - 通用音频语言模型（AudioLLMs），表现最好的Gemini-2.5-Flash仅达到不足70%的人类一致性。
  - 经典的Bradley-Terry奖励模型（准确率72.7%）。
- **主要指标**：与人类判断的一致性（准确率）。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力；若未明确说明，也请指出这一点
- **未明确说明**：提供的摘要和元数据中未提及GPU型号、数量、训练时长等算力信息。

## 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- **实验数量**：
  - 主要对比实验：SpeechJudge-GRM vs. Bradley-Terry奖励模型 vs. Gemini等AudioLLMs。
  - 可能包含不同训练阶段（SFT、RL）的效果对比，以及推理时缩放（test-time scaling @10）的实验。
- **充分性与客观性**：
  - 对比基线覆盖传统指标和大型音频语言模型，较为全面。
  - 使用大规模、多样化的自建数据集，减少领域偏差。
  - 但未提及与其他基于人类偏好的TTS奖励模型（如NISQA、SpeechLMScore等）的对比，可能存在对比覆盖不足的风险。

## 6. 论文的主要结论与发现
- **SpeechJudge-GRM在自然度评判上显著优于现有方法**：
  - 准确率达到77.2%，超过Gemini-2.5-Flash（<70%）和Bradley-Terry奖励模型（72.7%）。
  - 推理时扩展（@10）可将准确率进一步提升至79.4%。
- **Chain-of-Thought和RL训练策略对困难样本有增益**。
- **该奖励模型可以作为语音生成模型后训练阶段的奖励函数**，用于将生成模型与人类偏好对齐，印证了通用奖励模型在语音领域的可行性。

## 7. 优点：方法或实验设计上有哪些亮点
- **首个大规模、多语种、多风格的语音自然度人类反馈数据集**，弥补了社区空白。
- **采用生成式奖励模型（GRM）并引入思维链**，对判断过程具备一定可解释性。
- **两阶段训练（SFT+RL）策略**，尤其在困难样本上通过强化学习提升表现，具有创新性。
- **不仅作为评判工具，还可直接作为TTS优化的奖励函数**，展现出实际应用潜力。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖**：未提及与传统语音质量评估指标（如PESQ、STOI、NISQA等）的对比，也未讨论在更多下游TTS模型上的对齐效果。
- **偏差风险**：数据构建依赖特定的零样本TTS模型集合，可能引入对特定声学特征的系统性偏好；人类标注者文化、语言的多样性是否充分控制未说明。
- **应用限制**：奖励模型基于7B规模的多模态模型，推理成本较高，可能限制实时应用场景；通用性方面，是否适用于非英语/非朗读风格的语音（如情感语音、对话语音）有待验证。
- **自然度之外**：仅关注自然度，未涵盖音色相似度、情感表达等其他主观维度。

（完）
