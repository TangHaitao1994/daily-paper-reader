---
title: "Dual-View Predictive Diffusion: Lightweight Speech Enhancement via Spectrogram-Image Synergy"
title_zh: 双视图预测扩散：基于频谱-图像协同的轻量级语音增强
authors: "Ke Xue, Rongfei Fan, Kai Li, Shanping Yu, Puning Zhao, Jianping An"
date: 2026-04-30
pdf: "https://openreview.net/pdf/5e1f52d70aee94752604fb1b0ee89d999919c65e.pdf"
tags: ["query:speech-model"]
score: 6.0
evidence: 利用频谱图-图像协同的轻量级语音增强用于高效预处理
tldr: 针对扩散模型在语音增强中因忽略频谱结构导致计算开销大的问题，本文提出轻量级双视图预测扩散模型DVPD。该模型创新性地利用频谱图作为视觉图像和物理频率表示的双重属性，在训练和推理中采用频率自适应非均匀方法提升频谱利用效率。实验表明DVPD在显著降低计算量的同时保持了增强性能，适用于实时和移动端语音处理。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有扩散语音增强模型将频谱图当作通用图像处理，忽略音频内在结构稀疏性，导致计算效率低和频谱表示低效。
method: 提出双视图预测扩散模型DVPD，利用频谱图作为视觉纹理和物理频域表示的双重特性，通过频率自适应非均匀处理提升效率。
result: 实验显示DVPD在保持增强质量的同时大幅降低计算复杂度。
conclusion: 为资源受限场景的高效语音增强提供了新思路。
---

## Abstract
Diffusion models have recently set new benchmarks in Speech Enhancement (SE). However, most existing score-based models treat speech spectrograms merely as generic 2D images, applying uniform processing that ignores the intrinsic structural sparsity of audio, which results in inefficient spectral representation and prohibitive computational complexity. To bridge this gap, we propose **DVPD**, an extremely lightweight **D**ual-**V**iew **P**redictive **D**iffusion model, which uniquely exploits the dual nature of spectrograms as both visual textures and physical frequency-domain representations across both training and inference stages. Specifically, during training, we optimize spectral utilization via the Frequency-Adaptive Non-uniform Compression (FANC) encoder, which preserves critical low-frequency harmonics while pruning high-frequency redundancies. Simultaneously, we introduce a Lightweight Image-based Spectro-Awareness (LISA) module to capture features from a visual perspective with minimal overhead. During inference, we propose a Training-free Lossless Boost (TLB) strategy that leverages the same dual-view priors to refine generation quality without any additional fine-tuning. Extensive experiments across various benchmarks demonstrate that DVPD achieves state-of-the-art performance while requiring only **35%** of the parameters and **40%** of the inference MACs compared to SOTA lightweight model, PGUSE. These results highlight DVPD's superior ability to balance high-fidelity speech quality with extreme architectural efficiency. Code and audio samples are available at https://github.com/ke12345213/dvpd_demo

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：现有基于扩散模型的语音增强方法普遍将频谱图简单视作通用二维图像进行均匀处理，忽视了音频信号内在的结构稀疏性（尤其是低频信息密集、高频信息冗余），导致频谱表示效率低下，计算复杂度极高，难以在资源受限场景（如移动端、实时处理）中部署。
- **整体含义**：该研究旨在从频谱图的双重属性（视觉纹理与物理频域表示）出发，设计一种极其轻量化的预测扩散模型，在保持高保真增强质量的同时，大幅降低模型参数量和计算开销，为高效、实时的语音增强提供新的解决思路。

## 2. 方法论

本工作提出 **DVPD（Dual‑View Predictive Diffusion）**，其核心是将频谱图的“视觉图像”与“物理频率表示”这两种视角协同用于训练和推理全过程，关键组件包括：

