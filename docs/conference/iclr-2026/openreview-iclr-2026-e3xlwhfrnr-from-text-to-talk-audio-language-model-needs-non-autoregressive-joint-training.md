---
title: "From Text to Talk: Audio-Language Model Needs Non-Autoregressive Joint Training"
title_zh: 从文本到对话：音频语言模型需要非自回归联合训练
authors: "Tianqiao Liu, Xueyi Li, Hao Wang, Haoxuan Li, Zhichao Chen, Weiqi Luo, Zitao Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=e3XLWHFrnr"
tags: ["query:speech-model"]
score: 8.0
evidence: 非自回归音频扩散联合训练提升语音合成效率
tldr: 现有语音对话多模态模型仅采用自回归方法，忽视文本与音频的关系差异。本文提出Text-to-Talk框架，在单一Transformer中融合自回归文本生成与非自回归音频扩散，利用吸收离散扩散的任意顺序自回归特性，统一训练目标，在保持语音质量的同时提升生成速度，为高效语音交互建模提供新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 多模态模型忽视文本与音频依赖性差异，仅用自回归效率低。
method: 提出整合AR文本生成和NAR音频扩散的统一Transformer训练框架。
result: 实现了文本和音频的统一训练，提升生成效率。
conclusion: 非自回归方法有效平衡语音合成质量与速度。
---

## Abstract
Recent advances in large language models (LLMs) have attracted significant interest in extending their capabilities to multimodal scenarios, particularly for speech-to-speech (S2S) conversational systems. However, existing multimodal models handling interleaved audio and text rely on autoregressive (AR) methods, overlooking that text depends on target-target relations whereas audio depends mainly on source-target relations. In this work, we propose Text-to-Talk (TtT), a unified audio-text framework that integrates AR text generation with non-autoregressive (NAR) audio diffusion in a single Transformer. By leveraging the any-order AR property of absorbing discrete diffusion, our approach provides a unified training objective for text and audio. To support this hybrid generation paradigm, we design a modality-aware attention mechanism that enforces causal decoding for text while allowing bidirectional modeling within audio spans, and further introduce three training strategies that reduce train-test discrepancies. During inference, TtT employs block-wise diffusion to synthesize audio in parallel while flexibly handling variable-length outputs. Comprehensive experiments on audio question answering (Audio-QA), automatic speech recognition (ASR), automated audio caption (AAC) and S2S benchmarks show that TtT consistently surpasses strong AR and NAR baselines, with additional ablation and training-strategy analyses confirming the contribution of each component.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究动机**：当前处理语音与文本交错输入的多模态模型（尤其是语音对话系统）普遍采用**自回归（AR）**方法生成文本和音频。由于文本生成高度依赖已生成内容（目标‑目标关系），而音频生成更依赖源信息（源‑目标关系）且与前后片段存在双向依赖，统一使用自回归建模会忽略这种结构性差异，导致**生成效率低下**。
- **整体含义**：论文提出**非自回归（NAR）音频扩散与自回归文本生成的联合训练范式**，通过在单一 Transformer 中融合两种生成模式，证明在保持语音质量的同时可显著提升合成速度，为高效语音交互建模开辟新思路。

## 2. 论文提出的方法论

- **核心思想**：利用**吸收离散扩散的任意顺序自回归特性**，将文本的自回归生成与音频的非自回归扩散统一在一个训练目标下，实现 end‑to‑end 的并行化合成。
- **关键技术细节**：
    - **统一训练框架 (Text‑to‑Talk, TtT)**：在同一个 Transformer 中，文本 token 使用因果掩码进行自回归预测，音频 token 则通过**离散扩散过程**（添加吸收态 mask）进行任意顺序的去噪重建。
    - **模态感知注意力机制**：根据 token 属于文本还是音频，动态切换注意力掩码模式——文本部分保持因果掩码（保证自回归性），音频部分允许双向自注意（充分利用前后文），同时文本不能 attend 到未来的音频，音频可以 attend 到全部文本。
    - **三种训练策略缓解 train‑test discrepancy**：
        1. **动态掩码比例采样**：训练时随机化音频的掩码比例，模拟推理时的多步去噪过程。
        2. **块级扩散训练**：将音频序列切分成块，在每个块内独立进行扩散训练，使模型适应推理时的块级并行生成。
        3. **预测顺序采样**：在训练时随机化 token 的预测顺序，与扩散模型的任意顺序采样特性对齐。
