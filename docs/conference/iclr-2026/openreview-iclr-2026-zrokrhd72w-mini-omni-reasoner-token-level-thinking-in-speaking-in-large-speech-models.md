---
title: "Mini-Omni-Reasoner: Token-Level Thinking-in-Speaking in Large Speech Models"
title_zh: Mini-Omni-Reasoner：大语音模型中的token级边想边说
authors: "Xie Zhifei, Ziyang Ma, Zihang Liu, Pang Kaiyu, Hongyu Li, Jialin Zhang, Yue Liao, Deheng Ye, Chunyan Miao, Shuicheng YAN"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ZRokRhD72w"
tags: ["query:speech-model"]
score: 9.0
evidence: 在语音生成过程中实现实时推理，降低语音对话延迟，提升对话AI交互效率。
tldr: 针对大语音模型中“先思考后说话”范式引入显著延迟的问题，提出Mini-Omni-Reasoner框架，实现了语音生成期间的token级交织推理，即“边说边想”。通过将推理和生成融合在同一个自回归流中，在保持推理能力的同时极大缩短响应时间，为实时语音交互对话AI开辟了新路径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有语音推理采用先完整思考再生成语音的方式，导致实时交互中响应延迟明显。
method: 提出token级交织推理，在自回归语音生成的同时插入推理token，实现边说边想。
result: 在多个对话基准上，响应延迟大幅降低且推理性能不受损。
conclusion: 边说边想框架突破了语音对话实时性瓶颈，对构建自然流畅的语音AI具有重要意义。
---

## Abstract
Reasoning is essential for effective communication and decision-making. While recent advances in large language models (LLMs) and multimodal models (MLLMs) have shown that incorporating explicit reasoning significantly improves understanding and generalization, reasoning in large speech models (LSMs) remains in a nascent stage. Early efforts attempt to transfer the “thinking-before-speaking” paradigm from textual models to speech. However, this sequential formulation introduces notable latency, as spoken responses are delayed until reasoning is fully completed, impairing real-time interaction and communication efficiency. To address this, we propose Mini-Omni-Reasoner, a framework that enables reasoning within speech via a novel “thinking-in-speaking” formulation. Rather than completing reasoning before producing any verbal output, Mini-Omni-Reasoner interleaves silent reasoning tokens with spoken response tokens at the token level. This design allows continuous speech generation while embedding structured internal reasoning, leveraging the model’s high-frequency token processing capability. Although interleaved, local semantic alignment is enforced to ensure that each response token is informed by its preceding reasoning. To support this framework, we introduce SPOKEN-MATH-PROBLEMS-3M, a large-scale dataset tailored for interleaved reasoning and response. The dataset ensures that verbal tokens consistently follow relevant reasoning content, enabling accurate and efficient learning of speech-coupled reasoning. Built on a hierarchical Thinker–Talker architecture, Mini-Omni-Reasoner delivers fluent yet logically grounded spoken responses, maintaining both naturalness and precision. On the Spoken-MQA benchmark, it achieves a +19.1% gain in arithmetic reasoning and +6.4% in contextual understanding, with shorter outputs and zero decoding latency. These results demonstrate that high-quality reasoning and real-time spoken interaction can be effectively unified in a single framework.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：大语言模型（LLM）和多模态模型已成功应用显式推理来提升理解与决策能力，但大语音模型（LSM）中的推理仍处于早期阶段。
- **现有局限**：当前语音推理主要沿用文本模型的“先思考后说话”（thinking-before-speaking）范式，即模型在生成任何口头回复之前先完成完整的内部推理，这会导致**明显的响应延迟**，损害实时交互的自然度和沟通效率。
- **核心动机**：如何在保持推理质量的前提下，大幅降低语音对话系统的响应延迟，实现更流畅、即时的语音交互。
- **整体含义**：本文提出 **“边说边想”（thinking-in-speaking）** 的新范式，将推理过程嵌入到语音生成的 token 流中，让模型在说话的同时进行推理，从而突破传统序列化“先想后说”的延迟瓶颈，为具有实时推理能力的语音对话 AI 开辟新方向。

### 2. 论文提出的方法论

