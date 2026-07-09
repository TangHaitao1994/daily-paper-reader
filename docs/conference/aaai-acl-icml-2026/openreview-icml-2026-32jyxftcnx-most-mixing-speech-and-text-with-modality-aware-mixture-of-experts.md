---
title: "MoST: Mixing Speech and Text with Modality-Aware Mixture of Experts"
title_zh: MoST：通过模态感知混合专家混合语音与文本
authors: "Yuxuan Lou, Kai Yang, Yang You"
date: 2026-04-30
pdf: "https://openreview.net/pdf/4660b16c2bb238b0ad5f07d2481bbe15d15ae248.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 集成语音和文本的多模态大语言模型，采用模态感知混合专家架构
tldr: 提出MoST多模态大语言模型，通过模态感知混合专家架构融合语音与文本处理，利用模态特定专家和共享专家实现模态内学习与跨模态信息传递，有效提升多模态任务性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多模态模型通常用相同参数处理不同模态，忽略了语音与文本的表征差异。
method: 设计模态感知混合专家（MAMoE），包含模态特定专家组和共享专家，根据输入类型路由到不同专家。
result: MoST在语音识别、文本理解等任务上表现出色，证明其有效融合模态。
conclusion: 该模型为语音-文本多模态学习提供了有效框架，推动大语言模型向真正多模态拓展。
---

## Abstract
We present MoST (Mixture of Speech and Text), a novel multimodal large language  model that seamlessly integrates speech and text processing through our proposed Modality-Aware Mixture of Experts (MAMoE) architecture. While current multimodal models typically process diverse modality representations with identical parameters—disregarding their inherent representational differences, we introduce specialized routing pathways that direct tokens to modality-appropriate experts based on input type. MAMoE simultaneously enhances modality-specific learning and cross-modal understanding through two complementary components: modality-specific expert groups that capture domain-specific patterns and shared experts that facilitate information transfer between modalities. Building on this architecture, we develop an efficient transformation pipeline that adapts the pretrained MoE language model through strategic post-training on ASR and TTS datasets, followed by fine-tuning with a carefully curated speech-text instruction dataset. A key feature of this pipeline is that it relies exclusively on fully accessible, open-source datasets to achieve strong performance and data efficiency. Comprehensive evaluations across ASR, TTS, audio language modeling, and spoken question answering benchmarks show that MoST consistently outperforms existing models of comparable parameter counts. Our ablation studies confirm that the modality-specific routing mechanism and shared experts design significantly contribute to performance gains across all tested domains. To our knowledge, MoST represents the first fully open-source speech-text LLM built on a Mixture of Experts architecture.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义

- **研究动机**：现有的大语言多模态模型在处理语音和文本时，通常用**同一套参数**来加工不同模态的表征，这忽略了语音与文本在底层结构、信息密度和时序特性上的本质差异。这种“一刀切”的做法限制了模型对特定模态深层特征的捕捉，也影响了跨模态理解的效率。
- **整体含义**：论文提出 **MoST**，一个基于“模态感知混合专家”架构的开源多模态大语言模型，旨在**根据输入类型动态地将 token 分配给最合适的专家模块**，从而在统一框架下同时强化模态内学习与跨模态信息传递，推动语音-文本理解真正走向精细化与高效化。

## 2. 方法论

### 2.1 核心思想
- 引入 **MAMoE（Modality-Aware Mixture of Experts）** 架构，让模型具备**模态感知的路由能力**。
- 专家体系分为两类：
  - **模态特定专家组**：分别音语音和文本设计，捕获各自域内的独有模式。
  - **共享专家**：促进两种模态之间的信息对齐与迁移。
- 路由机制根据输入 token 的模态属性，动态选择激活哪些专家，实现“该用语音专家时用语音专家，该用文本专家时用文本专家，必要时调用共享专家协同处理”。

### 2.2 关键技术细节
- **基础模型**：预训练的 MoE 大语言模型。
- **转化流水线**：
  1. **策略性后训练**：在 ASR（自动语音识别）和 TTS（语音合成）数据集上对原语言模型进行适配，注入语音处理能力。
  2. **指令微调**：使用精心筛选的**语音-文本指令数据集**进行有监督微调，使模型学会遵循语音输入下的各类指令。
- **完全开源**：整个流水线仅依赖可公开获取的数据集，不涉及私有数据。

### 2.3 算法流程（文字描述）
1. 输入（语音或文本）经过编码器得到 token 序列。
2. 每个 token 进入 MAMoE 层，由**模态感知路由器**计算与各模态特定专家及共享专家的匹配分数。
3. 根据分数激活 top-k 个专家，聚合其输出。
4. 多层堆叠后，模型输出最终的多模态表征，用于生成文本或语音。

## 3. 实验设计

### 3.1 数据集 / 场景
- 后训练阶段：使用**公开的 ASR 和 TTS 数据集**（具体名称未在提供内容中列出）。
- 指令微调阶段：使用特别整理的**语音-文本指令数据集**。
- 评测基准覆盖四大类任务：
  - 自动语音识别（ASR）
  - 语音合成（TTS）
  - 音频语言建模（Audio Language Modeling）
  - 口语问答（Spoken Question Answering）

### 3.2 对比方法
- 与**参数量在同一量级的现有模型**进行横向比较（具体模型名未在提供内容中列出）。

### 3.3 消融实验
- 验证**模态感知路由机制**和**共享专家设计**的独立贡献，证明两者在所有测试领域均带来显著性能提升。

## 4. 资源与算力
- 所提供的论文内容（元数据与摘要）中**未明确提及 GPU 型号、数量、训练时长等算力信息**。因此无法确定具体资源消耗。

## 5. 实验数量与充分性
- 从摘要描述来看，实验覆盖了**四个不同性质的任务**（识别、合成、语言建模、问答），并进行了**消融实验**以验证核心设计，整体设计**较为充分**。
- 由于缺少具体实验表格、样本数、标准差等详细数据，难以量化实验规模，但从研究范式判断，实验具备必要的**多维度性**和**客观性**。
- 对比模型仅限于“参数量相近”，未说明是否完全复现或公平调参，可能存在对比基线的偏差风险，但无具体信息可供深究。

## 6. 主要结论与发现
- MoST 在所有评测基准上**一致地优于同参数量级的现有模型**。
- **模态特定的路由机制**和**共享专家设计**是性能提升的关键因素，二者缺一不可。
- 完全基于开放数据集的训练流水线证明了**数据效率和可复现性**，为开源社区提供了首个基于 MoE 架构的语音-文本大模型。

## 7. 优点
- **架构创新**：首次将模态感知的路由引入语音-文本多模态 MoE，贴合模态差异的本质。
- **设计解耦**：模态特定专家与共享专家分工明确，既能专精又能协作。
- **开源友好**：仅依赖公开数据，易于复现和社区扩展。
- **性能实证**：在跨度极大的任务上（从识别到生成）均获得提升，显示方法的通用性。

## 8. 不足与局限
- **细节缺失**：因仅获得摘要与元数据，许多关键信息不明：
  - 具体数据集、基座模型、对比方法名称。
  - 消融实验的具体数值与统计显著性。
  - 算力开销，可能限制对工程可行性的评估。
- **对比公平性**：仅提及“参数量相近”，未说明是否在完全相同的训练数据、训练步数下比较，可能存在外部变量干扰。
- **任务覆盖**：缺少对多语种、嘈杂环境鲁棒性、实时交互等场景的评估。
- **应用限制**：无任何关于推理延迟、模型部署轻量化或安全对齐方面的考量说明，离实用化可能尚有距离。

（完）
