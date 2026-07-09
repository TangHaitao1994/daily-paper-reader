---
title: An Exploration of Mamba for Speech Self-Supervised Models
title_zh: 探索Mamba在语音自监督模型中的应用
authors: "Tzu-Quan Lin, Heng-Cheng Kuo, Tzu-Chieh Wei, Hsi-Chun Cheng, Chun Wei Chen, Hsien-Fu Hsiao, Yu Tsao, Hung-yi Lee"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.470.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 探索基于Mamba的HuBERT作为自监督语音模型，改进长上下文和流式语音识别
tldr: 虽然Mamba在语言建模上取得成果，但其在语音自监督学习中的潜力尚未充分探索。本文构建基于Mamba的HuBERT模型，利用选择性状态空间实现线性时间处理，在长上下文和流式ASR微调中显著降低计算量，并在SUPERB基准上取得有竞争力的表现，分析还表明其量化表征质量更高且能更清晰地捕捉说话人特征。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1314, \"height\": 433, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 691, \"height\": 453, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1659, \"height\": 346, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 721, \"height\": 730, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 516, \"height\": 764, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 808, \"height\": 1052, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.470/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1029, \"height\": 639, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 786, \"height\": 148, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 779, \"height\": 222, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1600, \"height\": 465, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1544, \"height\": 318, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1661, \"height\": 365, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.470/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1634, \"height\": 277, \"label\": \"Table\"}]"
motivation: Mamba在语音SSL中的潜力尚未充分探索，先前工作仅限于孤立任务。
method: 提出基于Mamba的HuBERT模型，利用选择性状态空间进行自监督预训练。
result: 模型在长上下文和流式ASR上计算量更低，SUPERB基准上性能有竞争力。
conclusion: Mamba架构为语音自监督学习提供了高效且有效的替代方案。
---

## Abstract
While Mamba has demonstrated strong performance in language modeling, its potential as a speech self-supervised learning (SSL) model remains underexplored, with prior studies limited to isolated tasks. To address this, we explore Mamba-based HuBERT models as alternatives to Transformer-based SSL architectures. Leveraging the linear-time Selective State Space, these models enable fine-tuning on long-context ASR with significantly lower compute. Moreover, they show superior performance when fine-tuned for streaming ASR. Beyond fine-tuning, these models show competitive performance on SUPERB probing benchmarks, particularly in causal settings. Our analysis shows that they yield higher-quality quantized representations and capture speaker-related features more distinctly than Transformer-based models. These findings highlight Mamba-based SSL as a promising and complementary direction for long-sequence modeling, real-time speech modeling, and speech unit extraction. The codebase is available at https://github.com/hckuo145/Mamba-based-HuBERT.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：Transformer 的自注意力机制在长序列上存在二次计算复杂度，导致部署成本高。Mamba（选择性状态空间模型）在语言建模中表现优异，但在语音自监督学习（SSL）领域的潜力尚未充分探索，且先前研究仅局限于孤立任务。
- **整体含义**：系统性地探索基于 Mamba 的 HuBERT 模型，替代 Transformer 架构，旨在构建可在无标注语音上预训练、并通过轻量微调快速适配多种下游任务的语音基础模型，并验证其在长序列建模、实时语音处理和表示质量上的优势。

### 2. 论文提出的方法论
- **核心思想**：将 HuBERT 中的 Transformer 模块替换为 Mamba 模块，保留 CNN 特征编码器和卷积位置编码器。Mamba 基于选择性状态空间模型，通过对输入动态调整系统参数（B、C、Δ），在保持内容选择能力的同时将计算复杂度降至线性。
- **技术细节**：
  - 采用离散时间公式：\(h_t = \bar{A} h_{t-1} + \bar{B} x_t\)，\(y_t = C h_t\)，其中 \(\bar{A}, \bar{B}\) 由连续参数 \(A, B\) 经零阶保持（ZOH）离散化得到。
  - 引入选择机制：\(B = f_B(x), C = f_C(x), \Delta = \text{Broadcast}_D(f_\Delta(x))\)，使状态转换与输出映射自适应输入。
  - 构建因果（单向）Mamba 和双向 Mamba（ExtBiMamba/InnBiMamba）。双向版本通过前后向分支独立处理序列后相加实现；ExtBiMamba 采用独立的输入输出投影，InnBiMamba 共享投影但复制卷积与 SSM 模块。
  - 预训练流程遵循 HuBERT：第一轮用 MFCC 特征作目标训练 250k 步，第二轮用第六层输出作目标再训练 400k 步，仅在单张 V100 GPU 上进行，采用更大的 per-GPU batch size 以适配资源限制。

### 3. 实验设计
- **数据集**：
  - 预训练：LibriSpeech 960 小时。
  - 微调：ASR 上使用 LibriSpeech 100 小时（因果/流式 ASR）和 TEDLIUM3（长文档级 ASR）；SUPERB 基准测试。
