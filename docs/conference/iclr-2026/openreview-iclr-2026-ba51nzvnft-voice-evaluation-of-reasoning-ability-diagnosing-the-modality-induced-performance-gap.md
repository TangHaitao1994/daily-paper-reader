---
title: "Voice Evaluation of Reasoning Ability: Diagnosing the Modality-Induced Performance Gap"
title_zh: 语音推理能力评估：诊断模态引起的性能差距
authors: "Yueqian Lin, Zhengmian Hu, Qinsi Wang, Yudong Liu, Hengfan Zhang, Jayakumar Subramanian, Nikos Vlassis, Hai Helen Li, Yiran Chen"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=bA51NZVNFT"
tags: ["query:speech-model"]
score: 8.0
evidence: 用于评估语音交互系统推理能力的基准，揭示模态差距
tldr: 提出VERA基准，专门用于评估语音交互系统在实时对话约束下的推理能力。通过改编文本基准构建2931个语音原生试题，涵盖数学、网络等五个领域。评估12个现代语音系统后发现，文本与语音之间存在显著的模态性能差距，为语音AI的优化提供了明确方向。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音交互系统评估主要改编文本基准，未充分考虑语音特有挑战和实时交互约束。
method: 构建VERA基准，包含2931个源自文本基准的语音原生片段，涵盖五个推理领域。
result: "在主流语音系统上发现显著的文本-语音模态差距，如数学竞赛准确率从74.8%大幅下降。"
conclusion: 该基准为分析语音交互系统的推理可靠性提供了诊断工具，促进语音AI设计优化。
---

## Abstract
We present Voice Evaluation of Reasoning Ability (VERA), a benchmark for evaluating reasoning ability in voice-interactive systems under real-time conversational constraints. VERA comprises 2,931 voice-native episodes derived from established text benchmarks and organized into five tracks (Math, Web, Science, Long-Context, Factual). Each item is adapted for speech interaction while preserving reasoning difficulty. VERA enables direct text–voice comparison within model families and supports analysis of how architectural choices affect reliability. We assess 12 contemporary voice systems alongside strong text baselines and observe large, consistent modality gaps: on competition mathematics a leading text model attains 74.8% accuracy while its voice counterpart reaches 6.1%; macro-averaged across tracks the best text models achieve 54.0% versus 11.3% for voice. Latency–accuracy analyses reveal a low-latency plateau, where fast voice systems cluster around ~10% accuracy, while approaching text performance requires sacrificing real-time interaction. Diagnostic experiments indicate that common mitigations are insufficient. Increasing "thinking time" yields negligible gains; a decoupled cascade that separates reasoning from narration improves accuracy but still falls well short of text and introduces characteristic grounding/consistency errors. Failure analyses further show distinct error signatures across native streaming, end-to-end, and cascade designs. VERA provides a reproducible testbed and targeted diagnostics for architectures that decouple thinking from speaking, offering a principled way to measure progress toward real-time voice assistants that are both fluent and reliably reasoned.

---

## 论文详细总结（自动生成）

# 论文详细总结：Voice Evaluation of Reasoning Ability: Diagnosing the Modality-Induced Performance Gap

## 1. 论文的核心问题与整体含义

- **研究动机**：当前语音交互系统的评估方法多直接改编自文本基准（如数学、科学问答），未充分考虑实时语音交互带来的特有挑战，如延迟约束、口语化输入、流式处理等，导致无法准确衡量系统在语音模态下的真实推理能力。
- **核心问题**：文本到语音的模态转换是否会导致明显的推理性能下降？不同语音系统架构（级联、端到端、本地流式）如何影响推理可靠性？是否存在系统性的“模态差距”？
- **整体含义**：论文旨在为语音推理能力提供统一的诊断基准，揭示模态差距的规模、成因与缓解难度，为下一代兼具流利度与推理可靠性的实时语音助手设计提供方向。

## 2. 论文提出的方法论

- **基准数据集构建**：
  - 提出 **VERA**（Voice Evaluation of Reasoning Ability）基准，包含 **2,931** 个“语音原生”片段。
  - 所有片段均改编自已有的文本推理基准，但针对语音交互进行了适配，同时保留原始推理难度。
  - 内容按推理类型分为五个子领域：
    - **数学（Math）**
    - **网络（Web）**
    - **科学（Science）**
    - **长上下文（Long-Context）**
    - **事实性（Factual）**
- **评测框架设计**：
  - 支持 **同一模型家族内的文本-语音一对一对比较**，直观量化模态差距。
  - 提供**延迟-准确性联合分析**，考察实时交互与推理质量的权衡。
