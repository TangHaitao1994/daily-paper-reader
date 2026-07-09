---
title: "SepPrune: Structured Pruning for Efficient Deep Speech Separation"
title_zh: SepPrune：面向高效深度语音分离的结构化剪枝
authors: "Yuqi Li, Kai Li, Xin Yin, Zhifei Yang, Zeyu Dong, Zhengtao Yao, Haoyan Xu, Yingli Tian, Yao Lu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40455/44416"
tags: ["query:speech-model"]
score: 4.0
evidence: 结构化剪枝压缩语音分离模型以降低计算成本
tldr: 现有语音分离模型多聚焦于提升分离质量，忽略了计算效率。该论文提出SepPrune，首个针对深度语音分离模型的结构化剪枝框架。通过分析计算热点、引入可微掩膜策略进行梯度驱动通道选择，在保证分离效果的同时大幅降低计算开销。该方法可加速实时语音处理中的前端模块，间接支持语音识别等应用。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 深度语音分离模型计算开销大，不适合实时应用。
method: 分析计算瓶颈，设计可微掩膜进行梯度导向的通道剪枝并微调。
result: 在保持分离质量的前提下，显著减少模型参数量和计算量。
conclusion: 为低延迟语音分离提供了高效的压缩方案，有助实时系统部署。
---

## Abstract
Although deep learning has substantially advanced speech separation in recent years, most existing studies continue to prioritize separation quality while overlooking computational efficiency, an essential factor for low-latency speech processing in real-time applications. In this paper, we propose SepPrune, the first structured pruning framework specifically designed to compress deep speech separation models and reduce their computational cost. SepPrune begins by analyzing the computational structure of a given model to identify layers with the highest computational burden. It then introduces a differentiable masking strategy to enable gradient-driven channel selection. Based on the learned masks, SepPrune prunes redundant channels and fine-tunes the remaining parameters to recover performance. Extensive experiments demonstrate that this learnable pruning paradigm yields substantial advantages for channel pruning in speech separation models, outperforming existing methods. Notably, a model pruned with SepPrune can recover 85% of the performance of a pre-trained model (trained over hundreds of epochs) with only one epoch of fine-tuning, and achieves convergence 36x faster than training from scratch.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究背景**：近年来基于深度学习的语音分离模型（如 Conv-TasNet、DPT-Net、TF-GridNet 等）在分离质量上取得了巨大进展，但研究工作普遍以“性能优先”为导向，忽略了计算效率。
- **核心问题**：在实时语音通信、嵌入式听觉系统等低延迟、资源受限的场景中，现有模型难以满足实际部署需求。手动设计轻量化架构（如 TDANet、Tiger）虽然有效，却依赖专家知识且通用性差。
- **整体含义**：作者提出 **SepPrune**，第一个专为深度语音分离模型设计的结构化剪枝框架，旨在通过可学习的通道剪枝策略，在保持分离性能的同时大幅压缩模型，降低计算成本，弥补效率与质量之间的鸿沟。

## 2. 方法论
SepPrune 的整体流程分为三个阶段：

### 2.1 结构计算分析（Structural Computational Analysis）
- 对预训练模型（如 TDANet、A-FRCNN、SuDoRM-RF）统计各模块的参数量（Params）与浮点运算量（FLOPs），发现**分离网络（Separation Network）** 占模型总参数的 82% 以上、总 FLOPs 的 76% 以上，是计算瓶颈层。
- 据此将剪枝重点聚焦于分离网络，避免对已足够轻量的编码器/解码器造成伤害。

### 2.2 基于 Gumbel-Softmax 的可微掩码学习（Mask Learning）
- 为每一层的每个通道分配一个可学习的重要性分数 \( \alpha_i \in \mathbb{R}^{C_i} \)。
- 通过 **Gumbel-Softmax** 技巧将离散掩码连续化，生成概率分布 \( \pi_i \)（式 (5)），使梯度可以流通。
- 采用改进的 **Straight-Through Estimator** 二值化：前向传播时通过符号函数生成硬掩码 \( m_i \in \{0,1\} \)，反向传播时则将梯度裁剪至 \([-1,1]\)（式 (6)），并引入超参数 \(\epsilon\) 控制剪枝率。
- 掩码学习目标为最小化任务损失，仅更新重要性分数 \(\mathbf{A}\)，冻结预训练权重（式 (7)）。训练仅需 500 次迭代，成本极低。

### 2.3 通道剪枝与权重微调（Channel Pruning and Weight Refinement）
- 根据学到的二值掩码，保留重要通道（\(m_{i,j}=1\)），移除冗余通道。
- 对剪枝后的模型进行微调（式 (8)），以恢复因剪枝造成的性能下降。

整个框架将掩码优化与权重优化解耦，先搜索最优子结构，再高效恢复精度。

## 3. 实验设计
### 3.1 数据集与场景
- **LRS2-2Mix**（从唇语阅读数据集合成）
- **Libri2Mix**（基于 LibriSpeech 的标准混合数据集）
- **EchoSet**（更具挑战性的回声数据集）
- 全部采用 16 kHz 采样率，目标为两说话人分离。

