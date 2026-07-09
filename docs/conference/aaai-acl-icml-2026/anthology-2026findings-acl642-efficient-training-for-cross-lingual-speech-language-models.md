---
title: Efficient Training for Cross-lingual Speech Language Models
title_zh: 跨语言语音语言模型的高效训练
authors: "Yan Zhou, Qingkai Fang, Yun Hong, Yang Feng"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.642.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 针对跨语言语音大模型的高效训练方法，包括新颖的对齐策略和指令微调
tldr: 针对端到端跨语言语音大模型训练中数据稀缺和多语言扩展难题，本文提出高效训练方法CSLM。该方法基于离散语音标记，通过持续预训练实现跨模态与跨语言对齐，并采用语音-文本交织的链式模态生成指令微调增强细粒度对齐。实验表明CSLM有效提升了跨语言语音语言模型的性能，为语音大模型多语言扩展提供了可行路径。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.642/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 803, \"height\": 287, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.642/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1645, \"height\": 662, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.642/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 813, \"height\": 352, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 808, \"height\": 567, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1654, \"height\": 298, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1363, \"height\": 379, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 805, \"height\": 387, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 807, \"height\": 453, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 807, \"height\": 460, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 803, \"height\": 237, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 803, \"height\": 180, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 807, \"height\": 474, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.642/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 806, \"height\": 918, \"label\": \"Table\"}]"
motivation: 现有语音大模型主要关注文本模态，端到端语音大模型构建因数据有限和多语言扩展困难而面临挑战。
method: 提出基于离散语音标记的高效训练方法CSLM，通过持续预训练实现跨模态与跨语言对齐，并采用语音-文本交织的链式模态生成指令微调增强细粒度对齐。
result: 实验表明该方法有效提升了跨语言语音语言模型的生成能力。
conclusion: CSLM为高效训练跨语言语音大模型提供了新方案，促进了自然的人机语音交互。
---

## Abstract
Currently, large language models (LLMs) predominantly focus on the text modality. To enable more natural human-AI interaction, speech LLMs are emerging, but building effective end-to-end speech LLMs remains challenging due to limited data and the difficulty in expanding to more languages. In this paper, we introduce C ross-lingual S peech L anguage M odel ( CSLM ), an efficient training method for cross-lingual speech LLMs based on discrete speech tokens. We propose a novel alignment strategy that achieves cross-modal and cross-lingual alignment through continual pre-training. By conducting instruction fine-tuning following a speech-text interleaved chain-of-modality generation process, we enhance modal alignment at a finer granularity, thereby improving generation quality and reducing latency. CSLM aligns different modalities and languages simultaneously without the need for massive speech data, thus exhibiting good language scalability. Evaluations on cross-modal tasks, mono-lingual conversational tasks, and cross-lingual conversational tasks demonstrate CSLM’s strong cross-modal alignment capabilities and general task abilities.

---

## 论文详细总结（自动生成）

作为资深学术论文分析助手，以下是对该论文《Efficient Training for Cross-lingual Speech Language Models》的结构化、深入、客观的总结。

### 1. 论文的核心问题与整体含义

*   **研究背景**：当前主流的大语言模型主要聚焦文本模态。为实现更自然的人机交互，能够理解和生成语音的“语音大语言模型”应运而生。
*   **核心问题**：构建高效的端到端语音大模型面临两大挑战：
    1.  **数据稀缺**：相比于海量的文本数据，高质量语音数据极其有限，尤其是在非英文语言中。
    2.  **多语言扩展困难**：如何在有限数据条件下，同时实现跨语言（如中英文）和跨模态（语音与文本）的有效对齐，是一个核心难题。
*   **整体含义**：本文旨在提出一种**数据高效**的训练方法，在不依赖海量语音数据的前提下，训练出具备强大跨语言能力的语音大模型，从而降低开发门槛，推动语音大模型在更多语言中的应用。

### 2. 论文提出的方法论

