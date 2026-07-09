---
title: "You Are What You Train: Rethinking Training Data Quality, Targets, and Architectures for Universal Speech Enhancement"
title_zh: 你训练了什么就是什么：重新思考通用语音增强的训练数据质量、目标与架构
authors: "Szu-Wei Fu, Rong Chao, Xuesong Yang, Sung-Feng Huang, Ryandhimas E. Zezario, Rauf Nasretdinov, Ante Jukić, Yu Tsao, Yu-Chiang Frank Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=IFuoniZ7Pk"
tags: ["query:speech-model"]
score: 7.0
evidence: 重新设计语音增强的训练目标与数据质量，提升下游语音任务的预处理效果。
tldr: 针对通用语音增强中训练目标和架构选择存在的不足，提出使用时移无回声纯净语音替代早期反射目标，并设计两阶段框架融合回归与生成模型，既保持语音保真度又提升感知质量。实验证明该方法在去混响和降噪任务中显著优于现有方案，为高质量语音前端处理提供了新基准。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 常用早期反射目标损害感知质量，单一模型难以同时兼顾保真度和感知提升。
method: 提出时移纯净目标替代早期反射，结合回归-生成两阶段架构实现高质量增强。
result: 在去混响和降噪上，客观与主观指标均优于现有通用增强方法。
conclusion: 重新审视训练目标和架构对提升语音增强效果至关重要，为前端处理提供了新方向。
---

