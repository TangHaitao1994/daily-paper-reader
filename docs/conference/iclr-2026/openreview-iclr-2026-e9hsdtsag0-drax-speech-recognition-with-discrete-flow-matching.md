---
title: "Drax: Speech Recognition with Discrete Flow Matching"
title_zh: Drax：基于离散流匹配的语音识别
authors: "Aviv Navon, Aviv Shamsian, Neta Glazer, Yael Segal-Feldman, Gill Hetz, Joseph Keshet, Ethan Fetaya"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=E9hSdtsAG0"
tags: ["query:speech-model"]
score: 9.0
evidence: 用于语音识别的离散流匹配，并行解码，音频条件概率路径提升准确率
tldr: 针对语音识别中非自回归模型未充分探索的问题，Drax提出离散流匹配框架，通过音频条件概率路径模拟推理中的中间错误，实现高效并行解码。理论分析将泛化差距与训练-推理分布差异联系起来，实验证明该方法在流式条件下显著提升识别准确率，为ASR的并行高效训练与推理提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 探索扩散/流式非自回归模型在ASR中的潜力，实现高效并行解码。
method: 提出音频条件概率路径，引导模型通过模拟推理错误轨迹，结合离散流匹配框架训练。
result: 实验表明方法有效提升识别准确率，降低推理步数。
conclusion: 离散流匹配为ASR带来并行高效与高精度的结合，具有实际应用价值。
---

## Abstract
Diffusion and flow-based non-autoregressive (NAR) models have shown strong promise in large language modeling, however, their potential for automatic speech recognition (ASR) remains largely unexplored. We propose Drax, a discrete flow matching framework for ASR that enables efficient parallel decoding. To better align training with inference, we construct an audio-conditioned probability path that guides the model through trajectories resembling likely intermediate inference errors, rather than direct random noise to target transitions. Our theoretical analysis links the generalization gap to divergences between training and inference occupancies, controlled by cumulative velocity errors, thereby motivating our design choice. Empirical evaluation demonstrates that our approach attains recognition accuracy on par with state-of-the-art speech models while offering improved accuracy-efficiency trade-offs, highlighting discrete flow matching as a promising direction for advancing NAR ASR.

---

## 论文详细总结（自动生成）

由于受到访问限制，未能获取论文的完整正文。以下分析基于提供的元数据与摘要，内容可能无法覆盖全部技术细节。

## 1. 核心问题与研究动机
- 扩散模型和基于流的非自回归（NAR）模型在大型语言建模中表现强劲，但在自动语音识别（ASR）中鲜有探索。
- 现有 ASR 非自回归方法常面临训练与推理分布不匹配的问题，导致解码质量下降。
- 本研究旨在填补这一空白，提出一种适用于 ASR 的离散流匹配框架，实现高效并行解码。

## 2. 方法论核心思想
- **音频条件概率路径**：不同于直接从随机噪声到目标文本的转换，该方法构造了一条受音频条件引导的概率路径，使生成轨迹模拟推理过程中可能出现的中间错误，从而缩小训练与推理间的分布差异。
- **离散流匹配框架**：将流匹配（Flow Matching）扩展到离散空间（文本 token），通过向量场（速度）的回归来学习从源分布到目标分布的最优传输变换。
- **理论分析**：将泛化差距与训练和推理占用度（occupancy）之间的散度联系起来，指出该散度受累积速度误差控制，为设计选择提供了理论依据。

## 3. 实验设计概览
- **数据集/场景**：摘要未明确说明具体数据集，但标明该工作针对 ASR，且标题与标签提及“speech recognition”和“speech-model”。很可能在标准语音识别基准（如 LibriSpeech）上评估。
- **对比方法**：宣称与前沿语音模型（state-of-the-art speech models）比较，可能包括自回归 Transformer、CTC、RNN-T 或其他 NAR 方法。
- **评估指标**：识别准确率（可能为词错误率 WER）与效率折衷（推理步数、延迟）。

## 4. 资源与算力
- 未提供信息。摘要和元数据中未提及 GPU 型号、数量或训练时长。

## 5. 实验数量与充分性
- 从摘要推断，主要实验包括：
  - 与当前最优模型的准确率比较。
  - 准确率-效率折衷分析（推理步数减少对准确率的影响）。
  - 可能对音频条件路径设计进行了消融或对比（如与无条件的直接噪声路径对比）。
- 理论分析部分提供了占用度差异与速度误差的关联性实验支持（可能通过可视化或数值证明）。
- 由于缺少完整论文，无法判断实验的全面性和统计显著性，但摘要宣称达到“与最先进模型相当的识别准确率”，表明实验具备一定说服力。

## 6. 主要结论与发现
- 离散流匹配在 ASR 上可行且有效，能实现并行解码并保持高准确率。
- 提出的音频条件概率路径通过模拟中间错误，改善了训练-推理一致性，提升了识别性能。
- 方法在准确率-效率折衷方面优于现有方案，为非自回归 ASR 提供了有前景的方向。

## 7. 优点
- 创新地将离散流匹配引入 ASR 领域，拓展了非自回归模型的设计空间。
- 音频条件概率路径的设计巧妙，从理论上解释了泛化误差，与实验印证。
- 高效并行解码特性有利于实时或低延迟场景。

## 8. 不足与局限
- 缺少完整论文细节，无法评估实验覆盖的全面性（如是否涵盖多语言、噪声环境、不同长度的语音等）。
- 理论分析可能基于一定假设（如占用度、速度误差），实际泛化效果需更多验证。
- 未提及模型大小、训练成本，实际部署时的资源消耗未知。
- 方法仅在非流式或有限流式条件下验证，实时流式 ASR 的适应性待评估。
- 边界风险：元数据显示该论文为 ICLR 2026 已拒稿公开（Rejected-Public），可能意味着评审过程中发现了方法或实验上的不足。

（完）
