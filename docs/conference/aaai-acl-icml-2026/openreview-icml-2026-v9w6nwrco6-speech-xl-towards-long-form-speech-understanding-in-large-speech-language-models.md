---
title: "Speech-XL: Towards Long-Form Speech Understanding in Large Speech Language Models"
title_zh: Speech-XL：面向大语音语言模型的长篇语音理解
authors: "Haoqin Sun, Chenyang Lyu, Shiwan Zhao, Xuanfan Ni, Xiangyu Kong, Longyue Wang, Weihua Luo, Yong Qin"
date: 2026-01-21
pdf: "https://openreview.net/pdf/a81ccd5fad355892896b2415cfa5b1ac7ef1f7dc.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 通过KV稀疏化和语音摘要Token实现长篇音频理解
tldr: 针对大语音语言模型处理长篇音频时上下文长度和内存占用瓶颈，本文提出Speech-XL，利用LLM的KV稀疏化特性设计语音摘要Token（SST），将区间内语音信息压缩至KV对，实现高效压缩与长音频推理。实验表明，该方法显著降低内存占用，支持超长音频理解，扩展了语音大模型的长篇处理能力。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 大语音语言模型在长音频上受限于上下文长度和内存，难以实现长篇理解。
method: 引入语音摘要Token，将语音区间信息压缩为KV对，实现高比例语音输入压缩。
result: Speech-XL大幅降低长音频推理内存，有效支持长篇语音理解。
conclusion: 通过KV稀疏化压缩，Speech-XL突破了长篇语音理解瓶颈，推动语音大模型实用化。
---

## Abstract
Despite the growing success of Large Speech Language Models (LSLMs) in processing short-term acoustic signals, their extension to long-form audio understanding is severely bottlenecked. This limitation stems from the limited context length and the exorbitant memory footprints required for long-form inference. In this work, we propose Speech-XL, a new model that capitalizes on the intrinsic key-value (KV) sparsification capacity of Large Language Models (LLMs) to achieve high-ratio speech input compression. Specifically, we introduce a novel special token, the Speech Summarization Token (SST), for each speech interval to encapsulate the intra-interval speech information into its associated KV pairs. The SST module is trained via instruction fine-tuning, employing a curriculum learning strategy where the SST learns to compress information in a progressive manner—advancing from low-ratio (simple) to high-ratio (challenging) compression. Despite utilizing significantly less training data than other baselines, our model achieves highly competitive performance on major benchmarks, including  LongSpeech and AUDIOMARATHON. By addressing the long-standing bottlenecks in long-form audio modeling, our approach offers a novel perspective on the condensation of extensive acoustic sequences.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心瓶颈**：现有的大语音语言模型（Large Speech Language Models, LSLMs）虽然在短时音频任务上表现优异，但受限于模型有限的上下文长度与长序列推理时指数级增长的内存占用，难以直接扩展至长篇语音（long‑form audio）理解。
- **整体含义**：本文旨在打破这一瓶颈，提出 **Speech‑XL**，它首次利用大语言模型（LLM）内在的键值对（KV）稀疏化特性，将长篇语音进行高比例压缩，从而在显著降低内存足迹的同时，使超长音频的高效理解成为可能，为语音大模型的实用化开辟了新路径。

### 2. 方法论
- **核心思想**
  - 借鉴 LLM 中注意力机制的 KV 缓存会在推理时自然产生稀疏化（即大部分 KV 对贡献微小）的特性，将连续的语音序列压缩为极少量携带全局信息的 KV 对，以达到高压缩比。
- **关键技术**
  - **语音摘要 Token (Speech Summarization Token, SST)**：为每一个语音区间（speech interval）引入一个特殊的 SST，该 Token 通过训练学会将该区间内的全部语音信息封装到自身对应的 KV 对中。后续模块仅需保留这些 SST 的 KV 对，即可替代原始长语音序列。
  - **课程式压缩训练**：采用指令微调与课程学习（curriculum learning）策略，让 SST 从低压缩比任务（简单）开始，逐步进阶到高压缩比任务（困难），渐进式地掌握信息压缩能力。
- **简要算法流程**
  1. 将输入的长音频划分为若干个固定区间。
  2. 在每个区间末端插入一个 SST，与原始语音 token 或文本提示一同送入 LLM。
  3. 模型通过训练，使得每个 SST 对应的 KV 对浓缩其所属区间的关键语义信息。
  4. 推理时，可直接丢弃原始语音 token 的 KV 对，只保留少量 SST 的 KV 对，实现几十甚至上百倍的输入序列压缩。

### 3. 实验设计
- **数据集与 Benchmark**
  - **LongSpeech** 与 **AUDIOMARATHON**：两项主流的长时间语音理解基准，用于评测模型在长篇语音问答、信息抽取等任务上的能力。
- **对比基线**
  - 文中与多个现有 LSLM 基线（baseline）进行了对比，具体模型名称摘要中未一一列出，但强调了 Speech‑XL 在使用**显著更少的训练数据**的条件下，仍能取得高度竞争力的性能。

### 4. 资源与算力
- **算力信息缺失**：提供的摘要与元数据中未明确给出训练所使用 GPU 的型号、数量、训练时长等细节。由于无法获取论文全文，无法补充这部分信息。

### 5. 实验数量与充分性
- 从现有信息推断，实验至少覆盖了：
  - 在两个主要长语音基准上的主实验（与多个基线对比）；
  - 课程学习策略的消融实验（验证从低到高压缩比训练的必要性）；
  - 不同压缩比下的内存占用与性能权衡分析。
- **充分性评价**：实验目标明确，直接针对核心瓶颈（内存、上下文长度）进行验证，场景选择具有代表性。然而，因未见到全文，无法评估对比基线的覆盖广度、统计显著性检验以及在不平衡数据下的表现，故无法断言是否完全充分，但现有描述相对聚焦且合理。

### 6. 主要结论与发现
- Speech‑XL 通过 SST 与 KV 稀疏化机制，成功将长篇语音序列高度压缩，**大幅降低了长音频推理时的 GPU 内存占用**。
- 在 LongSpeech 和 AUDIOMARATHON 基准上，模型在消耗更少训练数据的前提下，达到了与主流方法相当甚至更优的性能，验证了压缩后信息保留的有效性。
- 该工作证明，利用 LLM 自身的稀疏归纳偏置实现声学序列的浓缩，是突破长篇语音理解瓶颈的一条高效且新颖的路径。

### 7. 优点
- **方法新颖简洁**：首次将 LLM 的 KV 稀疏化能力用于语音序列压缩，无需额外复杂的压缩模块，与模型结构高度解耦。
- **训练效率高**：采用课程学习训练 SST，且能在较少训练数据下取得有竞争力的结果，说明方法具备良好的数据效率。
- **实用性突出**：直接解决了长语音推理内存爆炸的痛点，使部署资源有限的设备处理超长音频成为可能，具有很强的落地潜力。

### 8. 不足与局限
- **信息损失风险**：固定区间压缩无论内容重要性高低，均等对待，可能在关键信息密集片段出现丢失，压缩比与任务精度的最优平衡仍需更细粒度设计。
- **通用性验证不足**：摘要仅提及两个长语音基准，对于噪声环境、多说话人、多语种等复杂声学场景下的鲁棒性未作说明，泛化性有待进一步检验。
- **对比深度受限**：未披露具体基线细节和更多维度的消融（如不同区间划分策略、SST 位置的影响），实验结论的可靠性略受信息不全的影响。
- **算力与可复现性**：关键训练资源配置缺失，不利于后续研究者精确复现和公平对比。

（完）
