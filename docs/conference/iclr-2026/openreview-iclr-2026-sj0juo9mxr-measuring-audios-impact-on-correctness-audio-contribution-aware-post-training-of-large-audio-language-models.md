---
title: "Measuring Audio's Impact on Correctness: Audio-Contribution-Aware Post-Training of Large Audio Language Models"
title_zh: 衡量音频对正确性的影响：大型音频语言模型的音频贡献感知后训练
authors: "Haolin He, Xingjian Du, Renhe Sun, Zheqi Dai, Yujia Xiao, Mingru Yang, Jiayi Zhou, Xiquan Li, Zhengxi Liu, Zining Liang, Chunyat Wu, Qianhua He, Tan Lee, Xie Chen, Wei-Long Zheng, Weiqiang Wang, Mark D Plumbley, Jian liu, Qiuqiang Kong"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=sJ0jUO9Mxr"
tags: ["query:speech-model"]
score: 6.0
evidence: 音频贡献感知的后训练方法优化多阶段数据分配
tldr: 现有大型音频语言模型后训练中多阶段数据分配策略尚未充分研究。本文提出音频贡献感知后训练方法，通过量化音频对模型正确性的影响来优化监督微调和强化学习阶段的数据分配。实验表明该方法能提升模型在音频任务上的表现，为高效训练音频语言模型提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有大型音频语言模型后训练中多阶段数据分配未充分优化，缺乏大规模高质量数据集。
method: 提出音频贡献感知的后训练方法，量化音频对正确性的影响以优化训练阶段数据分配。
result: 通过实验表明该方法能够提升模型在音频任务上的表现。
conclusion: 合理分配多阶段训练数据可显著提升音频语言模型的能力。
---

## Abstract
Large Audio Language Models (LALMs) represent an important frontier in multimodal AI, addressing diverse audio tasks. Recently, post-training of LALMs has received increasing attention due to significant performance improvements over foundation models. While single-stage post-training such as reinforcement learning (RL) has demonstrated promising results, multi-stage approaches such as supervised fine-tuning (SFT) followed by RL remain suboptimal. The allocation of data across multiple training stages to maximize LALM capabilities has not been fully explored, and large-scale, high-quality datasets for such research are also lacking. To address these problems, we firstly present AudioMCQ, a comprehensive audio multiple-choice question dataset comprising 571k samples with two kinds of chain-of-thought annotations. Secondly, we investigate the prevalent zero audio-contribution phenomenon in LALMs, where models derive correct answers solely from textual information without processing audio content. We propose Audio-Contribution Filtering to partition data into weak and strong audio-contribution subsets. Based on these insights, we develop two effective post-training paradigms: Weak-to-Strong (SFT on weak audio-contribution data followed by RL on strong audio-contribution data) and Mixed-to-Strong (SFT on mixed audio-contribution data followed by RL on strong audio-contribution data). We achieve first place in the DCASE 2025 Audio-Question-Answering challenge by using AudioMCQ. Additionally, leveraging our dataset with different training strategies, we achieve 78.2\% on MMAU-test-mini, 75.6\% on MMAU, 67.0\% on MMAR, and 71.7\% on MMSU, establishing new state-of-the-art performance.

---

## 论文详细总结（自动生成）

# 论文结构化总结：衡量音频对正确性的影响：大型音频语言模型的音频贡献感知后训练

## 1. 论文的核心问题与整体含义
- **研究背景**：大型音频语言模型（LALMs）是多模态人工智能的前沿方向，其后训练（post-training）阶段对性能提升至关重要。单阶段后训练（如强化学习）虽已显示潜力，但多阶段策略（监督微调 SFT 后接强化学习 RL）仍不够理想。
- **核心问题**：如何在不同后训练阶段（SFT、RL）之间合理分配数据，以最大化模型能力？现有工作缺乏对该分配策略的深入探索，也缺少大规模高质量数据集来支撑研究。
- **整体含义**：论文发现 LALMs 存在**零音频贡献现象**（模型仅凭文本信息即可答对，不依赖音频），提出 **音频贡献感知的后训练方法**，通过量化音频对答案正确性的贡献，将数据划分为弱音频贡献和强音频贡献子集，并基于此设计两种有效的多阶段后训练范式，最终在多个基准上取得最佳表现。

