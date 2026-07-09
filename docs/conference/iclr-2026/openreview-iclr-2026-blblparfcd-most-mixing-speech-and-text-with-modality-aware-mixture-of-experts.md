---
title: "MoST: Mixing Speech and Text with Modality-Aware Mixture of Experts"
title_zh: MoST：基于模态感知专家混合的语音与文本融合
authors: "Yuxuan Lou, Kai Yang, Yang You"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=BlbLPArfCD"
tags: ["query:speech-model"]
score: 8.0
evidence: 集成语音和文本的多模态大语言模型，模态感知专家混合架构促进跨模态理解
tldr: 针对多模态模型用相同参数处理不同模态忽视差异的问题，MoST提出模态感知专家混合架构，通过模态特定专家组捕获领域模式、共享专家促进信息传递，实现语音和文本的无缝融合。该模型在多项语音-文本理解任务上取得提升，展示了语音大模型与多模态学习的有效结合，为语音交互和跨模态理解提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有模型未考虑语音和文本模态的内在差异，用相同参数处理。
method: 设计模态感知专家路由，包括模态特定专家组和共享专家，实现分模态处理。
result: 在跨模态任务上性能优于统一参数模型。
conclusion: 模态感知专家混合有效提升了语音文本多模态大模型的效果。
---

## Abstract
We present MoST (Mixture of Speech and Text), a novel multimodal large language  model that seamlessly integrates speech and text processing through our proposed Modality-Aware Mixture of Experts (MAMoE) architecture. While current multimodal models typically process diverse modality representations with identical parameters—disregarding their inherent representational differences, we introduce specialized routing pathways that direct tokens to modality-appropriate experts based on input type. MAMoE simultaneously enhances modality-specific learning and cross-modal understanding through two complementary components: modality-specific expert groups that capture domain-specific patterns and shared experts that facilitate information transfer between modalities. Building on this architecture, we develop an efficient transformation pipeline that adapts the pretrained MoE language model through strategic post-training on ASR and TTS datasets, followed by fine-tuning with a carefully curated speech-text instruction dataset. A key feature of this pipeline is that it relies exclusively on fully accessible, open-source datasets to achieve strong performance and data efficiency. Comprehensive evaluations across ASR, TTS, audio language modeling, and spoken question answering benchmarks show that MoST consistently outperforms existing models of comparable parameter counts. Our ablation studies confirm that the modality-specific routing mechanism and shared experts design significantly contribute to performance gains across all tested domains. To our knowledge, MoST represents the first fully open-source speech-text LLM built on a Mixture of Experts architecture.

---

## 论文详细总结（自动生成）

# MoST：基于模态感知专家混合的语音与文本融合 详细中文总结

## 1. 论文的核心问题与整体含义
- **研究动机**：当前多模态大语言模型在处理语音和文本时，通常使用**完全相同的参数**进行表征与推理，忽视了两种模态在结构、信息密度和语义编码方式上的**内在差异**。这种“一刀切”的做法限制了对各自模态特性的深入捕捉和模态间的有效融合。
- **核心问题**：如何设计一种架构，既能**尊重语音和文本的模态特异性**，又能**促进跨模态知识的流动与整合**，从而提升语音‑文本多模态理解与生成的性能。
- **整体含义**：该工作试图通过**将模态感知引入混合专家（MoE）架构**，为语音‑文本大模型提供一条灵活、高效且可扩展的技术路径，强调在开源数据和有限参数规模下实现强竞争力。

## 2. 论文提出的方法论
- **核心架构：Modality‑Aware Mixture of Experts (MAMoE)**
  - **模态特定专家组（Modality‑specific expert groups）**：分别为语音令牌和文本令牌设计独立的专家子网络（如语音专家组、文本专家组），使每种模态能够通过专业化参数捕获领域特有模式（如声学特征、韵律、文本语法等）。
  - **共享专家（Shared experts）**：额外设置一组跨模态共享的专家，用于处理需要联合理解的令牌，促进语音和文本信息在隐空间中的对齐与传递。
  - **模态感知路由（Modality‑aware routing）**：对每个输入令牌，路由器根据其**模态身份**（语音或文本）动态选择激活哪些模态特定专家和共享专家，实现“分而治之”的稀疏激活。
