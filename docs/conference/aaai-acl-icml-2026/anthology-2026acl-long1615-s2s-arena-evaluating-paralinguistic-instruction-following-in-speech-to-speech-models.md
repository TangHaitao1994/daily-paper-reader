---
title: "S2S-Arena: Evaluating Paralinguistic Instruction Following in Speech-to-Speech Models"
title_zh: S2S-Arena：评估语音到语音模型的副语言指令遵循能力
authors: "Feng Jiang (蒋峰), Zhiyu Lin, Yiyang Liu, Liumeng Xue, Fan Bu, Yuhao Du, Xiangying Chen, Benyou Wang, Haizhou Li"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1615.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 评估语音对话AI副语言表达能力的基准
tldr: 现有语音到语音（S2S）模型评测多依赖文本指标，忽略副语言信息。该论文提出S2S-Arena，一个专为S2S模型设计的评测基准，在四个交互复杂度层次下系统评估模型的语义理解和副语言表达能力，采用两阶段数据构造流程。该基准为提升语音对话AI的自然度和表现力提供了关键评估工具。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1615/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 798, \"height\": 908, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1615/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1663, \"height\": 600, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1615/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 801, \"height\": 642, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1615/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 805, \"height\": 571, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1646, \"height\": 587, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 795, \"height\": 195, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 704, \"height\": 179, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 804, \"height\": 430, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 802, \"height\": 409, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1643, \"height\": 409, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1650, \"height\": 724, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 795, \"height\": 1167, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1636, \"height\": 968, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1561, \"height\": 1062, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1615/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 800, \"height\": 283, \"label\": \"Table\"}]"
motivation: 当前S2S评测缺乏对副语言线索的评估，如韵律和情绪。
method: 设计四级交互协议和两阶段数据构造流程，构建语音原生基准。
result: 基准能系统揭示S2S模型在副语言表达上的能力差异。
conclusion: 为语音交互AI提供更全面的评估维度，促进拟人化对话发展。
---

## Abstract
Recent advances in large language models (LLMs) have fundamentally reshaped speech-to-speech (S2S) systems, enabling increasingly natural spoken interaction. However, existing benchmarks still rely heavily on text-based evaluation and largely ignore paralinguistic cues such as prosody, emotion, and speaker traits, which are central to expressive and human-like communication. We introduce S2S-Arena, a speech-native benchmark for evaluating instruction-following S2S models with explicit assessment of both semantic understanding and paralinguistic expression. S2S-Arena features a four-level interaction protocol that systematically probes models under increasing paralinguistic complexity, a two-stage data construction pipeline that produces 1,243 speech samples spanning 100+ real-world tasks, and an arena-style evaluation framework that enables reference-free, pairwise comparison directly in the speech modality. Benchmarking 10 state-of-the-art S2S systems over 1,000+ comparisons reveals substantial performance gaps (especially under complex paralinguistic demands) between current academic and industrial systems. Our analysis further identifies key design factors governing expressive instruction following, providing actionable insights for building more natural, robust, and human-aligned speech agents.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的结构化分析总结。

### 1. 论文的核心问题与整体含义 (研究动机和背景)

*   **核心问题**：当前语音到语音（S2S）系统的评测基准存在重大局限，它们严重依赖基于文本的评估，忽略了韵律、情感、说话风格、说话人特征等副语言信息，而这些是拟人化和富有表现力沟通的核心。
*   **研究动机**：随着大语言模型（LLM）的发展，S2S系统变得更加自然，但评估方法未能跟上。需要一个新的基准，能够直接在语音模态中、同时评估**语义理解**和**副语言表达**两方面的指令遵循能力。
*   **整体含义**：本文旨在通过S2S-Arena基准，将S2S模型的评估从转录层面的正确性提升到交互层面的人类对齐，为构建更自然、更稳健的语音智能体提供诊断工具和行动指南。

### 2. 论文提出的方法论 (核心思想与技术细节)

S2S-Arena的设计围绕三个紧密耦合的组件：

*   **四级交互协议 (Four-Level Interaction Protocol)**：系统地将语音交互分解为四个难度递增的级别，以模拟副语言复杂度的提升：
    *   **L1 (仅指令)**：纯语义理解与执行，不考虑副语言因素。
    *   **L2 (副语言感知)**：需要从输入语音中感知副语言线索（如年龄、情绪）来调整回复内容，但输出保持中性。
    *   **L3 (副语言表达)**：输入是语义中性的，但指令要求模型在输出语音中表达特定的副语言属性（如不同的语速）。
    *   **L4 (全副语言交互)**：模型需同时感知输入中的副语言线索，并生成具有相应表达的语音输出。
*   **两阶段数据构建流程 (Two-Stage Dataset Construction)**：
    *   **第一阶段 (人工种子数据)**：专家手工为19个任务设计脚本，并通过配音或高质量语料库录制，涵盖不同难度级别。最终产出293个高质量种子样本。
    *   **第二阶段 (自动化自指令增强)**：使用GPT-4o，基于种子数据采用**5-shot自指令策略**生成新脚本。脚本通过可控TTS系统（如Doubao-TTS, Parler-TTS）合成为语音，为每个任务额外生成50个样本，共950个样本。最终基准包含**1,243个音频查询样本**。
