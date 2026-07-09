---
title: PRIM：Cooperative Dynamic Token Compression for Efficient Large Multimodal Models
title_zh: PRIM：面向高效大模型的多模态协同动态令牌压缩
authors: "Song Li, yongping xiong"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9a7bf8f689436e9efb65b4e8e487e809cd58deaf.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 基于注意力动态压缩音视频令牌以实现高效多模态推理
tldr: 处理长时音视频内容时，大模型因令牌数量过多导致计算和内存开销巨大。PRIM框架提出基于注意力动态和指令相关性的协同令牌压缩方法，自适应地剪枝冗余音视频表示。实验表明，该方法显著降低推理开销，同时保持多模态理解性能，为高效音视频处理提供了新途径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 长时音视频处理导致多模态大模型推理代价过高，现有统一压缩忽略模态差异。
method: 设计基于注意力动态和指令相关性的协同压缩框架，自适应剪枝音视频令牌。
result: 在多个多模态基准上降低了计算和内存开销，同时保持准确率。
conclusion: PRIM实现了高效的音视频多模态推理，为资源受限场景下的应用提供了可能。
---

## Abstract
Large multimodal models (LMMs) have shown strong capabilities in audio-visual understanding by jointly reasoning over visual, auditory, and linguistic inputs. However, processing long-form audio-visual content often requires a large number of tokens, leading to substantial computational and memory overhead during inference. Existing efficiency-oriented methods typically apply uniform compression or pruning strategies, which overlook modality-specific characteristics and instruction-dependent reasoning behaviors in multimodal models. In this work, we present PRIM, an inference framework for efficient multimodal reasoning that systematically compresses audio-visual representations based on attention dynamics and instruction relevance. Motivated by an attention-based analysis revealing modality imbalance and layer-wise redundancy in LMMs, PRIM introduces a cooperative compression pipeline that spans both multimodal encoders and the language model. Specifically, PRIM performs early text-conditioned audio-visual fusion to externalize cross-modal interactions, applies attention-guided and frequency-aware strategies to remove redundant audio and video tokens, and further adapts token retention inside the language model according to task demands. Extensive experiments on multiple audio-visual benchmark datasets demonstrate that PRIM consistently achieves stable and superior efficiency--accuracy trade-offs across diverse tasks and datasets. These results demonstrate that PRIM, a multimodal cooperative compression approach, provides an effective pathway toward scalable and efficient audio-visual reasoning.

---

## 论文详细总结（自动生成）

基于提供的论文内容无法生成详细的中文总结。分析如下：

- **实际获取内容**：您给出的“论文 PDF 提取文本”仅包含 OpenReview 的验证页面提示，并非论文全文，也没有提供摘要、方法论、实验细节等任何实质性学术内容。
- **元数据信息**：附带的“论文 Markdown 元数据”虽然列出了标题、作者、摘要片段、方法概括等，但缺少完整的摘要、具体技术细节、实验配置、资源消耗等必要信息，无法支撑要求中的分项深度总结。
- **建议下一步**：
  - 请核实是否成功绕过了验证或已完成人机验证，获取到了真正的 PDF 文本；
  - 或者，提供直接从原论文中提取的完整 Markdown 文本，以便我按要求的 8 个方面进行结构化分析。

若您希望基于现有的元数据片段生成一个极简的概括性摘要（不满足“详细”要求），我可以尝试，但无法满足您原定的分层详细总结标准。请提供完整内容后，我将立即为您生成符合要求的分析。

（完）