- **Benchmark 与评估指标**：
  - ASR：词错误率（WER）、计算量（MACs/sec）、实时因子（RTF）。
  - 表征质量：聚类后电话纯度（phone purity）、与音素/说话人/情绪嵌入的典型相关分析（CCA）相似度。
  - 下游任务：SUPERB 中的音素识别（PR）、说话人辨识（SID）、情绪识别（ER）、意图分类（IC），并计算综合分数 SUPERB S，部分模型进一步进行全量 SUPERB 评估。
- **对比方法**：
  - 因果设置：Causal Transformer、Transformer + 因果掩码、Mamba、Mamba+MLP（不同参数规模 Base/Small）。
  - 双向设置：标准 Transformer、ExtBiMamba、InnBiMamba、InnBiMamba+MLP，以及消融对比 ExtBiMamba 与 InnBiMamba。

### 4. 资源与算力
- **GPU**：预训练使用单张 NVIDIA V100 GPU；RTF 测量在单张 NVIDIA RTX A6000（48 GB）上进行。
- **训练时长与规模**：未明确给出总训练时长，但说明了预训练步数（250k+400k）和采用的批次大小约为原始 HuBERT 的四分之一。由于资源有限，仅进行单次训练运行，未报告多轮平均结果。

### 5. 实验数量与充分性
- **实验组数量**：包含多种模型规模（Base 约 94.7M 参数，Small 约 23.5M 参数）、因果与双向设置、多个下游任务（4 项 SUPERB 任务+全量 10 项任务）、不同音频长度下的效率测试、多种聚类数目（k=100, 500, 1000）的电话纯度分析、三种说话人嵌入的 CCA 分析，以及双向架构消融（ExtBiMamba vs. InnBiMamba）。
- **充分性与公平性**：对比基线全面，统一使用 HuBERT 训练流程和单 GPU 限制进行公平比较；效率测试覆盖 5 秒至 320 秒序列，充分展示复杂度趋势；分析了表征特性与下游性能的关系。但所有预训练结果来自单次试验，未提供误差范围或多次平均，且总预训练音频量受限于单 GPU，可能未完全体现大规模扩展能力；全量 SUPERB 仅对部分代表模型进行了测试。

### 6. 论文的主要结论与发现
- Mamba 的线性时间特性使其在长序列处理中 MACs 和 RTF 几乎恒定，而 Causal Transformer 在 80 秒后便出现显存溢出（OOM），凸显了 Mamba 在长上下文语音识别中的效率优势。
- 在长文档级 ASR（TEDLIUM3）中，ExtBiMamba 处理整段语音将 WER 从 13.37% 降至 11.08%，而 Transformer 因 OOM 无法运行。
- 因果 ASR 下，78.2M 的 Mamba 模型（WER 15.77%）优于 94.7M 的 Causal Transformer（16.66%）。
- Mamba 模型在因果 SUPERB 评测上整体优于 Causal Transformer，尤其在音素和说话人任务上表现突出。
- 量化表征的电话纯度更高，说明对口语语言模型等需要 SSL 离散单元的应用更有益；同时，Mamba 早期层对说话人嵌入的相似度更高，能更清晰地捕捉说话人特征。
- 双向设置下，小规模 ExtBiMamba 性能优于 Transformer，但基础规模 ExtBiMamba 落后于 Transformer，表明 Mamba 在双向条件下的可扩展性面临挑战；InnBiMamba 在大参数量时表现更好，训练稳定性可能是一个因素。

### 7. 优点
- **新颖的系统性研究**：首次全面探索 Mamba 架构在语音 SSL 中作为基础模型和特征提取器的潜力，弥补了以往孤立任务评估的不足。
- **效率与性能的兼顾**：清晰量化并验证了 Mamba 在长序列上的线性计算优势，并提供微调后实际任务的性能提升证据。
- **多维度的分析**：结合 ASR 微调、SUPERB 探针任务、聚类纯度、CCA 分析，从计算效率、下游精度到表征性质进行了多层次剖析。
- **消融与架构对比**：对双向 Mamba 变体（ExtBiMamba/InnBiMamba）进行了消融，并揭示了规模依赖的优劣，并初步关联训练稳定性问题。
- **开源代码**：提供可复现的代码库。

### 8. 不足与局限
- **训练规模与结果稳健性**：所有预训练仅在单张 V100 上以约四分之一原始批次大小进行，结果来自单次训练，未报告多次运行的均值和方差，泛化稳健性待进一步验证。
- **双向扩展性受限**：基础规模的 Mamba 双向模型未能超越同等规模 Transformer，说明 Mamba 在双向 SSL 预训练中的扩展性仍有瓶颈。
- **下游任务覆盖**：虽进行了部分全量 SUPERB 评估，但多数核心结论基于四项任务子集，某些类别任务（如副语言、语义任务）上 Mamba 仍落后于 Transformer。
- **硬件依赖**：效率测试和 OOM 结论基于特定 GPU 和实现，绝对处理长度可能因内存优化而异，报告反映实验设计边界。
- **未深入探究训练稳定性机制**：仅定性观察了 ExtBiMamba 的梯度溢出，未提出改进方案或系统性分析成因。

（完）
