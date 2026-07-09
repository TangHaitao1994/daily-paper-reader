---
title: "SpeechLLM-as-Judges: Towards General and Interpretable Speech Quality Evaluation"
title_zh: SpeechLLM-as-Judges：迈向通用且可解释的语音质量评估
authors: "Hui Wang, Jinghua Zhao, Yifan Yang, Shujie Liu, Junyang Chen, Yanzhe Zhang, Shiwan Zhao, Jinyu Li, Jiaming Zhou, Haoqin Sun, Yan Lu, Yong Qin"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.349.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用大语言模型对合成语音进行结构化和可解释的质量评估
tldr: 提出SpeechLLM-as-Judges范式，利用大语言模型进行结构化、可解释的语音质量评估，构建多语言多任务数据集SpeechEval并训练SQ-LLM模型，为语音合成质量评估提供通用且可解释的新方法。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1637, \"height\": 768, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 778, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1614, \"height\": 630, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 787, \"height\": 658, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 797, \"height\": 455, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 775, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 791, \"height\": 320, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 769, \"height\": 732, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 729, \"height\": 338, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 788, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 799, \"height\": 355, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 730, \"height\": 511, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.349/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1307, \"height\": 491, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1659, \"height\": 438, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 758, \"height\": 485, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 807, \"height\": 347, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1569, \"height\": 533, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 782, \"height\": 627, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 805, \"height\": 573, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 740, \"height\": 220, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1648, \"height\": 921, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1648, \"height\": 1052, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1646, \"height\": 276, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1648, \"height\": 1266, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 786, \"height\": 133, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 810, \"height\": 207, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 763, \"height\": 167, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1650, \"height\": 795, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-016.webp\", \"caption\": \"\", \"page\": 0, \"index\": 16, \"width\": 1649, \"height\": 751, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-017.webp\", \"caption\": \"\", \"page\": 0, \"index\": 17, \"width\": 1653, \"height\": 490, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-018.webp\", \"caption\": \"\", \"page\": 0, \"index\": 18, \"width\": 1502, \"height\": 483, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.349/table-019.webp\", \"caption\": \"\", \"page\": 0, \"index\": 19, \"width\": 810, \"height\": 312, \"label\": \"Table\"}]"
motivation: 生成语音技术发展迅速，但合成语音质量的感知评估仍缺乏可解释性和泛化性。
method: 提出SpeechLLM-as-Judges范式，利用大语言模型进行结构化、解释性的语音质量评估，并构建了多语言多任务数据集SpeechEval。
result: 基于SpeechEval训练的SQ-LLM模型在质量评估、比较、改进建议和深度伪造检测等任务上实现了可解释的评估。
conclusion: 该工作为语音质量评估提供了通用且可解释的新范式，有助于推动语音合成技术的进步。
---

## Abstract
Generative speech technologies are progressing rapidly, but evaluating the perceptual quality of synthetic speech remains a core challenge. Existing methods typically rely on scalar scores or binary decisions, which lack interpretability and generalization across tasks and languages. We present SpeechLLM-as-Judges, a new paradigm for enabling large language models (LLMs) to conduct structured and explanation-based speech quality evaluation. To support this direction, we introduce SpeechEval, a large-scale dataset containing 32,207 multilingual speech clips and 128,754 annotations spanning four tasks: quality assessment, pairwise comparison, improvement suggestion, and deepfake detection. Based on this resource, we develop SQ-LLM, a speech-quality-aware LLM trained with chain-of-thought reasoning and reward optimization to improve capability. Experimental results show that SQ-LLM delivers strong performance across tasks and languages, revealing the potential of this paradigm for advancing speech quality evaluation. The relevant code, models, and data are publicly available at https://github.com/NKU-HLT/SpeechLLM-as-Judges.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：生成式语音技术（如 TTS、语音翻译）迅速发展，但合成语音的感知质量评估仍面临核心挑战。
- **现有问题**：
    - 传统方法（如 MOS 评分、AB 偏好测试）只提供标量分数或二元判别，**缺乏可解释性**，无法指出影响质量的具体因素。
    - 客观指标（如 MCD）与人类感知对齐差，且多数模型**任务单一**、**语言受限**，难以泛化到多任务、多语言场景。
- **整体含义**：提出 **SpeechLLM-as-Judges** 新范式，借助大语言模型（LLM）实现**结构化、可解释**的通用语音质量评估，让模型不仅能打分，还能给出自然语言理由和改进建议。

### 2. 方法论
- **核心思想**：以预训练语音-语言大模型为基座，通过**指令微调 + 链式推理（CoT）**与**奖励优化（GRPO）**两阶段训练，使模型能够同时完成质量评估、成对比较、改进建议和深度伪造检测四项任务。
- **关键技术细节**：
    - **统一指令框架**：将不同任务格式化为统一提示模板，模型接收语音和文本指令，生成结构化回答。
    - **模型架构（SQ-LLM）**：基于 Qwen2.5-Omni-7B，包含冻结的语音编码器和 LoRA 微调的解码器。
    - **第一阶段：CoT 指令微调**。
        - 模型先生成 8 个维度（整体质量、可懂度、失真、语速、动态范围、情感影响力、艺术表达、主观体验）的中间预测，再生成最终描述。
        - 损失函数：\( L = \lambda \sum_{i=1}^{N} L_{\text{dim}}^{(i)} + L_{\text{ans}} \)，\(\lambda=0.3\)。
    - **第二阶段：GRPO 奖励优化**。
        - 设计四个自动奖励维度：帮助性、相关性、准确性、细节程度。
        - 使用冻结的 Qwen3 模型作为评估器，对生成结果打分，归一化后按权重聚合得到总分 \( R_{\text{total}} = \sum_{d} \lambda_d r_d \)。
        - 在 GRPO 框架下，以该总分作为学习信号更新 LoRA 适配器，提升输出与人类偏好的对齐度。
