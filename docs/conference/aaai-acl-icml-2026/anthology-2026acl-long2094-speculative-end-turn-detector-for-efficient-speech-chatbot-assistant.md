---
title: Speculative End-Turn Detector for Efficient Speech Chatbot Assistant
title_zh: 面向高效语音聊天助手的推测式话轮结束检测器
authors: "Hyunjong Ok, Suho Yoo, Jaeho Lee"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.2094.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 面向对话式语音助手的话轮结束检测
tldr: 在构建自然口语对话系统时，准确检测用户是否话轮结束是一个关键难题，现有语音助手常因误判导致交互卡顿或抢话。针对此问题，本文提出推测式话轮结束检测器SpeculativeETD，通过协同推理框架在语音理解过程中提前预测话轮边界，并发布首个公开ETD数据集OpenETD，涵盖多样化的合成与真实语音。实验结果显示，该方法显著提升了检测准确率和推理效率，从而改善语音助手的响应流畅度，对促进语音用户界面和对话式AI的设计与落地具有重要实践意义。该研究首次为ETD提供公开基准，对学术和工业界推动更智能的语音交互技术具有重要意义。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2094/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 808, \"height\": 301, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2094/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1653, \"height\": 750, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2094/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1642, \"height\": 561, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2094/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 644, \"height\": 444, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2094/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 639, \"height\": 487, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 540, \"height\": 217, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1210, \"height\": 886, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1209, \"height\": 912, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1583, \"height\": 287, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 810, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 656, \"height\": 289, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 803, \"height\": 239, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 813, \"height\": 480, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 808, \"height\": 206, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 808, \"height\": 133, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 815, \"height\": 431, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 813, \"height\": 646, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 805, \"height\": 168, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 795, \"height\": 590, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2094/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 795, \"height\": 329, \"label\": \"Table\"}]"
motivation: 在口语对话系统中，准确检测话轮结束至关重要，但现有方法难以区分话轮结束与思考停顿，导致交互不流畅。
method: 本文提出SpeculativeETD推测式协同推理框架，利用早期检测线索提前判断话轮边界，平衡效率与准确度，并构建OpenETD数据集。
result: 发布首个公开ETD数据集，包含合成和真实语音；实验表明该方法有效减少误触发与响应延迟，提升对话流畅性。
conclusion: 该工作为语音助手交互设计提供了有效的话轮管理方案，对提升对话AI自然度有重要价值，奠定了基础数据集。
---

## Abstract
Spoken dialogue systems powered by large language models have demonstrated remarkable abilities in understanding human speech and generating appropriate spoken responses.However, these systems struggle with end-turn detection (ETD)—the ability to distinguish between user turn completion and hesitation. This limitation often leads to premature or delayed responses, disrupting the flow of spoken conversations.In this paper, we introduce the OpenETD Dataset, the first public dataset for end-turn detection. The OpenETD dataset consists of both synthetic speech data generated with text-to-speech models and real-world speech data collected from web sources. We also propose SpeculativeETD, a novel collaborative inference framework that balances efficiency and accuracy to improve real-time ETD in resource-constrained environments. Our approach jointly employs a lightweight GRU-based model, which rapidly detects the non-speaking units in real-time on local devices, and a high-performance Wav2vec-based model running on the server to make a more challenging classification of distinguishing turn ends from mere pauses. Experiments demonstrate that the proposed SpeculativeETD significantly improves ETD accuracy while keeping the required computations low.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将以Markdown形式，对这篇论文进行结构化、深入、客观的总结。

### **1. 论文的核心问题与整体含义 (研究动机和背景)**

*   **核心问题**: 当前由大语言模型（LLM）驱动的语音对话系统在 **话轮结束检测 (End-Turn Detection, ETD)** 上面临关键挑战。系统难以准确地区分用户是 **“话轮结束 (Turn Completion)”** 还是 **“短暂犹豫/停顿 (Hesitation/Pause)”**。
*   **问题影响**: 这种误判常导致语音助手过早或延迟回应，从而打断对话的自然流程，严重影响用户体验。
*   **现有瓶颈**:
    *   缺乏用于训练和评估ETD模型的**公开可用数据集**。现有研究大多依赖私有或付费数据，限制了研究的可复现性和社区的共同进步。
    *   高精度的ETD模型（如基于Transformer的Wav2vec 2.0）计算成本高昂，难以在**资源受限的设备端**实现实时、高频的推理。

