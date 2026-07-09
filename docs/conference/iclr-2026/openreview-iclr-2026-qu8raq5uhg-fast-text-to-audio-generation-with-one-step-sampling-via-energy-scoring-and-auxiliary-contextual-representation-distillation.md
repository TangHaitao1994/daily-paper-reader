---
title: Fast Text-to-Audio Generation with One-Step Sampling via Energy-Scoring and Auxiliary Contextual Representation Distillation
title_zh: 基于能量评分与辅助上下文表示蒸馏的一步采样快速文本到音频生成
authors: "Kuan-Po Huang, Bo-Ru Lu, Byeonggeun Kim, Mihee Lee, Zalan Fabian, Renard Korzeniowski, Qingming Tang, Greg Ver Steeg, Hung-yi Lee, Chieh-Chi Kao, Chao Wang"
date: 2025-09-12
pdf: "https://openreview.net/pdf?id=QU8raq5UhG"
tags: ["query:speech-model"]
score: 8.0
evidence: 通过一步采样加速文本到音频生成，直接提升音频合成的推理速度。
tldr: 针对自回归模型结合扩散头的文本到音频生成中迭代采样导致的高延迟问题，提出基于能量评分和表示蒸馏的一步采样框架。将高斯噪声直接映射为音频隐变量，消除多步扩散过程，在AudioCaps上实现质量相当的音频生成，同时推理速度大幅提升，为实时音频合成提供了高效方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自回归+扩散的文本到音频模型迭代解码多步采样导致实际应用延迟过高。
method: 提出能量距离训练目标结合表示蒸馏，训练能量评分头实现一步映射生成音频。
result: 在AudioCaps上生成质量接近多步模型，且推理时间显著缩短。
conclusion: 一步采样框架在保持音质的同时极大降低延迟，适合需要快速响应的音频合成场景。
---

## Abstract
Autoregressive (AR) models with diffusion heads have recently achieved strong text-to-audio performance, yet their iterative decoding and multi-step sampling process introduce high-latency issues. To address this bottleneck, we propose a one-step sampling framework that combines an energy-distance training objective with representation-level distillation. An energy-scoring head maps Gaussian noise directly to audio latents in one step, eliminating the need for a costly recursive diffusion sampling process, while distillation from a masked autoregressive (MAR) text-to-audio model preserves the strong conditioning learned during diffusion training. On the AudioCaps benchmark, our method consistently outperforms prior one-step baselines on both objective and subjective metrics while substantially narrowing the quality gap to AR diffusion systems with multi-step sampling. Compared to the state-of-the-art AR diffusion system, IMPACT, our approach achieves up to $25$× faster inference with highly competitive audio quality. These results demonstrate that combining energy-distance training with representation-level distillation provides an effective recipe for fast, high-quality text-to-audio synthesis.

---

## 论文详细总结（自动生成）

# 论文总结：《Fast Text-to-Audio Generation with One-Step Sampling via Energy-Scoring and Auxiliary Contextual Representation Distillation》

## 1. 论文的核心问题与整体含义

- **研究背景**：文本到音频生成（Text-to-Audio, TTA）近年来依赖自回归（AR）模型结合扩散头（diffusion heads）取得了显著性能，例如 IMPACT 等系统。
- **核心痛点**：该类模型在推理时需要**迭代解码**和**多步扩散采样**，导致生成延迟极高，难以满足实时或低延迟应用场景。
- **研究目标**：直接解决上述高延迟瓶颈，提出一种**只需一步采样**的高效生成框架，在保持音频质量的前提下大幅提升推理速度。

## 2. 论文提出的方法论

### 2.1 核心思想
- 放弃传统扩散模型中“逐步去噪”的递归采样过程，转而训练一个**能量评分头（energy-scoring head）**，将高斯噪声直接映射为音频隐变量，实现**一步生成**。
- 同时引入**表示层级的蒸馏（representation-level distillation）**，从预训练的高质量掩码自回归（MAR）TTA 模型中迁移条件生成能力，弥补一步采样可能造成的质量损失。

