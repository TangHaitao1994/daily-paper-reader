---
title: "Mind the Pause: Disfluency-Aware Objective Tuning for Multilingual Speech Correction with LLMs"
title_zh: 注意停顿：基于不流利感知目标调优的多语言语音纠正与LLM
authors: "Deepak Kumar, Baban Gain, Asif Ekbal"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.2137.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 解决ASR转录中的不流利问题，以提升聊天机器人和语音助手的可靠性
tldr: ASR转录中常包含填充词、重复等不流利现象，影响下游对话系统。本文提出基于LLM的不流利感知目标调优方法，不仅识别而且纠正不流利部分，保持语法和语义连贯性，实验表明在多语言设置中有效提升转录可读性，从而增强聊天机器人和语音助手的可靠性。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2137/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1258, \"height\": 549, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2137/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 687, \"height\": 344, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2137/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1307, \"height\": 1165, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 787, \"height\": 174, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 730, \"height\": 365, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1668, \"height\": 336, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1669, \"height\": 411, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1250, \"height\": 420, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1182, \"height\": 411, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1405, \"height\": 980, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1240, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 795, \"height\": 718, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1493, \"height\": 459, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 714, \"height\": 266, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2137/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1379, \"height\": 482, \"label\": \"Table\"}]"
motivation: ASR不流利现象降低可读性，干扰聊天机器人和语音助手等下游应用。
method: 提出不流利感知目标调优，利用LLM纠正ASR转录保持连贯性。
result: 多语言实验中有效提升转录质量，增强下游系统可靠性。
conclusion: LLM调优为语音交互系统的鲁棒性提供了新的纠错方案。
---

## Abstract
Automatic Speech Recognition (ASR) transcripts often contain disfluencies, such as fillers, repetitions, and false starts, which reduce readability and hinder downstream applications like chatbots and voice assistants. If left unaddressed, such disfluencies can significantly degrade the reliability of downstream systems. Most existing approaches rely on classical models that focus on identifying disfluent tokens for removal. While this strategy is effective to some extent, it often disrupts grammatical structure and semantic coherence, leading to incomplete or unnatural sentences. Recent literature explored the use of large language models (LLMs); however, these efforts have primarily focused on disfluency detection or data augmentation, rather than performing comprehensive correction. We propose a multilingual correction pipeline where a sequence tagger first marks disfluent tokens, and these signals guide instruction fine-tuning of an LLM to rewrite transcripts into fluent text. To further improve reliability, we add a contrastive learning objective that penalizes the reproduction of disfluent tokens, encouraging the model to preserve grammar and meaning while removing disfluent artifacts. Our experiments across three Indian languages, namely Hindi, Bengali, and Marathi show consistent improvements over strong baselines, including multilingual sequence-to-sequence models. These results highlight that detection-only strategies are insufficient. Combining token-level cues with instruction tuning and contrastive learning provides a practical and scalable solution for multilingual disfluency correction in speech-driven NLP systems. We make the codes publicly available at https://github.com/deepak-kumar-98/Mind-the-Pause.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的结构化深入总结。

### 1. 论文的核心问题与整体含义

-   **核心问题**：自动语音识别（ASR）生成的口语转录文本常包含不流利现象，如填充词（“呃”、“嗯”）、重复、错误起始等。这些不流利现象会降低文本可读性，并严重损害下游自然语言处理应用（如聊天机器人、语音助手、问答系统和机器翻译）的可靠性。
-   **研究动机**：
    -   **现有方法不足**：大多数现有方法将问题视为“先检测后删除”，直接移除被标记的不流利词元。这种方法常破坏语法结构和语义连贯性，生成不完整或不自然的句子。
    -   **多语言研究匮乏**：对于印地语、孟加拉语、马拉地语等印度语言，现有工作几乎只关注语料稀缺情况下的不流利检测，缺乏完整的纠错方案。
    -   **大语言模型未充分利用**：近期研究虽探索了LLM在不流利处理中的作用，但主要用于数据增强或检测，而非执行全面的文本重写与纠正。
