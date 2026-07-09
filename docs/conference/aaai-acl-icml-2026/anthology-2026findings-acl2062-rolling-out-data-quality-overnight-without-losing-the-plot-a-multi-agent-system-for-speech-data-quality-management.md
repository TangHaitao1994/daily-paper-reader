---
title: "Rolling Out Data Quality Overnight, without losing the plot: A Multi-Agent System for Speech Data Quality Management"
title_zh: 一夜之间实现数据质量管理，不失主线：面向语音数据质量管理的多智能体系统
authors: "Rishabh Kumar, Abhinav Painuli, Chriss Philip Saji, Devesh Soni, Amrith Krishna, Ganesh Ramakrishnan"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.2062.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: 用于自动化语音数据质量验证和流程管理的多智能体系统
tldr: 针对大规模语音数据集质量管理流程脆弱且依赖专家的问题，本文提出SpeechQM-Agent多智能体框架，将用户需求编译为依赖感知的DAG工作流，自动执行音频、文本和元数据校验，并支持运行时重规划。该框架提高了多语言、多格式数据的验证鲁棒性，并发布了验证数据集，为语音数据预处理提供了高效自动化工具。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1539, \"height\": 882, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1610, \"height\": 389, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 750, \"height\": 448, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 767, \"height\": 353, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1589, \"height\": 967, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1571, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 790, \"height\": 528, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 541, \"height\": 212, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2062/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 771, \"height\": 418, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1609, \"height\": 865, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 788, \"height\": 293, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 800, \"height\": 265, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 677, \"height\": 238, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 684, \"height\": 242, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 802, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 803, \"height\": 445, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 499, \"height\": 324, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 795, \"height\": 1660, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 801, \"height\": 579, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1620, \"height\": 451, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1086, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1628, \"height\": 692, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 596, \"height\": 245, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2062/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1614, \"height\": 359, \"label\": \"Table\"}]"
motivation: 语音数据质量管理流程依赖人工，易出错且难以扩展。
method: 构建由LLM驱动的多智能体系统，自动编译需求为可重规划的DAG校验工作流。
result: 框架在多语言多格式数据上提高了验证鲁棒性和效率。
conclusion: SpeechQM-Agent为语音数据质量保障提供了可扩展的自动化解决方案。
---

## Abstract
Quality management when creating large-scale speech datasets is essential for building reliable downstream models, yet verification pipelines are often brittle, domain-specific, and expertise-intensive. We introduce SpeechQM-Agent , a natural language-driven agentic framework that compiles user requirements into dependency-aware DAG workflows over modular tools for audio, transcript, and metadata verification. A central planner LLM enforces prerequisites and supports execution-time replanning (e.g., re-running failed steps or swapping tools), reducing manual pipeline engineering and improving robustness across heterogeneous vendor formats and multilingual settings. We also release SpeechQM-Dataset , a multilingual benchmark with controlled, vendor-inspired quality artifacts spanning 24 verification tasks. Across experiments, SpeechQM-Agent attains 80-90% agreement with expert verification while requiring <20% of the cost and time of manual QC, and we further validate transfer to real vendor-supplied corpora. Planner LLM comparisons highlight fidelity-efficiency trade-offs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- 大规模语音数据质量管理（DQM）对构建可靠的语音模型至关重要，但当前的质量控制（QC）流程存在显著痛点：
  - **流程脆弱且依赖专家**：不同供应商格式、领域特定规则、多语言脚本导致手动构建校验流水线耗时且易出错。
  - **自动化程度低**：现有工具多为固定功能检查，难以灵活组合成完整的依赖感知工作流。
  - **低资源场景挑战**：尤其在印度等多语言低资源环境下，领域专家稀缺，人工校验成本极高，无法规模化。
- 因此，论文旨在提出一个可理解自然语言指令、自动编排校验任务、并支持执行时动态重规划的多智能体框架，以替代脆弱的静态流水线，降低人工参与和成本。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **整体框架**：SpeechQM-Agent，一个由中心规划大语言模型（Planner LLM）协调的多智能体系统，将自然语言需求编译为具有依赖关系的DAG工作流。
- **核心流程**（如图1所示）：
  1. **任务解析与动作生成**：用户输入自然语言请求，中心规划LLM（PlannerLLM）将其分解为原子校验动作列表（例如检查采样率、检测音频损坏等）。
  2. **节点生成与依赖图构建**：动作列表转换为可执行节点，并显式捕获节点间的依赖关系，构造有向无环图（DAG）。采用**拓扑排序**确保执行顺序，并利用代码表达并行与条件逻辑。
  3. **工具合成与检索**：每个校验节点关联一个可执行工具，来源包括：动态LLM工具合成、预定义工具库（如GPT-4o预生成），并通过验证确保工具可靠。
  4. **工作流执行与监控**：按拓扑顺序执行图，支持依赖感知的并行执行。监控智能体跟踪状态，若节点输出为空或异常，则重试工具最多`r`次，保证执行完整性。
  5. **输出聚合与仪表盘生成**：聚合所有节点输出，生成结构化报告和可视化面板，供人工审核。
  6. **执行时重规划**：当中间步骤失败或输出置信度低时，规划器可调用替代工具或使用更新参数重跑，提升鲁棒性。
- **校验任务体系**：框架覆盖24个校验任务，分为两大类：
  - **QC1（音频与元数据）**：语言识别、文件格式、损坏检测、采样率、静音时长、上采样检测、说话人数量与时长、说话人有效性等。
  - **QC2（转录与内容）**：音文对齐、时间戳分割、脚本一致性、代码混合检测、WER/CER/CTC/LM评分、转录归一化、音译、字素/词汇分布、领域分类、内容重复检查等。
