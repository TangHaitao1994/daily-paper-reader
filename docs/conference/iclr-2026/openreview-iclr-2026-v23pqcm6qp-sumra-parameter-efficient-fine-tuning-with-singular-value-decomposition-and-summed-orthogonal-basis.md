---
title: "SumRA: Parameter Efficient Fine-tuning with Singular Value Decomposition and Summed Orthogonal Basis"
title_zh: SumRA：基于奇异值分解和求和正交基的参数高效微调
authors: "Chin Yuen Kwok, Yongsen Zheng, Jia Qi Yip, Kwok-Yan Lam, Eng Siong Chng"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=v23Pqcm6qp"
tags: ["query:speech-model"]
score: 9.0
evidence: 参数高效微调大型语音模型，利用SVD和正交基
tldr: 参数高效微调旨在减少大型语音模型适配的可训练参数。现有LoRA方法固定A矩阵限制了表征能力。本文提出SumRA，利用SVD初始化并学习多个正交基的线性组合，扩展低秩矩阵的表征空间。实验证明在相当性能下可进一步减少参数，为语音模型的高效迁移提供新方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有低秩适应方法中固定A矩阵导致表征容量受限。
method: 提出SumRA，使用SVD初始化并学习多个正交基的线性组合，扩展低秩矩阵的表示空间。
result: 相比LoRA，在保持性能的同时进一步减少了可训练参数。
conclusion: SumRA为大型语音模型的高效微调提供了更优方案。
---

## Abstract
Parameter-efficient fine-tuning (PEFT) aims to adapt large pretrained speech models using fewer trainable parameters while maintaining performance. Low-Rank Adaptation (LoRA) achieves this by decomposing weight updates into two low-rank matrices, $A$ and $B$, such that $W'=W_0+BA$. Previous studies showed that freezing $A$ and only updating $B$ can reduce trainable parameters and achieve performance close to standard LoRA, where $A$ is initialized using the principal singular vectors of $W_0$ obtained via singular value decomposition (SVD). However, because $A$ is typically initialized with only the leading singular vectors, its representation capacity is confined to a narrow subspace of the model’s knowledge. To overcome this limitation, we propose SumRA, which initializes each row of $A$ as a sum of multiple singular vectors chosen from beyond the leading components, thereby enabling $A$ to influence a larger portion of the model’s knowledge space. Experiments on multilingual automatic speech recognition (ASR) tasks show that by adapting Whisper to five new languages from Common Voice with only 10 hours of data each, our method improves word error rate from 14.42\% to 12.41\% over LoRA baselines while using 50\% less trainable parameters.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**  
  大型预训练语音模型（如 Whisper）在下游任务适配时，全参数微调成本极高。参数高效微调（PEFT）旨在大幅减少可训练参数的同时保持性能。

- **核心问题**  
  现有低秩适应（LoRA）方法将权重更新分解为两个低秩矩阵 \(A\) 和 \(B\)，其中固定 \(A\)、仅更新 \(B\) 可进一步减参数，但 \(A\) 通常仅由权重矩阵的前几个主奇异向量初始化，导致其表征能力被困在模型知识的狭窄子空间内，限制了适配效果。

- **整体含义**  
  寻找一种既能保持参数效率，又能扩展低秩矩阵表征空间的微调方法，使大型语音模型在多语言自动语音识别（ASR）等任务上以更少参数取得更好性能。

## 2. 论文提出的方法论

- **核心思想**  
  突破固定 \(A\) 仅使用主奇异向量的限制，通过让 \(A\) 的每一行由多个选自主成分之外的不同奇异向量线性组合而成，从而扩大其对模型知识空间的影响范围。

- **关键技术细节**  
  - **初始化改进**：对预训练权重矩阵 \(W_0\) 进行奇异值分解（SVD，即 Singular Value Decomposition），但不再仅取最大的若干个奇异向量，而是选取超出主要成分的多个奇异向量。  
  - **求和正交基（Summed Orthogonal Basis）**：\(A\) 的每一行被定义为所选奇异向量的加权求和（线性组合），这些奇异向量相互正交，构成扩展的表征基底。  
  - **训练过程**：\(A\) 初始化后固定，仅训练 \(B\) 矩阵（或可能涉及组合权重的学习？摘要未详述，但强调 \(A\) 通过这种方式获得更强表达能力）。  
  - **公式概述**：  
    \[
    W' = W_0 + BA
    \]  
    其中 \(A\) 由 \(k\) 个奇异向量的线性组合构成，而非常规 LoRA 中仅由前 \(r\) 个奇异向量直接组成。

