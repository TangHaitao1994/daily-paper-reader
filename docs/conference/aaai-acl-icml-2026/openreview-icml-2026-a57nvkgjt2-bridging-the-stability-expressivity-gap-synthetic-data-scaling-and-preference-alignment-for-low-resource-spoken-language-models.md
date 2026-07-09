---
title: "Bridging the Stability-Expressivity Gap: Synthetic Data Scaling and Preference Alignment for Low-Resource Spoken Language Models"
title_zh: 弥合稳定性-表达力鸿沟：低资源语音语言模型的合成数据扩展与偏好对齐
authors: "Yizhong Geng, Yanliang Li, Jinghan Yang, Tianhan Jiang, Boxun An, Ya Li, Xiaoyu Shen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/13d6b6e86a14d86828c14c3a23a8b4f26611e154.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 偏好对齐方法在低资源语音合成中平衡准确性和表达力
tldr: 针对低资源语言语音合成中合成数据扩展导致的语音准确性与韵律表达力失衡问题，本文提出稳定性-表达力鸿沟概念。研究发现在数据扩展时合成数据提升音素准确率却严重压制韵律多样性。为此，提出偏好对齐方法，通过精心设计的偏好信号协调准确性与表达力。实验表明该方法能有效恢复语音的韵律自然度，为低资源语音合成质量提升提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 低资源语音语言模型依赖合成数据扩展时，虽提升语音准确性却抑制韵律变化，导致表达力大幅下降。
method: 提出偏好对齐方法，在合成数据扩展中平衡语音准确性与韵律多样性，缓解稳定性-表达力鸿沟。
result: 该方法在不牺牲准确性的前提下，显著恢复了合成语音的韵律自然度。
conclusion: 为低资源场景下高表现力语音合成提供了切实可行的优化策略。
---

## Abstract
Spoken Language Models (SLMs) have emerged as a promising paradigm for speech synthesis by bypassing explicit grapheme-to-phoneme pipelines. However, their effectiveness in low-resource languages remains fundamentally limited by the scarcity of transcribed speech. In practice, synthetic data has become the primary strategy for scaling SLMs in such settings, providing reliable phonetic supervision when real data is insufficient. In this work, we show that this reliance introduces a fundamental trade-off, which we term the Stability-Expressivity Gap: while synthetic data improves phonetic accuracy, it progressively suppresses prosodic variability, ultimately leading to a collapse of expressivity (Synthetic Erosion). To bridge this gap, we propose two self-alignment frameworks. Disentanglement-Guided Self-Alignment (DGSA) recovers expressivity for complex languages by exploiting prosody-timbre separation. For regimes where authentic references are exceptionally limited, Temperature-Driven Self-Critique (TDSC) stabilizes generation through automated exploration and filtering. Our approach outperforms strong commercial systems, including ElevenLabs and Gemini Pro, and enables the first zero-shot voice cloning capability for Lao. Audio Samples are available at: https://luoji.cn/static/multilantts-demo-main/.

---

## 论文详细总结（自动生成）

# 论文总结：《弥合稳定性-表达力鸿沟：低资源语音语言模型的合成数据扩展与偏好对齐》

## 1. 论文的核心问题与整体含义
- **研究背景**：口语语言模型（Spoken Language Models, SLMs）跳过了显式的字音转换，成为语音合成的新范式。但在低资源语言中，由于真实转录语音稀缺，必须依赖合成数据来扩展模型。
- **核心矛盾**：合成数据的使用带来一个根本性权衡——虽然提升了语音的音素准确性，却逐渐压制了韵律可变性，最终导致表达力坍缩。作者将这一现象命名为**稳定性-表达力鸿沟（Stability-Expressivity Gap）**，并指出合成数据造成的表达力退化（Synthetic Erosion）。
- **整体含义**：如何在低资源场景下，既保证语音合成的准确性（稳定性），又保留自然的韵律多样性（表达力），是本文要解决的关键问题。

## 2. 论文提出的方法论
- **核心思想**：通过设计偏好对齐（preference alignment）框架，在合成数据扩展中主动协调准确性与表达力，从而弥合稳定性-表达力鸿沟。
- **两种自对齐框架**：
  - **解耦引导自对齐（Disentanglement-Guided Self-Alignment, DGSA）**：适用于有一定真实参考的语言。利用韵律-音色分离技术，将表达力从音色中解耦，再以解耦后的信号引导对齐过程，恢复复杂语言的表达力。
  - **温度驱动自批判（Temperature-Driven Self-Critique, TDSC）**：适用于真实参考极度稀缺的场景。通过自动探索（如生成采样的温度控制）与过滤机制，稳定生成质量，无需外部参考。
