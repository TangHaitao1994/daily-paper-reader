---
title: Closing the Modality Reasoning Gap for Speech Large Language Models
title_zh: 缩小语音大模型的模态推理差距
authors: "Chaoren Wang, Heng Lu, Xueyao Zhang, Shujie Liu, Yan Lu, Jinyu Li, Zhizheng Wu"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.857.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 使用强化学习对齐语音和文本轨迹以缩小语音大模型的推理差距
tldr: 语音大模型在语音输入上的推理能力明显弱于文字。本文提出TARS框架，通过非对称奖励的强化学习，对齐语音和文本条件下的表示与行为轨迹。在多个推理基准上，该方法有效缩小模态推理差距，增强了语音模型的理解和逻辑能力。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.857/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1656, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.857/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1639, \"height\": 550, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.857/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 788, \"height\": 381, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.857/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 791, \"height\": 442, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1646, \"height\": 950, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1643, \"height\": 782, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 792, \"height\": 350, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 784, \"height\": 330, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 787, \"height\": 935, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 798, \"height\": 807, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 800, \"height\": 446, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 791, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1642, \"height\": 1036, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.857/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1643, \"height\": 873, \"label\": \"Table\"}]"
motivation: 语音大模型存在严重的模态推理差距，语音推理性能远落后于文本。
method: 提出基于强化学习的TARS框架，使用表示对齐和行为对齐两个稠密信号。
result: 在多项推理任务上显著提高了语音输入的推理准确率。
conclusion: TARS为语音大模型的推理能力提升提供了有效训练方法，促进了语音理解的发展。
---

## Abstract
Although Speech Large Language Models have achieved notable progress, a substantial modality reasoning gap remains: their reasoning performance on speech inputs is markedly weaker than on text. This gap could be associated with representational drift across Transformer layers and behavior deviations in long-chain reasoning. To address this issue, we introduce TARS, a reinforcement-learning framework that aligns text-conditioned and speech-conditioned trajectories through an asymmetric reward design. The framework employs two dense and complementary signals: representation alignment, which measures layer-wise hidden-state similarity between speech- and text-conditioned trajectories, and behavior alignment, which evaluates semantic consistency between generated outputs and reference text completions. Experiments on challenging reasoning benchmarks, including MMSU and OBQA, show that our approach significantly narrows the modality reasoning gap and achieves state-of-the-art performance among 7B-scale Speech LLMs.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将以 Markdown 格式，对这篇论文进行结构化、深入且客观的总结。

### 1. 核心问题与整体含义

*   **研究动机与背景**:
    *   **语音大模型的进展**：当前的语音大语言模型（Speech LLMs）大多采用“语音编码器 + 适配器 + 纯文本大语言模型”的三层架构，旨在让语音输入也能复用文本大语言模型强大的理解和推理能力。
    *   **核心痛点——模态推理差距**：尽管进展显著，但该类模型存在一个重大挑战——模态推理差距。即，模型在接收语音输入时的推理性能，远低于其接收纯文本输入时的性能。
    *   **问题根源推断**：论文将此差距归因于两点：1) **表征漂移**：语音输入在通过 Transformer 各层时，其隐藏状态逐渐偏离了对应的文本输入表征；2) **行为偏差**：在长链推理过程中，语音条件下的生成行为与文本条件下的逻辑产生了分歧。
    *   **整体含义**：本文旨在通过强化学习框架，直接对齐语音和文本两种模态下的内部表征和推理轨迹，从而系统性地缩小乃至弥合这个“模态推理差距”。

### 2. 方法论

*   **核心思想**：
    提出 **TARS (Trajectory Alignment for Reasoning in Speech)** 框架。其核心是在线强化学习过程中，除了优化任务准确度，还通过设计非对称的奖励函数，让语音条件下的模型生成轨迹向文本条件下的轨迹看齐，后者作为在线策略提供的动态参考。

*   **关键技术细节**：
    *   **非对称奖励设计**：为语音补全设计的完整奖励函数 `R_total` 由三部分组成：
        1.  **基础奖励**：`R_base = R_acc + λ * R_fmt`，鼓励正确答案和格式遵守。
        2.  **表征对齐奖励**：`R_rep`，计算语音生成轨迹和正确文本参考轨迹在每一层 Transformer 的池化隐藏状态之间的余弦相似度平均值。这是一个稠密信号，旨在缓解层级的表征漂移。
        3.  **行为对齐奖励**：`R_beh`，使用外部嵌入模型计算最终生成的语音文本与正确参考文本之间的语义向量余弦相似度。这是一个相对稀疏但精准的信号，确保输出结果的语义一致性。
        *   **信号互补性**：`R_rep` 和 `R_beh` 两者互补，联合从内部表征和外部行为两个层面进行对齐。此完整奖励仅应用于语音条件的补全，文本条件的补全仅用 `R_base` 优化。
    *   **优化算法与关键技巧**：
        *   **强化学习算法**：基于 **群组相对策略优化** 框架，并采用 **DAPO** 损失函数。
        *   **模态特定归一化**：为避免文本模态天然得分高导致语音补全的优势函数 `A_i` 恒为负，论文将文本和语音补全分在两个独立的组内进行奖励均值和优势计算。

### 3. 实验设计