- **训练流程**
  - **阶段一：后训练（Post‑training）** – 在自动语音识别（ASR）和文本转语音（TTS）数据集上对预训练 MoE 语言模型进行适配，使其初步获得语音与文本的转换能力。
  - **阶段二：指令微调（Instruction fine‑tuning）** – 精心构建语音‑文本指令数据集进行监督微调，使模型掌握口语问答、音频语言建模等任务。
- **数据策略**：整个流程**完全依赖可公开获取的开源数据集**，无高成本私有数据依赖，保证了复现性和数据效率。

## 3. 实验设计
- **评估基准**：
  - 自动语音识别（ASR）
  - 文本转语音（TTS）
  - 音频语言建模（Audio Language Modeling）
  - 口语问答（Spoken Question Answering）
- **对比方法**：与同等参数规模的现有模型进行性能对比（文中摘要未列出具体名称，但明确指出MoST在多个基准上一致优于可比模型）。
- **消融研究**：专门验证了模态特定路由机制和共享专家设计的贡献，确认二者对各类任务性能均有显著增益。

## 4. 资源与算力
- 提供的摘要与元数据中**未提及**所使用的GPU型号、数量、训练时长或具体算力消耗。仅知该工作依赖开源数据集，可能在典型学术计算资源下完成，但更详细信息需查阅原文。

## 5. 实验数量与充分性
- **实验组数概览**：至少涵盖ASR、TTS、音频语言建模、口语问答**四个不同性质的任务基准**，并进行了**消融实验**以验证关键组件。整体实验虽未在摘要中逐一列举规模，但覆盖了感知、生成、理解和交互等核心能力，具备较好的多样性和说服力。
- **充分性评价**：基于现有信息，实验设计**较为充足且合理**，能够支撑核心论断。同时，所有实验均基于开源数据，对比对象为同参数规模模型，保证了**客观性和公平性**。
- **潜在缺失**：摘要未报告统计显著性检验、多轮对话或更复杂的真实场景评估，这些在完整版论文中可能有所补充，但此处无法确认。

## 6. 论文的主要结论与发现
- MoST 在所有评测基准上性能**一致优于同等参数量的传统统一参数模型**，证明了模态感知专家混合的有效性。
- 模态特定路由和共享专家的设计对性能提升**具有根本性贡献**，消融实验证实移除任何一组件都会导致测试域上的表现下降。
- MoST 是**首个完全基于混合专家架构、完全开源**的语音‑文本大语言模型，为社区提供了可复现的基线。
- 仅利用开源数据和高效训练策略即可达到强竞争力，展示了该范式在数据效率和实用性上的优势。

## 7. 优点
- **模态感知的稀疏激活**：创新性地将“模态身份”纳入路由决策，既保留了MoE的稀疏高效，又避免了均匀处理造成的模态混淆。
- **双专家机制**：模态特定和共享专家的组合在保证专业化的同时促进跨模态对齐，设计简洁而有效。
- **数据高效与开源**：完全基于开源数据集，降低了复现门槛，且训练流程清晰（后训练＋指令微调），便于社区采纳和扩展。
- **任务覆盖面广**：涵盖识别、合成和口语理解，证明模型的通用性，而非仅针对单一任务优化。

## 8. 不足与局限
- **细节信息缺失**：从已知摘要和元数据无法获知具体模型大小、算力开销、训练超参数以及详细的对比基线列表，难以完全评估其计算成本和技术边际。
- **应用场景可能受限**：本文设定为语音‑文本双模态，未涉及更多模态（如视频、图像），多模态扩展性有待探索。
- **拒稿背景**：该论文投稿ICLR 2026后被标注为“Rejected”，意味着评审可能指出了某些未在摘要中揭示的局限，如理论深度、大规模验证或与更强基线（如商用系统）的对比不足等。
- **口语生成评估单一**：TTS评估可能侧重客观指标，未提主观质量（如自然度MOS）或韵律控制能力，实际部署部署能力仍有待检验。

（完）
