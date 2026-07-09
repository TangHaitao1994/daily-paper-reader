---
title: "SpeechMedAssist: Efficiently and Effectively Adapting Speech Language Model for Medical Consultation"
title_zh: SpeechMedAssist：高效适配语音语言模型用于医疗咨询
authors: "Sirry Chen, Jieyi Wang, Wei Chen, zhongyu wei"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=d4zZEeUC1J"
tags: ["query:speech-model"]
score: 7.0
evidence: 适配语音语言模型实现多轮医疗语音咨询，应对数据短缺
tldr: 医疗咨询天然语音化，但现有语音语言模型缺乏医学语音数据导致微调困难。SpeechMedAssist通过数学分析解耦单阶段训练为两阶段：先知识注入，后语音对话学习，大幅减少数据需求，实现多轮语音问诊交互，为领域语音对话提供可复制范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 医学语音数据稀少，阻碍语音语言模型在医疗对话中的应用。
method: 解耦训练为知识注入和语音对话学习的两阶段范式。
result: 在缺少大量医学语音数据下实现有效多轮语音问诊。
conclusion: 两阶段适配策略为垂直领域语音交互提供数据高效方案。
---

## Abstract
Medical consultations are inherently speech-based, yet current works focus on fine-tuning large language models (LLMs) to perform patient-unfriendly long-text interaction. While existing speech language models (SpeechLMs) enable efficient speech-based interaction, the scarcity of speech data in medical domain prevents them from being directly fine-tuned for practical applications.
In this paper, we propose SpeechMedAssist, a SpeechLM natively capable of conducting multi-turn speech-based interactions with patients. To mitigate data scarcity, we mathematically analyze the architecture of SpeechLMs and decouple one-stage training that requires a large corpus of medical speech data into a two-stage training paradigm. (1) Knowledge\&Capability injection: train the LLM core with rewritten and filtered medical text data to inject medical knowledge and endow it with diagnostic and treatment capabilities. (2) Modality alignment: train the SpeechLM using a small amount of synthetic medical speech data that matches patient characteristics to realign the speech and text modalities. 
After two-stage training, SpeechMedAssist performs excellent on our designed speech-based medical benchmark. Experiments further show that the second stage only requires 10k speech dialogue samples to achieve modality alignment, allowing the knowledge and capabilities acquired in the text modality during the first stage to generalize to the speech modality, which demonstrates the effectiveness and efficiency of our approach.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**  
  医疗咨询本质上是语音交互场景，但当前主流方法多基于大语言模型（LLM）进行长文本交互，对患者不友好。  
  语音语言模型（SpeechLM）虽能支持高效语音对话，却因医学领域语音数据极度稀缺而难以直接微调。

- **核心问题**  
  如何在缺乏大量医学语音数据的情况下，让语音语言模型获得多轮医疗语音问诊能力。  
  需要解决数据稀缺带来的训练困难，并实现从文本模态到语音模态的知识与能力迁移。

- **整体含义**  
  SpeechMedAssist 给出一种将单阶段训练解耦为“知识注入 + 模态对齐”的两阶段范式，以极少语音数据便可使语音模型掌握医学对话能力，为语音对话在垂直领域的低资源适配提供可复制路线。

## 2. 方法论

- **核心思想**  
  将传统需要大量医学语音数据的单阶段训练，拆分为两个独立阶段，分别解决医学知识注入和语音-文本模态对齐，从而大幅降低对稀缺语音数据的依赖。

- **技术细节**
  - **阶段一：知识注入与能力赋予（Knowledge & Capability injection）**  
    - 基于改写和筛选后的医学文本数据训练语音语言模型的 LLM 核心部分。  
    - 注入医学知识并赋予诊断和治疗推理能力。  
    - 仅在文本模态下进行，不需要语音数据。
  - **阶段二：模态对齐（Modality alignment）**  
    - 使用少量的、匹配患者特征的合成医学语音数据，对整个 SpeechLM 进行训练。  
    - 重新对齐语音与文本两种模态，使阶段一在文本中习得的知识和能力泛化到语音模态。  
    - 实验表明仅需 1 万条语音对话样本即可完成对齐。