*   **数据集与场景**：
    *   **训练数据**：使用 **UnifiedQA** 训练集，通过 TTS 系统（CosyVoice2, openaudio-s1-mini）合成语音，构建成对的语音-文本数据。最终有 `9,953` 个样本，约 `203` 小时。
    *   **评测基准**：
        1.  **多领域知识推理**：**MMSU**（来自 VoiceBench，3,074 个多选题）。
        2.  **常识推理**：**OBQA**（来自 VoiceBench，455 个多选题）。
        3.  **辅助诊断**：**LibriSpeech** 用于评估词错率，以确认推理提升非单纯来自语音识别改善。
    *   **基线方法**：
        1.  **级联与商业系统**：ASR + Llama3.1/Qwen2.5/Phi-4、GPT-4o-mini-Audio。
        2.  **跨模态对齐方法**：SALAD、DeSTA2.5-Audio、AlignChat、知识蒸馏。
        3.  **通用后训练方法**：监督微调、直接偏好优化、仅使用基础奖励的标准 GRPO。

### 4. 资源与算力

*   **算力配置**：
    *   **GPU**：训练使用 `4 × A100` 或 `8 × H200` GPU。
    *   **训练时长**：在 Qwen2.5-Omni 模型上训练约需 `55` 小时，在 Phi-4-MM 模型上约需 `35` 小时。
*   **训练效率对比**：
    *   与标准 GRPO 相比，TARS 方法仅在计算对齐奖励时引入了 `4.4%` 的微小额外计算开销，且显存占用几乎相同。

### 5. 实验数量与充分性

*   **实验覆盖度**：实验设计相当充分，主要体现在以下几个方面：
    *   **多模型验证**：在两个不同架构的 7B 级 Speech LLM（公开的 Qwen2.5-Omni 和内部的 Phi-4-MM）上进行了验证，证明了方法的泛化性。
    *   **多基线对比**：对比了包括级联系统、跨模态对齐、通用后训练在内的多种范式，比较全面公平。
    *   **细致的消融实验**：
        1.  逐个剥离 `R_rep` 和 `R_beh` 奖励项，验证了其互补性。
        2.  对比了不同训练策略（SFT、DPO、Text-only GRPO、Standard GRPO）。
        3.  对 `R_rep` 适用的层深度（浅层、中层、深层、全层）进行了敏感性分析。
    *   **深入的分析**：
        1.  通过定量的层级余弦相似度曲线，可视化了表征漂移的缓解效果。
        2.  进行了按领域的错误分析和自由生成场景下的奖励曲线分析。
        3.  提供了具体的定性案例（Chain-of-Thought 推理过程对比），直观展示了改进点和剩余缺陷。
        4.  考察了对真实语音和副语言现象的泛化性能（SD-QA, MMSU基准）。
*   **公平性**：所有后训练基线均使用相同的基座模型、数据和 LoRA 配置，确保了对比的公平性。

### 6. 主要结论与发现

*   **有效缩小差距**：TARS 框架在 7B 规模的语音大模型上显著缩小了模态推理差距，在 MMSU 和 OBQA 基准上取得了当时最优性能。
*   **性能超越基准**：该方法不仅大幅超越同类端到端对齐方法（如 SALAD、知识蒸馏），甚至超越了语音识别 + 同规模纯文本大语言模型的级联系统。
*   **文本能力无损且提升**：在对齐语音推理能力的同时，模型的纯文本推理能力非但没有损失，反而得到增强，表明学到的推理能力可以跨模态泛化。
*   **奖励组件互补**：表征对齐奖励和行为对齐奖励是互补的，前者缓解内部表征漂移，后者约束外部输出语义，两者结合效果最佳。
*   **方法普适性**：该方法的有效性在不同基座模型及不同下游任务（含副语言感知）上均得到验证，展现了良好的泛化能力。

### 7. 优点

*   **新颖的方法论**：首次将在线策略强化学习与稠密、非对称的多粒度对齐信号（表征层+行为层）相结合，来解决语音大语言模型的模态推理差距，有别于传统的输入侧适配或离线的行为模仿。
*   **深入的分析视角**：通过层级表征相似度分析，不仅在结果上验证了有效性，更深入剖析了模型内部机制，揭示了“中层”表征对齐的关键作用。
*   **巧妙的实验设计**：模态特定归一化技巧解决了多模态RL中的优势计算偏差问题；使用词错率指标作为辅助诊断，有力地证明了性能提升源于真正的推理对齐而非简单的语音识别变好。
*   **实用性强**：方法模型无关，且计算开销低，易于在现有框架上实现，具有良好的工业应用前景。

### 8. 不足与局限

*   **模型规模单一**：仅在 7B 规模的模型上进行了验证，该方法在更小或更大规模模型上的缩放行为尚不明确。
*   **任务形式有限**：此次研究聚焦于单轮的多选题问答推理，未探索多轮对话、交互式或更开放式的推理场景。
*   **对副语言信息的忽视**：行为对齐奖励完全依赖于文本语义的相似度，可能惩罚那些能够准确捕捉并利用文本中没有的副语言信息（如情绪、讽刺、重音）的有效生成，有过度“文本规格化”的风险。尽管在MMSU副语言测试上有正向表现，但这仍是一个潜在的理论局限。
*   **依赖合成数据**：训练数据主要依赖 TTS 生成，在真实场景中复杂的声学环境下的表现可能需要进一步检验。虽然已在部分真实数据上验证，但覆盖的声学多样性可能依然不足。

（完）
