---
title: "SPEAR: A Unified SSL Framework for Learning Speech and Audio Representations"
title_zh: SPEAR：学习语音与音频统一表示的自监督框架
authors: "Xiaoyu Yang, Yifan Yang, Zengrui Jin, Ziyun Cui, Wen Wu, Baoxiangli, Chao Zhang, Phil Woodland"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=LXTf8GUg8f"
tags: ["query:speech-model"]
score: 9.0
evidence: 语音与音频统一自监督表示学习框架
tldr: 现有自监督学习多为语音或音频单一领域设计，SPEAR首次提出统一框架，通过多码本矢量量化和掩码预测细粒度离散token，从混合数据中学习域通用的语音与音频表示。实验表明其跨域能力显著提升，为通用声学表征学习提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 语音与音频自监督学习长期领域割裂，缺乏统一表示模型。
method: 提出基于多码本矢量量化的离散token掩码预测统一预训练目标。
result: 首次成功从混合数据学习跨域统一表示，性能优于单域模型。
conclusion: SPEAR为通用声学表征学习开辟了新方向，跨域能力显著提升。
---

## Abstract
Self-Supervised Learning (SSL) excels at learning generic representations of acoustic signals, yet prevailing methods remain domain-specific, tailored to either speech or general audio, hindering the development of a unified representation model with a comprehensive capability over both domains. To address this, we present SPEAR (SPEech and Audio Representations), the first SSL framework to successfully learn unified speech and audio representations from a mixture of speech and audio data. SPEAR proposes a unified pre-training objective based on masked prediction of fine-grained discrete tokens for both speech and general audio. These tokens are derived from continuous speech and audio representations using a Multi-codebook Vector Quantisation (MVQ) method, retaining rich acoustic detail essential for modelling both speech and complex audio events. SPEAR is applied to pre-train both single-domain and unified speech-and-audio SSL models. Our speech-domain model establishes a new state-of-the-art on the SUPERB benchmark, a speech processing benchmark for SSL models, matching or surpassing the highly competitive WavLM Large on 12 out of 15 tasks with the same pre-training corpora and a similar model size. Crucially, our unified model learns complementary features and demonstrates comprehensive capabilities across two major benchmarks, SUPERB and HEAR, for evaluating audio representations. By further scaling up the model size and pre-training data, we present a unified model with 600M parameters that excels in both domains, establishing it as one of the most powerful and versatile open-source SSL models for auditory understanding. The inference code and pre-trained models will be made publicly available.

---

## 论文详细总结（自动生成）

# SPEAR 论文总结

## 1. 论文的核心问题与整体含义
- **问题背景**：当前自监督学习（SSL）在声学信号表示学习中表现优异，但主流方法均为领域特定设计，要么专注于语音，要么专注于通用音频，缺乏一个能够同时处理两类信号的统一表示模型。
- **研究动机**：语音与音频 SSL 长期领域割裂，阻碍了通用听觉理解模型的开发。论文提出 SPEAR，旨在从混合的语音与音频数据中学习统一的跨域表示，填补这一空白。
- **整体含义**：SPEAR 是首个成功在混合数据上训练出统一语音与音频表示的 SSL 框架，为构建通用的声学表征模型提供了全新范式。

## 2. 方法论
- **核心思想**：设计一种统一的预训练目标，让模型在语音和通用音频两种数据上都能进行掩码预测。
- **关键技术细节**：
  - 使用**多码本矢量量化（Multi‑codebook Vector Quantisation, MVQ）** 方法，将连续的语音与音频表示转换为细粒度的离散 token。
  - 这些离散 token 保留了丰富的声学细节，对建模语音的细微变化和复杂音频事件均至关重要。
  - 预训练任务为**掩码预测**：随机遮蔽输入的部分帧，要求模型预测对应位置被量化后的离散 token。
  - 整个过程在混合的语音和音频数据上进行，实现领域通用的表示学习。
- **模型扩展**：单域模型规模与 WavLM Large 类似；进一步扩展得到 600M 参数的统一模型。

## 3. 实验设计
- **数据集/场景**：
  - 预训练数据：混合了语音和通用音频的数据集（摘要未列出具体名称）。
  - 下游评估基准：
    - 语音领域：**SUPERB** 基准（涵盖 15 项语音处理任务）。
    - 音频领域：**HEAR** 基准（音频表示评估）。
- **对比方法**：
  - 单域语音模型：与高度竞争力的 WavLM Large 进行直接比较。
  - 统一模型：在 SUPERB 和 HEAR 上展示跨域综合能力，并与单域模型进行消融对比（具体消融实验细节未在摘要中展开）。
- **主要结果**：
  - 单域语音模型在 SUPERB 的 15 项任务中，12 项匹配或超越同数据量的 WavLM Large，达到新 SOTA。
  - 统一模型在两个基准上均表现出互补特征和全面能力；600M 参数版本在两个领域均表现卓越，成为最强开源 SSL 模型之一。

## 4. 资源与算力
- 论文摘要中**未明确说明**训练所使用的 GPU 型号、数量、训练时长或具体算力消耗。
- 仅提及“进一步扩展模型规模与预训练数据”，暗示使用了大规模计算资源，但缺乏量化数据。

## 5. 实验数量与充分性
- **实验覆盖**：
  - 包含单域语音模型在 SUPERB（15 个任务）上的完整评估。
  - 统一模型同时在 SUPERB 和 HEAR 两个基准上进行跨域评估。
  - 比较了不同模型规模（基模型与 600M 大模型）的效果。
- **充分性与公平性**：
  - 与当前 SOTA（WavLM Large）在相同数据量和相似参数规模下进行对比，保证了公平。
  - 因摘要未展示具体消融实验的设计（如不同量化方式、码书数量、混合数据比例等），无法判断实验的全面性，但从已有结果看，关键结论得到了充分支撑。
  - 整体实验设计客观、公平，数量足以论证声明的核心贡献。

## 6. 主要结论与发现
- SPEAR 首次证明**可以从混合语音与音频数据中学习到统一的声学表示**，填补了领域割裂的空白。
- 单域模型在 SUPERB 上达到新的 SOTA，表明离散 token 掩码预测范式具有极强的领域内竞争力。
- 统一模型在语音和音频两大基准上均展现优势，且通过扩大规模可进一步提升表现，为通用听觉 SSL 模型开辟了新方向。

## 7. 优点
- **创新性统一框架**：率先将语音和音频的 SSL 统一到一个预训练目标下，具有较强的引领性。
- **技术简洁有效**：多码本矢量量化结合掩码预测，既保留了丰富的声学细节，又便于跨域推广。
- **性能强大**：单域即超越强基线，统一模型跨域能力突出，600M 版本成为最强大的开源 SSL 模型之一。
- **可复现与开放**：声称将公开推理代码和预训练模型，利于社区发展。

## 8. 不足与局限
- **训练细节缺失**：摘要未提供预训练数据的具体构成、规模、计算开销，难以评估资源需求。
- **实验覆盖有限**：消融实验与更细粒度的分析（如对不同类型音频事件的表现、量化配置的影响）未见展示。
- **潜在偏差**：依赖特定基准（SUPERB、HEAR），在更多样化的实际场景（如多语言语音、环境声音分类等）上的泛化性尚未验证。
- **应用限制**：虽然实现了统一表示，但在极细粒度任务或特定域上的适应能力仍需进一步探索。

（完）