- **算法流程（文字描述）**  
  1. 以预训练 SpeechLM 为基座，冻结或部分更新参数（文中未详细说明）。  
  2. 第一阶段：用清洗后的医学文本对话数据更新语言模型部分，培养医学诊断对话能力。  
  3. 第二阶段：用少量语音对话数据联合训练，恢复语音编码器与语言模型的跨模态连接，输出语音回答。  
  4. 最终模型可直接接受语音输入并进行多轮语音问诊交互。

- **数学分析**  
  论文声称通过数学分析 SpeechLM 的架构，解耦训练过程以证明两阶段训练的可行性和高效性，但摘要未给出具体公式。

## 3. 实验设计

- **数据集 / 场景**  
  - 第一阶段使用 **改写和过滤的医学文本数据**（具体来源及规模未在摘要中说明）。  
  - 第二阶段使用 **匹配患者特征的合成医学语音数据**（仅需 1 万条语音对话样本）。  
  - 评估基于专门设计的 **语音医疗 benchmark**（名称与细节未提供）。

- **对比方法**  
  摘要未列出对比的具体基线模型，但从上下文推断可能包括：
  - 仅文本微调的 LLM 方案（长文本交互）。
  - 传统单阶段训练 SpeechLM（需大量语音数据，不可行或性能差）。

- **实验目标**  
  验证两阶段方法的有效性（最终语音对话性能）和高效性（第二阶段极少量语音数据即可实现对齐）。

## 4. 资源与算力

- **算力信息缺失**  
  提供的摘要及元数据未提及 GPU 型号、数量、训练时长或具体算力成本。论文正文可能包含这些细节，但在可获取的文本中并未出现，因此此处无法总结。

## 5. 实验数量与充分性

- **可观察的实验组数**  
  - 至少包含**两阶段训练的有效性验证**（最终 Benchmark 表现）。  
  - **模态对齐效率实验**：证明第二阶段仅需 1 万条语音样本即可完成。  
  - 摘要未明确列出消融实验（如仅第一阶段、端到端微调、不同语音数据量的对比），但从“experiments further show...”推断可能进行了数据量消融。

- **实验充分性与公平性**  
  - 自建语音医疗 benchmark，可能与真实分布存在偏差，且未提及是否包含多种子领域（如不同疾病、不同科室）。  
  - 对比基线未详述，难以判断是否与业界最佳方案（如级联 ASR+LLM+TTS）比较，公平性存疑。  
  - 仅用一个模型架构（未说明基座 SpeechLM 类型）进行验证，通用性有待考察。  
  - 整体上，实验能支撑核心主张（少量语音数据对齐），但覆盖面和客观性需结合完整论文进一步确认。

## 6. 主要结论与发现

- SpeechMedAssist 在自建语音医疗基准上表现优异，证明它原生具备多轮语音交互能力。  
- 两阶段训练范式极其高效：第二阶段仅需 1 万条语音对话样本即可实现模态对齐，使第一阶段在文本上学到的医学知识和对话能力成功泛化到语音模态。  
- 该方法显著降低了对医学语音数据的需求，为低资源领域的语音对话模型适配提供了通用框架。

## 7. 优点

- **理论驱动**：通过数学分析解耦训练过程，而非经验性尝试，增加了方法的可解释性和泛化潜力。  
- **数据高效**：仅需极少量人工合成语音数据，解决了医学语音数据稀缺这一根本痛点。  
- **阶段分明**：知识注入与模态对齐分离，可以复用现有的文本医学数据资源，工程实现灵活。  
- **面向实用**：直接输出语音对话模型，避免传统级联方案的延迟和错误累积，更贴合真实医疗咨询场景。

## 8. 不足与局限

- **验证范围有限**：仅在自建 benchmark 上测试，未与其他公共语音医疗数据集或真实临床环境进行对比，外部效度未知。  
- **合成语音依赖性**：第二阶段采用合成语音，可能存在分布偏移（如音色、语气、噪声等），对齐后的模型在真实患者语音、方言或口音下的鲁棒性未经验证。  
- **方法细节缺失**：从现有信息无法得知第一阶段具体使用了多少文本数据、基座 SpeechLM 细节、合成语音如何生成、安全评测（幻觉、误诊风险）等。  
- **对比基线薄弱**：未明确比较级联 ASR→LLM→TTS 这一强基线，难以凸显端到端语音模型的实际优势。  
- **算力与可复现性**：未提供资源消耗信息，其他研究者难以评估成本或复现。

（完）