- **核心组件**：中央规划LLM负责高级调度，多重智能体分别负责解析、执行、监控，形成可插拔的模块化架构。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **评测数据集**：
  - **SpeechQM-Dataset**：新构建的合成多语言基准，聚焦印地语（Hindi），涵盖11个领域、55个场景、110个说话人。数据集通过LLM对话生成→跨语言翻译→元数据扰动→TTS合成并注入受控错误（如标签插入、脚本错误、音频损坏、上采样等）构建。按错误复杂度分为单扰动、双扰动、三重扰动的子集（QC1/2-1到-3）。该数据集用于系统化评估24个校验任务。
  - **真实供应商数据**：一个12,000小时的多供应商、8种印度语言的语音语料库，用于验证系统在实际生产环境中的迁移能力。
  - **下游ASR基准**：Bhashini语料子集与Shrutilipi数据集，用于对比不同数据筛选策略对词错率（WER）的影响。
- **对比基线**：
  - 人类专家校验（金标准）。
  - 非智能体的静态流水线：单一智能体（顺序执行）和多智能体（无中央规划器、无依赖图）。
  - 不同规模的规划LLM：GPT-4o、GPT-4.1-mini、DeepSeek-R1-distill、LLaMA-3.3-70B、LLaMA-3.1-8B，比较执行准确率、幻觉率、运行时间、成本。
- **评估指标**：执行准确率（与真值一致性）、幻觉率、端到端耗时、词错率（WER）等。

### 4. 资源与算力
- 论文中关于下游ASR模型的**预训练和微调**实验提到：使用了 **2 块 NVIDIA A100-SXM4-80GB GPU**，每次训练耗时 **4 至 48 小时**。
- 对于 SpeechQM-Agent 框架本身的推理（即调用各LLM执行校验任务），仅提供了推理成本（每千词美元）和运行时间，未说明所需GPU卡数，可能为按需调用的云端API或单卡推理。

### 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- 实验覆盖较为全面：
  - **QC1与QC2子任务评估**：在SpeechQM-Dataset的不同难度的子集上，报告了5种LLM在多项子任务上的准确率、幻觉率。
  - **规划LLM对比**：专门针对QC1指令和QC2指令，对比了各模型的准确率和耗时。
  - **下游任务验证**：对比了随机抽样、SpeechQM-Agent遴选和半自动人工遴选（Shrutilipi）对ASR模型WER的影响。
  - **真实世界部署**：审计了12,000小时多语言供应商数据，量化了各类错误分布。
  - **架构消融对比**：比较了单一智能体、无中心规划的多智能体与完整框架的准确性和耗时。
- **充分性与公平性**：实验考虑了多模型、多错误类型、多语言场景，并在受控合成的基准与真实数据上交叉验证。对比基线包括了人工和不同智能体配置，比较公平。但限于计算开销，部分大模型（如LLaMA-3.1-8B）在低资源语言上的零能力并未深入优化，但已如实报告其受限表现。

### 6. 论文的主要结论与发现
- **高一致性与高效率**：SpeechQM-Agent 能达到专家校验 **80-90% 的一致性**，同时人工成本和时间降至完全人工流程的 **不足20%**。
- **规划器存在保真度-效率权衡**：GPT-4.1-mini 转录级精度最高、无幻觉；LLaMA-3.3-70B 在保持较高准确率的同时提供最佳性价比和速度；小模型因缺乏印度语言训练数据而严重退化。
- **下游收益显著**：通过框架自动遴选的数据子集训练的ASR模型，WER为9.8%，优于随机抽样的12.9%，且与半自动人工遴选的性能相当（约9.8%）。
- **真实数据集审计能力**：在12,000小时多供应商语料中，系统自动标记出32.5%的问题数据（低采样率、脚本错误、静音、重复等），验证了其大规模生产环境的实用性。
- **依赖感知与重规划有效**：相较于无依赖管理的多智能体或单一智能体基线，完整框架的准确率大幅提升（至0.8+），接近人类水平。

### 7. 优点：方法或实验设计上有哪些亮点
- **自然语言驱动**：将领域外行人/专家的意图直接转化为可执行DAG，大幅降低使用门槛。
- **依赖感知与动态重规划**：中央规划LLM可强制执行先决条件（如先转录再分类），并在失败时自动替换工具或重跑，提升鲁棒性。
- **全面且模块化**：覆盖24种校验任务，支持音频、转录、元数据的端到端检验，工具可插拔、可动态合成。
- **基准数据集贡献**：提供了首个专为校验系统评估设计的SpeechQM-Dataset，包含受控、现实的质量缺陷，推动了该方向研究。
- **实践价值突出**：在真实供应商数据集上展示出分类错误、指导采购的能力，证明其可落地性。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **依赖规划LLM能力**：性能严重受限于所用LLM的对语言和推理的掌握，小模型（如LLaMA-3.1-8B）无法处理低资源语言任务。
- **评估范围局限**：目前仅聚焦音频-转录对齐数据，未扩展至多模态（如语音翻译、对话语料）；**尚未检测方言/口音不平衡或社会语言偏差**。
- **运行开销**：尽管远快于人工，但处理超大规模语料（数十万小时）时，运行时间仍不可忽视，需要进一步的分布式优化。
- **合成数据偏差**：SpeechQM-Dataset为合成构建，虽注入仿真错误，可能未完全覆盖真实世界的罕见缺陷模式。
- **定性校验依赖LLM**：对于领域分类、转录评判等定性任务，以LLM作为代理评估者，可能存在与人类专家判断不一致的隐性偏差。

（完）