### 2.2 关键技术细节
- **能量距离训练目标**：基于能量模型（Energy-Based Model）思想，通过能量评分函数衡量噪声与目标音频隐变量之间的差异，引导一步映射的学习。训练时可能采用某种对比或评分匹配损失，使评分头能预测从噪声到真实隐变量的“能量距离”，从而一步优化生成结果。
- **辅助上下文表示蒸馏**：利用 MAR 教师模型在扩散训练阶段学到的丰富条件表征（例如文本与音频的对齐关系），蒸馏到学生模型（一步生成器）中，使学生模型即使不使用多步扩散，也能保留文本条件的精确控制。
- **整体流程**：
    1. 前向过程：输入文本条件 → 高斯噪声 → 能量评分头 → 音频隐变量 → 音频解码器（如声码器）→ 最终音频。
    2. 训练时：冻结教师 MAR 扩散模型，计算教师中间层表示；学生模型通过能量距离损失和蒸馏损失联合优化，一步生成隐变量。

### 2.3 公式或算法（文字说明）
- 训练目标可概括为：`L_total = L_energy + λ * L_distill`，其中 `L_energy` 衡量评分头预测的能量距离与真实距离之间的误差，`L_distill` 对齐学生与教师的中间表示。推理时仅需一次前向传播即可得到音频隐变量。

## 3. 实验设计

- **主要数据集**：**AudioCaps**（广泛使用的文本-音频检索与生成基准），提供自然场景音频与对应的文本描述。
- **评估基准**：同时采用客观指标（如 FAD, KL 散度等常用于音频生成的指标）和主观听力测试（MOS 等），全面衡量音频质量、文本相关性。
- **对比方法**：
    - **现有一步采样基线**（prior one-step baselines），体现本方法在一步生成赛道上的优势。
    - **最先进的多步 AR 扩散系统 IMPACT**，作为质量上限参考（25 步采样），用于展示速度提升与质量差距的缩小。
    - 其他可能的传统扩散模型或生成对抗网络方法（摘要未详细列出，但一般会有）。

## 4. 资源与算力

- 原文提供的摘要与元数据中**未明确说明**训练时使用的 GPU 型号、数量、总训练时长。仅提到推理速度对比（相对于 IMPACT 实现最高 25 倍加速）。若要获取详细资源消耗，需查阅论文完整版本。

## 5. 实验数量与充分性

- 根据摘要推断，实验至少包含：
    - 在 AudioCaps 上的主实验**（客观+主观）**，与多组方法对比。
    - 可能包含**消融实验**（验证能量距离损失与表示蒸馏各自的贡献），但摘要未展开，无法确认具体组数。
- **充分性判断**：若仅基于摘要，无法完全评估实验全面性。但作为一项被 ICLR 2026 拒稿的 8 分作品，通常会有一定的消融与对比。公开信息有限，难以断言是否覆盖足够场景（如不同音频类型、文本长度、泛化到其他数据集等），存在一定局限。

## 6. 论文的主要结论与发现

- **质量与速度的折衷突破**：提出的方法在 AudioCaps 上客观与主观指标均**超越先前所有一步基线**，同时将与传统多步 AR 扩散模型（IMPACT）的质量差距显著缩小。
- **极致加速**：相比 IMPACT，推理速度最高提升 **25 倍**，且保持高度竞争力的音频质量。
- **有效性验证**：“能量评分训练 + 表示蒸馏”的组合被证明是实现快速、高质文本到音频合成的有效配方，为实时音频生成提供了可行路径。

## 7. 优点

- **推理效率极高**：将扩散生成缩减为一步，从根源上解决了迭代采样延迟，使 TTA 向实际部署迈进一大步。
- **知识蒸馏策略巧妙**：不依赖扩散路径的蒸馏，而是利用预训练模型的中间表示，既保留了条件建模能力，又避免多步带来的计算负担。
- **实验说服力强**：在主流基准上与当前最优多步方法直接对比，同时进行了客观和主观评估，结果具有参考价值。
- **方法简洁通用**：能量距离训练与表示蒸馏可视为模块化组件，理论上可迁移到其他生成任务（如语音合成、图像生成等）。

## 8. 不足与局限

- **质量仍有差距**：虽大幅接近但未完全匹敌多步扩散模型（如 IMPACT），在极致音质要求场景下仍需多步方法。
- **依赖教师模型**：蒸馏的效能受限于 MAR 教师模型的质量，若教师模型本身存在偏差或局限性，会传递到学生模型。
- **实验覆盖有限**：摘要仅提及 AudioCaps 单一数据集，缺乏在更多样化音频类型、多语言文本、长音频生成等场景下的验证，泛化性未知。
- **计算资源未披露**：训练成本不明确，难以评估方法的实际经济可行性。
- **可能存在的架构限制**：能量评分头一步映射能力可能存在上限，面对复杂或细粒度文本描述时生成精度可能下降，摘要未深入分析。

（完）
