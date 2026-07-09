---
title: "VAPO: End-to-end Slide-Enhanced Speech Recognition with Omni-modal Large Language Models"
title_zh: VAPO：基于全模态大语言模型的端到端幻灯片增强语音识别
authors: "Rui Hu, Delai Qiu, Yining Wang, Shengping Liu, Jitao Sang (桑基韬)"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.425.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 利用幻灯片视觉信息增强语音识别，解决视觉干扰问题
tldr: 针对幻灯片增强语音识别中的视觉干扰问题，提出视觉锚定策略优化VAPO，通过解耦的Look-then-Listen推理链重塑模型推理，避免无关文本幻觉，提升转录准确性。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 800, \"height\": 641, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 785, \"height\": 507, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1649, \"height\": 454, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 794, \"height\": 332, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1655, \"height\": 220, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 722, \"height\": 429, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1638, \"height\": 1427, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1596, \"height\": 638, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.425/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1010, \"height\": 270, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 730, \"height\": 286, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1654, \"height\": 955, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1647, \"height\": 799, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 808, \"height\": 515, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 807, \"height\": 306, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 816, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 803, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 559, \"height\": 793, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1634, \"height\": 306, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1440, \"height\": 305, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.425/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1334, \"height\": 246, \"label\": \"Table\"}]"
motivation: 全模态大语言模型在幻灯片增强语音识别中存在视觉干扰，模型偏向可见文本导致产生无关幻觉。
method: 设计视觉锚定策略优化，将推理链分解为先思考提取视觉先验再回答的两阶段，用视觉先验作为语义锚点指导转录。
result: 实验表明VAPO有效减少了幻觉，显著提高了幻灯片场景下的语音识别准确率。
conclusion: 为利用视觉信号辅助语音识别提供了新思路，推动了全模态大模型在真实场景中的应用。
---

## Abstract
Omni-modal large language models (OLLMs) offer a promising end-to-end solution for slide-enhanced speech recognition due to their inherent multimodal capabilities. However, we found a fundamental issue faced by OLLMs: Visual Interference , where models show a bias towards visible text over auditory signals, causing them to hallucinate slide content that was never spoken. To address this, we propose Visually-Anchored Policy Optimization (VAPO), which aims to reshape models’ inference process to follow the human-like “Look-then-Listen” inference chain. Specifically, we design a temporally decoupled policy: the model first extracts visual priors in a think> block to serve as semantic anchors, then generates the transcription in an answer> block. The policy is optimized via multi-objective reinforcement learning. Furthermore, we introduce SlideASR-Bench, a comprehensive benchmark designed to address the scarcity of entity-rich data, comprising a large-scale synthetic corpus for training and a challenging real-world test set for evaluation. We conduct extensive evaluations demonstrating that VAPO effectively eliminates visual interference and achieves state-of-the-art performance on SlideASR-Bench and public datasets, significantly reducing entity recognition errors in specialized domains.

---

## 论文详细总结（自动生成）

好的，作为资深学术论文分析助手，我将使用中文、以 Markdown 形式，对提供的论文进行结构化、深入、客观的总结。

# 论文深度解析：VAPO – 基于全模态大语言模型的端到端幻灯片增强语音识别

## 1. 论文的核心问题与整体含义
- **核心问题（视觉干扰）**：该论文发现并系统性地定义了一个名为 **“视觉干扰”（Visual Interference）** 的关键问题。当使用全模态大语言模型（OLLMs）进行端到端的幻灯片增强语音识别（SlideASR）时，模型会严重偏向于幻灯片上的**可见文本**，而忽略实际的**听觉信号**，从而导致模型幻觉，转录出幻灯片上有、但语音中从未出现的内容。
- **研究动机**：
    - **现有范式的局限**：目前主流的解决方案是“管道”范式（先OCR提取文字，再将其作为上下文输入语音模型），这种方式系统复杂，存在误差累积。
    - **OLLMs的潜力与缺陷**：OLLMs理论上具备同时处理视觉和听觉模态的能力，是实现端到端SlideASR的理想选择。但实验发现，未经处理的OLLMs普遍存在上述的“视觉干扰”问题，导致性能不升反降。
