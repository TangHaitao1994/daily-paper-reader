---
title: "MelTrim: Coarse-to-Fine Data Pruning for Speech Classification"
title_zh: MelTrim：语音分类的粗到细数据剪枝
authors: "Shaobo Wang, Tianle Niu, Xuan Ouyang, Xintong Li, Zhengkun Ge, Yue Min, Xiaoqian Liu, Hankun Wang, Linfeng Zhang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.672.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 基于MFCC特征的两步语音分类数据集剪枝方法
tldr: 语音分类任务中数据集剪枝研究不足，传统方法难以捕获声学语义表示。本文提出MelTrim，通过粗到细两步框架：先用DBSCAN对MFCC特征聚类粗滤冗余样本，再进一步精细筛选。实验表明，该方法构建的核心集在多个语音分类任务上性能接近全量数据，为高效语音模型训练提供了数据预处理方案。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.672/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 798, \"height\": 456, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.672/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1625, \"height\": 525, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.672/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 800, \"height\": 917, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1650, \"height\": 1698, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 797, \"height\": 334, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 796, \"height\": 191, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1656, \"height\": 665, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1657, \"height\": 554, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 814, \"height\": 497, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 802, \"height\": 369, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 800, \"height\": 740, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 799, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 797, \"height\": 244, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 796, \"height\": 211, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.672/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 797, \"height\": 206, \"label\": \"Table\"}]"
motivation: 语音分类中数据集剪枝研究匮乏，需有效捕获声学语义表示进行样本筛选。
method: 提出基于MFCC的DBSCAN聚类粗滤和精细筛选的两步数据剪枝方法。
result: 在多个语音分类基准上，用少量核心集达到近似全量数据的性能。
conclusion: MelTrim为语音数据处理提供了高效剪枝技术，有助于减少模型训练成本。
---

## Abstract
Dataset Pruning (DP) aims to construct a coreset that achieves performance comparable to the original, full dataset. However, few studies have explored DP in the context of Speech Classification (SC) tasks. Unlike image or text classification, SC is particularly challenging due to the difficulty in capturing the acoustic, semantic, and contextual representations. In this study, we propose a novel dataset pruning method for speech datasets, termed Meltrim, which uses a two-step coarse-to-fine framework designed to address these challenges. Specifically, in Step 1, Meltrim coarsely filters utterance-level redundant samples using DBSCAN clustering on Mel-Frequency Cepstral Coefficients (MFCC) features, which are first flattened and then reduced in dimensionality using UMAP. In Step 2, we perform frame-level redundancy pruning for each utterance via utility pruning, which aims to eliminate irrelevant frames within each utterance. To the best of our knowledge, this is the first dataset pruning approach designed for Speech Classification tasks, demonstrating outstanding performance compared to classical general DP methods. Notably, for the Speech Emotion Recognition, our method achieves up to a 49.5% improvement in WA (Weighted Accuracy) on the MEAD dataset. For the Speaker Identification tasks, it results in a 41.9% reduction in EER (Equal Error Rate) on the VoxCeleb1 dataset.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：语音数据存在显著的**样本间冗余**（不同语句在声学‑语义模式上高度重叠）和**样本内冗余**（同一语句的相邻帧包含大量重复信息），导致全量数据训练成本高昂且数据利用率低下。
- **研究背景**：通用数据剪枝方法多面向图像、文本，难以直接适配语音分类任务，因为语音分类需要同时捕获声学、语义和上下文表征。
- **整体含义**：本文提出 **MelTrim**，旨在通过从粗到细的两阶段剪枝，在语音分类任务中构建既能大幅压缩数据量、又几乎无损模型性能的核心集。

---

### 2. 方法论：核心思想与关键技术
#### 总体框架：粗到细两步剪枝
- **第一步：语句级粗过滤**
  - 提取每句语音的 **MFCC 特征**（20 维 × 帧数）。
  - 统一补零/截断后**展开**为固定长度向量。
  - 使用 **UMAP** 将高维特征降至 2 维空间以保留局部流形结构。
  - 采用 **DBSCAN** 进行密度聚类，剔除噪声点，再按簇大小比例从每个簇中选取**离质心最近**的若干代表样本。
  - 目的是**保留声学多样性**，避免后续任务偏差导致的选择坍塌。

- **第二步：帧级精细剪枝**
  - 将粗选出的语句切分为固定长度的交叉段（段长 = 原句长的 1/4，起始点随机采样）。
  - 预训练一个**与任务对齐、架构轻量的 Judge 模型**（如 Whisper‑tiny 对情感识别，RawNet3 对说话人识别），在全量数据上训练至收敛后冻结。
  - 对每段输入 Judge 模型，计算**梯度范数**作为**效用分数**（Utility），理论证明该效用可被梯度范数界定。
  - 保留每条语句中效用分数**最高的一个段**，再按分数对所有候选段排序，**取前 m 条**作为最终剪枝后的数据集。

#### 关键公式/概念
- **效用函数** \(U(x_i, y_i; f_{\theta^{(t)}})\) 衡量除去某个数据点对梯度流的最大影响，可被梯度范数 \( \|\nabla_{\theta^{(t)}} L_t(f_{\theta^{(t)}}(x_i), y_i)\| \) 上界约束。
- 实际计算时直接使用该梯度范数近似效用进行排序与选择。

---