论文提出了**跨语言语音语言模型**，其核心思想是通过一种新颖的对齐策略和训练流程，高效地融合语音、文本以及不同语言的信息。

*   **模型架构**：CSLM由三个关键模块组成：
    *   **语音分词器**：采用CosyVoice模型的离散化技术，将连续语音波形转化为频率为25Hz、词表大小为4096的离散语音标记，并合并连续重复标记以提升效率。
    *   **语音-文本联合LLM**：将离散语音标记的词表与文本LLM的词表合并，使单一LLM能够同时对语音和文本标记进行自回归建模。基础模型为Llama-3.1-8B-Instruct。
    *   **语音解码器**：包含一个时长预测器、一个流匹配模型和一个HiFi-GAN声码器，负责将生成的离散语音标记序列重新合成为语音波形。

*   **核心训练流程**：
    1.  **对齐策略**：实现跨模态与跨语言对齐的核心思想是：在单一语言内部，通过平行语料进行语音-文本的**跨模态对齐**；在不同语言之间，通过翻译数据在**文本模态**上实现**跨语言对齐**。
    2.  **持续预训练**：在文本LLM的基础上，使用混合数据（语音识别ASR、语音合成TTS、机器翻译MT、单语文本指令数据）进行持续预训练，让模型初步建立语音-文本的映射关系以及语言的桥梁，得到`CSLM-base`模型。
    3.  **监督指令微调**：这是本文的一个关键创新点，称为**语音-文本交织的链式模态生成**。
        *   **问题**：传统的链式模态生成（TQ → 完整TA → 完整SA）会导致生成长文本回答时延迟过高。
        *   **方案**：提出“TQ → TA → SA → TA → SA...”的交织生成模式，即以更小的语块为单位，交替生成文本和语音。
        *   **数据构建**：利用基于CTC的强制对齐器，在单词或更细的层级上对齐语音和文本，然后将对齐后的响应切割成多个`(TA, SA)`语块，作为指令微调的数据格式。这能在更细粒度上增强语音和文本的对齐，同时支持流式输出，显著降低首响延迟。

### 3. 实验设计

作者设计了全面的实验来验证CSLM的性能，覆盖了基础任务和对话任务，并广泛对比了现有方法。

*   **数据集与场景**：
    *   **持续预训练**：
        *   **跨模态数据**：英文（LibriSpeech, GigaSpeech），中文（WenetSpeech, WenetSpeech4TTS）。
        *   **跨语言数据**：WMT17中英翻译子集。
        *   **单语文本数据**：InfinityInstruct数据集。
    *   **监督微调**：
        *   **单语对话**：InstructS2S-200K（英文）及其翻译版本（中文），使用CosyVoice合成为语音。
        *   **跨语言对话**：Alpaca数据集的中英文配对版本，合成为语音。
*   **评测基准与指标**：
    *   **基础跨模态任务**：
        *   **ASR**：在LibriSpeech（英文）和AISHELL-1/2/3（中文）上评估，采用WER/CER。
        *   **TTS**：在LibriSpeech、LibriTTS、VCTK（英文）和AISHELL-1/2/3（中文）上评估，将生成语音用ASR转写后计算WER/CER。
    *   **语音对话任务**：
        *   **数据集**：InstructS2S-Eval（英文），BELLE-eval-S2S（中文），以及它们对应的跨语言版本。
        *   **自动化指标**：GPT-4o评分（内容质量），UTMOS（语音自然度），ASR词/字错率（语音-文本一致性），Off-Target Ratio（语言准确性）。
        *   **人工评估**：对生成内容的自然度（C-MOS）和语音音质（A-MOS）进行人工双盲打分。
*   **对比方法**：SpeechGPT, AnyGPT, GLM-4-Voice, Moshi（均为基于离散标记的语音大模型），以及专用模型Whisper、CosyVoice。

### 4. 资源与算力