- **整体含义**：论文旨在解决OLLMs在SlideASR任务中的“视觉干扰”瓶颈，通过重塑模型的推理过程，使其能像人类一样“先看后听”，从而有效利用视觉上下文提高对专业术语的识别准确率，而非简单地将视觉信息当作语音来转录。

## 2. 论文提出的方法论
- **核心思想**：受到人类在听专业演讲时“**先看后听（Look-then-Listen）**”行为的启发，提出一种**视觉锚定策略优化（Visually-Anchored Policy Optimization, VAPO）**，将视觉感知和听觉转录从时序上解耦，强制模型遵循一个结构化的推理链。
- **关键技术细节**：
    - **结构化推理链（`<think> <answer>`）**：
        - **`<think>` 阶段（“看”阶段）**：模型首先处理幻灯片图像，执行光学字符识别（OCR）任务，提取出视觉文本先验知识，作为后续转录的“语义锚点”。
        - **`<answer>` 阶段（“听”阶段）**：模型基于音频输入生成最终的语音转录文本，并以`<think>`阶段提取的内容作为可靠参考，解决音频中的歧义。
    - **多目标强化学习优化**：使用GRPO算法，并结合四种互补的奖励函数来训练模型：
        1.  **格式奖励（Format Reward）**：确保模型输出严格遵循`<think> <answer>`格式。
        2.  **OCR奖励（OCR Reward）**：评估`<think>`块中提取的文本与幻灯片真实文本的相似度（基于词错误率），以促进精确的视觉感知。
        3.  **ASR奖励（ASR Reward）**：评估`<answer>`块中生成的转录文本与语音真实内容的一致性（基于词错误率），以保持整体转录准确性。
        4.  **视觉锚定奖励（Visual Anchoring Reward）**：**核心创新奖励**。它专门鼓励模型在`<answer>`块中成功复现那些**同时出现在“幻灯片真实文本”和“语音真实转录”中的关键实体**，从而有效连接“看”和“听”两个阶段。
    - **总奖励函数**： $R_{total} = \lambda_1 R_{Format} + \lambda_2 R_{OCR} + \lambda_3 R_{ASR} + \lambda_4 R_{VA}$。实验中所有$\lambda$权重均设为1。

## 3. 实验设计
- **数据集与场景**：
    - **SlideASR-Bench（自建基准）**：为弥补实体丰富数据的稀缺性而构建，包含两个子集：
        - **SlideASR-S**：大规模**合成语料库**，基于`ContextASR-Bench`，使用LLM生成富含命名实体的幻灯片风格文本并渲染为图片，用于训练和测试。
        - **SlideASR-R**：小规模**真实世界**测试集，从公开学术报告视频中手工收集和标注，专用于评估在复杂真实环境中的泛化能力。
    - **SlideSpeech**：公开的、真实世界的英语SlideASR通用场景数据集。
    - **ChineseLips**：公开的真实世界中文SlideASR数据集，用于泛化性验证。
- **对比方法（基准模型）**：论文设置了三个严格的对比维度：
    1.  **无上下文（Contextless）**：仅使用音频输入，包括主流的LALMs（如 Qwen2-Audio, Mi-Dasheng）和 OLLMs（如 MiniCPM-o, Qwen2.5/3-Omni）。
    2.  **幻灯片文本作为上下文（Pipeline）**：同样使用上述模型，但采用管道方式，先用PaddleOCR提取文本，再将其作为文本提示输入模型。
    3.  **幻灯片图像作为上下文（End-to-End）**：直接使用OLLMs以图像和音频作为输入。这是VAPO的直接比较对象。
- **评估指标**：
    - **SlideSpeech**: 词错误率（WER）、偏向词错误率（B-WER）、无偏词错误率（U-WER）、召回率（Recall）。
    - **SlideASR-Bench**: 词错误率（WER）、命名实体词错误率（NE-WER）、命名实体假阴性率（NE-FNR）。
    - **自建指标**: 定义并测量了 **视觉干扰率（VIR）** 来量化幻觉现象的严重程度。