## 2. 论文提出的方法论
- **数据集构建**：提出 `AudioMCQ`，一个包含 **571k 样本**的综合音频多选题数据集，附带两种思维链（chain-of-thought）标注。
- **音频贡献过滤（Audio-Contribution Filtering）**：
  - 量化每个样本中音频信息对模型正确预测的贡献程度。
  - 将数据划分为**弱音频贡献**（模型可不依赖音频答对）和**强音频贡献**（必须结合音频才能答对）两个子集。
- **后训练范式**：
  - **弱到强（Weak-to-Strong）**：先用弱音频贡献数据做 SFT，再用强音频贡献数据做 RL。
  - **混合到强（Mixed-to-Strong）**：先用混合音频贡献数据做 SFT，再用强音频贡献数据做 RL。
- **技术流程**：通过音频贡献量化 → 数据划分 → 分阶段训练（SFT + RL），使模型在最终 RL 阶段专注于真正需要音频理解的任务，提升整体性能。

## 3. 实验设计
- **使用数据集**：
  - 自建的 `AudioMCQ`（571k 样本，两种思维链标注）。
  - 多个公开音频理解基准：`MMAU-test-mini`、`MMAU`、`MMAR`、`MMSU`，以及 DCASE 2025 音频问答挑战赛。
- **对比方法**：未在摘要中详细列出，但可推断对比了不同后训练策略（单阶段、无感知的多阶段基线等）。
- **评估指标**：以各基准上的正确率（Accuracy）为主要指标。

## 4. 资源与算力
- 论文摘要和元数据中**未明确提及**使用的 GPU 型号、数量、训练时长等算力细节。
- 仅能从大规模数据集（571k）和 RL 训练推断，实验需要相当的 GPU 资源，但具体配置未知。

## 5. 实验数量与充分性
- **实验组数**：至少覆盖了：
  - 两种后训练范式（Weak-to-Strong 和 Mixed-to-Strong）。
  - 四个外部基准测试集 + 一个挑战赛（DCASE 2025）。
  - 可能包含消融实验（如仅用 SFT、仅用 RL、不同数据划分等），但摘要未展开。
- **充分性评估**：基于摘要来看，实验覆盖了主流音频语言理解基准，并在挑战赛中夺冠，显示方法有效性。但因无法查阅全文，无法评估内部消融实验的完备性、重复实验次数等。
- **客观与公平性**：在公开基准上对比并达到最优，具有可比性；自建数据集 `AudioMCQ` 规模大、标注丰富，为研究提供了基础。

## 6. 论文的主要结论与发现
- LALMs 普遍存在**零音频贡献**问题，许多问题可仅凭文本常识回答，忽略真实音频理解。
- 通过**音频贡献过滤**显式划分数据，并在后训练中采用 **Weak-to-Strong 或 Mixed-to-Strong** 范式，可显著提升模型在需要深度音频理解的任务上的表现。
- 提出的方法在多个基准上取得新的最优性能（MMAU-test-mini 78.2%、MMAU 75.6%、MMAR 67.0%、MMSU 71.7%），并在 DCASE 2025 音频问答挑战赛中夺冠。

## 7. 优点
- **新颖视角**：首次系统研究 LALMs 后训练的多阶段数据分配，并提出音频贡献量化方法。
- **高质量数据**：构建了大规模、含双思维链的 `AudioMCQ` 数据集，弥补了领域数据缺口。
- **验证充分**：在多个竞争性基准和挑战赛上取得最优效果，方法实用性强。
- **训练策略清晰**：两种后训练范式逻辑简单，易于复现和扩展。

## 8. 不足与局限
- **全文不可见**：因 PDF 提取遇到验证页面，无法全面了解实验细节（如算力消耗、超参数、消融实验占比），可能导致总结不完整。
- **音频贡献定义**：零音频贡献现象的量化标准和过滤阈值是否具有鲁棒性，需审阅全文才能判断。
- **任务覆盖面**：实验集中在多选题评测，实际应用中音频理解形式更丰富（如开放式问答、声音事件描述），方法泛化性需进一步检验。
- **偏差风险**：数据集构造可能存在文本先验偏差，过滤策略若设计不当可能误删部分有效样本。
- **计算成本未透明**：未披露资源需求，不利于业界复现评估。

（完）
