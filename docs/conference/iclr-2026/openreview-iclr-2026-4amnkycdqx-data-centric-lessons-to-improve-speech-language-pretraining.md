---
title: Data-Centric Lessons To Improve Speech-Language Pretraining
title_zh: 以数据为中心的语音-语言预训练改进经验
authors: "Vishaal Udandarao, Zhiyun Lu, Xuankai Chang, Yongqiang Wang, Albin Madappally Jose, Fartash Faghri, Joshua P Gardner, Chung-Cheng Chiu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=4amNkYCDqX"
tags: ["query:speech-model"]
score: 9.0
evidence: 以数据为中心的语音-语言预训练数据处理和策展探索
tldr: 语音-语言模型预训练中数据处理与策展的影响尚不清晰。本文通过系统消融实验，研究网络爬取音频处理、合成数据集构建等核心数据问题。结果表明数据策展策略显著影响下游问答性能，为语音-语言预训练的数据管线设计提供了指导。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 缺乏对语音-语言预训练数据处理和策展的受控消融研究，影响SQA性能提升。
method: 通过系统数据消融实验，研究网络爬取数据处理、合成数据构建等关键数据问题。
result: 揭示了数据策展策略对语音-语言模型性能的重要影响。
conclusion: 以数据为中心的优化是提升语音-语言预训练效果的关键。
---

## Abstract
Spoken Question-Answering (SQA) is a core capability for useful and interactive artificial intelligence systems. Recently, several speech-language models (SpeechLMs) have been released with a specific focus on improving their SQA performance. However, a lack of controlled ablations of pretraining data processing and curation makes it challenging to understand what factors account for performance, despite substantial gains from similar studies in other data modalities. In this work, we address this gap by conducting a data-centric exploration for pretraining SpeechLMs. We focus on three questions fundamental to speech-language pretraining data: (1) how to process raw web-crawled audio content for speech-text pretraining, (2) how to construct synthetic datasets to augment web-crawled data and (3) how to interleave (text, audio) segments into training sequences. We apply the insights from our controlled data-centric ablations to pretrain a 3.8B-parameter SpeechLM, called SpeLangy, that outperforms models that are up to 3x larger by 10.2% absolute performance. We hope our findings highlight the impact of effective data curation and guide future data-centric exploration in SpeechLMs.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的结构化中文总结。

---

### 1. 论文的核心问题与整体含义
- **研究背景**：口语问答（Spoken Question-Answering, SQA）是构建交互式人工智能系统的核心能力。近期，许多语音-语言模型（SpeechLMs）被发布以提升 SQA 性能，但关于预训练数据处理与策展（curation）的受控消融实验严重缺乏，这使得业界难以理解究竟哪些因素真正推动了性能提升。
- **核心问题**：在语音-语言预训练中，数据的处理与策展策略对下游任务性能的影响尚不清晰，亟需系统性的、以数据为中心的实证研究。
- **整体含义**：本文旨在填补这一空白，通过一系列受控的数据消融实验，揭示数据策展对 SpeechLM 性能的关键影响，并为未来语音-语言预训练的数据管线设计提供可操作的指导原则。

### 2. 论文提出的方法论
- **核心思想**：采用“以数据为中心”的研究范式，不侧重于模型架构创新，而是通过系统控制变量，探究预训练数据的构建与处理方式如何影响最终模型的口语问答能力。
- **关键技术细节**：围绕语音-语言预训练数据的三个根本问题展开深入探索：
  1.  **原始网络爬取音频的处理**：研究如何将嘈杂、异构的网络音频数据转化为适合语音-文本联合预训练的高质量训练样本。
  2.  **合成数据集的构建**：探索如何构建合成数据集，以有效扩充和增强网络爬取的真实数据，改善数据多样性和覆盖面。
  3.  **多模态序列的交错排列**：研究在训练序列中，文本片段（text）和音频片段（audio）应该如何以最优的策略交错组合（interleave），以最大化预训练的学习信号。
- **验证手段**：将从上述受控消融实验中得到的关键见解应用于训练一个名为 **SpeLangy** 的 3.8B 参数 SpeechLM，以此验证数据策展策略的整体有效性。

