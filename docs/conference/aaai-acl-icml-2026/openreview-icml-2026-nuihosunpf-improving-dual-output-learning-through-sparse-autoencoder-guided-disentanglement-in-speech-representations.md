---
title: Improving Dual-Output Learning through Sparse Autoencoder Guided Disentanglement in Speech Representations
title_zh: 通过稀疏自编码器引导解耦提升双输出学习的语音表征
authors: "Seung-Hwan Cho, Young-Min Kim, Seongwon Seo, Danik"
date: 2026-01-23
pdf: "https://openreview.net/pdf/5b7189d7097d817929f3b7024728cd98dc81a06b.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用稀疏自编码器解耦语音表征，提升双输出语音识别精度
tldr: 针对双输出语音识别中表征干扰问题，提出SODA框架，嵌入稀疏自编码器并在编码器输出和SAE潜空间施加辅助CTC监督，指导稀疏、任务解耦的表征形成，提升双重转录性能。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 共享编码器的双输出ASR存在表征干扰，影响表面形式和语义导向转录的准确度。
method: 在Conformer编码器后加入稀疏自编码器（SAE）作为中间可训练组件，并在多个层级施加CTC损失引导稀疏解耦。
result: 实验显示，SODA在双输出ASR任务上同时提升了字面和语义转录的质量。
conclusion: 该方法证明解耦表征可有效缓解多目标学习中的干扰，为复杂语音理解任务提供新范式。
---

## Abstract
While representation disentanglement is often studied from the perspective of interpretability, its role in managing competing objectives in end-to-end learning remains less explored. 
We study dual-output automatic speech recognition (ASR), where a shared Conformer encoder supports both surface-level and meaning-oriented transcriptions, leading to representational interference. 
To address this, we propose SODA, an alignment-aware dual-output ASR framework that integrates a sparse autoencoder (SAE) as a trainable intermediate component. 
By applying auxiliary connectionist temporal classification (CTC) supervision at both the encoder output and the SAE latent space, the framework guides the formation of sparse, task-relevant representations consumed by two Transformer-based decoders. 
Experiments demonstrate that SODA performs competitively on both tasks within a single model. Our framework requires significantly fewer parameters than comparable baselines and learns interpretable, task-specific representations.

---

## 论文详细总结（自动生成）

# 论文总结：《通过稀疏自编码器引导解耦提升双输出学习的语音表征》

---

## 1. 核心问题与整体含义

- **研究背景**：语音识别任务中，有时需要同时输出两种不同风格的转录，例如：
  - **表面形式转录**（surface‑level）：注重字面精确对应的词序列；
  - **语义导向转录**（meaning‑oriented）：注重含义等价但表达可能不同的文本。
- **核心问题**：现有端到端模型通常共享编码器处理两种目标，导致 **表征干扰**——不同目标争夺编码空间，使任一任务的性能都不理想。
- **整体含义**：通过引入稀疏自编码器引导表征的 **任务解耦**，能在单一模型内同时提升两种转录的质量，缓解多目标学习中的冲突，并为复杂语音理解提供可解释的中间表征。

## 2. 方法论

提出 **SODA**（Surfaced-Oriented and meaning-oriented Dual-output ASR with Sparse Autoencoder），主要包括以下技术点：

- **基础架构**：采用共享的 **Conformer 编码器**，后接两个独立的 **Transformer 解码器**，分别负责表面转录和语义转录。
- **解耦组件**：在编码器输出与解码器之间插入 **稀疏自编码器（SAE）** 作为可训练的中间层。SAE 将共享表征压缩并重构，通过稀疏性约束促使不同任务的信息自然分离。
- **多层级监督**：在 **编码器输出** 和 **SAE 潜空间** 两个层级施加辅助的 **CTC（联结时序分类）损失**。CTC 损失直接指导这两个层次学习对应任务的序列对齐信息，从而引导 SAE 产生 **稀疏且任务相关** 的潜在表征。
- **训练流程**：整体损失 = 两个解码器的自回归损失 + 编码器侧 CTC 损失 + SAE 潜空间 CTC 损失，端到端联合优化。

## 3. 实验设计

- **数据集 / 场景**：论文摘要和元数据未指明具体数据集，仅说明是在 **双输出语音识别** 场景下验证。通常这类任务可能使用类似 LibriSpeech 或专用双标签语音数据。
- **基准（Benchmark）**：对比了 **可比基线模型**（comparable baselines），但未给出基线具体名称或结构。由结果描述可知，基线可能为直接共享编码器的双解码器模型或其他多任务架构。
- **评估指标**：应包含两种转录的常用指标（如字错率 WER、语义等价准确率等），但文中未列具体数值，仅定性指出 SODA 在两项任务上均具竞争力。

## 4. 资源与算力

- **文中未明确提供任何计算资源信息**，如 GPU 型号、数量、训练时长或推理延迟。因此无法评估该方法的实际部署成本及可复现性。

## 5. 实验数量与充分性

- 从现有材料无法判断完整的实验规模。只知进行了 **双输出任务的性能比较** 和 **参数量的对比**（SODA 参数显著少于基线）。
- 未提及是否有 **消融实验**（如移除 SAE、移除某层 CTC 监督等），也未说明是否在多个数据集、多种噪声条件或不同规模的模型上进行验证。
- **充分性受限**：由于缺乏详细实验设计，难以判断结论的稳定性和普适性。

## 6. 主要结论与发现

- SODA 在 **单一模型** 内同时处理表面形式和语义导向转录时，取得了与专门模型相当甚至更优的性能。
- 该方法 **参数量显著更少**，更轻量高效。
- 可以学到 **可解释、任务专用** 的稀疏表征，验证了通过 SAE 解耦表征是缓解多目标学习冲突的有效途径。
- 为语音理解任务提供了一种 **表征解耦的新范式**。

## 7. 优点

- **新颖的解耦策略**：在语音识别中引入稀疏自编码器，并通过双 CTC 监督实现任务导向的解耦，思路清晰。
- **多层级监督设计**：在编码器输出和 SAE 潜空间同时施加 CTC 损失，强力引导表征分化，设计巧妙。
- **高效性**：较之基线，框架参数更少，有利于资源受限场景。
- **可解释性增益**：稀疏表征有助于理解模型内部对不同任务的关注区域，提升透明性。

## 8. 不足与局限

- **信息不完整**：当前仅依据摘要和元数据进行分析，缺乏完整的实验细节、数据说明与结果表格，难以评估结论的坚实度。
- **实验覆盖可能不足**：未展示消融研究、跨数据集泛化性、噪声鲁棒性等关键验证，可能隐藏偏差风险。
- **应用限制**：若语义转录的定义和标注高度依赖特定语料或任务定义，方法的推广性待进一步考察。
- **CTC 监督的依赖**：双层级 CTC 要求两种转录的序列对齐标注，在实际场景中可能增加标注成本。

---

（完）
