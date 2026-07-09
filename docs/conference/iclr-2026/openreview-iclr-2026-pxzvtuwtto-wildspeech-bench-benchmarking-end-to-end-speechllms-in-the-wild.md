---
title: "WildSpeech-Bench: Benchmarking End-to-End SpeechLLMs in the Wild"
title_zh: WildSpeech-Bench：评估端到端语音LLM的真实场景基准
authors: "Linhao Zhang, Jian Zhang, Bokai Lei, Chuhan Wu, Aiwei Liu, Wei Jia, Zhou Xiao"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=pxZvtuWTtO"
tags: ["query:speech-model"]
score: 8.0
evidence: 面向端到端语音LLM真实对话的综合基准
tldr: 推出WildSpeech-Bench，首个系统评估端到端语音大语言模型在真实口语对话中表现的基准。通过收集实际聊天数据并引入说话人属性、口吃等多样性，该基准弥补了现有评估对语音特有挑战的忽视，为提升语音LLM用户体验提供了重要工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 缺乏针对端到端语音LLM的专门基准，现有评估忽视语音特有的韵律、同音异义词等挑战。
method: 创建首个综合基准，系统策划真实口语对话数据，引入说话人属性、口吃等多样性。
result: 为在真实语音对话场景中评估语音LLM提供了标准化工具。
conclusion: 该基准推动了对语音LLM用户体验的深入理解，有助于指导模型优化。
---

## Abstract
Recent multi-modal Large Language Models (LLMs) such as GPT-4o have demonstrated strong capabilities of direct speech interaction. However, the lack of specialized and comprehensive benchmarks for end-to-end speech LLM evaluation hinders optimizing the user experience of Audio LLMs in real-world applications. Existing evaluation methods often adapt text-based benchmarks, overlooking speech's unique characteristics and challenges, including prosody, homophones, stuttering, and differing user expectations. Here, we introduce the first comprehensive benchmark designed to systematically evaluate end-to-end speechLLMs in practical speech conversations. We systematically curate real-world chat data relevant to spoken scenarios, introduce diversity in speaker attributes and acoustic conditions, and augment the dataset with speech-specific phenomena. We further design a query-aware evaluation method to use customized evaluation checklists and prompts to enhance the accuracy of automatic evaluation. We conduct comprehensive testing and detailed analysis of various mainstream speech models, revealing significant differences in model performance across different speech scenarios. The use of query-aware evaluation further enables a finer-grained assessment under various speech-specific scenarios. Our benchmark can provide valuable insights for speech model development and evaluation.

---

## 论文详细总结（自动生成）

# WildSpeech-Bench：评估端到端语音LLM的真实场景基准

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前多模态大语言模型（如GPT-4o）已具备直接语音交互的能力，但缺乏专门用于评估端到端语音LLM的综合基准。现有的评估方法大多直接迁移于文本基准，忽略了语音的独特特性（韵律、同音异义词、口吃等）以及真实口语对话中的用户预期差异，导致无法准确衡量语音LLM在真实应用场景下的用户体验。
- **整体含义**：该工作旨在填补评估空白，为语音LLM提供一个贴近真实口语对话的系统性测试平台。通过揭示不同语音场景下模型表现的显著差异，为语音模型的开发和优化提供关键洞察，推动语音交互体验的实质性提升。

## 2. 论文提出的方法论
- **核心思想**：构建首个专为端到端语音LLM设计的综合基准（WildSpeech-Bench），从数据层面系统引入真实口语对话的特性，并从评估层面设计查询感知的自动评估方法，以更精细地衡量模型在特定语音挑战场景下的能力。
- **关键技术细节**：
    - **数据策划**：
        - 系统收集与语音场景相关的真实世界聊天数据。
        - 引入说话人属性（如年龄、口音等）和声学条件（如环境噪声）的多样性。
        - 人工增强语音特有现象，如口吃、同音词混淆等。
    - **查询感知评估**：
        - 设计定制化的评估清单和提示（prompt），针对不同查询类型使用不同评估标准，提升自动评估的准确性。
    - **流程**：先构建多样化的语音对话专项数据集，再使用基于查询分类的细粒度自动评分策略对模型输出进行多维度评价。

## 3. 实验设计
- **数据集 / 场景**：使用自建的WildSpeech-Bench数据集，覆盖多种真实口语交互场景，并明确包含语音特有挑战（如口吃、同音词、不同说话人属性等）。原始公开摘要未提及是否使用了其他外部数据集。
- **基准对比**：对多种主流语音模型进行了全面的横向测试和详细分析，但未在提供材料中列明具体模型名称。
- **评估方法对比**：与传统的直接套用文本基准的评估方式形成对比，突出其查询感知评估在语音专项场景下的细粒度和准确性。

## 4. 资源与算力
- 目前提供的摘要和元数据中**未明确说明**所使用的GPU型号、数量及计算时长等算力相关信息。推断其为基准构建与模型评估性质的工作，主要开销可能在模型推理与评估数据处理上，但具体资源消耗无法从现有信息中获知。

## 5. 实验数量与充分性
- **实验数量**：摘要中提及进行了“全面的测试和详细分析”，以及利用查询感知评估实现了“更细粒度的评估”，但未给出明确的实验组数（如多少种子场景、多少次消融等）。仅从元数据中“score: 8.0”可推测该工作至少通过了ICLR 2026的初步评审，实验部分应具有一定完整性。
- **充分性、客观性与公平性**：基于其自述的设计，通过系统化引入真实语音特性、查询特定评估等方式，实验设计在理论上更加客观地反映了语音LLM的真实能力，但缺乏具体消融实验或对评估方法本身的可靠性分析说明，公平性与充分性有待更详细的论文正文验证。

## 6. 主要结论与发现
- 现有文本评估基准无法体现语音LLM在真实口语对话中的表现，模型在不同语音场景下存在显著性能差异。
- 所提出的WildSpeech-Bench能够揭示这些差异，并提供细粒度的语音专项能力诊断。
- 查询感知评估方法提升了自动评估的准确性和场景针对性。
- 该基准能为语音模型的开发与评估提供有价值的指导，帮助优化真实用户体验。

## 7. 优点
- **问题新颖且实际**：首次专门针对端到端语音LLM的真实对话场景构建评测基准，切中当前应用痛点。
- **语音特性覆盖全面**：考虑到口吃、同音词、韵律等传统文本基准忽视的维度，并系统化引入数据集。
- **评估方法创新**：提出查询感知评估，通过动态调整评估标准提升评判的准确性和细粒度，避免了“一刀切”式评分的局限。
- **实践指向强**：能够为模型开发者提供明确的改进方向，直接服务于语音LLM的用户体验优化。

## 8. 不足与局限
- **具体细节缺失**：当前仅依据摘要和部分元数据，缺少对数据集规模、具体模型名称、实验组数、消融分析等内容的披露，难以判断实验的完备性和结论的稳健程度。
- **基准泛化性未知**：基准的真实聊天数据是否覆盖足够丰富的语言、口音和文化背景，能否泛化到其他语种或非常规口语场景，目前无法确认。
- **评估方法可靠性**：查询感知评估的准确度依赖于检查清单与提示的合理设计，其与人工评估的一致性、对各类语音现象的敏感度等未在摘要中体现，可能存在系统性偏差风险。
- **算力与可复现性**：未提及算力需求和代码/数据开源计划，可复现性尚待验证。
- **静态基准的局限**：基准是静态数据集，可能无法完全适应快速演变的语音LLM能力，固定测试可能被未来模型“过拟合”。

（完）
