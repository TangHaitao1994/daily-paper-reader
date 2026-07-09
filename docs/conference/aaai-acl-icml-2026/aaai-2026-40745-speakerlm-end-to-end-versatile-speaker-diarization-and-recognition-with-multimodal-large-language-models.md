---
title: "SpeakerLM: End-to-End Versatile Speaker Diarization and Recognition with Multimodal Large Language Models"
title_zh: SpeakerLM：基于多模态大语言模型的端到端多功能说话人分割与识别
authors: "Han Yin, Yafeng Chen, Chong Deng, Luyao Cheng, Hui Wang, Chao-Hong Tan, Qian Chen, Wen Wang, Xiangang Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40745/44706"
tags: ["query:speech-model"]
score: 9.0
evidence: 用多模态大语言模型联合说话人分割和语音识别以提升ASR准确率
tldr: 在会议转录等真实多说话人场景中，说话人分割与识别（SDR）任务至关重要，但现有级联系统存在误差传播和缺乏联合优化等问题。本文提出SpeakerLM，一种基于多模态大语言模型的端到端框架，统一处理“谁在何时说了什么”，同时进行说话人分割和语音识别。通过联合训练，SpeakerLM能够更好地处理重叠语音，减少级联误差，在识别准确率上显著优于传统方法。该工作为复杂音频环境下的语音识别和对话系统提供了一种高效、集成的解决方案，对智能会议、对话分析等应用具有重要价值。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 级联式说话人分割与识别系统存在误差传播、重叠语音难处理、缺乏联合优化等问题，影响ASR精度。
method: 提出SpeakerLM，一个统一的多模态大语言模型，端到端联合执行说话人分割和语音识别，实现协同优化。
result: 在多说话人场景如会议转录中，SpeakerLM优于级联系统，有效处理重叠语音并提升识别准确率。
conclusion: 该模型为多说话人语音识别提供了一个端到端解决方案，显著增强了复杂场景下的语音交互能力。
---

## Abstract
The Speaker Diarization and Recognition (SDR) task aims to predict ``who spoke when and what'' within an audio clip, which is a crucial task in various real-world multi-speaker scenarios such as meeting transcription and dialogue systems. Existing SDR systems typically adopt a cascaded framework, combining multiple modules such as speaker diarization (SD) and automatic speech recognition (ASR). The cascaded systems suffer from several limitations, such as error propagation, difficulty in handling overlapping speech, and lack of joint optimization for exploring the synergy between SD and ASR tasks. To address these limitations, we introduce SpeakerLM, a unified multimodal large language model for SDR that jointly performs SD and ASR in an end-to-end manner. Moreover, to facilitate diverse real-world scenarios, we incorporate a flexible speaker registration mechanism into SpeakerLM, enabling SDR under different speaker registration settings. SpeakerLM is progressively developed with a multi-stage training strategy on large-scale real data. Extensive experiments show that SpeakerLM demonstrates strong data scaling capability and generalizability, outperforming state-of-the-art cascaded baselines on both in-domain and out-of-domain public SDR benchmarks. Furthermore, experimental results show that the proposed speaker registration mechanism effectively ensures robust SDR performance of SpeakerLM across diverse speaker registration conditions and varying numbers of registered speakers.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的结构化中文总结。

### 1. 论文的核心问题与整体含义

*   **研究背景与核心问题**：说话人分割与识别（Speaker Diarization and Recognition, SDR）旨在回答“谁在何时说了什么”，是多说话人场景（如会议转录）的关键任务。现有的 SDR 系统主要采用**级联架构**，即串联一个说话人分割（SD）模块和一个自动语音识别（ASR）模块。
*   **级联架构的局限性**：
    *   **误差传播**：SD模块的错误（如错误的说话人边界或标签）会直接导致ASR转录质量下降。
    *   **难以处理重叠语音**：传统SD模块在处理多人同时说话的常见真实场景时性能有限。
    *   **缺乏联合优化**：SD和ASR模块通常独立训练，无法充分利用两个任务间的协同效应，限制了整体性能。
*   **论文动机与目标**：为了解决上述问题，论文提出了 **SpeakerLM**，一个统一的、端到端的多模态大语言模型（MLLM），旨在单一模型内联合优化SD和ASR任务，从而提升SDR系统的整体性能和鲁棒性。

### 2. 论文提出的方法论