### 3. 实验设计
- **核心任务与场景**：主要评估基准为**口语问答（SQA）**任务，即模型需“听懂”语音提问，并生成正确回答。
- **评估基准与对比方法**：
  - **基准**：文中未明确列出具体的数据集名称（如 Spoken SQuAD 等），但聚焦于 SQA 的绝对性能指标。
  - **对比对象**：将使用优化数据策展训练的 3.8B 参数 SpeLangy 模型，与业界现有的、参数规模是其 **3 倍** 的更大 SpeechLM 进行性能对比。
- **性能提升**：SpeLangy 在 SQA 任务上，以 **10.2%** 的绝对性能提升，超越了比其大 3 倍的模型，生动展示了数据质量优于模型规模的潜力。

### 4. 资源与算力
- 在所提供的论文元数据及摘要中，**未明确提及** 训练所使用的 GPU 型号、数量、训练时长或具体的算力消耗等资源细节。这属于需要在完整论文中查证的缺失信息。

### 5. 实验数量与充分性
- **实验组合**：论文的核心实验主体是一系列围绕上述三大数据问题展开的**系统数据消融实验**。虽然没有给出精确的实验组数，但可以推断其对每个关键数据处理环节（网络数据处理、合成、交错方式）都设置了不同的控制变量和对比组。
- **充分性与公平性**：
  - **充分性**：研究选题切中领域痛点，三个探索问题覆盖了从原始数据到训练输入的全流程，构成了一个较为完整的实验框架。最终通过训练一个大一统模型，对之前得到的局部结论进行了端到端的集成验证，逻辑链条充分。
  - **公平性**：采用“受控消融”的方法，意味着在探究某个特定因素时，会固定其余变量。通过将小得多的模型与更大的公开模型进行对比并胜出，也体现了其数据策展方法带来的公平且显著的优势。

### 6. 论文的主要结论与发现
- **数据策展的决定性影响**：数据处理与策展策略对语音-语言模型的口语问答性能有根本性、实质性的影响，其重要性不亚于甚至可能超过单纯扩大模型参数规模。
- **有效性验证**：通过运用从消融实验中提炼出的数据优化原则，一个 **3.8B** 参数、名为 SpeLangy 的模型，能够以 **10.2% 的绝对性能优势**，击败参数规模是其 **3 倍** 的同类模型。
- **实践指南**：研究为如何有效处理网络音频、构建合成数据以及编排多模态训练序列等问题提供了明确、可操作的指导见解，证明以数据为中心的优化是提升语音-语言预训练效果的关键路径。

### 7. 优点
- **问题聚焦且有现实意义**：直面领域内被长期忽视但至关重要的数据工程问题，填补了语音-语言预训练中数据策展系统性研究的空白。
- **方法论严谨**：采用受控消融实验设计，能有效分离各个数据因素的独立影响，得出的结论具有较高的信度和可解释性。
- **结果震撼且具说服力**：“3.8B 模型以 10.2% 的绝对优势超越 3 倍规模模型”，这一结论有力地证明了数据质量的杠杆效应，结论清晰且令人印象深刻。
- **提供端到端验证**：不仅进行了细粒度的消融，还通过训出最终模型 SpeLangy 完成了从微观洞察到宏观应用的闭环验证，增强了研究的实用价值。

### 8. 不足与局限
- **任务覆盖单一**：所有结论均基于 **口语问答（SQA）** 这一核心任务得出，其在更广泛的语音处理任务（如语音翻译、情感识别、通用语音理解）上的普适性有待进一步验证。
- **实验细节缺失**：摘要未交代合成数据的生成方式、网络数据处理的具体清洗规则等操作细节，外部复现和借鉴的成本可能较高。资源算力信息的缺失也使得难以评估其方法的经济成本。
- **潜在的数据偏差风险**：论文未讨论网络爬取数据和合成数据中可能引入的社会偏见、语言偏向或领域偏向问题，这些偏差可能会被模型放大并影响其在下游应用中的公平性。
- **对比的局限性**：仅与参数规模大 3 倍的模型对比，没有提供与同规模、不同数据策展策略训练模型的直接消融对比，无法完全排除模型架构等其他因素的干扰。

（完）