### **2. 论文提出的方法论 (核心思想、关键技术细节)**

论文贡献分为数据集构建和新方法提出两部分。

#### **2.1 OpenETD数据集**

这是首个专为ETD任务设计的公开数据集，旨在填补领域空白。它将语音流的每个时间点定义为三种状态之一：
*   **Speaking Unit (SU)**: 说话中。
*   **Pause (停顿)**: 非说话状态，但说话者意图继续发言。
*   **Gap (话轮结束)**: 非说话状态，且标志着说话者话轮结束。

数据来源包括：
*   **合成数据**: 基于MultiWOZ文本对话数据集，使用TTS模型（Google Cloud Text-to-Speech）生成。为了模拟真实对话中的犹豫，人为地插入了三种变化。
    1.  **基础版 (Base)**: 不插入任何停顿。
    2.  **插入停顿 (w/ Pause)**: 随机将TTS自然产生的微停顿延长为符合Erlang分布的暂停。
    3.  **插入填充词 (w/ Filler Words)**: 在文本中随机注入“um”、“uh”等填充词后，再用TTS生成并在其后添加停顿。
*   **真实数据**: 从YouTube播客和Buckeye语音语料库中收集真实双人对话，并使用说话人日志化工具（`pyannote.audio`）进行说话人分割和状态标注。

#### **2.2 SpeculativeETD (推测式话轮结束检测)**

这是一种**协同推理框架**，旨在平衡ETD的效率和准确度，其灵感部分来源于大语言模型的推测性解码（Speculative Decoding）。

*   **核心思想**: 采用一个“小而快”的端侧模型和一个“大而准”的云端模型，通过分阶段、非对称的任务分工来降低整体计算负载。
*   **技术流程 (两阶段推理)**:
    1.  **端侧阶段 (On-device)**:
        *   部署一个**轻量级GRU模型**（约202K参数），在本地设备上实时处理流式音频帧（每100ms一个chunk）。
        *   该模型执行一个相对简单的**二分类任务**：判断当前状态是“说话中 (SU)”还是“非说话中 (Non-SU, 包括Pause和Gap)”。
        *   一旦模型检测到**非说话中 (Non-SU)** 的状态持续时间超过200ms，就将该段静音前的累积音频发送至服务器。
    2.  **云端阶段 (Server-side)**:
        *   部署一个**高性能Wav2vec 2.0模型**（约94M参数）。
        *   该模型只在接收到端侧查询时才被触发，执行一个更困难的**细粒度二分类任务**：判断该段“非说话中”的静音是“话轮结束 (Gap)”还是“犹豫停顿 (Pause)”。
        *   若判断为Gap，则触发语言模型生成回应。

*   **优势**:
    *   计算量大的云端模型仅在每次出现静音时才执行一次推理，而非对每个时间帧都运行，极大节省了计算资源（如图4和表6所示，FLOPs降低了38倍）。
    *   端侧和云端之间的通信是异步的、稀疏的，不再需要持续的实时数据流，减轻了网络压力。

### **3. 实验设计 (数据集、基准、对比方法)**

*   **数据集与场景**: 在提出的 **OpenETD数据集** 上进行训练和评估，包含合成和真实两部分，并按对话级别划分为训练/验证/测试集。
*   **评估任务**:
    1.  **二分类 (离线)**: 给定一段语音片段，判断其结尾是Gap还是Pause (仅评估Wav2vec2-level的分类能力)。指标为精确率、召回率、F1和准确率。
    2.  **实时音频分割 (在线)**: 在流式音频中，每100ms判断当前状态是SU、Pause还是Gap。这是评估完整SpeculativeETD框架的主任务。指标为**F1分数**和**交并比 (IoU)**。
*   **对比方法 (Benchmarks)**:
    *   **VAP (Voice Activity Projection)**: 一个流行的开源预训练话轮转换模型，冻结其编码器并在OpenETD上训练预测头。
    *   **GRU**: 从头训练的轻量级GRU模型，作为端侧模型的基准。
    *   **Wav2vec 2.0**: 对整个94M参数的模型进行微调，作为性能上限（Oracle）基准。
    *   **SpeculativeETD (Ours)**: 本文提出的GRU+Wav2vec 2.0协同框架。

### **4. 资源与算力**