-   **整体含义**：论文旨在构建一个实用的、可扩展的多语言不流利纠正管道，将检测信号与LLM的生成能力相结合，不仅移除不流利部分，更要生成语法正确、语义完整的流畅文本，从而打通从ASR输出到可靠下游应用的“最后一公里”。

### 2. 论文提出的方法论

-   **核心思想**：提出一个两阶段的不流利纠正框架。第一阶段利用序列标注模型提供词元级别的不流利信号；第二阶段将这些信号作为辅助信息，指导LLM进行指令微调，并引入对比学习损失函数来显式抑制模型复现不流利词元。

-   **关键技术细节与流程**：
    1.  **不流利检测**：采用多语言预训练模型 MuRIL 作为骨干网络，微调成一个序列标注器。输入为不流利句子，输出为每个词元的标签（0=流畅, 1=不流利）。其训练损失为标准交叉熵损失（\( L_{\text{detect}} \)）。
    2.  **基于标签的指令微调**：
        -   将不流利纠正任务构建为指令微调格式。LLM的输入包括：自然语言指令、不流利的句子、以及由MuRIL预测的词元标签序列。输出为目标流畅句子。
        -   仅使用交叉熵损失（\( L_{\text{CE}} \)）进行训练，让模型学会根据检测信号生成流畅文本。
    3.  **引入对比学习目标**：
        -   **动机**：仅使用交叉熵损失的模型偶尔仍会复现输入中的不流利词元。
        -   **方法**：在交叉熵损失基础上，引入一个**对比损失项（\( L_{\text{contrastive}} \)）** 。该损失项会在模型生成序列的每一步，计算已知的不流利词元集（\( D_i \)）中所有词元的预测概率之和（\( s_{i,t} \)），然后惩罚模型输出这些词元的高概率。公式为 \( L_{\text{contrastive}} = -\log(1 - s_{i,t}) \)。
        -   **总损失**：\( L_{\text{total}} = L_{\text{CE}} + \lambda \cdot L_{\text{contrastive}} \)，其中 \( \lambda \) 是一个带预热的超参数。该“推-拉”机制让模型既模仿流畅目标，又主动远离不流利的表达模式。

### 3. 实验设计

-   **数据集**：
    -   使用了 Kundu 等人 (2022) 发布的**平行流畅-不流利数据集**，涵盖印地语、孟加拉语和马拉地语三种印度语言。
    -   **训练集**：约12万对合成句对（每语言约4万对）。
    -   **测试集**：包含两个部分：1) **人工编辑数据**（高质量合成数据）；2) **真实数据**（来自真实对话录音的转录文本，是评估的关键基准）。

-   **基准模型对比**：
    -   **微调mBART**：将多语言序列到序列模型 mBART 在平行数据上微调，作为强编码器-解码器基线。
    -   **零样本提示LLM**：直接提示 LLaMA-3.2-3B 和 Qwen2.5-3B 指令模型进行纠正，不做任何微调。
    -   **多语言指令微调（无标签）**：直接在<不流利，流畅>句对上微调LLM，但不提供任何词元标签信息。
    -   **本文提出的方法**：对比了两种变体：
        -   **无对比损失**：仅使用MuRIL标签进行指令微调。
        -   **有对比损失**：完整的、结合了对比学习目标的微调方案（Proposed）。
    -   还对比了 GPT-4o 和 Gemini 2.5 Pro 等前沿闭源模型。

-   **评估指标**：BLEU, chrF2 (越高越好), TER (越低越好)，以及“LLM-as-a-Judge”和人工评估。

### 4. 资源与算力

-   文中明确提到，所有实验均在**单张NVIDIA A100 GPU (80GB)**上完成。
-   训练采用了`bfloat16`精度和梯度检查点技术以减少内存占用。
-   使用的LLM为3B参数规模（LLaMA-3.2-3B-Instruct和Qwen2.5-3B-Instruct），作者在局限性部分指出，由于计算资源限制，未探索更大规模的模型。