SpeakerLM 的核心思想是利用大语言模型的强大能力，将SDR任务建模为一个端到端的序列生成问题。

*   **核心架构**：模型采用典型的 **编码器-投影器-大语言模型（Encoder-Projector-LLM）** 架构。
    *   **音频编码器**：使用预训练的 SenseVoice-large 模型提取鲁棒的音频表示。
    *   **音频投影器**：一个随机初始化的两层 Transformer 加一个卷积层，负责将音频嵌入映射到文本LLM的特征空间。
    *   **大语言模型**：使用预训练的 Qwen2.5-7B-Instruct 作为主干网络，利用其强大的指令遵循和语言理解能力生成带有说话人标签的转录文本。
    *   **说话人注册组件（可选）**：包含一个预训练的说话人嵌入提取器（ERes2NetV2）和一个线性投影层，将注册说话人的声纹信息注入LLM。

*   **灵活的说话人注册机制**：为了使模型适应不同的现实场景，论文提出了三种注册策略，通过输入不同的先验信息来实现：
    1.  **No-Regist（未注册）**：不提供任何说话人信息，模型输出匿名说话人ID（如 spk 0, spk 1）。
    2.  **Match-Regist（匹配注册）**：提前注册所有实际出现在音频中的说话人，模型需正确将说话人与姓名关联。
    3.  **Over-Regist（冗余注册）**：注册的说话人多于实际出现的说话人，模型需要忽略不相关的说话人信息，并为活跃说话人正确分配身份。该设置的注册人数 \( N_{rg} \) 与真实说话人数 \( N_{gt} \) 的关系为：\( N_{rg} = N_{gt} + N_{ov} \)，其中 \( N_{ov} > 0 \) 是冗余注册的说话人数量。

*   **多阶段训练策略**：模型分四个阶段逐步训练，以高效对齐音频、说话人和文本模态。
    1.  **阶段一（ASR预训练）**：仅使用600,000小时的单说话人ASR数据，通过 LoRA 微调LLM，得到一个强大的语音识别基础模型 `SpeakerLM-ASR`。
    2.  **阶段二（模拟数据对齐）**：冻结LLM和音频编码器，仅用**模拟的SDR数据**训练两个投影器，实现模态间的快速粗对齐。
    3.  **阶段三（真实数据适配）**：使用真实的SDR数据，联合微调解冻的音频编码器和投影器，而LLM保持冻结，以适应真实声学环境。
    4.  **阶段四（联合微调）**：解冻所有模块，使用 LoRA 微调LLM，实现声学和语言信息的深度集成与联合优化。

### 3. 实验设计

*   **数据集**：
    *   **训练集**：包含真实数据和模拟数据。
        *   **真实数据**：包括公开数据集（AliMeeting, AISHELL4）和大量内部数据（In-House），总计约 **7,695小时**的中文多说话人音频。
        *   **模拟数据 (Stage 2用)**：通过随机混合AliMeeting、AISHELL2、Librispeech和内部数据的干净语音模拟生成，共5,000小时。
    *   **测试集（评估基准）**：
        *   **域内（In-domain）**：AliMeeting-Eval, AISHELL4-Eval。
        *   **域外（Out-of-domain）**：AISHELL5-Eval，这是一个更具挑战性的车内多说话人场景，用于评估模型的泛化能力。

*   **评估指标**：
    *   **CER（字错率）**：仅评估ASR性能（忽略说话人标签）。
    *   **cpCER（串联最小排列字错率）**：在**未注册**场景下，通过最优排列匹配参考和预测的“说话人-文本”对后计算的CER，是SDR任务的核心指标。
    *   **saCER（说话人归因字错率）**：在**已注册**场景下，直接根据说话人姓名对齐后计算的CER。
    *   **∆cp / ∆sa**：等于 `cpCER - CER` 或 `saCER - CER`，代表因说话人识别错误导致的性能下降，用于衡量SD性能。

*   **对比方法**：
    *   **级联系统（SD+ASR）**：组合最先进的ASR模型（Paraformer-large）和四个SD模型（3D-Speaker, Pyannote 3.1, Diarizen-base, Diarizen-large）。
    *   **LLM后处理系统（SD+ASR+LLM）**：使用大语言模型（如Qwen2.5-7B, ChatGPT4.5）对最佳级联系统（Diarizen-large+Para）的输出进行纠错。
    *   **端到端系统（E2E-SDR）**：对比了不同数据量训练的SpeakerLM及之前的方法SA-Transformer。

