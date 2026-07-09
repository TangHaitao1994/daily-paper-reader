---
title: "SpeechOp: Inference-Time Task Composition for Generative Speech Processing"
title_zh: SpeechOp：生成式语音处理的推理时任务组合
authors: "Justin Lovelace, Rithesh Kumar, Jiaqi Su, Ke Chen, Kilian Q Weinberger, Zeyu Jin"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eLsEjjFODE"
tags: ["query:speech-model"]
score: 9.0
evidence: 多任务模型提升TTS质量并统一语音处理
tldr: 提出SpeechOp，一种多任务潜扩散模型，将预训练TTS系统转化为通用语音处理器。通过继承TTS的丰富自然语音理解能力，该模型在多种语音任务上取得优异表现，同时增强了核心TTS质量。该方法为高效且高质量的语音处理提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音处理任务（如增强）数据稀缺，导致生成模型失真；TTS系统强大但未能直接用于这些任务。
method: 提出多任务潜扩散模型，将预训练TTS转化为通用语音处理器，支持推理时组合多种任务。
result: 在提升TTS质量的同时，改善了语音到语音任务（如增强）的性能。
conclusion: 该框架有效结合了TTS的丰富先验与多种语音处理能力，推动了统一语音生成模型的发展。
---

## Abstract
While generative Text-to-Speech (TTS) systems leverage vast "in-the-wild" data to achieve remarkable success, speech-to-speech processing tasks like enhancement face data limitations, which lead data-hungry generative approaches to distort speech content and speaker identity. To bridge this gap, we present SpeechOp, a multi-task latent diffusion model that transforms pre-trained TTS models into a universal speech processor capable of performing a wide range of speech tasks and composing them in novel ways at inference time. By adapting a pre-trained TTS model, SpeechOp inherits a rich understanding of natural speech, accelerating training and improving S2S task quality, while simultaneously enhancing core TTS performance. Finally, we introduce Implicit Task Composition (ITC), a novel pipeline where ASR-derived transcripts (e.g., from Whisper) guide SpeechOp's enhancement via our principled inference-time task composition. ITC achieves state-of-the-art content preservation by robustly combining web-scale speech understanding with SpeechOp's generative capabilities.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义
- **研究背景**：文本到语音（TTS）系统依赖大量“野外”数据取得显著成功，但语音到语音（S2S）处理任务（如增强、降噪）面临标注数据稀缺的困境，导致数据饥渴的生成式方法容易扭曲语音内容和说话人身份。
- **核心问题**：如何将预训练TTS系统中丰富的自然语音理解能力迁移到数据匮乏的语音处理任务，同时不牺牲语音内容与说话人保真度。
- **整体含义**：提出SpeechOp，将预训练TTS模型改造为通用语音处理器，通过继承其先验知识，在多个语音任务上获得高质量输出，并提升TTS本身的质量。

### 2. 方法论
- **核心思想**：构建一个多任务潜扩散模型（Latent Diffusion Model），以预训练TTS模型为基座，统一执行语音生成（TTS）与语音处理（S2S）任务，并支持在推理时以新颖方式组合多个任务。
- **关键技术细节**：
  - **基座模型**：采用预训练的TTS潜扩散模型，保留其对自然语音发音、韵律、内容-声学映射的强大先验。
  - **多任务适配**：将语音增强、转换等S2S任务统一融入同一扩散框架，通过特定条件输入（如含噪语音、源语音）引导生成。
  - **Implicit Task Composition (ITC)**：一种推理时组合策略，利用ASR（如Whisper）转录文本指导SpeechOp的增强过程，实现鲁棒的内容保留。该管道将大规模语音识别理解与扩散生成能力稳健结合。
- **公式/流程**（文字描述）：未提供具体公式，流程大致为：以含噪语音或其他任务输入作为条件，结合可选的文本转录（ITC模式），在扩散反向过程中逐步将潜变量去噪为目标语音，模型共享TTS基座参数。

### 3. 实验设计
- **数据集/场景**：
  - TTS任务：基于大规模“野外”数据预训练（未指明具体数据集）。
  - S2S任务：语音增强等任务的标准benchmark（如VoiceBank-Demand等可能相关数据集，摘要未详列）。
- **基准对比**：与现有数据驱动的生成式语音增强/处理方法、以及可能的独立TTS与S2S模型对比。
- **对比方法**：摘要未明确列举，推测包括典型扩散S2S模型、端到端增强模型。

### 4. 资源与算力
- 文中未明确说明使用的GPU型号、数量或训练时长。由于采用预训练TTS并加以适配，预期训练成本低于从零开始训练多任务S2S模型，但具体算力信息缺失。

### 5. 实验数量与充分性
- **实验数量**：摘要未给出精确数量，但暗示包含以下维度：
  - TTS质量提升实验（衡量核心TTS改善）。
  - 多种S2S任务性能实验（如增强、可能转换等）。
  - 消融研究（验证继承TTS先验、ITC策略的有效性）。
  - 不同组合方式的推理测试。
- **充分性与公平性**：基于摘要评价（score 9.0，ACCEPT），实验设计应较全面，对比基准客观。但缺乏原文具体表格只能做概要判断。

### 6. 主要结论与发现
- SpeechOp在提升TTS自身质量的同时，显著改善了语音到语音处理任务的性能。
- ITC策略通过结合大规模ASR，实现了最先进的内容保留能力。
- 继承TTS先验有效加速训练、提升S2S任务质量。
- 统一模型能灵活组合任务，无需针对每种任务/组合单独训练。

### 7. 优点
- **模型统一性**：用单一模型覆盖TTS与多种S2S任务，避免重复开发。
- **推理时组合**：ITC提供原则性的任务组合方式，灵活性高。
- **质量提升**：罕见地同时增强TTS任务性能，体现正向迁移。
- **数据效率**：利用预训练TTS先验缓解S2S数据稀缺问题。

### 8. 不足与局限
- **具体算力未公开**：无法评估模型实际部署成本。
- **任务覆盖局限**：摘要仅强调增强任务，其他S2S任务（如分离、编辑）的验证程度未知。
- **依赖ASR系统**：ITC效果受制于外部ASR精度，在低资源语言或有噪场景可能下降。
- **未提及推理延迟**：扩散模型通常迭代步数多，实时性未讨论。
- **潜在偏差**：基于TTS预训练数据分布的语音风格可能引入偏向，降低极低资源域的表现。

（完）
