---
title: "Dual-Reasoner: Bridging Interleaved Atomicity and Streaming Latency via Thinking-while-Talking"
title_zh: Dual-Reasoner：通过边说边想弥合交织原子性与流式延迟
authors: "Yangzhuo Li, Shengpeng Ji, Yifu Chen, Tianle Liang, Haoyu Yang, Junboli, Jun Fang, Lin Li, Qingyang Hong"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.199.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: 带思维链的口语对话模型，边说边想，降低延迟
tldr: 针对端到端口语对话模型集成思维链（CoT）导致高延迟的问题，本文提出Dual-Reasoner，采用流式掩码机制保证音频流不中断，并引入原子一致性恢复框架对齐碎片化思维块，实现边说边想。实验结果显示该模型在保持推理深度的同时大幅降低延迟，为高性能实时口语对话系统提供了可行方案。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.199/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1652, \"height\": 878, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.199/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1658, \"height\": 925, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.199/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 481, \"height\": 500, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.199/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1658, \"height\": 1140, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1658, \"height\": 562, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1660, \"height\": 357, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 805, \"height\": 227, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 806, \"height\": 506, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1652, \"height\": 402, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1657, \"height\": 500, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1649, \"height\": 516, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.199/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1647, \"height\": 654, \"label\": \"Table\"}]"
motivation: 口语对话集成CoT提升智能但延迟过高，现有边说边想方案损害思维原子性。
method: 提出流式掩码机制和原子一致性恢复，实现边推理边生成语音。
result: Dual-Reasoner平衡了推理深度与延迟，优于基线模型。
conclusion: 该框架为低延迟、高智力的口语对话AI设计提供了新范式。
---

## Abstract
Integrating explicit Chain-of-Thought (CoT) into end-to-end spoken dialogue models enhances intelligence but incurs prohibitive latency. While the "Thinking-while-Talking" paradigm alleviates this delay, it fundamentally compromises block atomicity, severing the logical connection between interleaved thought and speech. To address this, we present Dual-Reasoner, employing a Streaming Masking Mechanism underpinned by our Dual-Think-30k dataset to guarantee uninterrupted audio streaming. Crucially, to strictly align the fragmented thinking blocks to service speech generation, we introduce the Atomic-Consistency Restoration framework. To secure comprehensive capabilities in high-difficulty reasoning, this mechanism utilizes a quadruple-constraint system to reconstruct logical atomicity, ensuring that "think" chunks act as a rigorous anchor for "talk" outputs. Experimental results demonstrate that Dual-Reasoner achieves comprehensive reasoning enhancements within ultra-low latency constraints: it elevates the VoiceBench score from 67.24 to 73.41 over the baseline, while significantly reducing the Time-to-First-Audio (TTFA) from 20.35s to 3.65s and the Real-Time Factor (RTF) from 7.04 to 1.05.

---

## 论文详细总结（自动生成）

好的，这是对论文《Dual-Reasoner: Bridging Interleaved Atomicity and Streaming Latency via Thinking-while-Talking》的结构化深度总结。

### 1. 论文的核心问题与整体含义

-   **研究动机**：在端到端口语对话模型中集成思维链（CoT）可以显著提升模型的复杂推理能力和可解释性，但这带来了严重的实时交互延迟问题。
    -   传统的“先想后说”（Thinking-then-Talking）范式要求模型在开始生成语音前完成全部推理，导致长达数十秒的首次音频延迟（TTFA），违背了实时交互的需求。
    -   现有的“边说边想”（Thinking-while-Talking）方法虽然通过交织“思考块”和“说话块”来降低延迟，但依赖简单的固定长度截断，导致**块原子性**被破坏：思维块内部语义不完整，推理步骤之间逻辑脱节，引发幻觉和上下文遗忘。
-   **整体含义**：该研究旨在从根本上解决“边说边想”范式下**物理连续性**与**逻辑原子性**之间的冲突。其核心贡献在于提出一个全新的架构和训练框架，在严格保证低延迟、不间断音频流的前提下，通过重构交织块之间的逻辑一致性，实现与“先想后说”模式相媲美的深度推理能力。

### 2. 论文提出的方法论

论文提出的 **Dual-Reasoner** 架构由两个正交的核心支柱构成，分别解决物理层和认知层的难题。