### 4. 资源与算力

*   **硬件配置**：使用 **4张 NVIDIA A800 GPU** 进行训练。
*   **训练设置**：模型在每个阶段训练 **100万步（1M steps）**，每1万步进行一次验证。
*   **优化器与批次处理**：采用 AdamW 优化器，学习率经历预热、线性增长到5e-5后按余弦计划衰减。采用动态批次策略，最大token限制为6K，以平衡效率和内存。

### 5. 实验数量与充分性

论文进行了大量且全面的实验，验证了方法的有效性和鲁棒性。

*   **多数据集评估**：在三个公开的中文SDR基准测试集（AliMeeting, AISHELL4, AISHELL5）上进行了评估，覆盖了域内和域外场景，实验设计客观。
*   **全面的基线对比**：对比了多个级联系统和LLM后处理系统，对比方法具有代表性。
*   **数据规模缩放实验**：通过使用212.25h、694.06h、2269.57h和7638.95h不同规模的数据训练SpeakerLM，清晰展示了模型的**数据扩展能力**。
*   **多阶段训练消融**：分析了从Stage 1到Stage 4模型在多个测试集上的性能变化，验证了多阶段策略的必要性。
*   **注册机制影响分析**：分别测试了No-Regist、Match-Regist和Over-Regist设置下的性能，并深入探究了Over-Regist场景下冗余注册说话人数量（0到100）对性能的影响。
*   **模块选择分析**：对比了使用不同说话人嵌入提取器（ERes2NetV2 vs CAM++ ）对性能的影响，证明了选用更优基础模块的价值。
*   **实验公平性**：所有对比方法均基于统一的数据和指标，确保了比较的公正性。

### 6. 论文的主要结论与发现

1.  **性能超越级联系统**：在使用了足够训练数据（7,638.95小时）后，SpeakerLM 在域内和域外的所有基准测试上，均显著优于最强的级联系统基线。例如，在AISHELL5-Eval上，cpCER相对最好基线降低了 **13.82%** (从61.63%降至47.81%)。
2.  **强大的数据扩展能力和泛化性**：SpeakerLM 的性能随训练数据量增加而显著提升，并且在未见过的、具有挑战性的声学环境（AISHELL5-Eval）下表现出色（∆cp仅0.57%），证明其强大的泛化能力。
3.  **联合优化的有效性**：与级联系统或LLM后处理系统相比，端到端联合建模能更好地对齐说话人和文本信息，有效抑制误差传播。零样本的LLM后处理甚至可能因幻觉问题而损害性能。
4.  **灵活的注册机制有效**：SpeakerLM可无缝适应不同的注册场景。在Over-Regist场景下，即使冗余说话人数量增加至100，模型性能也未显著下降，表现出高度的鲁棒性。
5.  **多阶段训练策略有效**：从模拟数据到真实数据，从粗对齐到联合微调，分阶段训练对于逐步弥合模态间差异、提升最终性能至关重要。

### 7. 优点

*   **开创性工作**：首次将多模态大语言模型应用于端到端的SDR任务，统一了说话人分割和语音识别。
*   **方法灵活且实用**：提出的说话人注册机制覆盖了多种现实应用场景，增强了模型的通用性。
*   **系统性的实验验证**：实验设计全面、严谨，涵盖多个数据集、多样基线、消融研究，并用数据充分论证了模型的可扩展性、泛化性和鲁棒性。
*   **性能优异**：在性能上达到了新的最优水平，特别是在具有挑战性的域外场景中优势明显。

### 8. 不足与局限

*   **语言单一性**：目前的实验和验证均基于**中文普通话**数据集，其在其他语言或多语种混合场景下的性能尚待验证。
*   **模型规模与推理效率**：基于7B参数的大语言模型，计算成本和推理延迟可能较高，论文未讨论实时性或资源受限场景下的部署问题。
*   **对预训练组件的依赖**：模型的性能高度依赖于音频编码器、说话人嵌入提取器和大语言模型等预训练组件的质量，训练流程较为复杂。
*   **长音频处理未明确**：论文将音频裁剪为40-50秒片段进行处理，对于现实世界中远超此长度的会议录音，如何处理跨片段的长距离说话人一致性未作说明。
*   **公平性偏差风险**：训练数据来源和构成可能引入人口统计学偏差，论文未对此进行分析和讨论。

（完）
