---
title: Can Speech LLMs Think while Listening?
title_zh: 语音大语言模型能在聆听时思考吗？
authors: "Yi-Jen Shih, Desh Raj, Chunyang Wu, Wei Zhou, SK Bong, Yashesh Gaur, Jay Mahadeokar, Ozlem Kalinli, Mike Seltzer"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=dFVenZdVbX"
tags: ["query:speech-model"]
score: 9.0
evidence: 通过思链微调提升语音LLM推理能力并降低交互延迟
tldr: 针对语音大语言模型的推理能力与响应延迟问题，研究思链微调的影响。通过在文本空间进行推理，语音LLM的准确率平均提升2.4倍；并提出“边听边想”方法，在聆听过程中启动推理以减少延迟。该工作为构建更智能、更实时的语音交互代理奠定了基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音LLM在复杂推理任务上表现不佳，且响应延迟影响交互体验。
method: 对多流语音LLM进行思链微调，并提出在聆听过程中提前生成推理步骤以减少延迟。
result: 推理准确率平均提升2.4倍，并有效降低额外延迟。
conclusion: 该方法显著增强了语音LLM的推理能力和实时性，推动了口语对话系统发展。
---

## Abstract
Recent advances in speech large language models (speech LLMs) have enabled seamless spoken interactions, but these systems still struggle with complex reasoning tasks. Previously, chain-of-thought (CoT) prompting or fine-tuning has been shown to significantly improve the reasoning abilities of text-based LLMs. In this work, we investigate the effect of CoT fine-tuning for multi-stream speech LLMs, demonstrating that reasoning in text space improves the accuracy of speech LLMs by 2.4x, on average, over a suite of spoken reasoning tasks. Beyond accuracy, the latency of the spoken response is a crucial factor for interacting with voice-based agents. Inspired by the human behavior of "thinking while listening," we propose methods to reduce the additional latency from reasoning by allowing the model to start reasoning before the user query has ended. To achieve this, we introduce an entropy-based metric, "question completeness," which acts as an indicator to guide the model on the optimal time to start reasoning. This method provides greater control over the accuracy-latency trade-off compared with heuristic-based approaches and, under equivalent latency conditions, yields a 4% accuracy gain on ARC-Easy. Finally, we use Direct Preference Optimization (DPO) on preference data created using rejection sampling to push the accuracy-latency pareto frontier further, resulting in a 70% reduction in latency without loss in accuracy.

---

## 论文详细总结（自动生成）

# 论文总结：Can Speech LLMs Think while Listening?

## 1. 核心问题与整体含义
- **研究背景**：语音大语言模型（Speech LLMs）已能支持流畅的语音交互，但在复杂推理任务上表现欠佳，且语音响应的延迟严重影响交互体验。
- **核心问题**：
  - 如何提升语音LLM在口语推理任务中的准确率？
  - 如何在引入推理过程后控制甚至降低整体响应延迟？
- **整体含义**：借鉴人类“边听边想”的行为，探索让语音LLM在用户话语未结束时即开始推理，以实现更智能、更实时的语音代理。

## 2. 方法论
- **核心思想**：
  - 采用**链式思考微调（CoT Fine-tuning）** 将文本空间中的推理能力迁移到多流语音LLM中。
  - 设计**“边听边想”机制**，使得模型在聆听过程中提前启动推理，缩短交互延迟。
- **关键技术细节**：
  1. **多流语音LLM的CoT微调**：在训练数据中注入推理步骤文本，引导模型在生成答案前先在文本空间进行逐步推理。
  2. **基于熵的“问题完整性”指标**：引入一个基于熵值的度量，用于判断用户问题是否已经足够完整，从而指导模型在最佳时机开始推理，以此控制准确率与延迟的权衡。
  3. **偏好优化（DPO）**：利用拒绝采样构建偏好数据，应用Direct Preference Optimization进一步推动准确率-延迟的帕累托前沿。

## 3. 实验设计
- **数据集/场景**：使用一套口语推理任务评测套件（原文提及“a suite of spoken reasoning tasks”），其中包括ARC-Easy等基准。
- **对比方法**：
  - 基线语音LLM（无CoT微调）
  - 基于启发式规则（heuristic-based）的推理启动策略
  - 最终比较了所提出的“熵指标引导”与“DPO优化”组合方法
- **评估指标**：推理准确率、响应延迟、准确率-延迟权衡。

## 4. 资源与算力
- **算力说明**：所提供的摘要及元数据中**未明确提及**GPU型号、数量或训练时长等具体算力使用情况。无法从现有信息中获知计算资源细节。

## 5. 实验充分性
- **实验组数**：从描述推断至少包含以下维度：
  - 多个口语推理任务上的评估；
  - CoT微调前后的准确率对比（2.4倍提升为平均值）；
  - 不同推理启动策略的延迟与准确率对比；
  - DPO进一步优化的帕累托前沿分析。
- **充分性与客观性**：
  - 实验覆盖了多个任务，避免了单一任务偏倚。
  - 对比了启发式方法与基于熵的动态策略，并有消融式的DPO验证，设计较为周全。
  - 客观性：使用标准基准任务和定量指标，对比公平。但未提供具体的实验组数统计，细节依靠摘要概括，可能省略了部分验证完备性的说明。

## 6. 主要结论与发现
1. 在文本空间应用CoT微调，可使语音LLM在口语推理任务上的准确率平均提升**2.4倍**。
2. 提出的“问题完整性”熵指标能够更精细地控制推理启动时机，相比启发式方法，在同等延迟下ARC-Easy准确率提高**4%**。
3. 结合DPO后，可在**无精度损失情况下将延迟降低70%**，显著优化了实时交互体验。

## 7. 优点
- 将文本LLM的CoT能力成功适配到多流语音LLM，解决了语音模型的推理短板。
- 创新性地将“边听边想”概念落地为可训练、可控的推理启动策略，并用信息熵提供理论指导。
- 实验展示了从准确率提升到延迟优化的完整链路，最终实现帕累托前沿的实质改进。
- 引入DPO微调，从偏好优化的角度进一步突破准确率-延迟权衡。

## 8. 不足与局限
- **实验细节缺失**：摘要未提供具体数据集列表、任务数量、模型规模及算力等关键信息，难以评估结论的泛化性和可复现性。
- **潜在偏差风险**：仅提及推出口语推理任务，但任务构成、语言、说话风格等可能受限。
- **应用限制**：“边听边想”要求部分聆听后进行推理，可能不适合极短指令或高度依赖完整上下文信息的任务；延迟降低70%的场景可能只在特定条件下成立。
- **未讨论**：多流架构本身的计算开销、实时流式处理的工程复杂性、推理错误对用户体验的影响等均未在摘要中涉及。

（完）