-   **核心思想**：将“边说边想”定义为一个**结构重建问题**，目标是在超低延迟交互的限制下，重建交织生成过程中的逻辑原子性。
-   **物理连续性支柱：Dual-Reasoner架构与流式掩码机制**
    -   **生成范式**：模型生成交替的思考块（`<think>`标签包裹）和语音块（`<tts-pad>`标签，文本与声学令牌比例为1：4）。
    -   **流式掩码机制**：为解决音频缓存区欠载的问题，论文提出了一个物理约束不等式：
        `D(Pt-1) + L(TTFTt-1) > L(Thinkt + Talkt + TTFTt)`
        （前一播放块的时长与解码延迟之和）必须大于（当前思考与语音的生成总延迟）。该不等式确保了当前一段音频播放完毕的“关键连续点”，下一段音频已经准备就绪，实现了无缝流式传输。
    -   **令牌预算**：基于经验数据（生成速度45 tok/s，声学令牌播放时长0.04s），该不等式被简化为一个线性关系：**思考令牌数量必须少于语音令牌数量的一半**（`Nthink < 0.5 * Ntalk`）。论文进一步通过实验优化绝对长度，选择了配置 `(Nthink=60, Ntalk=125)`。
-   **逻辑原子性支柱：原子一致性恢复（ACR）框架**
    -   该框架应用于 GRPO 强化学习阶段，旨在将一个物理上碎片化的生成过程，在认知上统一起来。它通过一个**四重约束奖励系统**来评估和优化交织轨迹`C = {Block1, Block2, ...}`，总奖励是四个奖励的加权和：`Rtotal = wconst·Rconst + wcomp·Rcomp + wflow·Rflow + wout·Rout`。
    -   **四重约束奖励**:
        1.  **语义完整性奖励（`Rcomp`）**：确保单一块的内部信息是自洽和完整的，避免产生“开环”或截断的片段。
        2.  **内外一致性奖励（`Rconst`）**：强制“说话块”的内容逻辑严格地锚定在之前的“思考块”中，惩罚“说话走在思考前面”的幻觉增量。
        3.  **稀疏逻辑流奖励（`Rflow`）**：通过随机采样非相邻的块对 (`Bi, Bj`)，检测长距离的逻辑矛盾（例如变量值在未被操作的情况下发生突变），保证全局推理链路的连贯性。
        4.  **多模态结果奖励（`Rout`）**：作为最终质量把关，验证文本答案的准确性和合成语音的自然度与情感恰当性。
-   **算法流程**：采用 GRPO 算法，模型对每个输入采样 G 条轨迹，并用组合 ACR 奖励进行评分。通过组内归一化计算优势函数 `Â(k)`，并最大化带裁剪和 KL 惩罚的策略目标函数来更新模型参数。
-   **数据基础：Dual-Think-30k 数据集**
    -   **构建策略**：从五个语料库（OpenOrca， Competition Math， APPS， MedMCQA， VoiceAssistant-400K）中整合数据，并引入 **R-P-T-S（角色-问题-任务-符号化）动态推理范式**。
    -   **快慢双系统**：对日常交互采用“快思考”（意图锁定），对复杂逻辑任务采用“慢思考”（R-P-T-S架构，要求在首个块中输出全局导航标志，实现“先定调，后推理”）。
    -   **构造流程**：通过难度分级、数学公式口语化重写和 TTS 合成及 ASR 质量过滤（WER < 5%）等步骤构建了约31，567个样本的高质量语音 CoT 数据集。

### 3. 实验设计

-   **数据集/场景与基准**：论文在三个主流基准上进行了评估：
    -   **VoiceBench**: 用于评估全面交互能力，包括对话、问答、指令遵循、安全性等。
    -   **MMAR**: 一个深度音频推理基准，包含单模态（声音、音乐、语音）和混合模态任务。
    -   **GSM8K**: 用于评估数学推理能力。
-   **对比方法**：对比了三类方法：
    -   **自身变体（双基线策略）**：无思考的 `Step-Audio-2-Mini-Base`（作为性能基线）和串行思考的 `Step-Audio-2-mini-Think`（作为延迟基线）。
    -   **大型音频语言模型**：Qwen2-Audio, SALMONN, GLM-4-Voice, VITA-1.5, MiniCPM-o， Qwen2.5-Omni-7B， Kimi-Audio。
    -   **大型音频推理模型**：Audio-Reasoner， Audio-CoT， Mellow。
    -   **全模态模型**：Qwen-2.5-Omni, Baichuan-Omni-1.5。

### 4. 资源与算力

-   **硬件配置**：论文明确提到实验使用了 **8 块 NVIDIA H800 GPU** 组成的集群。
-   **软件环境**：Ubuntu 22.04操作系统，PyTorch 2.6.0+cu124， Transformers 4.49.0， CUDA 12.4。
-   **训练阶段**：采用三阶段课程学习，包括两阶段监督微调和一阶段 GRPO 强化学习。具体训练时长未明确说明。