- **技术特点**：方法属于自对齐（self-alignment），不依赖外部标注或强监督；偏好信号的设计是关键，旨在平衡音素准确度和韵律多样性。

## 3. 实验设计
- **数据集/场景**：
  - 主要针对低资源语言，文中所提具体语言为**老挝语（Lao）**。
  - 实验涵盖了不同资源程度的设定，包括有一定真实参考和参考极度稀缺两种场景。
- **Benchmark与对比方法**：
  - 对比了强商业系统，如 **ElevenLabs** 和 **Gemini Pro**。
  - 实现了**老挝语的首次零样本语音克隆（zero-shot voice cloning）**，表明方法在极端低资源下的突破。
- **评估维度**：音素准确性（稳定性）与韵律自然度（表达力），两者需要兼顾评估。

## 4. 资源与算力
- **文中所提**：摘要及元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力信息。
- **推断**：作为一篇提出新方法并对比商业系统的研究，该工作应涉及模型训练与合成数据生成，但具体资源消耗需参阅论文全文。

## 5. 实验数量与充分性
- **已提及的实验**：
  - 对比商业系统（ElevenLabs、Gemini Pro）。
  - 两种自对齐框架（DGSA 和 TDSC）分别在相应设定下验证。
  - 老挝语零样本语音克隆，作为低资源的极端案例。
  - 摘要中虽未罗列消融实验，但通常此类研究会包含合成数据规模、对齐策略、解耦模块等方面的消融分析。
- **充分性与公平性**：
  - 对比对象包含业界领先的商业系统，具有较强的说服力。
  - 场景覆盖了“有一定参考”和“极度稀缺”两种资源条件，设计较为全面。
  - 由于仅提供摘要，无法判断实验的统计稳健性（如多次运行的方差）以及消融实验的细致程度，但论文接收于 ICML-2026 且评分 8.0，可推测实验证据较为充分。

## 6. 论文的主要结论与发现
- 合成数据扩展在低资源 SLM 中会导致表达力显著下降，形成稳定性-表达力鸿沟。
- 提出的偏好对齐方法（DGSA 和 TDSC）能够在不牺牲音素准确性的前提下，显著恢复合成语音的韵律自然度。
- 方法不仅超越了 ElevenLabs、Gemini Pro 等强基线，还首次实现了老挝语的零样本语音克隆，证明了其在低资源场景下的有效性。
- 为低资源语音合成提供了一条切实可行的优化路径，即在数据扩展中主动嵌入表达力偏好，而非仅追求准确性。

## 7. 优点
- **概念创新**：明确提出“稳定性-表达力鸿沟”和“合成侵蚀”现象，为低资源语音合成提供了新的理解视角。
- **方法论新颖**：两种自对齐框架针对不同资源条件，无需外部监督，实用性强；DGSA 巧妙结合解耦学习，TDSC 创新地使用温度探索与自过滤。
- **实验说服力**：直接对标顶级商业系统且表现更优，并完成老挝语零样本克隆，验证了方法的先进性和泛化性。
- **应用价值**：对大量缺乏语音资源的语言具有直接指导意义，有助于缩小数字语音鸿沟。

## 8. 不足与局限
- **信息不完整**：仅从摘要推测，缺少实验细节（如算力、数据集规模、具体评价指标和统计检验），难以评估结果的可复现性和鲁棒性。
- **语言多样性**：主要展示了老挝语的极端案例，虽具代表性，但更多低资源语言的跨语言迁移效果尚不明确。
- **偏好信号的设计**：摘要未说明偏好信号的具体构建方式，该设计可能依赖先验知识，在完全没有真实语音参考时的泛化能力有待检验。
- **对比方法范围**：虽对比了商业系统，但未提及与其他低资源语音合成学术基线（如基于微调的 SLM 或传统 TTS + 数据增强）的比较，公平性可能受限。
- **应用限制**：TDSC 的“自动探索与过滤”可能引入额外的计算开销或生成不稳定，实际部署效率需进一步评估。

（完）
