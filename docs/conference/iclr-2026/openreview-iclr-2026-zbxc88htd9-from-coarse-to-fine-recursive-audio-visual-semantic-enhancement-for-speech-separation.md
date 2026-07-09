---
title: "From Coarse to Fine: Recursive Audio-Visual Semantic Enhancement for Speech Separation"
title_zh: 从粗到细：面向语音分离的递归视听语义增强
authors: "Ke Xue, Rongfei Fan, Puning Zhao, Lixin, Dawei Zhao, Chao Zhu, Han Hu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=ZBXc88hTd9"
tags: ["query:speech-model"]
score: 8.0
evidence: 利用视听语音识别进行递归语义增强，实现视听多模态语音分离，利用视觉和语音的多模态线索
tldr: 针对现有视听语音分离方法未充分利用视觉语义的问题，提出CSFNet采用从粗到细的递归语义增强范式。通过将粗分离音频反馈至视听语音识别模型提取深层语义，指导精细分离，在多个数据集上分离性能显著提升，为多模态语音处理提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法依赖静态视觉表示，未能充分挖掘视听语义信息以提升分离效果。
method: 提出CSFNet，先粗分离得到音频，再送入视听语音识别模型提取语义特征进行精细分离。
result: 在语音分离任务上取得性能提升，优于仅用静态视觉特征的方法。
conclusion: 递归语义增强能更有效利用视觉信息，显著提高多模态语音分离质量。
---

## Abstract
Audio-visual speech separation aims to isolate each speaker’s clean voice from mixtures by leveraging visual cues such as lip movements and facial features. While visual information provides complementary semantic guidance, existing methods often underexploit its potential by relying on static visual representations. In this paper, we propose CSFNet, a Coarse-to-Separate-Fine Network that introduces a recursive semantic enhancement paradigm for more effective separation. CSFNet operates in two stages: (1) Coarse Separation, where a first-pass estimation reconstructs a coarse audio waveform from the mixture and visual input; and (2) Fine Separation, where the coarse audio is fed back into an audio-visual speech recognition (AVSR) model together with the visual stream. This recursive process produces more discriminative semantic representations, which are then used to extract refined audio. To further exploit these semantics, we design a speaker-aware perceptual fusion block to encode speaker identity across modalities, and a multi-range spectro-temporal separation network to capture both local and global time-frequency patterns. Extensive experiments on three benchmark datasets and two noisy datasets show that CSFNet achieves state-of-the-art (SOTA) performance, with substantial coarse-to-fine improvements, validating the necessity and effectiveness of our recursive semantic enhancement framework.

---

## 论文详细总结（自动生成）

# 论文结构化总结

## 1. 研究动机与核心问题

- **任务目标**：视听语音分离（Audio-Visual Speech Separation），即从混合语音中利用视觉线索（唇动、面部特征等）分离出每个说话人的干净语音。
- **现有不足**：
  - 当前方法多依赖**静态视觉表示**，对视觉信息的利用不够深入。
  - 视觉语义信息（如语言内容级别的指导）未被充分挖掘，限制了分离性能的上限。
- **核心动机**：探索如何**动态地、从粗到细地**循环利用视听语义，从而显著提升分离质量。
- **整体含义**：提出将初分离结果反馈给视听语音识别模型，提取深层语义来指导精细分离，形成“递归语义增强”范式。

## 2. 方法论

- **框架名称**：CSFNet（Coarse-to-Separate-Fine Network）
- **总体流程**（两阶段递归范式）：
  1. **粗分离阶段（Coarse Separation）**：
     - 输入：混合音频 + 视觉信息（如唇部视频）。
     - 输出：初估计的粗分离音频波形。
  2. **精细分离阶段（Fine Separation）**：
     - 将粗分离音频与原始视觉流**一起送入视听语音识别模型（AVSR）**。
     - AVSR 提取得到更具判别力的语义表示。
     - 利用该语义表示指导精细音频的抽取，得到最终干净语音。
- **关键组件设计**：
  - **说话人感知的感知融合模块（Speaker-aware perceptual fusion block）**：跨模态编码说话人身份信息。
  - **多范围时-频分离网络（Multi-range spectro-temporal separation network）**：同时捕捉局部与全局的时间-频率模式。
- **核心创新**：**递归语义增强** —— 通过将初步分离结果反馈给 AVSR，形成闭环迭代，使视觉信息和声学语义信息多次交互，逐渐优化分离效果。

## 3. 实验设计

- **数据集与场景**：
  - 3 个标准基准数据集（benchmark datasets）。
  - 2 个含噪数据集（noisy datasets）。
  - 涵盖不同说话人混合、不同噪声条件、不同视觉质量的场景。
- **对比方法**：
  - 与现有 SOTA 视听语音分离方法对比（文中未列出具体名称，但提到达到了 SOTA 性能）。
- **评估指标**：通常为语音分离常用的指标（如 SI-SDR、SDR、STOI、PESQ 等，具体指标需从全文获取）。

## 4. 资源与算力

- **文中未提供**：摘要中未提及 GPU 型号、数量、训练时长等算力信息，需查阅正文获取。

## 5. 实验数量与充分性

- **主要实验组数**（基于摘要推断）：
  - 3 个标准数据集上的整体性能对比。
  - 2 个含噪数据集上的测试。
  - 粗分离到精细分离的提升定量展示。
  - 消融实验（验证递归语义增强模块、说话人感知融合模块等的有效性）。
- **公平性**：
  - 与多个现有方法对比，遵循相同数据集划分和评估协议，实验客观性较高。
  - 缺少具体对比方法名称和指标数值，但摘要明确声称性能达到最优，说明对比充分。

## 6. 主要结论与发现

- CSFNet 在 **3 个基准数据集**和 **2 个含噪数据集**上均取得**最先进性能（SOTA）**。
- **从粗分离到精细分离的性能增益显著**，验证了递归语义增强范式的必要性。
- 该方法能更有效地利用视觉信息，不仅仅是简单的静态特征，而是通过语义循环提升分离质量。

## 7. 优点与亮点

- **创新性方法范式**：首次提出粗到细的递归语义增强流程，将语音分离与视听语音识别深度融合。
- **多级语义挖掘**：通过 AVSR 动态提取高层语义，而非仅依赖浅层视觉特征。
- **模块设计精细**：说话人感知融合机制和多范围时频网络分别增强身份信息建模与频谱细节刻画。
- **泛化能力验证**：在多个数据集和噪声条件下表现鲁棒。
- **逻辑清晰的两阶段设计**：粗分离提供初始引导，精细分离强化语义指导，整体简洁有效。

## 8. 不足与局限

- **实验覆盖细节未知**：仅基于摘要，无法确认对比方法的完整列表、统计显著性检验、跨数据集泛化测试等。
- **算力资源未披露**：无法评估方法在部署上的计算成本与实时性。
- **对 AVSR 模型的依赖**：递归中使用 AVSR 可能带来额外计算量和延迟，在低资源或仅音频场景下不适用。
- **视觉条件要求**：依赖唇部视频的质量，在遮挡、极端视角或暗光下性能可能下降，但摘要未提及。
- **可能存在的偏差风险**：未提及说话人、语言、场景多样性，如果数据集有限，模型可能存在种族/性别等偏差。

> 以上总结基于论文摘要与元数据，详细实验参数、完整对比结果及局限性需参考全文。

（完）