### 5. 实验数量与充分性

-   **实验数量**：实验设计较为全面，包括：
    -   **主实验**：在至少 3 个大规模基准数据集（VoiceBench， MMAR, GSM8K）上进行了性能评估。
    -   **延迟评估**：对 TTFA 和 RTF 指标进行了详尽的、跨数据子集的测试。
    -   **消融实验**：进行了多维度、分层的消融实验，以验证各组件的有效性：
        1.  **ACR 框架组件消融**：分别移除了 ACR 体系中的各个奖励（完整性、一致性、流、多模态）以验证其独立贡献。
        2.  **奖励权重分配消融**：测试了多种奖励权重组合的影响。
        3.  **块粒度优化消融**：比较了不同的（思考令牌数， 语音令牌数）配置的效果。
        4.  **训练策略消融**：分析了 SFT 阶段对控制令牌施加特殊惩罚权重的影响。
-   **充分性与公平性**：
    -   **充分性**：实验覆盖了从对话到复杂推理的多个维度，消融实验也涵盖了方法论的核心环节，对提出的物理和逻辑机制均有验证，可以说比较充分。
    -   **公平性**：采用了“双基线”对比策略，同时与自己模型的非推理和串行推理版本比较，清晰地隔离了性能和延迟的 trade-off，对比设计公平且具有说服力。与外部多种类型 SOTA 模型的比较也增强了结果的客观性。

### 6. 论文的主要结论与发现

-   **性能与延迟的卓越平衡**：Dual-Reasoner 在 VoiceBench 上得分 **73.41**，超越性能基线 Step-Audio-2-Mini-Base（67.24），接近计算量更大的串行思考变体（73.80）。同时，TTFA 从 20.35s 降至 **3.65s**，RTF 从 7.04 降至 **1.05**，接近实时。
-   **逻辑完整性的有效重建**：ACR 框架成功地解决了模式切换带来的上下文遗忘问题，其在指令遵循任务（IFEval， 40.62）上的表现显著优于串行思考变体（34.10）。
-   **复杂推理能力保持**：在 MMAR 基准上，Dual-Reasoner 相比自身基线和外部 LARM 模型均有显著提升，表明其并未因低延迟设计而牺牲深层推理能力。
-   **全局逻辑流是核心**：消融实验揭示，**稀疏逻辑流奖励**对模型性能的影响最大（移除后性能下降 11.1%），说明在流式架构中，维持长距离逻辑连贯性是最关键的因素。
-   **非推理任务的鲁棒性**：在不需要深度推理的简单问答任务上，模型也表现出色，验证了其“快慢系统”切换的有效性，避免了不必要的过度推理。

### 7. 优点

-   **创新性的问题定义**：不仅关注降低延迟，更深刻洞察并定义了“边说边想”中逻辑原子性被破坏这一根本问题。
-   **优雅的双层架构设计**：将问题分解为物理连续性（流式掩码）和逻辑原子性（ACR）两个正交维度，方法论清晰且极具创新性。
-   **新颖且严谨的训练框架**：ACR 框架通过四重约束，从块内语义到全局逻辑进行了系统性重建，超越了简单的端到端奖励聚合。GRPO 算法的应用也紧跟前沿。
-   **高质量的数据集构建**：Dual-Think-30k 数据集的“R-P-T-S”范式和“快慢系统”设计，为流式推理任务提供了宝贵的数据基础，考虑到了语调、情感等口语特性。
-   **实验设计全面且具有说服力**：双基线对比策略和细致的消融实验，强有力地证明了方法的有效性。

### 8. 不足与局限

-   **依赖闭源商业模型**：ACR 的奖励信号和 Dual-Think-30k 的构造，高度依赖 GPT-4o 和 Qwen-235B-Thinking 等外部高性能模型。这不仅引入了较高的训练成本，也可能导致模型的推理能力天花板受限于教师模型，且限制了快速迭代和社区复现。
-   **数据集覆盖范围有限**：数据主要来自策划好的语料库，与现实世界中充满环境噪音、口语不流利现象、非结构化语境的真实对话分布存在差距。在此类复杂真实场景下的鲁棒性有待验证。
-   **绝对延迟未达理想实时**：虽然 TTFA 大幅降低，但 3.65 秒的首次音频延迟在一些对延迟极度敏感的应用场景（如语音助手唤醒后的即时反馈）中可能仍然偏高。
-   **块长度配置为固定值**：论文对思考块和语音块的令牌长度进行了离散优化并选择了固定配置（60，125），这可能不是对所有任务类型都是最优的，缺乏动态调整能力。

（完）