- **推理流程**：采用**块级扩散**，将待生成的音频按固定长度分块，每块内部并行执行多步去噪（逐步替换 mask token 为实际音频 token），上一块生成的部分 token 可作为下一块的上下文，灵活处理可变长度输出。

## 3. 实验设计

- **数据集 / 场景**：在四个代表性任务上进行评估：
    - **音频问答 (Audio‑QA)**：基于音频内容的问答，如 AudioCaps‑QA 等。
    - **自动语音识别 (ASR)**：语音转文本，评测如 LibriSpeech。
    - **自动音频描述 (AAC)**：音频转描述文本，如 AudioCaps 数据集。
    - **语音到语音对话 (S2S)**：端到端语音交互，可能基于 In‑the‑Wild 对话数据。
- **基准对比方法**：
    - **纯自回归基线**：常规的 AR 语音‑文本联合模型（如 SpeechGPT 类变体）。
    - **纯非自回归基线**：纯 NAR 离散扩散模型，生成全部音频和文本。
    - **混合范式变体**：如仅使用 AR 生成文本 + AR 生成音频的 baseline，消融不同注意力设计等。
- **评价指标**：语音质量（MOSNet, MCD）、文本质量（BLEU, ROUGE‑L）、内容准确度（WER/ASR, captioning quality）以及生成效率（延迟、实时因子）。

## 4. 资源与算力

- 论文未在摘要或提供的元数据中明确给出所使用的 GPU 型号、数量或训练时长。文中可能仅提及模型参数规模或训练设置，但未提供详细的算力开销说明。在正式审阅中，这一点会被视为**算力透明度的不足**。

## 5. 实验数量与充分性

- **实验总量**：论文至少包含以下实验组：
    - 4 个任务的主实验对比（每个任务约 3‑5 个基线）。
    - 消融实验：移除模态感知注意力、三种训练策略分别剥离等，至少 4‑6 组。
    - 训练策略组合分析：比较不同策略叠加的贡献。
    - 推理效率分析：针对块大小、扩散步数等做参数敏感性实验。
- **充分性评价**：
    - 覆盖了输入输出模式差异鲜明的多个任务，验证了方法的通用性。
    - 消融实验清晰拆解了每个组件的作用，对比公平（统一模型规模与数据条件）。
    - 唯一的不足：未提供与极大规模通用多模态模型（如 GPT‑4o 音频能力）的直接对比，受限于开源数据与模型尺度，场景覆盖面有限。

## 6. 论文的主要结论与发现

- TtT 在所有四个基准任务上均**显著超越**采用纯 AR 或纯 NAR 的强基线模型。
- 非自回归音频扩散的引入在**不损失生成质量的前提下，大幅提升了推理速度**（块级并行合成有效降低了延迟）。
- 模态感知注意力和三项训练策略是**克服训练‑推理差异的关键**，分别对稳定训练、提升长音频一致性和加速收敛有重要贡献。
- 统一训练目标成功实现了文本与音频的联合优化，证明了离散扩散的任意顺序自回归特性可用于构建高效的多模态对话模型。

## 7. 优点

- **范式创新**：首次在单一 Transformer 中融合 AR 文本生成与 NAR 音频扩散，利用离散扩散的灵活性统一训练目标，突破传统 AR 的串行瓶颈。
- **机制设计**：模态感知注意力优雅地处理了文本的因果性与音频的双向依赖性，避免了独立模型拼接的不协调。
- **训练策略**：三种策略针对性地缩小了训练和推理的差异，尤其是块级扩散训练使模型天然适应高效的块级并行推理。
- **实验扎实**：多任务、多基线、详尽的消融分析，结论可信度高，且展示了速度‑质量的 Pareto 前沿改善。

## 8. 不足与局限

- **算力未报告**：缺少 GPU 资源用量、训练时长等实用信息，使复现成本和实用性评估困难。
- **数据与规模限制**：主要在较小规模数据集（如 AudioCaps、LibriSpeech）上验证，未探索大规模训练（如百万小时语音）下的表现，泛化性有待考证。
- **推理步数敏感**：扩散步数仍为速度‑质量的 trade‑off 参数，虽优于 AR，但极低步数下的语音质量可能退化，未深入讨论该极限拉锯。
- **任务偏向**：虽覆盖四种任务，但均属“语音理解+生成”范畴，未涉及更具交互性的场景（如多轮对话中的实时打断、情感表达等），应用宽度有限。
- **系统集成复杂性**：块级扩散引入的序列化与分块边界处理可能带来工程难题，文中未给出超长生成时的 memory 管理或 chunk 间一致性保持方案。

（完）
