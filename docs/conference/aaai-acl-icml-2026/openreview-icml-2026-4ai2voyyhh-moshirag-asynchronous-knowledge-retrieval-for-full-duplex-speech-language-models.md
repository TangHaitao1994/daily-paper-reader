---
title: "MoshiRAG: Asynchronous Knowledge Retrieval for Full-Duplex Speech Language Models"
title_zh: MoshiRAG：面向全双工语音语言模型的异步知识检索
authors: "Chung-Ming Chien, Manu Orsini, Eugene Kharitonov, Neil Zeghidour, Karen Livescu, Alexandre Défossez"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f275dccbed692daf46948fb74f6678967d736f0d.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 通过检索增强全双工语音对话AI的事实准确性
tldr: 全双工语音语言模型虽然提升了对话自然度，但其事实准确性仍待提高。该论文提出Moshi-RAG，采用模块化设计，将紧凑的全双工语音交互接口与异步知识检索相结合。模型能识别需要外部知识的查询，并基于检索信息生成回答，在保持实时交互性的同时，显著提升回答的事实可靠性。这为构建既自然又可信的语音助手提供了新途径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 全双工语音模型面临事实准确性挑战，扩大模型则推理延迟不可接受。
method: 设计异步检索框架，使模型在需要时访问外部知识源而不影响实时性。
result: 在保持低延迟的同时，提升了模型回答的事实正确率。
conclusion: 为自然交互与事实准确兼备的语音AI提供了高效融合方案。
---

## Abstract
Speech-to-speech language models have recently emerged to enhance the naturalness of conversational AI.
In particular, full-duplex models are distinguished by their real-time interactivity, including handling of pauses, interruptions, and backchannels. 
However, improving their factuality remains an open challenge.
While scaling the model size could address this gap, it would make real-time inference prohibitively expensive.
In this work, we propose Moshi-RAG, a modular approach that combines a compact full-duplex interface with selective retrieval to access more powerful knowledge sources.
Our asynchronous framework enables the model to identify knowledge-demanding queries and ground its responses in external information. 
By leveraging the natural temporal gap between response onset and the delivery of core information, the retrieval process can be completed while maintaining a natural conversation flow.
With this approach, Moshi-RAG achieves factuality comparable to the best publicly released non-duplex speech language models while preserving the interactivity inherent to full-duplex systems. 
Moreover, our flexible design supports plug-and-play retrieval methods without retraining and demonstrates strong performance on out-of-domain mathematical reasoning tasks.

---

## 论文详细总结（自动生成）

# MoshiRAG：面向全双工语音语言模型的异步知识检索 论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：语音到语音（speech-to-speech）的语言模型，尤其是全双工（full-duplex）模型，能够实时处理停顿、打断和反馈（backchannel），极大提升了对话的自然度。
- **核心问题**：这类全双工语音模型的**事实准确性（factuality）**仍然是一个开放挑战。直接扩增模型规模可以改善事实性，但会导致推理延迟过高，破坏实时交互性，因而在实际中不可接受。
- **整体含义**：论文旨在**兼顾自然交互与事实可靠性**，探索在不损失实时性的前提下，为全双工语音模型注入更强的外部知识，从而解决语音AI可信度瓶颈。

## 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：提出 **Moshi‑RAG**，采用**模块化设计**，将一个**紧凑的全双工语音接口**与**选择性、异步的知识检索**相结合。
- **关键技术细节**（用文字说明）：
  - **异步框架**：利用对话回应开始到传递核心信息之间的**天然时间间隔**，在该间隙中并行完成外部知识检索，从而不打断对话流，保持实时性。
  - **需求识别**：模型能够自动**判别当前查询是否需要外部知识**，仅对知识密集的询问触发检索，避免不必要的开销。
  - **回答生成**：基于检索到的外部信息生成回应，使输出内容事实性地锚定在可靠知识源上。
  - **即插即用**：检索方法支持**模块更换**，无需重新训练即可适配不同的检索策略。