*   **竞技场式评估框架 (Arena-style Evaluation)**：
    *   **语音原生评判**：与以往基于文本的评判不同，评估直接在语音上进行。将任务指令音频和两个待评估模型响应音频拼接成一个音频片段，交由评判模型进行评估。
    *   **无参考成对比较**：采用Gemini 2.5-Pro作为自动裁判（经实验证实与人类判断高度一致，Cohen’s κ = 0.6553），进行“赢/输”二元判断，不设平局。
    *   **Elo评分更新**：所有模型初始评分为1000。使用标准的Elo公式更新评分：预期得分 \(E_A = \frac{1}{1 + 10^{(R_B - R_A)/400}}\)，评分更新为 \(R'_A = R_A + K(S_A - E_A)\)，其中K固定为32。
    *   **模型配对策略**：为避免无效比较，偏向采样Elo评分差距适中的模型对，其权重计算公式为 \(w_{ij} = \exp\left(-\frac{(\Delta_{ij} - \mu)^2}{2\sigma^2}\right)\)，使Elo分收敛更快更稳。

### 3. 实验设计 (数据集、Benchmark与对比方法)

*   **数据集/场景**：
    *   **S2S-Arena基准**：包含1,243个语音样本，分为293个种子样本和950个增强样本。
    *   **四个应用领域**：教育、社交、娱乐和医疗咨询，共覆盖100多个真实世界任务。
*   **对比方法 (10个SOTA S2S系统)**：
    *   **工业界模型**：GPT-4o-realtime, Doubao, FunAudioLLM, GLM-4-Voice, Qwen2.5-Omni, Kimi-Audio。
    *   **学术界模型**：SpeechGPT, Mini-Omni, Mini-Omni2, LLaMA-Omni。
*   **评估流程**：
    1.  **初步研究**：在种子数据集上，由19位人工标注员进行432次成对比较，验证了自动裁判Gemini 2.5-Pro与人类判断的可靠性。
    2.  **大规模评估**：在增强数据集上，使用Gemini 2.5-Pro对10个模型进行了超过1001次成对比较，并计算其Elo评分与胜率。
    3.  **深入分析**：从任务类别（四个领域）和任务难度（L1-L4）两个维度剖析模型表现。

### 4. 资源与算力

*   文中提到，对于本地执行的模型（除开通过API访问的工业模型），推理过程使用**单个NVIDIA A6000 GPU (48GB显存)** 完成。
*   未提及模型训练所需的算力。

### 5. 实验数量与充分性

*   **实验数量**：
    *   **1项初步研究**：人工评估432次成对比较，验证自动评估可靠性。
    *   **1项主评估实验**：在增强数据集上进行1001次成对比较，生成Elo排名。
    *   **2项深入分析**：按任务领域和任务难度级别对模型性能进行拆解分析。
    *   **1项统计可靠性验证**：在主评估实验上进行了1000次引导重采样，以确认排名的稳健性。
*   **充分性与公平性**：
    *   **充分性**：实验设计非常充分。多层次、多维度的分析（整体排名、领域分析、难度分析）和统计验证，为结论提供了坚实支撑。
    *   **公平性**：为确保公平，所有输入音频统一重采样至模型所需格式；同时对比了工业界和学术界模型；API和本地模型均采用官方代码，形成了客观、公正的比较环境。

### 6. 论文的主要结论与发现

*   **性能鸿沟显著**：工业界系统（Qwen 2.5-Omni、GPT-4o-realtime、Doubao）全面领先学术界系统（LLaMA-Omni最佳），平均Elo分差高达近200点，且此差距在复杂副语言任务（L3/L4）下急剧扩大至300点以上。
*   **领域决定能力**：没有模型在所有领域均占优，呈现明显的领域特异性。例如，GPT-4o-realtime在知识和事实性要求高的教育、医疗领域最强，而Qwen 2.5-Omni和FunAudioLLM在注重表达的社交、娱乐领域更出色。
*   **架构因素分析**：揭示了关键设计因素对副语言表现的影响。例如，更强的LLM骨干模型有助于指令遵循，流匹配（Flow Matching）语音解码器对L4级别的表达生成至关重要，而向量量化（VQ）并未显示出明显优势。
*   **竞争力分化**：即使在工业界模型内部，竞争力也体现在不同维度（最高的Elo分、最多的胜场数、最高的胜率），而非单一主导维度。

### 7. 优点 (方法或实验设计亮点)

*   **评估模态原生性**：直接在语音模态中进行评估，完整保留副语言信息，这是对现有依赖文本评估方法的根本性改进。
*   **层次化评估框架**：提出的四级交互协议，清晰界定了从纯语义到全副语言交互的难度演进，为诊断模型能力的薄弱环节提供了精细化的工具。
*   **高度可复现与可扩展**：两阶段数据构建法，结合人工保证质量与自动化保证规模，为未来持续扩充基准提供了可行路径。
*   **竞技场式评估**：采用Elo评分的成对比较方式，避免了固定参考答案的局限，能更灵活地比较不同风格模型的优劣，并通过配对策略提高了评估效率。

### 8. 不足与局限 (实验覆盖、偏差风险、应用限制)

*   **数据分布偏差**：自动构建部分使用了高质量TTS合成语音，可能导致评估结果偏向于与此类数据分布更适配的模型。
*   **交互复杂度局限**：基准目前主要针对**话语级和短程交互**，未能捕捉长期对话中的角色一致性、长期情感动态或篇章级连贯性等高级现象。
*   **数据集规模**：尽管设计精巧，但与真实世界口语交互的巨大多样性相比，1,243个样本的规模仍然相对有限。
*   **语言与文化覆盖**：样本主要为英语（75.79%）和中文（24%），少量日语和印地语，其结论对不同语言和文化的泛化能力有待验证。

（完）