- **诊断实验方法**（论文中进行的额外分析）：
  - **思考时间消融**：增加模型的“思考时间”（即推理前的等待或计算时间），观察其对语音准确性的影响。
  - **级联系统解耦**：将“推理”与“语音叙述”分离为两个独立阶段（先文本推理，再由语音复述），分析这种架构对准确性和一致性/接地（grounding）错误的影响。
  - **错误特征分析**：对比原生流式、端到端和级联设计下的错误模式。

## 3. 实验设计

- **评估对象**：
  - **12个当代语音交互系统**，覆盖主流的语音架构范式（如级联、端到端、本地流式部署）。
  - **强文本基线**，用于提供相同任务在纯文本条件下的性能上界。
- **评估场景**：基于 VER A 基准的五个子领域，所有评估均在实时对话约束下进行（低延迟要求）。
- **对比维度**：
  - 文本 vs. 语音模态的性能差距。
  - 不同语音系统架构之间的性能差异。
  - 解耦级联相对于原生语音系统的改进程度。
  - 不同延迟预算下的准确性变化。

## 4. 资源与算力

- **论文摘要中未提及任何算力信息**，包括 GPU 型号、数量、训练时长或推理硬件配置。全文细节可能需要查阅论文正文，但就所提供内容而言，无法总结算力情况。

## 5. 实验数量与充分性

- **主要实验组数**（由摘要可推知）：
  - 12个语音系统 × 5个推理子领域，共形成大量评测点。
  - 每个系统还涉及其文本对应版本的对比，进一步增加了实验组合。
  - 额外进行了至少两类诊断实验：思考时间消融、解耦级联分析。
  - 还包括延迟-准确性曲线的绘制及错误特征剖分。
- **充分性与公平性**：
  - 覆盖面较广：涵盖多个主流系统、多个推理领域，且提供统一的文本基线，比较基础公平。
  - 诊断维度深入：不仅报告现象（模态差距），还通过消融和架构变体分析原因，实验设计具有较强的诊断导向。
  - **潜在局限**：摘要未提及重复实验或统计检验，无法判断结果的稳健性；2931个样本的体量在语音评测中属于中等规模，可能对某些低资源领域（如长上下文）的泛化结论支撑有限。

## 6. 论文的主要结论与发现

- **模态差距巨大且一致**：
  - 在竞赛数学任务上，领先文本模型准确率 **74.8%**，其语音版本仅 **6.1%**。
  - 五个子领域的宏平均：最佳文本模型 **54.0%**，最佳语音系统 **11.3%**。
- **实时性显著制约推理质量**：
  - 低延迟语音系统普遍陷入 **~10% 左右的准确性平台期**（低延迟高原），继续压降延迟难以换取性能提升。
  - 要达到接近文本的性能，必须**牺牲实时交互**，打破低延迟要求。
- **常见缓解手段效果有限**：
  - 增加“思考时间”对语音准确性的增益可忽略不计。
  - 解耦的级联架构虽能提升准确率，但仍**远低于文本水平**，并引入典型的基础/一致性错误（如语音叙述与内部推理逻辑脱节）。
- **架构决定错误特征**：
  - 原生流式、端到端、级联设计各自表现出截然不同的错误签名，为针对性改进提供了诊断指引。
- **VERA 基准的价值**：可作为可复现的测试平台，系统衡量语音助手从“流利应答”迈向“可靠推理”的进展。

## 7. 优点

- **问题定位精准**：率先将“实时语音推理的模态差距”作为核心诊断主题，而非笼统的语音问答评价。
- **基准设计科学**：从文本权威基准改编而来，既保证了推理难度，又引入语音特有约束，且支持模态内直接对比，可复现性强。
- **分析深度突出**：不仅量化差距，更通过思考时间消融、架构解耦、错误剖分等手段探究根源，超越“榜单刷分”式评价。
- **现实指导意义**：明确指出了当前低延迟语音系统在复杂推理上近乎失效，并且简单工程技巧（加时、解耦）不足以弥合差距，可能引导社区重新审视语音推理的基本架构。

## 8. 不足与局限

- **提供的摘要未提及局限性**。基于已有信息可推导的潜在局限包括：
  - **基准覆盖**：所有题目均改编自文本基准，可能无法完全涵盖真实语音对话中特有的口语理解、犹豫修正、噪声环境等挑战。
  - **样本规模**：2931个样本在跨五个子领域后，每个子集的平均体量较小，可能影响细粒度结论的统计可靠性。
  - **系统代表性**：虽有12个系统，但不排除遗漏某些新兴模型或特定部署条件（如极低资源设备）下的评估。
  - **实时性定义**：延迟的具体阈值和约束条件未在摘要中明确，不同系统对“实时”的遵从程度可能存在差异，影响横向对比的严格性。
  - **语言限制**：未提及是否支持多语种、口音等，基准可能局限于英语单一设置。

（完）