## 4. 资源与算力
- **模型基础**：对 Qwen2.5-Omni（3B和7B参数版本）进行后训练。
- **训练配置**：使用4块A100 GPU进行训练。
- **训练步数与超参数**：总训练步数为800步；优化器为`AdamW`，学习率$1e^{-6}$，全局批大小为32；强化学习的组大小（group size）为4，采样温度1.0，KL惩罚系数0.01。

## 5. 实验数量与充分性
- **实验数量**：论文进行了超过10组主要实验和补充实验，覆盖全面，分量充足。
    - **初步探索**：1组实验（针对VIR现象的定量分析）。
    - **主要结果**：3组实验（分别在SlideSpeech、SlideASR-Bench、ChineseLips上对比）。
    - **消融与分析**：4组关键实验（奖励函数消融、奖励权重敏感性分析、训练范式对比SFT vs. RL、对错配幻灯片的鲁棒性分析）。
    - **可视化与效率分析**：提供了2组可视化案例（成功/失败案例、注意力机制图）和1组推理延迟分析。
- **充分性、客观性与公平性**：
    - **充分性**：实验设计逻辑严密，从问题发现、方法验证到机制分析，形成闭环。消融实验清晰地揭示了方法各组成部分的贡献。
    - **客观性**：采用了广泛使用的公开数据集和标准评价指标，结论有数据支撑。
    - **公平性**：在同等的输入条件下（无上下文、文本上下文、图像上下文）与多个不同架构和大小的前沿模型进行了全面对比，基线选择合理且具有代表性。

## 6. 论文的主要结论与发现
- **视觉干扰是普遍瓶颈**：所有评测的OLLMs在端到端SlideASR中都存在严重的视觉干扰现象（VIR高达63.28%），这解释了为何朴素的端到端方法会失败。
- **VAPO有效消除干扰并取得最优性能**：VAPO在SlideASR-Bench和SlideSpeech上均显著超越了所有基线模型，特别是在实体相关指标上，如将SlideASR-R上的NE-FNR从最佳基线的28.22降至15.35，充分证明了其有效性。
- **推理链结构和RL优化缺一不可**：结构化`<think><answer>`格式比直接预测（SFT w/o think）效果更好，而VAPO的RL优化又比单纯的监督微调（SFT w/ think）有巨大提升，证明RL对于建立“看”和“听”的有效连接至关重要。
- **VAPO具备鲁棒性**：当提供错配的幻灯片时，模型性能不会崩溃，而是回退到纯音频识别的水平，表明它是“锚定”而非“复制”视觉内容。

## 7. 优点
- **问题定义清晰深刻**：“视觉干扰”这一概念的提出，准确概括了OLLMs在该任务中的核心失败模式，为后续研究指明了方向。
- **方法论简洁高效**：VAPO方法巧妙地借鉴了人类认知过程和思维链（CoT）思想，通过一个简单的`<think><answer>`格式和精心设计的奖励函数组合，优雅地解决了模态间干扰问题，无需复杂的架构修改。
- **数据贡献重要**：SlideASR-Bench填补了该领域在实体丰富场景下的评测空白，特别是SlideASR-R真实世界子集，为衡量模型在专业领域的实用性提供了高价值基准。
- **实验论证严谨**：通过多维度对比、详尽的消融实验、鲁棒性测试和注意力可视化，全面、立体地验证了方法的有效性，说服力强。

## 8. 不足与局限
- **任务泛化性有限**：当前方法专门针对幻灯片中的**文本**信息，无法利用更广泛的视觉线索（如产品图片、图表）。
- **真实世界鲁棒性受限于数据**：训练主要依赖合成的SlideASR-S，尽管在多个真实数据集上验证有效，但可能无法完全覆盖真实世界中复杂的幻灯片样式、低分辨率图像等视觉噪声带来的影响。论文中的失败案例也暴露了其OCR模块对低质量文本的敏感性。
- **推理效率存在开销**：结构化的推理过程导致推理延迟增加（VAPO-7B的推理时间约为基础模型Qwen2.5-Omni-7B的3倍），限制了其在实时场景中的应用。论文也明确指出了它更适合离线、高精度要求的场景。
- **对预训练模型存在依赖**：方法的有效性是在强大的基础模型（Qwen2.5-Omni）上验证的，当基础模型的OCR或ASR能力较弱时，整个系统的性能上限可能会受到影响。

（完）