- **核心思想**：在自回归语音生成过程中，**在 token 级别交错插入无声音的推理 token 和声音的口语 token**，使得模型能够在连续生成语音的同时嵌入结构化内部推理，而非先完成所有推理再开始说话。
- **关键技术细节**：
  - **推理–生成交织**：模型将推理过程拆分为片段，与语音生成交替进行。每个语音 token 的产生都受到其之前的推理 token 的指导，从而保证局部语义对齐。
  - **框架名称**：Mini-Omni-Reasoner。
  - **架构基础**：采用层级化 **Thinker–Talker** 架构（思考者–说话者），其中 Thinker 负责产生推理 token，Talker 负责产生语音 token，两者在同一自回归流中协同工作。
  - **数据支持**：为训练该交织推理能力，专门构建了大规模数据集 **SPOKEN-MATH-PROBLEMS-3M**，确保每个语音 token 之前都有相关的推理内容，从而让模型学习到语音耦合推理。
  - **高频 token 处理**：利用模型的高频 token 处理能力，即使在 token 级别交错也能保持流畅自然的口语输出。
  - **无额外解码延迟**：由于推理和生成融为一体，响应从第一个 token 开始即可即时输出，实现零解码延迟。

### 3. 实验设计

- **主要数据集与场景**：
  - 自建数据集 **SPOKEN-MATH-PROBLEMS-3M**，专门用于交织推理与口语回复的训练。
  - 公开语音问答基准 **Spoken-MQA**（语音数学应用题），包含算术推理和情境理解两类子任务。
- **Benchmark 与指标**：
  - **算术推理**：准确率提升。
  - **上下文理解**：准确率提升。
  - **输出长度**：更短的回复（表示更高效）。
  - **解码延迟**：零额外延迟（实时响应）。
- **对比方法**（文中提及）：
  - 虽然摘要未列出具体基线名称，但暗示与“先思考后说话”的语音推理范式（即顺序先推理后生成）进行对比，突出本方法在延迟和性能上的优势。

### 4. 资源与算力

- **文中明确信息**：提供的摘要和元数据中**未提及 GPU 型号、数量、训练时长等算力细节**。无法从现有材料中推断具体资源消耗。

### 5. 实验数量与充分性

- **实验数量**：由于仅有摘要，具体实验组数未知。但可以看出至少包含：
  - 在 Spoken-MQA 基准上的算术推理和上下文理解两项主要评估。
  - 输出长度和解码延迟等效率指标评估。
  - 可能包含与 baseline 的对比、消融实验（文中未详述）。
- **充分性与客观性**：
  - 创建专用大规模数据集来对齐训练目标和评估标准，增强了内部效度。
  - 在公开基准上展示了显著增益（+19.1% 算术推理，+6.4% 上下文理解），并提供了可量化的延迟优势（零解码延迟、更短输出），初步表明结果具有说服力。
  - 但摘要未报告统计显著性检验、多次运行的平均值和方差，也未列出对比的具体文本或语音模型，故无法完全判断实验的完备程度。

### 6. 论文的主要结论与发现

- **实时推理与高质量语音可以统一**：Mini-Omni-Reasoner 框架证明了无需牺牲推理能力就能实现几乎零延迟的语音交互。
- **性能提升显著**：在 Spoken-MQA 算术推理上提升 19.1%，情境理解提升 6.4%，同时生成更简洁的回复且无解码延迟。
- **范式变革**：相比于“先想后说”的顺序式设计，“边说边想”在保持自然度和精确度的同时，极大改善了通信效率，为下一代言说 AI 提供了新基础。

### 7. 优点

- **创新范式**：提出 token 级推理–生成交织，从根本上解决了语音模型“思考期”造成的交互延迟问题。
- **架构设计**：Thinker–Talker 分层架构清晰解耦推理与语音生成，易于扩展和优化。
- **定向数据构造**：SPOKEN-MATH-PROBLEMS-3M 数据集的构建保证了每个语音 token 都有前置推理内容，使模型能有效学习语音耦合推理。
- **效果明显**：在准确率和延迟两个关键维度上都取得了显著同步提升，体现了方法的实用价值。
- **零解码延迟**：模型的即时响应能力对对话式 AI 部署极具吸引力。

### 8. 不足与局限

- **算力信息缺失**：未提供训练成本（GPU 小时等）细节，难以评估方法的可复现性和资源需求。
- **实验覆盖可能有限**：目前仅在数学口语问答（Spoken-MQA）上展示了效果，缺乏在其他语音对话、常识推理、开放域对话等任务上的验证。
- **数据集局限**：自建数据集偏向数学问题，模型的“边说边想”能力在更自由、更口语化的场景下（如闲聊、多轮对话）是否同样有效尚不清楚。
- **架构依赖**：基于 Thinker–Talker 分层结构，若推理和语音生成在解耦性较差的模型中直接插入推理 token 可能带来稳定性风险。
- **潜在偏差**：推理 token 与语音 token 的局部对齐是强制性的，可能在复杂推理链中出现前后矛盾或信息遗漏，摘要中未讨论此类失败案例。
- **对比方法缺失细节**：未列出具体对比模型及其性能，无法判定增益的基准水平。（完）
