---
title: "MGM-Omni: Scaling Omni LLMs to Personalized Long-Horizon Speech"
title_zh: MGM-Omni：面向个性化长时语音的规模化全模态大语言模型
authors: "Chengyao Wang, Zhisheng Zhong, Bohao PENG, Senqiao Yang, Yuqi Liu, Haokun GUI, Bin Xia, Jingyao Li, Bei Yu, Jiaya Jia"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=Cry2mpEocV"
tags: ["query:speech-model"]
score: 8.0
evidence: 基于块的并行解码加速语音合成推理，支持流式声音克隆
tldr: 传统语音合成级联系统实时性差，MGM-Omni采用脑嘴分离双轨架构，通过分块并行解码缩小文本与语音令牌速率差距，加速推理并实现流式零样本声音克隆，同时支持全模态理解与长时语音生成，为高效自然交互提供新方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 统一全模态模型面临推理延迟和实时生成瓶颈。
method: 采用双轨令牌架构和分块并行解码，解耦推理与生成。
result: 实现低延迟流式语音生成和高质量零样本声音克隆。
conclusion: MGM-Omni在效率和个性化方面显著提升，推动实时语音交互应用。
---

## Abstract
We present MGM-Omni, a unified Omni LLM for omni-modal understanding and expressive, long-horizon speech generation. Unlike cascaded pipelines that isolate speech synthesis, MGM-Omni adopts a “brain–mouth” design with a dual‑track, token-based architecture that cleanly decouples multimodal reasoning from real-time speech generation. This design enables efficient cross-modal interaction and low-latency, streaming speech generation. For understanding, a unified training strategy coupled with a dual audio encoder design enables long-form audio perception across diverse acoustic conditions. For generation, a chunk-based parallel decoding scheme narrows the text–speech token-rate gap, accelerating inference and supporting streaming zero-shot voice cloning with stable timbre over extended durations. Compared to concurrent work, MGM-Omni achieves these capabilities with markedly data-efficient training. Extensive experiments demonstrate that MGM-Omni outperforms existing open source models in preserving timbre identity across extended sequences, producing natural and context-aware speech, and achieving superior long-form audio and omnimodal understanding. MGM-Omni establishes an efficient, end-to-end paradigm for omnimodal understanding and controllable, personalized long-horizon speech generation.

---

## 论文详细总结（自动生成）

# MGM-Omni：面向个性化长时语音的规模化全模态大语言模型 详细总结

## 1. 核心问题与整体含义
- **研究动机与背景**：现有多模态大模型在处理语音时普遍采用级联（cascade）架构，即将语音合成作为一个独立后处理步骤，导致以下问题：
  - 实时性差：文本到语音的合成速度远慢于文本生成，造成推理延迟，无法支持流式交互。
  - 声线一致性难保持：长语音生成过程中说话人音色易漂移。
  - 模态割裂：理解（听）与生成（说）分离，难以实现端到端、高交互性的全模态智能。
- **整体含义**：该工作旨在构建一个统一的、能同时进行全模态理解与长时、个性化语音生成的模型，从架构上解耦多模态推理与实时语音生成，从而在不牺牲表达能力和实时性的前提下，实现高效、可控、自然的语音交互。

## 2. 方法论
- **核心思想**：采用“大脑-嘴巴”双轨架构（brain-mouth design），将模型分为两个逻辑角色：
  - **大脑**：负责多模态理解和推理，产生控制信号（文本/语义令牌）。
  - **嘴巴**：专责实时语音生成，以令牌形式接收大脑输出并合成语音。
- **关键技术细节**：
  1. **双轨令牌架构**：解耦理解（推理）与生成（合成），两条轨道以不同频率与机制运行，但共享底层表示空间。
  2. **统一训练策略 + 双音频编码器**：针对长音频输入，在不同声学环境下增强感知能力。
  3. **分块并行解码（Chunk-based parallel decoding）**：在生成阶段，将语音令牌划分为块，并行预测，从而缩小文本令牌与语音令牌的速率差距，实现：
     - 推理加速
     - 流式输出
     - 支持零样本声音克隆，长时间保持音色稳定。
  4. **数据高效的训练**：相比同期工作，用更少的数据达到同等或更强性能。

## 3. 实验设计
- **使用的数据集**：文中摘要未列明具体数据集名称，但提及涉及长音频理解、全模态理解及声音克隆等任务，可能包含公有长语音数据集、多模态问答数据以及说话人验证数据集。完整信息需查阅正文。
- **Benchmark 与对比方法**：
  - 与现有的开源模型进行对比，重点比较：
    - 长时声音克隆中的音色保持度（identity preservation）
    - 自然度与上下文感知能力
    - 长音频和全模态理解任务的性能
  - 声称性能优于已有开源模型。

## 4. 资源与算力
- 摘要未提及具体 GPU 型号、数量、训练时长。全文若包含相关信息，如“数据高效训练”暗示可能使用了中等规模算力，但此处无法确认。

## 5. 实验数量与充分性
- **实验数量与类型**：根据摘要推断，至少包含：
  - 理解任务评估：长音频理解、全模态理解
  - 生成任务评估：零样本声音克隆（评价音色一致性与自然度）、流式生成质量
  - 消融实验：可能围绕双轨架构、分块并行解码、双编码器设计等模块展开（具体需查原文）。
- **充分性与客观性**：摘要声称“大量实验”且结果优于开源模型，表明覆盖了主要能力维度。但缺乏具体指标和误差线，需阅读全文方可评判公平性与复现细节。

## 6. 主要结论与发现
- MGM-Omni 通过双轨解耦和分块并行解码实现了：
  - 低延迟、流式的个性化语音生成；
  - 长时间内稳定的说话人音色保持；
  - 优异的全模态理解能力。
- 该模型在理解与生成的多个基准上超越主流开源模型，且训练数据需求更少，证明了其效率和高效性。
- 确立了一种端到端、可控、个性化的长时语音生成新范式。

## 7. 优点（亮点）
- **架构解耦**：“大脑-嘴巴”双轨设计清晰分离推理和生成，解决了级联系统的实时性瓶颈。
- **加速与克隆**：分块并行解码显著缩小令牌速率差距，支持流式零样本声音克隆，且长期音色稳定。
- **数据效率**：用更少的数据即可达到 SOTA 性能，降低了训练成本，提高了实用性。
- **全模态统一**：单一模型覆盖理解与生成，避免多模型拼接的复杂度。

## 8. 不足与局限
- **信息不完整**：受限于摘要，缺少具体数据集、指标、消融细节以及算力消耗，难以全面评估实验公正性和可复现性。
- **潜在局限**：
  - 未见用户主观评测数据（MOS 等），自然度仅凭客观指标可能不充分。
  - 声音克隆的泛化边界不明（跨语言、带噪声、情绪表达等）。
  - 长时生成是否真正保证内容-语音同步（如断句、韵律）未详述。
  - 仅对比开源模型，与闭源商业系统的差距未知。
  - 实时性指标（如首帧延迟、吞吐率）未在摘要中给出。

（完）