### 3.2 基准模型与对比方法
- **模型骨干**：TDANet、A-FRCNN-12、SuDoRM-RF1.0x（均具有典型的 “编码器‑分离网络‑解码器” 结构）。
- **对比剪枝方法**：
  - Random（随机移除通道）
  - Hrank（基于特征图秩的剪枝）
  - UDSP（双层剪枝）
- 评价指标：SDRi、SI-SDRi（单位 dB），模型效率指标包括 Params、FLOPs、GPU 推理/训练时间及显存占用。

### 3.3 其他实验
- **快速收敛实验**：剪枝后仅微调 1 个 epoch，考察性能恢复率，并与从头训练相同规模模型达到可比性能所需的 epoch 数对比，计算训练加速比。
- **消融实验**：
  - 联合优化掩码与权重 vs. 分步优化
  - 掩码学习迭代次数（300–1100）
  - 剪枝率控制参数 \(\epsilon\)（0.5–0.9）
  - 以及不同剪枝方法在不同数据集与模型上的横向对比。

## 4. 资源与算力
- **训练硬件**：全部实验使用 **8×NVIDIA V100** 和 **4×NVIDIA A100** GPU。
- **预训练成本**：原始模型均按前人工作训练 500 个 epoch。
- **掩码学习成本**：固定为 500 次迭代，学习率 0.1。
- **微调成本**：剪枝后微调保持一致超参数（Adam，初始 lr=0.001）；收敛性实验中特意考察仅微调 1 个 epoch 的效果。
- **测速环境**：效率评估时使用单张 NVIDIA A100，统计 1 秒音频（16 kHz）处理 1000 次的平均时间。

## 5. 实验数量与充分性
- **核心对比实验**：3 种数据集 × 3 种模型 × 4 种方法（含原始模型），共计约 36 组主要对比结果（见表 2）。
- **效率实测**：3 种模型在剪枝前后的训练/推理时间、GPU 内存对比（表 3）。
- **快速收敛实验**：3 种模型的 1‑epoch 微调恢复率，以及从头训练的 epoch 加速比（表 4、表 5），合计约 9 组比较。
- **消融研究**：针对 A-FRCNN-12 和 TDANet，从优化方式、迭代次数、剪枝率三个维度各设 3‑5 种配置，共 10 余组实验。
- **公平性**：所有剪枝方法的 Params/FLOPs 控制在相同水平（剪枝后数值一致或非常接近），对比客观；超参数统一，训练配置与基线一致。
- 实验设计覆盖多数据集、多模型、多剪枝方法及多种压缩率，消融维度丰富，整体**实验量充分且公平**。

## 6. 主要结论与发现
- SepPrune 在所有数据集和模型上均**优于已有通道剪枝方法**；在某些设置下，剪枝后模型的 SDRi/SI-SDRi 甚至可**超越原始未剪枝模型**（如 A‑FRCNN‑12 在 LRS2‑2Mix 上）。
- 剪枝**显著降低计算开销**：训练显存最多可节省 50.2%，推理速度最高可加速 1.13×。
- 剪枝仅需 **1 个 epoch 微调即可恢复原模型 85% 以上**的性能；与从头训练相比，达到同等性能所需的训练 epoch 数可**减少 36 倍**，大幅降低训练成本。
- 分步优化（先求掩码再微调权重）明显优于联合优化，因为前者能让子结构选择更精确。

## 7. 优点
- **首次引入结构化剪枝于语音分离**，填补了该领域的空白。
- 通过**可微 Gumbel-Softmax 掩码**避免了手动规则或强化学习带来的搜索复杂度，实现梯度驱动的自动通道选择。
- **模块化设计**：先定位计算瓶颈，再专注剪枝分离网络，避免对轻量编码器/解码器的破坏。
- **快速恢复能力**：极少量微调即可接近原始性能，实际部署中可大幅缩短模型压缩与调优周期。
- 方法具有较好的**通用性**，可适配 TDANet、A‑FRCNN、SuDoRM‑RF 等多种架构。

## 8. 不足与局限
- **模型覆盖面有限**：尚未在最新的 SOTA 模型（如 Tiger、SPMamba）上验证，方法的泛化边界有待进一步探索。
- **加速效果存在瓶颈**：某些模型（如 TDANet）因内部 RNN 结构导致的串行计算依赖，剪枝后实测加速比不高；推理显存节约不明显（因固定开销占比大）。
- **低参数模型恢复困难**：SuDoRM‑RF1.0x 剪枝后仅 1 epoch 微调的恢复率不到 50%，说明当移除大量结构时，极少量微调难以快速重建性能。
- **剪切率依赖超参数**：\(\epsilon\) 等超参数需手动调整，未实现自适应剪枝率搜索。
- 论文未深入分析为何某些架构加速效果差，也未探讨剪枝对不同类型语音信号（如噪声强度、混响）的鲁棒性差异。

（完）