*   **训练硬件**: 所有实验在单GPU上完成，部分实验使用**NVIDIA L40S**，部分使用**NVIDIA RTX 6000 Ada**。
*   **推理延迟测试**: 使用 **iPhone 12 mini** 的**A14仿生芯片**，在端侧部署并测量了GRU模型的**LiteRT**推理延迟。云端模型的延迟则是在服务器端测量。
*   **训练细节**: 使用AdamW优化器，训练10个epochs，并报告了各模型（GRU, VAP, Wav2vec 2.0）经过随机搜索得到的最优超参数（如学习率、批大小）。

### **5. 实验数量与充分性**

实验设计较为全面和严谨。
*   **主要实验组别**:
    *   **任务维度**: 对比了离线二分类和在线实时分割两种任务设定。
    *   **数据维度**: 在合成数据集和真实数据集上分别评估了所有模型的性能，并提供了混合训练与单一数据源训练的消融对比（附录 Table 9）。
    *   **性能维度**: 全面对比了准确率、效率（FLOPs）和推理延迟。
    *   **分析维度**: 包含了对合成数据与真实数据之间域差异的深入统计分析（SNR、语速、情感分布等），并通过人工验证评估了自动标注的可靠性。
*   **充分性与公平性**: 实验数量充足，覆盖了多个模型、任务和数据维度。对比方法选择合理，包含了轻量级和重量级模型，并为SpeculativeETD设置了清晰的性能上限（Wav2vec 2.0）和下限（GRU）。所有模型在相同的数据集和设定下进行训练和评估，确保了公平性。

### **6. 论文的主要结论与发现**

1.  **数据集价值**: 发布的OpenETD数据集有效填补了ETD研究的空白，并且混合使用合成数据和真实数据进行训练，能获得最佳的模型性能，证明了合成数据可作为一种有效的数据增强手段。
2.  **方法有效性**: SpeculativeETD框架成功地在**准确率和计算效率之间取得了优越的平衡**。
    *   它在合成数据集上的实时分割性能（F1: 94.0， IoU: 88.9）**接近**计算量巨大的Wav2vec 2.0模型（F1: 94.7， IoU: 90.2），并在真实数据集上显著超越了VAP和GRU。
    *   同时，其总计算量（MFLOPs: 919.64）仅为Wav2vec 2.0（MFLOPs: 34,971.68）的 **1/38**。
3.  **部署可行性**: 端侧GRU模型的执行延迟小于1ms/100ms，结合经过实测的网络传输延迟，整个框架的响应时间完全满足实时对话对200ms话轮转换阈值的需求，证明了其实际部署的可行性。

### **7. 优点**

*   **新颖且高效的方法论**: SpeculativeETD的分阶段、非对称协同推理框架设计巧妙，有效解决了端侧AI模型“小而不准”和云端模型“准而太慢”的矛盾，具有高度的工程实用性。
*   **填补领域空白的贡献**: OpenETD是首个公开的、大规模的ETD数据集，为社区的进一步研究和公平比较提供了宝贵的基准。
*   **工程实现细节扎实**: 论文不仅提供了算法，还深入探讨了数据生成与清洗（插入停顿、语言过滤）、端侧延迟测量、网络延迟分析等实际部署问题，完整性高。
*   **全面的评估**: 实验设计不仅关注单一精度指标，还对计算效率、延迟等部署关键因素进行了详细分析和可视化，论证充分。

### **8. 不足与局限**

*   **语言限制**: OpenETD数据集目前仅包含英语对话，其在其他语言（尤其是话轮转换文化差异较大的语言）上的通用性有待验证。
*   **合成数据的真实度差距**: 虽然统计分析证明了合成数据作为增强的有效性，但其在停顿位置、信噪比、情感表达丰富度等方面与真实数据仍存在明显差距（附录 Table 11），可能无法完全捕捉自然口语的复杂性。
*   **真实数据标注噪声**: 真实数据的标注是基于自动说话人日志化工具和200ms规则生成的，人工验证显示Gap标签准确率（76.1%）低于Pause标签（94.0%），这意味着训练和评估标签存在一定的噪声。
*   **网络依赖性**: 尽管测试了网络延迟，但框架的性能仍依赖于将音频发送到云端进行细粒度分类，在网络不稳定或高延迟环境下可能会影响最终响应速度。
*   **隐私考量**: 尽管论文将云端推理设计为可降低隐私风险（仅传输静音片段），但在实际应用中，将用户语音片段发送至服务器的隐私问题仍是需要严肃考量的因素。

（完）