## Abstract
Universal Speech Enhancement (USE) aims to restore the quality of diverse degraded speech while preserving fidelity. Despite recent progress, several challenges remain. In this paper, we address three key issues. (1) In speech dereverberation, the conventional use of early-reflected speech as the training target simplifies model training, but we found that it still harms perceptual quality. We therefore apply time-shifted anechoic clean speech as a simple yet more effective target. (2) Regression models preserve fidelity but produce over-smoothed outputs under severe degradation, while generative models improve perceptual quality but risk hallucination. We introduce a two-stage framework that effectively combines the strengths of both approaches, inspired by a recent theoretical finding. (3) We study the trade-off between training data scale and quality, a critical factor when scaling to large, imperfect corpora. Experimental results demonstrate that using time-shifted anechoic clean speech as the learning target significantly improves both speech quality and downstream automatic speech recognition (ASR) performance, while the two-stage framework further boosts quality without compromising fidelity.  In addition, our model demonstrates strong language-agnostic capability, making it well-suited for enhancing training data in other speech generative tasks. To ensure reproducibility, the code will be made publicly available
upon acceptance of the paper. Several enhanced real noisy speech examples are provided on the demo page: \url{https://anonymous.4open.science/w/USE-5232/}

---

## 论文详细总结（自动生成）

# 论文《你训练了什么就是什么：重新思考通用语音增强的训练数据质量、目标与架构》详细总结

## 1. 论文的核心问题与整体含义
通用语音增强（USE）旨在恢复多种退化语音的质量，同时保持其信号保真度。尽管已有进步，该领域仍面临三个关键挑战，这也是本研究的核心动机：
- **训练目标不当**：在语音去混响任务中，业界普遍将“早期反射语音”作为学习目标以简化训练，但作者发现该目标实际上会损害感知质量。
- **架构能力单一**：回归模型虽能保持保真度，但在严重退化场景下输出会过于平滑；生成模型能提升感知质量，却有产生“幻觉”（编造语音内容）的风险。单一模型难以同时兼顾这两个方面。
- **数据规模与质量的权衡**：当扩展至大规模但不完美的训练语料时，如何平衡数据规模与质量是一个关键因素。

整体含义在于，必须重新审视并优化训练数据质量、学习目标以及模型架构，才能构建出更强大、更通用的语音增强前端。

## 2. 论文提出的方法论
研究从目标设计、架构融合和数据策略三个层面提出创新方法。

- **新训练目标：时移无回声纯净语音**
  - **核心思想**：直接放弃包含混响的“早期反射语音”目标，转而使用经过时间偏移的无回声纯净语音作为训练目标。该方法更简单，却能避免早期反射成分对感知质量的负面影响，引导模型直接恢复干净信号。
  - **实现方式**：对原始纯净语音进行适当的时移，形成监督信号。

- **两阶段生成-回归融合框架**
  - **核心思想**：受近期理论发现的启发，设计一个两阶段流水线，有效结合回归与生成模型的优势。
  - **流程细节**：
    1.  **第一阶段（回归）**：以保真度为导向，对退化语音进行初步恢复，生成高保真但可能过度平滑的输出。
    2.  **第二阶段（生成）**：以感知质量为向导，在第一阶段输出的基础上进行精细化生成，显著提升自然度与听觉质量，同时因为输入已具备高保真骨架，有效抑制了幻觉风险。两步共同实现了保真与质量的平衡。

- **数据质量与规模权衡研究**
  - 系统探索了在大规模、可能有缺陷的数据集上训练时，如何通过质量筛选与控制，最大化模型性能。

## 3. 实验设计
- **应用场景与数据集**：论文涵盖**语音去混响**和**降噪**两大类退化场景。摘要虽未列出具体数据集名称，但明确提到了使用**真实噪声**语音样本进行增强演示，并验证了模型的**语言无关**能力。下游任务评估使用了**自动语音识别（ASR）**。
- **基准与对比方法**：
  - **内部消融对比**：对比了使用“早期反射目标”与所提“时移纯净目标”的模型；比较了单独回归模型、单独生成模型与所提两阶段框架的表现。
  - **外部基准对比**：将所提方法与现有的其他通用语音增强方法进行性能对比。
  - **评估指标**：同时采用客观语音质量指标、主观感知质量测听以及下游ASR的单词错误率（WER）等。

## 4. 资源与算力
论文的摘要与提供的元数据中**未明确说明**所使用的GPU型号、数量、训练时长或任何算力消耗细节。这一信息仅在论文全文中可能提及，目前无法评估。

## 5. 实验数量与充分性
基于摘要可推断，论文设计了**较为充分**的实验组，以支撑其核心主张：
- **至少三个维度的消融/控制实验**：
  1. 训练目标对比（时移纯净语音 vs. 早期反射语音）。
  2. 模型架构对比（纯回归、纯生成、两阶段融合）。
  3. 训练数据质量/规模的影响研究。
- **多场景与泛化性验证**：包含去混响、降噪任务，以及在非特定语言数据集上的ASR性能测试，证明其语言不敏感特性。
- **主观评价**：提供了真实噪声语音的增强示例，隐含进行了主观听感评估。
- 以上实验设计层次清晰，针对所提出的三个核心问题逐一验证，具备**客观性与公平性**。但由于缺失数据集细节和具体对比方法的完整列表，外部可复现性和更广泛对比的充分性尚无法完全确认。

## 6. 论文的主要结论与发现
- **时移无回声目标显著更优**：使用该目标训练，不仅在语音感知质量上取得显著提升，还能有效提高下游ASR的准确率。
- **两阶段框架达成最优平衡**：该框架在保持信号保真度的前提下，进一步大幅提升语音的感知质量，成功融合了回归与生成的双重优势。
- **出色的语言泛化性**：模型展现出强大的跨语言能力，表明其非常适合作为数据增强前端，服务于其他语音生成类任务。
- **代码开源承诺**：为促进复现，论文承诺接收后将公开所有代码。

## 7. 优点
- **方法论层面**：
  - 对“早期反射目标”这一常规做法提出有力质疑，并用简单高效的“时移纯净目标”替代，颠覆了旧有认知。
  - 两阶段流水线巧妙地解耦了“保真”与“感知”两个目标，为语音增强架构设计提供了新思路。
- **实验设计层面**：
  - 实验逻辑清晰，从目标、架构和数据三个关键轴心出发进行系统性验证，结论扎实。
  - 关注到实际部署中的语言泛化能力，实用性强。

## 8. 不足与局限
- **信息不透明风险**：摘要及元数据完全未披露所用数据集、对比方法的名称和任何算力信息，严重限制了对其结论普遍性与复现条件的外部评估。
- **潜在应用限制**：
  - 引入“时移”操作可能破坏音视频同步等需要精确时间对齐的任务。
  - 两阶段串行结构可能增加计算延迟和资源开销，在实时场景中的可行性未经讨论。
  - 论文被ICLR 2026拒绝，可能表明存在未解决的评审关切（例如，与最新SOTA的对比广度不足、理论支撑不够深入等），但其被拒原因在提供材料中无法得知。
- **评估风险**：缺乏在多通道、不同混响类型或极低信噪比条件下的鲁棒性分析。

（完）