- **数据支撑**：构建大规模数据集 **SpeechEval**，含 32,207 条多语种语音（中、英、日、法）和 128,754 条人工标注，覆盖四项任务及细粒度维度标签。

### 3. 实验设计
- **数据集与分割**：使用自建的 SpeechEval，按 70%/15%/15% 划分训练/验证/测试集，确保测试集包含未见过的说话人、系统和文本。深度伪造检测任务采用符合真实分布的不平衡划分，包含分布内和零样本测试。
- **对比方法（基线）**：
    - **直接评估的多模态 LLM**：Qwen2-Audio、Qwen2.5-Omni、MiDashengLM。
    - **定制构建管道或微调音频 LLM**：Whisper+Qwen3、Audiobox 美学编码器+Qwen2.5、WavLM+Qwen3，以及微调后的 Qwen2-Audio。
    - **任务特定专家模型**：MOSNet、UTMOS、Audiobox Aesthetics（质量评估/比较）；RawNet2、AASIST、AASIST2（深度伪造检测）。
- **评价指标**：
    - **生成任务**：LLM 评分（LScore）、自动指标（SBERT、FENSE）、维度级皮尔逊相关系数（PCC）与准确率（ACC），并辅以人工评分（1-5 分）。
    - **检测任务**：等错误率（EER）、最小检测成本函数（minDCF）、准确率。

### 4. 资源与算力
- **训练算力**：文中明确说明训练 SQ-LLM 共约需 **43 个 A100 GPU 小时**，其中监督微调（SFT）约 12 GPU 小时，GRPO 阶段约 31 GPU 小时。
- **训练配置**：使用 LoRA 高效微调，语音编码器冻结，SFT 阶段批次大小 4、学习率 1e-4，GRPO 阶段批次大小 1、每指令采样 4 个候选、学习率 1e-6。

### 5. 实验数量与充分性
- **实验组数**：设计了丰富的实验矩阵。
    - **主结果对比**：分别在四项任务上对比 SQ-LLM 与三类基线（直接评估、微调变体、专家模型），参见表 4-6。
    - **精细维度分析**：对比较任务进行 8 维度准确率热力图分析（图 5）；对评估任务报告各维度 PCC 和语速准确率（表 13）。
    - **消融实验**：剥离 CoT 和 GRPO，通过自动指标（表 7）和人工评估（图 7）验证各组件贡献。
    - **多语言与细粒度分析**：展示 SQ-LLM 跨语言性能（图 6）；按语言和假语音来源拆解深伪检测准确性（表 14）；分析 CoT 对主观维度 PCC 的提升（图 23）。
- **充分性与公平性**：实验覆盖多任务、多语言、多维度和多类基线，包含自动评估与人工评估，消融研究清晰，评估标准统一，对比公平、客观。

### 6. 主要结论与发现
- SQ-LLM 在质量评估、成对比较、改进建议和深度伪造检测四项任务上均取得最佳性能，显著优于直接评估的 LLM 和传统专家模型。
- 链式推理（CoT）使模型能进行细粒度、结构化的质量归因，尤其在动态范围、情感表达等主观维度上提升明显。
- GRPO 奖励优化进一步提升了生成文本的帮助性和细节质量，尤其在开放式建议任务中收益最大。
- 该范式展现出良好的跨语言、跨域泛化能力，为可解释的语音质量评估提供了一条新路径。

### 7. 优点
- **范式创新**：首次系统性地将 LLM 用作解释性语音质量评估器，支持自然语言理由和多任务统一处理。
- **数据贡献**：构建了大规模、多语种、细粒度的 SpeechEval 数据集，标注详尽，覆盖质量评估、比较、建议、真伪检测等多种实用场景。
- **方法扎实**：两阶段训练（CoT 指令微调 + GRPO）有效提升了模型的可解释性和与人类偏好的对齐度，设计清晰、复现性强。
- **实验全面**：对比基线丰富，评估维度多元（自动指标、LLM 评分、人类评估），消融和分析实验深入，验证充分。

### 8. 不足与局限
- **语言与任务覆盖有限**：当前数据集仅包含四种语言，未涉及低资源语言或代码切换场景；任务类型仍可进一步扩展（如情感表达一致性等）。
- **深伪检测泛化性不足**：模型在中文开源假语音和法语商业假语音上准确率下降，表明对特定生成渠道的泛化仍有挑战。
- **数据与标注偏差**：数据集虽规模较大，但语种分布不均（中、英语占多数），标注者背景和人数可能引入一定主观偏差。
- **算力要求**：虽使用 LoRA 等高效微调，整体训练仍需约 43 A100 GPU 小时，对资源有限的研究者有一定门槛。

（完）