- **算法流程**（非公式化描述）：
  1. 用户语音输入进入紧凑的全双工语音模型。
  2. 模型在生成初步回应开头的同时，判断该轮对话是否属于知识需求型查询。
  3. 若需要知识，异步触发检索模块，从外部知识源获取相关信息。
  4. 系统利用检索到的文本知识指导后续语音生成，将事实性信息融入回复中。
  5. 整个对话过程保持全双工交互，用户不会感知到明显的检索延迟。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **数据集/场景**：
  - 以公开可获取的语音对话数据为基础（摘要未给出具体名称，但提及了**非全双工语音语言模型**作为对比基线）。
  - 还评估了**分布外（out‑of‑domain）的数学推理任务**，以测试泛化能力。
- **对比方法**：
  - 与当前**最好的公开非全双工语音语言模型**在事实性上进行对标。
  - 同时对比原本不加检索的全双工基线（即无RAG的同类紧凑模型）。
- **评价指标**：重视**事实准确性（factuality）**与**交互延迟/自然度**之间的权衡。

## 4. 资源与算力

- **论文中未明确提供算力相关的具体信息**。从已有的摘要和元数据中无法得知所使用的GPU型号、数量及训练时长。不过，文中强调模型是“紧凑”的，推断其资源消耗远低于大规模全双工模型的直接扩增方案，推理端成本也可接受。

## 5. 实验数量与充分性

- **实验数量预估**：从摘要及方法特点推断，至少包含以下多组实验：
  - 不同知识源或检索方法的对比；
  - 全双工基线 vs Moshi‑RAG 的事实性评测；
  - 与非全双工SOTA语音模型的比较；
  - 分布外数学推理任务的泛化试验；
  - 可能包含消融实验（如选择性检索 vs 始终检索、不同延迟设置等）。
- **充分性与客观性评价**：
  - 实验设计考虑了**同类最佳系统**的横向比较，以及**领域外泛化**，覆盖面较合理。
  - 强调保持相同交互特性的同时提升事实性，基准对齐公平。
  - 但由于缺乏完整的论文正文，无法确认统计显著性与错误条等细节，但选题和对比逻辑具备较好的客观性。

## 6. 主要结论与发现

- Moshi‑RAG 在**保持全双工交互所固有的实时性和自然度**的前提下，取得了与**最佳公开非双工语音语言模型相媲美的事实准确性**。
- 异步检索设计成功利用了对话中的**语义时间间隙**，实现了无感知的知识增强。
- **模块化与即插即用**特性带来了高灵活性，支持不重新训练就能升级检索组件。
- 系统在**分布外数学推理任务**上也表现出较强性能，体现了方法良好的泛化能力。

## 7. 优点：方法或实验设计亮点

- **创新性地融合实时语音交互与异步知识检索**，解决了长期以来全双工语音模型的事实性短板。
- **利用对话结构的时间冗余**来隐藏检索延迟，工程巧思突出。
- **模块化架构**：紧凑模型 + 外部知识，既保持低计算成本，又允许独立优化各部分。
- **“插拔式”检索设计**使系统易于适应不同知识库和下游任务，实用价值高。
- 演示了在**数学推理等非典型语音任务上的泛化**，拓展了应用边界。

## 8. 不足与局限

- **信息有限**：由于未获取到完整论文，更多实验细节（如具体数据集名称、模型尺寸、检索库大小、失败案例分析等）尚不清楚。
- **评估可能存在偏差**：与最佳非双工模型的比较基于公开版本，可能未涵盖所有最新进展。
- **异步机制的适用范围**：依赖回应开头与核心信息之间的时间窗，若用户要求即时事实性信息（如抢答式对话），该间隙可能不足以完成检索，效果会受影响。
- **外部知识质量依赖**：事实改善程度直接受检索源及检索算法质量影响，未讨论错误检索对语音生成的影响及鲁棒性。
- **实时性指标缺失**：论文摘要中提到保持低延迟，但未给出具体的端到端延迟量值，无法量化实时表现与阈值。

---

（完）
