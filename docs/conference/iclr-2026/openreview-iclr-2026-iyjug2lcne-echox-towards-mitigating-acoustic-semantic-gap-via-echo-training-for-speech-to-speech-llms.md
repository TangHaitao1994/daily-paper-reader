---
title: "EchoX: Towards Mitigating Acoustic-Semantic Gap via Echo Training for Speech-to-Speech LLMs"
title_zh: EchoX：通过回声训练弥合声学-语义鸿沟以改进语音到语音大语言模型
authors: "Yuhao Zhang, Yuhao Du, Zhanchen Dai, Xiangnan Ma, Kaiqi Kou, Benyou Wang, Haizhou Li"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=iYjUG2LcnE"
tags: ["query:speech-model"]
score: 9.0
evidence: 弥合语音到语音大模型的声学-语义鸿沟，保持对话AI推理能力
tldr: 针对语音到语音大模型中因声学-语义鸿沟导致的知识和推理能力退化问题，提出EchoX框架，利用语义表示动态生成语音训练目标，融合声学与语义学习，仅用约六千小时数据即可保持强推理能力，显著提升了口语对话AI的智能水平。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 语音到语音大模型常因训练范式未能弥合声学-语义鸿沟而导致推理能力下降。
method: 提出EchoX，利用语义表示动态生成语音训练目标，融合声学与语义学习。
result: EchoX在约六千小时数据上训练，保持了强大的推理能力。
conclusion: EchoX为语音大模型提供了有效的声学-语义对齐训练方法，增强了对话AI的智能性。
---

## Abstract
Speech-to-speech large language models (SLLMs) are attracting increasing attention. Derived from text-based large language models (LLMs), SLLMs often exhibit degradation in knowledge and reasoning capabilities. We hypothesize that this limitation arises because current training paradigms for SLLMs fail to bridge the acoustic-semantic gap in the feature representation space. To address this issue, we propose EchoX, which leverages semantic representations and dynamically generates speech training targets. This approach integrates both acoustic and semantic learning, enabling EchoX to preserve strong reasoning abilities as a speech LLM. Experimental results demonstrate that EchoX, with about six thousand hours of training data, achieves advanced performance on multiple knowledge-based question-answering benchmarks.

---

## 论文详细总结（自动生成）

很抱歉，由于提供的论文提取文本内容并非论文正文，而是 OpenReview 的验证页面（“Complete the check below to continue to OpenReview”），我无法获取论文的实际内容。我只能依据给定的元数据（标题、作者、摘要等信息）来进行概括性总结。请知悉，以下总结仅基于元数据，缺乏详细方法、实验和结论的具体描述。

---

## 1. 论文的核心问题与整体含义
- **研究动机**：语音到语音大语言模型（SLLMs）通常由文本大语言模型（LLMs）衍生而来，但在知识记忆和推理能力上往往出现明显退化。
- **核心问题**：当前 SLLMs 的训练范式未能有效弥合声学特征与语义特征之间的差距（acoustic-semantic gap），导致模型难以同时保留语言理解能力和高质量语音生成能力。
- **整体含义**：为解决这一鸿沟，需要一种新的训练方法，使语音 LLM 既能保持文本 LLM 的强推理能力，又能自然地进行语音对话。

## 2. 论文提出的方法论
- **方法名称**：EchoX
- **核心思想**：利用语义表征动态生成语音训练目标，将声学学习和语义学习融合在同一个训练框架中。
- **技术细节**（基于摘要推断）：
  - 通过语义表示（semantic representations）作为桥梁，动态构建语音输出目标，而不是直接使用离散语音编码或纯文本目标。
  - 在训练过程中同时优化声学特征预测和语义一致性，从而拉近声学空间与语义空间的距离。
- **关键流程**（推测）：
  1. 从语音输入提取声学特征和语义特征；
  2. 语义表示用于动态生成训练目标，指导语音解码；
  3. 损失函数可能同时包含声学重建损失和语义对齐损失。

## 3. 实验设计
- **数据集**：使用了约六千小时（~6k hours）的训练数据，具体数据集名称未提供。
- **评测基准**：多个人工智能知识问答基准（knowledge-based question-answering benchmarks），如可能包含 CommonsenseQA、MMLU 等（未详细说明）。
- **对比方法**：应与现有 SLLMs 或级联式语音系统对比，但元数据未列出具体方法名称。
- **场景**：聚焦于保持推理能力的语音到语音对话。

## 4. 资源与算力
- 元数据中**未明确说明** GPU 型号、数量或训练时长。仅提及使用了约六千小时数据训练，缺乏算力细节。

## 5. 实验数量与充分性
- **实验数量**：无法从元数据得知具体进行了多少组实验，但预期包含：
  - 多个知识问答基准上的主实验；
  - 消融实验验证 EchoX 各组件的效果；
  - 与多种基线方法的比较。
- **充分性与客观性**：由于缺乏全文，无法评估实验是否充分，也无法判断对比是否公平、是否避免了数据泄露。

## 6. 论文的主要结论与发现
- EchoX 在约六千小时训练数据上取得了先进的知识问答表现，证明该方法能够**有效保持语音 LLM 的推理能力**。
- 将声学与语义学习融合的训练范式是弥合声学-语义鸿沟的关键。

## 7. 优点
- **方法创新性**：提出动态生成语音训练目标的思想，不同于传统的将语音离散化为 token 的做法。
- **效率**：仅用六千小时数据即取得 strong reasoning abilities，相比可能需要数万小时数据的方案更高效。
- **任务导向明确**：针对实际问题（推理退化）设计解决方案，实用性强。

## 8. 不足与局限
- **信息缺失严重**：无法分析具体实验细节、数据集、对比方法，因此无法判断其局限的真实程度。
- **依赖元数据风险**：元数据中的结论可能未经充分验证，存在自评偏差。
- **可能局限**（推测）：
  - 对语义表示模型有依赖性，若语义提取不准确会影响整体效果；
  - 仅在知识问答场景下测试，其他语音交互任务（如情感对话、多轮闲聊）的泛化性未知；
  - 六千小时数据虽少，但数据来源和语言覆盖度不明。

（完）
