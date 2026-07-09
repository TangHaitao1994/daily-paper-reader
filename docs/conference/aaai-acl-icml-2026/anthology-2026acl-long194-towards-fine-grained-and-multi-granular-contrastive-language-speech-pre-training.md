---
title: Towards Fine-Grained and Multi-Granular Contrastive Language-Speech Pre-training
title_zh: 面向细粒度和多粒度对比语言-语音预训练
authors: "Yifan Yang, Bing Han, Hui Wang, Wei Wang, Ziyang Ma, Long Zhou, Zengrui Jin, Guanrou Yang, Tianrui Wang, Xu Tan, Xie Chen"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.194.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 面向语言-语音预训练的细粒度语音描述标注流水线
tldr: 语言-语音预训练中，细粒度说话风格建模因缺乏大规模标注而进展缓慢。本文构建了FCaps数据集，含47k小时语音和19M细粒度自由文本描述，并提出一种新型端到端标注流水线，直接基于音频生成详细描述，避免传统级联流水线的误差传播。LLM-as-a-judge评估显示，我们的标注在正确性、覆盖率和细粒度上均优于现有方法。该数据集和流水线为提升语音模型的表现力和多风格控制能力提供了关键资源，对语音合成、识别及多模态学习均有重要价值。该工作为大规模语音预训练提供了高质量的细粒度监督信号。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 814, \"height\": 461, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1654, \"height\": 609, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 813, \"height\": 621, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 782, \"height\": 255, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1645, \"height\": 1042, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1663, \"height\": 1915, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.194/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1521, \"height\": 548, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1531, \"height\": 558, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 807, \"height\": 178, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1580, \"height\": 546, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1560, \"height\": 344, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1533, \"height\": 252, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1640, \"height\": 1290, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1646, \"height\": 870, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1530, \"height\": 308, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1313, \"height\": 378, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.194/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1592, \"height\": 794, \"label\": \"Table\"}]"
motivation: 当前语言-语音模型因缺少可扩展的细粒度风格标注，训练通常依赖粗糙标题或任务特定监督，限制了说话风格建模能力。
method: 我们构建了FCaps数据集，包含47k小时语音和19M细粒度自由文本描述，并提出直接音频接地的端到端标注流水线，避免LLM重写的误差传播。
result: 使用LLM-as-a-judge评估，证明我们的标注在正确性、覆盖率上超越现有级联方法，为细粒度对比预训练提供高质量数据。
conclusion: FCaps显著提升语言-语音模型对说话风格的理解和生成能力，为大规模自监督语音模型训练奠定了数据基础。
---

## Abstract
Modeling fine-grained speaking styles remains challenging for language-speech representation pre-training, as existing speech-text models are typically trained with coarse captions or task-specific supervision, and scalable fine-grained style annotations are unavailable. We present FCaps, a large-scale dataset with fine-grained free-text style descriptions, encompassing 47k hours of speech and 19M fine-grained captions annotated via a novel end-to-end pipeline that directly grounds detailed captions in audio, thereby avoiding the error propagation caused by LLM-based rewriting in existing cascaded pipelines. Evaluations using LLM-as-a-judge demonstrate that our annotations surpass existing cascaded annotations in terms of correctness, coverage, and naturalness. Building on FCaps, we propose CLSP, a contrastive language-speech pre-trained model that integrates global and fine-grained supervision, enabling unified representations across multiple granularities. Extensive experiments demonstrate that CLSP learns fine-grained and multi-granular speech-text representations that perform reliably across global and fine-grained speech-text retrieval, zero-shot paralinguistic classification, and speech style similarity scoring, with strong alignment to human judgments. Code and dataset are publicly available at https://github.com/yfyeung/CLSP.

---

## 论文详细总结（自动生成）

好的，以下是对这篇论文的结构化、深入、客观的中文总结。

### 1. 论文的核心问题与整体含义

*   **核心问题**：当前的语言-语音预训练模型在建模**细粒度的说话风格**方面存在重大挑战。这主要是因为现有的语音-文本模型通常使用粗糙的、话语级别的文本描述（如“一个男性在说话”）或任务特定的离散标签进行训练，缺乏可扩展的、可靠的、细粒度的自然语言风格标注。
*   **研究动机**：为了解决上述瓶颈，需要构建一个大规模、高质量、包含细粒度自由文本描述的语音数据集，并探索能够有效利用这类多粒度监督信号的预训练方法，从而学习到能同时理解全局特征和局部细腻风格的统一语音-文本表征。
*   **整体含义**：本文旨在推动说话风格建模从预定义的离散属性向开放词汇、跨粒度、与语音信号直接对齐的自然语言描述转变，为更灵活、通用的语音-语言表征学习奠定基础。

### 2. 论文提出的方法论

本文的核心贡献包括一个数据集构建流水线（FCaps）和一个预训练模型（CLSP）。

*   **FCaps数据集与端到端标注流水线**
    *   **核心思想**：提出一种**端到端的标注流水线**，直接从音频信号生成细粒度的风格描述，避免了现有“语音->离散标签->LLM重写为文本”级联式流水线中的信息瓶颈和误差传播问题。
    *   **关键技术细节**：
        *   **详细描述生成**：使用多模态大模型作为详细描述器，输入语音，输出捕捉了情感、意图、语调变化乃至非语言发声等细腻风格的细粒度自然语言描述。
        *   **多正例描述**：通过设置不同随机种子多次运行描述器，为同一段语音生成多个语义类似但措辞和侧重点不同的正面文本视图，用于后续的对比学习。
        *   **智能体验证**：引入一个基于文本推理的验证智能体，使用预定义的检查清单和专用工具（如规则匹配），对生成的描述进行质量控制，过滤掉包含环境音、音频质量评价、语音内容转录等无关或错误的描述，确保标注质量。