*   **算力配置**：论文明确提到，所有训练任务均使用**24块 NVIDIA H800 80G GPU** 完成，并采用了DeepSpeed ZeRO Stage 1优化策略。
*   **训练细节**：持续预训练阶段进行了1个epoch的训练，监督微调阶段也进行了1个epoch的训练。部分较小模块（如时长预测器）的训练则使用少量数据训练15个epoch。

### 5. 实验数量与充分性

*   **实验数量**：论文进行了相当充分的实验，大致可分为以下几组：
    1.  **基础任务评估**：英文和中文的ASR和TTS性能测试。
    2.  **对话任务评估**：涵盖单语（英、中）和跨语言（英→中，中→英）多个方向的语音对话自动化评测。
    3.  **人工评估**：对对话任务进行了人工评价作为补充。
    4.  **消融实验**：
        *   **跨模态对齐有效性**：通过表示相似度分析。
        *   **MT数据影响**：移除机器翻译数据后观察性能变化。
        *   **链式模态形式对比**：对比了不同语块大小、完整链式模态生成，以及移除文本问题生成等多种设置。
*   **充分性与公平性**：实验设计**相当充分和公平**。它不仅覆盖了基础的感知任务和高层次的对话任务，还在多语言和多语言转换的场景下进行了评估。消融实验直接验证了核心方法中各组件（MT数据、交织式生成模式）的有效性，对比对象也选取了同类型的主流工作。

### 6. 论文的主要结论与发现

1.  **高效对齐能力**：CSLM在使用极少量的语音-文本平行数据（约为GLM-4-Voice和Moshi的百分之一）的情况下，在基础的ASR和TTS任务上取得了与这些模型相当甚至更好的性能，证明了其数据高效性。
2.  **强大的跨语言对话能力**：在单语和跨语言语音对话任务上，CSLM展现了优秀的指令遵循、内容生成、语音自然度以及极低的语言混淆率（Off-Target Ratio）。
3.  **交织式链式模态生成的有效性**：所提出的语音-文本交织式生成方法，相比传统的完整链式模态生成，不仅显著降低了响应延迟（平均**2.93倍**推理加速），还略微提升了生成质量。
4.  **跨语言对齐机制的关键作用**：消融实验证明，在预训练中加入机器翻译数据是实现高质量跨语言对话的关键，移除它会显著损害跨语言任务的内容质量和语音质量。同时，在链式模态中生成“文本问题”是必不可少的中间桥梁，省略它将导致性能大幅下降。

### 7. 优点

*   **数据高效**：核心亮点。提出了一种训练策略，使训练高质量的跨语言语音大模型不再依赖海量语音数据，显著降低了数据获取门槛。
*   **语言可扩展性强**：该方法仅需少量平行数据和翻译数据即可扩展新语言，为解决低资源语言语音建模问题提供了有前景的思路。
*   **方法简洁有效**：“交织式链式模态生成”的创新简单而实用，既解决了流式对话的延迟问题，又通过更细粒度对齐提升了性能。
*   **评估全面**：实验设计考虑周全，从基础任务到对话任务，从自动化指标到人工评估，从单语到跨语言，从整体到消融，论据扎实。

### 8. 不足与局限

*   **资源上限未探索**：作者自己也提到，受限于数据和计算资源，CSLM未能在更大规模的数据集上进行训练，其性能上限和可扩展性边界是未知的。
*   **支持语言数量有限**：目前仅在中文和英语两种语言上进行验证，其推广到形态更丰富、资源更稀缺的其他语言族系上的效果有待检验。
*   **对语音分词器的依赖**：模型性能与下游任务效果高度依赖所选用的语音分词器（CosyVoice）的质量，更换分词器可能需要重新进行调整。
*   **实时对话的鲁棒性**：虽然设计了流式输出，但在真实嘈杂环境下的全双工对话鲁棒性、打断处理等高级交互特性未进行深入探讨和评测。

（完）
