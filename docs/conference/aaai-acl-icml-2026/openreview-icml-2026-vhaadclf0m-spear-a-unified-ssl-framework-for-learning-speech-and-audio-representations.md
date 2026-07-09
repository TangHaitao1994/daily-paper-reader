---
title: "SPEAR: A Unified SSL Framework for Learning Speech and Audio Representations"
title_zh: SPEAR：学习语音和音频统一表示的自监督框架
authors: "Xiaoyu Yang, Yifan Yang, Zengrui Jin, Ziyun Cui, Wen Wu, Baoxiang Li, Chao Zhang, Phil Woodland"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3ea11b440b58c278d9ab3222b426bdbff1edcfc3.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: 提出自监督框架，通过知识蒸馏统一语音和音频表示学习
tldr: 自监督学习在语音和音频理解领域各自发展，存在表示鸿沟。SPEAR框架通过知识蒸馏从语音和音频两个教师模型中学习，利用多码本向量量化生成细粒度离散令牌，联合预测掩码输入。在多种语音和音频任务上，统一模型性能接近甚至超越单任务专家，为通用声学理解打下基础。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有自监督模型大多针对语音或音频单一领域优化，存在领域差距。
method: 提出多码本向量量化和知识蒸馏结合的自监督学习，统一语音和音频表示。
result: 在语音和音频理解任务上统一模型达到与专家模型相当的性能。
conclusion: SPEAR为构建通用声学表示提供了有效方法，促进语音和音频技术的融合。
---

## Abstract
Self-supervised learning (SSL) has significantly advanced acoustic representation learning. However, most existing models are optimised for either speech or audio event understanding, resulting in a persistent gap between these two domains. We address this gap with SPEAR (SPEech and Audio Representations), a self-supervised framework that distils complementary knowledge from a speech-focused SSL teacher and a general-audio SSL teacher into a single unified model. SPEAR applies multi-codebook vector quantisation to continuous teacher representations to produce fine-grained discrete tokens that capture both semantic and acoustic information. To effectively integrate these heterogeneous representations, SPEAR jointly predicts them given a masked input with an asymmetric pre-training loss. We further improve robustness in complex sound scenes through a novel token mixing mechanism. Extensive experiments demonstrate that SPEAR consistently outperforms existing unified speech and audio models. SPEAR establishes a new state-of-the-art on the SUPERB benchmark, surpassing WavLM Large on 12 of 15 tasks, while achieving competitive performance on the HEAR benchmark. These results position SPEAR as a versatile foundation for general-purpose speech and audio representation learning. The code and pre-trained models will be released.

---

## 论文详细总结（自动生成）

# SPEAR：学习语音和音频统一表示的自监督框架

## 1. 论文的核心问题与整体含义
- **背景与动机**：自监督学习（SSL）在语音和音频理解领域各自取得了显著进展，但现有模型通常针对单一领域（纯语音或纯音频事件）进行优化，导致两个领域的表示之间存在明显的**语义与声学鸿沟**。
- **核心问题**：如何设计一个**统一的预训练框架**，能够同时从语音和通用音频两个领域学习高质量的特征表示，从而在两类下游任务上均达到甚至超越专用模型的性能。
- **整体含义**：SPEAR 旨在弥合语音与音频表示之间的差距，通过蒸馏两个专家教师模型的知识构建一个多功能的基础模型，为**通用声学理解**奠定基础。

## 2. 论文提出的方法论
- **核心思想**：通过知识蒸馏，将专注于语音的 SSL 教师模型和专注于通用音频的 SSL 教师模型的知识，蒸馏到一个单一的统一学生模型中。
- **关键技术细节**：
    - **多码本向量量化**：将连续型的教师表示通过多个码本量化为**细粒度的离散令牌**，这些令牌同时捕捉语义和声学信息，构建异构教师信号的统一预测目标。
    - **联合掩码预测**：输入语音或音频片段后，对输入进行掩码，学生模型**联合预测多个教师的离散令牌**。使用**非对称预训练损失**来有效整合这些异构的表示。
    - **令牌混合机制**：提出了一种新颖的**令牌混合**技术，用于提升模型在复杂声学场景下的鲁棒性，使多源知识能更好地融合。
- **算法流程概述**：输入音频 → 掩码处理 → 学生模型（SPEAR）编码 → 分别预测来自语音教师和音频教师的量化令牌 → 结合非对称损失进行优化。

## 3. 实验设计
- **评估基准**：
    - **SUPERB 基准**：涵盖多种语音处理任务（如语音识别、说话人识别、情绪识别等），共有 15 个下游任务。
    - **HEAR 基准**：主要面向环境音频和通用音频事件理解任务。
- **对比方法**：
    - 现有统一语音与音频的自监督模型。
    - 专用语音模型 **WavLM Large** 等。
- **主要实验结果**：
    - 在 SUPERB 基准的 15 个任务中，SPEAR 在 **12 个任务上超越了 WavLM Large**，达到最新水平。
    - 在 HEAR 基准上取得了**有竞争力的性能**。

## 4. 资源与算力
- 文中**未明确说明**所使用的 GPU 型号、数量、训练批次大小或总训练时长等算力细节。

## 5. 实验数量与充分性
- **实验规模**：文中提到进行了**广泛的实验**，覆盖 SUPERB（15 项任务）和 HEAR 两个大型公开基准，并在 SUPERB 上给出了与 WavLM Large 的具体任务数对比（12/15 项超越）。
- **充分性与公正性**：
    - 基准选择客观，覆盖多类任务和领域，与领域内强基线（如 WavLM Large）及通用统一模型对比，具有公平性。
    - 未具体给出消融实验的组数（如多码本作用、令牌混合贡献等），但摘要中提及的“广泛实验”暗示包含相应分析，细节未在提供的文本中展开。

## 6. 论文的主要结论与发现
- SPEAR 框架有效统一了语音与音频的表示学习，**单一模型在两大类任务上均表现出色**。
- 性能上，SPEAR 在语音任务上超越 WavLM Large，在音频任务上保持竞争力，证明了**跨域知识蒸馏与多码本量化**的有效性。
- 该工作为构建**通用声学表征基础模型**提供了可行的技术路线，推动了语音与音频技术的融合。

## 7. 优点
- **跨域统一设计**：首次通过双教师蒸馏与多码本量化将语音和音频表示无缝集成到一个模型中，解决了领域隔阂。
- **方法创新性**：多码本向量量化产生细粒度离散目标，非对称损失和令牌混合机制提升了异构知识整合的质量与鲁棒性。
- **性能领先**：在权威的 SUPERB 基准上取得全面领先，显示了优于强专用模型的潜力，具有实际应用价值。

## 8. 不足与局限
- **算力与开销未明**：文中未给出训练算力、模型参数量以及令牌混合带来的额外计算开销，影响复现与可行性评估。
- **对教师模型的依赖**：学生模型的性能上限受限于所选两个教师模型的质量，教师的选择与影响未深入讨论。
- **评估细节缺失**：从提供的文本看，未列出与更多基线的对比（如数据增强、其他统一模型变体），消融实验的具体贡献度不够清晰，可能影响结论的全面性。

（完）