*   **CLSP预训练模型**
    *   **核心思想**：提出一种**对比语言-语音预训练**模型，通过整合全局和细粒度的监督信号，实现**多粒度的统一表征学习**。
    *   **模型架构**：采用经典的双塔架构。语音编码器使用统一语音和音频表征模型SPEAR，文本编码器使用RoBERTa。两者输出分别经过MLP投影到共享嵌入空间。
    *   **两阶段课程式训练策略**：
        *   **第一阶段**：在大规模细粒度描述数据上进行标准对比学习，专注于语音与细粒度文本的对齐。
        *   **第二阶段**：引入**多正例对比学习**，在一个批次中，每段语音配有两条正面文本（如一条全局描述和一条细粒度描述，或两条不同的细粒度描述），并设计了一个任务调度器动态采样这两种配对任务，以实现跨粒度泛化和细粒度判别能力的平衡。

### 3. 实验设计

*   **数据集与场景**：
    *   **构建数据集**：使用自建的**FCaps**数据集（包括从Emilia和PSC-Base派生的两个子集）进行模型训练。
    *   **评估基准与场景**：
        *   **语音-文本检索**：在PSC-Base的测试集上进行全局和细粒度的双向检索任务。
        *   **零样本副语言分类**：在IEMOCAP、RAVDESS、CREMA-D数据集上评估模型对情感、性别、年龄的直接识别能力。
        *   **语音风格相似度评分**：在PSC-Base的测试集上，计算模型给出的语音-文本相似度分数与人类主观评分的相关性，评估其作为自动化评估指标的可靠性。
    *   **对比方法**：实验对比了多个先进的开放源代码音频-文本模型，包括 LAION-AI CLAP, GLAP, ParaCLAP, 以及 Auden-Voice CLAP，展示了从粗粒度到特定任务监督的不同预训练范式。
*   **实验充分性、客观性与公平性分析**：
    *   **充分性**：实验覆盖了检索、分类、相似度打分及与人类评判对齐性等多种任务，并进行了多阶段训练、损失权重、任务调度器等消融研究，验证了各组件的有效性。
    *   **客观性**：使用了标准的自动评估指标（Recall@k, mAP, WA/UA）和统计学相关系数（Pearson, Spearman， Kendall's Tau）。
    *   **公平性**：所有对比基线均使用公开可用的模型检查点进行评估；零样本分类设置了严格条件，排除任何微调；LLM-as-a-Judge和人类评估都设计了详细的评分标准和流程，确保了比较的公平性。

### 4. 资源与算力

*   **算力信息**：论文明确提到，CLSP模型的训练使用了 **8 块 NVIDIA A100 80GB GPU**。第一阶段训练了 120万步，第二阶段微调了 4000 步。

### 5. 实验数量与充分性

*   **实验数量**：论文包含了多组实验。
    *   **对比实验**：与5种主流基线模型在3大类任务（检索、分类、相似度）的多个数据集上进行比较。
    *   **消融实验**：针对CLSP模型，设计了3组消融研究，分别验证多阶段训练、损失权重λ、任务调度策略的影响，每组内包含多个变体（如不同的训练阶段组合、不同的λ值、不同的静态/动态调度参数）。
*   **充分性评价**：实验设计全面，从数据集质量、下游任务性能、与人类感知一致性、模型组件等多个维度进行了验证，实验数量充分，能够有力支撑论文结论。评估过程采用了客观指标和严格对照，确保了公平性。

### 6. 论文的主要结论与发现

*   **数据集质量优越**：通过LLM-as-a-Judge评估，端到端流水线生成的FCaps标注在**准确性、覆盖度和自然度**上均大幅优于传统的级联式标注方法。
*   **模型性能领先**：基于FCaps训练的CLSP模型在全局和细粒度语音-文本检索、零样本副语言分类任务中全面超越现有模型，尤其在细粒度任务上优势显著，泛化能力强。
*   **与人类判断高度一致**：在语音风格相似度评分任务上，CLSP给出的分数与人类主观评价的多个相关系数均很高，证明其可作为与人类感知对齐的自动化评估指标，有望替代成本高昂的人类评估或大模型评估。

### 7. 优点

*   **方法创新性强**：
    *   提出了创新的**端到端标注流水线**，有效解决了级联方法存在的固有信息损失问题。
    *   首次提出针对**细粒度和多粒度**语音风格建模的对比学习框架，并设计了有效的多正例训练和任务调度策略。
*   **实验设计全面扎实**：
    *   结合了**LLM-as-a-Judge**和**人类主观评估**来验证数据集和模型的质量，评估维度丰富且有说服力。
    *   在多个下游任务和多个基准上进行了系统性评估，并提供详尽的消融实验，结果可靠。
*   **实用价值高**：
    *   发布的FCaps数据集和CLSP模型为语音风格理解和生成任务提供了强大的工具，特别是CLSP作为**人类对齐的自动化指标**，在语音生成评估领域有广阔的应用前景。

### 8. 不足与局限

*   **语言单一**：当前工作仅聚焦于**英语**语音，数据集和模型均未覆盖多语言场景，限制了其全球范围内的直接适用性。
*   **数据覆盖偏差**：
    *   CLSP模型依赖一个仅在英语语音上预训练的编码器，其性能可能受限于该预训练阶段的潜在偏差。
    *   公开可用的副语言数据集在口音、情感表达等维度覆盖有限，可能导致模型在稀有或代表性不足的风格上表现较差。
*   **标注流水线依赖**：
    *   端到端标注流水线的性能依赖于底层的多模态详细描述大模型的能力，其输出存在内在不确定性。对于音频本身难以区分的歧义属性（如特定口音），仍需要额外的人工标注标签作为指导。

（完）