- **算法流程**  
  1. 对预训练权重进行 SVD。  
  2. 选择超出主成分的多个奇异向量作为基函数。  
  3. 将 \(A\) 初始化为这些基向量的特定线性组合（每行为一个组合）。  
  4. 冻结 \(A\)，仅训练 \(B\)（可能包括组合系数，视实现而定，但摘要强调“learning summed orthogonal basis”，暗示组合权重也是可学习的）。  
  5. 将原始权重与低秩更新相加得到适配后的权重。

## 3. 实验设计

- **数据集与场景**  
  - 使用 Common Voice 多语言数据集。  
  - 任务：将 Whisper 模型适配到 5 种新语言的多语言自动语音识别（ASR）。  
  - 每种语言仅使用 10 小时数据，属于低资源适配场景。

- **基准方法（Benchmark）**  
  - 标准 LoRA（同时更新 \(A\) 和 \(B\)）。  
  - 固定 \(A\)（仅用主奇异向量初始化）并仅更新 \(B\) 的 LoRA 变体。

- **对比方法**  
  - 所提方法 SumRA（固定 \(A\)，但 \(A\) 由多个奇异向量线性组合得到）。

- **评价指标**  
  词错误率（Word Error Rate, WER）。

## 4. 资源与算力

- 提供的摘要与元数据中**未明确说明** GPU 型号、数量、训练时长等算力信息。原文可能包含在完整论文中，但基于现有内容无法总结。

## 5. 实验数量与充分性

- **实验组数**  
  摘要仅报告了在 5 种语言、10 小时/语言的 Common Voice 上的 ASR 结果对比，并以词错误率作为唯一指标。可能还有消融实验（如不同奇异向量数量、组合方式等），但摘要未提及。

- **充分性与公平性**  
  - 对比了标准 LoRA 及固定 \(A\) 的 LoRA 基线，方法比较直接且公平。  
  - 以参数量和词错误率双维度评估，验证了“参数减少 50% 同时性能提升”的核心主张，但仅提供单一任务、单一模型（Whisper）的结果，实验范围较窄。  
  - 缺乏多模型、多任务、统计分析（如标准差）等佐证，目前的证据充分性有限。

## 6. 论文的主要结论与发现

- **参数效率与性能兼得**：SumRA 在仅使用 LoRA 基准 50% 可训练参数的情况下，将多语言 ASR 的词错误率从 14.42% 降至 12.41%，显著优于固定 \(A\) 的标准做法。  
- **表征空间扩展有效**：通过让 \(A\) 影响模型知识的更大范围（而非仅限于主成分子空间），能够以更少参数达到更好的微调效果，验证了求和正交基初始化策略的有效性。  
- **低资源适配潜力**：在每语言仅 10 小时数据的低资源场景下展现出强大适应性，为大型语音模型的高效迁移提供了新路径。

## 7. 优点

- **方法创新性**：提出将多个非主奇异向量求和形成正交基来构建冻结矩阵 \(A\)，这个想法简单而新颖，有效扩展了低秩适配的表示容量。  
- **参数效率突出**：在减少一半可训练参数的前提下仍实现性能提升，对边缘部署、多任务学习等场景十分有利。  
- **与现有框架兼容**：基于 LoRA 范式，可自然集成到各类 Transformer 架构中，无需修改模型结构。  
- **聚焦低资源**：针对每语言仅 10 小时数据的极端低资源环境验证有效性，实用价值高。

## 8. 不足与局限

- **实验规模有限**：仅在一个模型（Whisper）、一个数据集（Common Voice）的 5 种语言上测试，缺乏其他语音模型（如 HuBERT、WavLM）和其他任务（如语音翻译、说话人验证）的验证。  
- **缺乏消融分析**：摘要未显示对奇异向量数量、组合方式、不同冻结/训练策略的消融实验，难以确定方法的关键超参数影响。  
- **计算开销未评估**：虽然可训练参数减少，但 SVD 分解和组合系数的学习可能带来额外的计算或内存负担，文中无相关讨论。  
- **理论解释不足**：为何选择非主奇异向量、如何选取最有益的子集等，缺少深层理论支撑或直观解释。  
- **泛化性风险**：仅在低资源 ASR 场景下得到验证，其在资源充足或多任务设置下的表现未知，可能存在过拟合或性能增益消失的情况。

（完）