### 3. 实验设计
#### 数据集与任务
- **情感识别**：M3ED、MEAD、MELD（均取自 EmoBox，四分类：中性、生气、开心、伤心）。
- **说话人识别**：VoxCeleb1（选取数据量最大的 10 个说话人）。
- **自动语音识别（ASR）**：在 MELD 上进行小规模验证（附录 C，以 WER 为指标）。

#### 评价指标
- 情感识别：**Weighted Accuracy (WA)**、**Unweighted Accuracy (UA)**、**F1-score**。
- 说话人识别：**Equal Error Rate (EER)**（越低越好）。
- ASR：**Word Error Rate (WER)**（越低越好）。

#### 核心集规模
- 每类选取 1、5、10、50 个样本，对应不同极端低数据比例。

#### 对比方法（11 种基线）
- 多样性类：Random、Herding、K‑Center、Contextual Diversity (CD)
- 不确定性类：Least Confidence (LC)、Entropy、Margin
- 影响力/优化类：Craig、Cal、GraND、Forgetting

---

### 4. 资源与算力
- **硬件**：所有实验在 **单张 NVIDIA H20（NVLink）GPU** 上完成。
- **时间统计**（见表 8）：
  - 剪枝耗时包括：MFCC 提取、UMAP+DBSCAN 聚类、Judge 模型预训练、梯度范数评分。
  - 端到端时间（剪枝 + 核心集训练）明显低于全量数据训练。例如在 MELD 上，从 3360 秒降至约 1250‑1314 秒（降幅 60‑63%）。
- **其他配置**：Judge 模型（如 Whisper‑tiny）仅训练少数 epoch（情感识别任务为 7 个 epoch）后冻结，评分阶段的计算开销相对可控。

---

### 5. 实验数量与充分性
- **主实验**：3 个情感数据集 × 4 种核心集比例 + 1 个说话人数据集 × 4 种比例，共约 **16 组主实验**，每组均 10 次重复，报告均值和方差。
- **消融实验**：
  - 粗过滤特征对比：MFCC vs. Whisper 深层特征；
  - Judge 模型对比：ASR 预训练的 Whisper‑tiny vs. 自监督 Wav2Vec2；
  - 评分策略对比：效用 vs. 交叉熵损失；
  - 降维方法：UMAP、PCA、t‑SNE；
  - 聚类方法：DBSCAN、KNN、K‑Means；
  - 切分段数（4/5/6）与段长比例（1/3、1/4、1/5）；
  - 跨架构泛化：Whisper‑T/B/S/M 四种模型规模。
- **额外实验**：MELD 上的 ASR 剪枝效果；时间效率分析。
- **公平性**：所有方法使用相同数据划分和评测指标，多次重复保证统计可靠性；对比基线覆盖广泛且具有代表性。

---

### 6. 主要结论与发现
- MelTrim 在所有任务和数据规模下均**显著优于所有通用基线方法**。
  - 情感识别：在 MEAD 数据集 1.813% 核心集比例下，WA 达到 64.67%（全量数据 84.68%），提升幅度最大达 **49.5%**（相对最优基线）。
  - 说话人识别：VoxCeleb1 上 EER 降至 4.79%（全量数据 2.38%），相对最优基线降低 **41.9%**。
  - ASR：在 MELD 上同样取得更低 WER。
- 消融实验证实：
  - MFCC 在**保留声学多样性**上优于深层语义特征；
  - 任务对齐的**语义 Judge 模型**是效用评分的必要条件；
  - 梯度范数效用显著优于简单损失评分。
- 方法对**分割段数和段长不敏感**，具有较强的鲁棒性。
- 使用小型 Judge 模型剪枝，可在更大的 Whisper 架构上实现**弱到强的迁移**，泛化性好。
- 端到端训练时间大幅缩短，数据效率提升显著。

---

### 7. 优点
- **问题新颖**：首次针对语音分类设计专用的粗到细剪枝框架，同时处理语句间和帧间冗余。
- **理论基础清晰**：通过梯度流和梯度范数关系形式化效用度量，让剪枝有理论依据。
- **声学‑语义解耦设计**：粗过滤用 MFCC 保持声学多样性，精剪枝用任务相关 Judge 模型捕获语义信息。
- **轻量 Judge 策略**：只需少量微调即可获得强评价信号，开销小且易于跨任务复用。
- **实验全面扎实**：覆盖多类语音任务、多种核心集比例、大量基线、系列消融及时效分析，统计严格（10 次重复），结论可信。
- **实用性强**：给出详细时间成本，并展示弱到强模型迁移能力，贴近实际应用。

---

### 8. 不足与局限
- **模态局限**：仅处理单一声学模态，未结合文本或其他多模态信息，可能遗漏重要的上下文冗余线索。
- **Judge 依赖**：效用评分依赖于预训练的 Judge 模型，其潜在的**数据偏见**可能使剪枝后子集对少数群体、稀有情绪或口音的代表性不足。
- **任务范围**：目前验证集中在语音分类（情绪、说话人识别），仅附带小规模 ASR 实验；对语音生成、翻译等更复杂的生成式任务尚未探索。
- **全量预训练需求**：Judge 模型仍需在完整数据集上预训练，并未完全消除对全量数据的依赖，仅减轻下游训练负担。
- **隐私与公平性**：帧级精细剪枝可能引入上下文割裂，且若部署时缺少公平审计，可能放大训练数据中的固有偏差。

（完）