### 5. 实验数量与充分性

-   **实验数量**：实验设计相当全面，涵盖了大量实验组。
    -   在3种语言上进行了实验。
    -   针对2种测试集（人工编辑、真实数据）进行评估。
    -   基于2个骨干模型（LLaMA-3.2-3B 和 Qwen2.5-3B）测试了4种主要方法：mBART、指令微调（无标签）、无对比损失（有标签）、有对比损失（完整方法）。
    -   进行了零样本跨语言迁移实验。
    -   包含多个消融对比：零样本、少样本、思维链(CoT)提示工程、与GPT-4o/Gemini 2.5 Pro的对比、下游任务（QA/MT/TTS）影响评估、LLM-as-a-Judge和人工评估。
-   **充分性与公平性**：实验非常充分。对比基线丰富且合理，涵盖了从经典模型到最新LLM的不同范式。所有对比均在相同的数据集划分和评估指标下进行，客观公平。下游任务评估和人工评估进一步增强了结论的说服力。

### 6. 论文的主要结论与发现

1.  **检测与纠正结合显著优于仅检测**：仅依赖词元删除的检测策略会破坏语法，而结合标签信号指导LLM生成能有效解决此问题。
2.  **对比学习目标带来显著提升**：引入对比损失是本文最核心的技术贡献。与不使用对比损失的变体相比，它在所有语言和数据集上都实现了一致且显著的提升。例如，在Qwen2.5-3B模型上，BLEU平均提升+4.68点。
3.  **方法适用于不同模型家族**：该框架在LLaMA和Qwen两种不同架构的LLM上均有效，证明了其鲁棒性和可泛化性。
4.  **具有实际应用价值**：经过本框架纠正的文本，在问答、机器翻译和语音合成等下游任务上的性能有明显恢复，缩小了与使用纯金标准流畅文本之间的性能差距。
5.  **小模型可与大模型匹敌**：本文用3B参数模型训练的纠错器，在多数指标上超越了GPT-4o的少样本效果，并大幅优于零样本的Gemini 2.5 Pro。

### 7. 优点

-   **创新的损失函数设计**：提出的对比损失（\( L_{\text{contrastive}} \)）直观且有效，通过显式惩罚不流利词元的生成，弥补了标准交叉熵在否定性信号上的不足，是方法上的亮点。
-   **完整的管道打通**：首次将检测（MuRIL标注）与纠正（LLM重写）在统一的框架内有效结合，打通了从ASR原始输出到可读文本的完整链路。
-   **实验设计扎实**：不仅汇报了自动指标，还引入了LLM评审和人工评估，并分析了纠正前后对多种下游任务（QA、MT、TTS）性能的影响，使论证链条非常完整且贴近实际应用。
-   **多语言低资源场景的聚焦**：选择资源相对稀缺的三种印度语言，挑战性更大，且成果表明其方法模型无关且语言无关，具有良好的可扩展性。

### 8. 不足与局限

-   **模型规模有限**：由于算力限制，仅在3B参数的LLM上进行了验证，未探索在更大规模模型上的效果。
-   **数据集单一且规模小**：仅依赖一个公开数据集，且真实测试集规模很小（每种语言150-300句），这使得统计结论的鲁棒性受限，并可能影响对模型泛化能力的评估。
-   **语言和领域覆盖不足**：仅在三种印度语言上实验，其结论能否推广到其他语系或领域（如医疗、法律）尚待考证。
-   **对比基线有缺失**：文中提到多语言不流利纠正领域缺乏现成基线，因此主要与自行实现的基线对比，缺少与该任务上可能存在的其他专门设计的端到端模型的横向比较。
-   **人工评估规模有限**：虽然进行了人工评估，但样本量（每语言100句）和评估者的细节未详细披露，可能存在一定的主观偏差风险。

（完）