- **频率自适应非均匀压缩编码器（FANC）**：在训练阶段，以非均匀方式压缩频谱输入，重点保留对人耳感知和语音清晰度至关重要的低频谐波成分，同时剪除高频域的冗余信息，从而提升频谱利用效率。
- **轻量级图像频谱感知模块（LISA）**：从纯粹的视觉纹理角度捕获空间特征，并以极小的算力开销将视觉先验注入模型，补全物理视角可能忽略的结构模式。
- **无训练无损提升策略（TLB）**：在推理阶段，充分利用训练时获得的双视图先验（频率先验与视觉先验）对生成结果进行精炼，无需任何额外微调即可提升增强质量。

总体流程可概括为：**FANC** 对输入频谱进行频域非均匀编码 → 扩散模型在双视图先验引导下生成增强后的频谱 → **TLB** 利用同一对先验在采样过程中修复细节 → 输出最终增强语音。

## 3. 实验设计

- **数据集与场景**：论文提及“跨多个基准的广泛实验”，但摘要未明确列出具体数据集名称。结合语音增强领域惯例，可能包含常规的 Voice‑Bank+DEMAND 等合成场景，以及 DNS Challenge 等真实噪声场景。
- **对比基准**：主要与当前领先的轻量级模型 **PGUSE** 进行对标，同时也应包含其他基于扩散的语音增强模型或轻量级增强方法（具体列表未在摘要中给出）。
- **评估指标**：推断会使用 PESQ、STOI、SI‑SNR 等语音质量与可懂度指标，以及参数量、MACs 等效率指标，但摘要中未详细罗列。

## 4. 资源与算力

- **摘要中未提及**所使用的 GPU 型号、数量、单次训练时长或总 GPU‑小时数。文中仅对比了推理阶段的参数规模（35%）与计算量（40%），训练阶段的资源消耗细节尚不明确，无法评估训练成本。

## 5. 实验数量与充分性

- **实验数量**：原文摘要仅用“广泛实验”概括，并未说明具体设置了哪些子实验（如不同噪声类型、不同信噪比、消融实验、跨数据集泛化实验等）。因此无法确切统计实验组数。
- **充分性与客观性**：声称与 SOTA 模型 PGUSE 对比并取得优势，且论文被 ICML‑2026 接收，通常表明实验经过同行评审，具有一定充分性。但由于摘要信息有限，无法判断是否涵盖了所有关键变量（如未见噪声、低资源语言等），也难以评价对比是否基于统一的训练/测试配置。需阅读正文后做更准确判断。

## 6. 主要结论与发现

- DVPD 仅需 PGUSE **35% 的参数量**和 **40% 的推理 MACs**，即可达到甚至超越当前最优的语音增强性能。
- 双视图协同设计是成功的关键：频率视角（FANC）保证了物理保真度，视觉视角（LISA）提供了结构完整性，两者结合在极轻架构下仍能输出高保真语音。
- 无训练提升策略（TLB）有效且无额外成本，进一步证明了从训练到推理双视图协同的可行性与优越性。

## 7. 优点

- **极致的轻量化与高效性**：参数量和计算量大幅低于现有轻量级 SOTA，适合实时和移动端部署。
- **新颖的双视图协同建模**：首次在扩散语音增强的训练和推理全阶段，同时利用频谱的物理频域属性和图像纹理属性，视角独特。
- **训练与推理解耦的优化**：TLB 无需微调即可提升质量，增强了模型在部署中的灵活性和实用性。
- **设计简洁且针对性**：FANC 仅压缩冗余高频，LISA 只引入极小视觉模块，技术路径清晰，没有引入复杂机制。

## 8. 不足与局限

- **实验细节缺失**：摘要未报告数据集、消融配置、计算资源等，无法评估方法的普适性边界（如对非平稳噪声、极度低信噪比的鲁棒性）和复现难度。
- **可能仍受扩散模型固有限制**：尽管轻量，扩散模型推理通常需要多步采样，延迟可能仍高于一些单步增强网络，实时性需在具体硬件上验证。
- **双视角设计的适用范围**：FANC 的频域压缩策略是否对所有类型语音（如音乐、情感语音）同样有效，以及 LISA 视觉特征是否可能导致纹理过拟合，尚不可知。
- **缺乏主观听力测试**：摘要仅提及客观指标，未包含 MOS 等人耳听觉评价，而语音增强的用户体验至关重要。

（完）
