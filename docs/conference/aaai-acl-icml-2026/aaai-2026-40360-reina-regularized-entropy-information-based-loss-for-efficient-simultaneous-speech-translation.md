---
title: "REINA: Regularized Entropy Information-Based Loss for Efficient Simultaneous Speech Translation"
title_zh: REINA：基于正则化熵信息的损失用于高效同步语音翻译
authors: "Nameer Hirschkind, Joseph Liu, Xiao Yu, Mahesh Kumar Nandwana"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40360/44321"
tags: ["query:speech-model"]
score: 4.0
evidence: 信息论损失优化语音翻译的质量-延时权衡
tldr: 同步语音翻译系统需在翻译质量与延迟间取得平衡。该论文利用信息论原理，提出REINA损失函数，通过量化“等待更输入能带来的信息增益”来训练自适应策略，从而在不增加延迟的情况下提升翻译质量。在多语种实验中，REINA推动质量-延迟帕累托前沿向前迈进，为实时翻译系统提供了理论驱动的优化方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 同步语音翻译面临质量与延迟平衡难题，需优化等待策略。
method: 基于信息论推导REINA损失，训练自适应等待策略，增益越大越值得等待。
result: 在法语、西班牙语、德语翻译上实现了更好的质量-延迟折衷。
conclusion: REINA为高效流式语音翻译提供了理论依据和实用改进。
---

## Abstract
Simultaneous Speech Translation (SimulST) systems stream in audio while simultaneously emitting translated text or speech. Such systems face the significant challenge of balancing translation quality and latency. We introduce a strategy to optimize this tradeoff: wait for more input only if you gain information by doing so. Based on this strategy, we present Regularized Entropy INformation Adaptation (REINA), a novel loss to train an adaptive policy using an existing non-streaming translation model. We derive REINA from information theory principles and show that REINA helps push the reported Pareto frontier of the latency/quality tradeoff over prior works.
Utilizing REINA, we train a SimulST model on French, Spanish and German, both from and into English. Training on only open source or synthetically generated data, we achieve state-of-the-art (SOTA) streaming results for models of comparable size. We also introduce a metric for streaming efficiency, quantitatively showing REINA improves the latency/quality trade-off by as much as 21 percent compared to prior approaches, normalized against non-streaming baseline BLEU scores.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：同步语音翻译（SimulST）需要一边接收音频流、一边生成翻译文本，面临**翻译质量**与**延迟**之间的权衡，即何时等待更多音频（READ）、何时输出译文（WRITE）的决策难题。
- **整体含义**：本文提出一种基于信息论的自适应策略训练方法，**仅在能获得信息增益时才等待**，从而更高效地在不增加延迟的前提下提升翻译质量。该方法可将一个训练好的非流式语音到文本翻译（S2TT）模型，低成本地转化为流式同步翻译模型，并通过正则化熵信息自适应损失（REINA）实现。

## 2. 论文提出的方法论

- **核心思想**：定义信息增益 `F(a,S,n,t)` 为已知部分音频和已生成部分译文时，等待完整音频后对下一译文词元获取的互信息增量。若该增益超过阈值，则应等待（READ），否则输出（WRITE）。
- **信息论公式**：  
  `F(a,S,n,t) = I(s_{n+1}; a_T, S^n) - I(s_{n+1}; a_t, S^n) = H(s_{n+1}|a_t, S^n) - H(s_{n+1}|a_T, S^n)`  
  在实践中用非流式 S2TT 模型对部分音频和完整音频的 log 概率差进行估计：  
  `ˆF ≈ log p(s_{n+1}|a_T, S^n) - log p(s_{n+1}|a_t, S^n)`
- **策略网络训练**：训练一个轻量 Transformer 策略网络 `qθ`，使其输出与 `ˆF` 的协方差最大化（批内标准化后），形成策略损失 `Lp`。  
  总损失 `L_REINA = Lp + Lm + λLr`，其中：
  - `Lm` 为单调性正则化，强制策略输出沿词元顺序近似非降，鼓励尽早产生 READ 动作。
  - `Lr` 为 L2 正则化，防止 q 值发散。
- **模型架构与训练三阶段**：
  1. **多任务训练**：声学编码器（Whisper Medium）、文本解码器（Transformer）和附加 MT 编码器（T5）联合训练 ASR、NMT、S2TT 任务，得到非流式 S2TT 模型。
  2. **截断音频微调**：冻结主干网络，仅用部分音频微调解码器，使 partial‑audio 的 log 概率估计更准。
  3. **策略网络训练**：冻结所有参数，仅训练 6M 参数的策略网络（2 层 Transformer + 线性层 + Sigmoid），使用 REINA 损失，数据仅用 S2TT 样本。

## 3. 实验设计

- **数据集与场景**：
  - 训练数据：MLS（多语言语音库）、CVSS‑C、MUST‑C、Mosel，以及用内部 NMT 合成的伪 S2TT 数据；MT 数据来自 CCMatrix（6000 万句对）。
  - 评估集：**MUST‑C**（en→{de,es,fr}）、**CVSS‑C**（{de,es,fr}→en）。
- **对比方法**：
  - 主对比：DiG‑SST（基于散度的策略训练）、DiSeg、EdAtt（基于注意力）；
  - 还包含 StreamSpeech（CVSS，仅 ASR‑BLEU）、SimulS2S‑LLM；
  - 本文复现的 DiG‑SST（同为 MUST‑C 数据）作为消蚀对比。
- **评价指标**：
  - 翻译质量：SacreBLEU。
  - 延迟：Average Lagging (AL)、Length‑Adaptive LAAL。
  - 新提出**NoSE（Normalized Streaming Efficiency）**：在给定延迟区间内，AL‑BLEU 曲线下面积除以非流式 BLEU 线下的面积，使得不同非流式性能的模型可以进行公平比较。
- **实验变体（消融）**：
  - REINA（全量数据 + 完整损失）
  - REINA w/o monotonicity（去除单调性损失）
  - REINA（MUST‑C only）（仅 MUST‑C 数据，与 DiG‑SST 公平比较）
  - REINA w/o truncated training（跳过阶段 2，仅 MUST‑C）
  - DiG‑SST（本文实现，MUST‑C only）

## 4. 资源与算力

- **硬件**：所有阶段均使用 24 张 A100‑80G GPU。
- **训练时长**：
  - 阶段 1（非流式多任务训练）：5 天。
  - 阶段 2（截断微调）：2 天。
  - 阶段 3（策略网络训练）：20 个 epoch，耗时不到 12 小时。
- **模型规模**：训练时共 445M 参数（Whisper Medium 307M + 解码器 101M + MT 编码器 38M），推理时仅用 408M（去除 MT 编码器），策略头额外 6M 参数。

## 5. 实验数量与充分性

- **实验组数**：在 6 个语言对（MUST‑C 3 个方向 + CVSS‑C 3 个方向）上完整评估，每种至少包含 5 个变体及多个延迟操作点，共计数十组曲线对比。
- **充分性**：  
  - 覆盖了两种主流评估集（中长句 MUST‑C，短句 CVSS‑C），且包含英‑外和外‑英方向。
  - 消融实验系统地考察损失组件（单调性）、训练阶段（截断）和数据规模对策略的影响。
  - 与多个近期 SOTA 同类方法对比，并尝试复现最强基线 DiG‑SST。
  - 提出了面向流策略自身的评价指标 NoSE，减少非流式能力差异带来的干扰，更聚焦策略优化。
- **客观性与公平性**：在仅用 MUST‑C 数据的设置下，REINA 仍显著优于 DiG‑SST 和 DiSeg，证明提升源于方法本身而非数据量差异。消融实验中保持其他训练细节一致，仅改变一个因素。

## 6. 论文的主要结论与发现

- REINA 策略可将任意非流式 S2TT 模型高效转换为流式 SimulST，**低延迟场景下翻译质量显著优于 DiG‑SST、DiSeg、EdAtt 等方法**。
- 单调性正则化对**极度低延迟**的设置尤为重要（如 AL 从 1.95 降至 1.57，BLEU 不变）。
- 使用截断音频微调解码器对策略训练至关重要，缺失该阶段导致 NoSE 大幅下降。
- 基于开源和合成数据训练的模型，在无专有数据的情况下达到同量级模型的 SOTA 流式性能。
- 提出的 NoSE 指标能更纯粹地衡量**策略优化能力**，避免非流式基线差异影响对比公平性。

## 7. 优点

- **理论优雅且可解释**：从互信息出发定义何时等待，比基于散度或 RL 的方法更稳定、理论基础更强。
- **训练高效**：仅需微调解码器和训练一个 6M 小策略头，无需复杂单调对齐或 RL 的不稳定搜索。
- **性能突出**：在多个语言对和两种数据集上均取得更优的质量‑延迟折衷，尤其在短句场景（CVSS‑C）表现优秀。
- **评估创新**：引入 NoSE 指标，提供更聚焦于流策略本身的比较方式。
- **资源友好**：完全基于开源数据，训练可在数天内完成（24×A100），具有可复现性和实用价值。
- **工程实用性强**：模型规模适中（408M），推理仅需策略头小开销，适合实际聊天或视频场景。

## 8. 不足与局限

- **语言覆盖有限**：仅涉及英语、德语、法语、西班牙语等高频资源语言，未验证低资源语言及跨语系场景。
- **对非流式模型的依赖**：策略质量严重受制于基础 S2TT 模型对部分音频的 log 概率估计精度，若基础模型本身对截断音频推理不佳，REINA 改善可能受限。
- **数据合成偏差**：使用内部 NMT 合成 S2TT 数据可能引入风格或质量偏差，论文未详细分析该偏差的影响。
- **复现受限**：对比的 DiG‑SST 为本文自实现，原文结果可能因细节差异而无法完全对齐，削弱了一些对比的确定性。
- **噪声与鲁棒性**：未探讨在嘈杂环境、不同口音或低带宽实时流式输入下的性能，实际部署可行性需额外验证。
- **扩展到 S2ST 未实现**：虽然提及未来可接文本到语音合成，但当前工作仅停留在文本输出，未能直接展示端到端语音输出效果。
- **单调性参数的敏感性**：`ε`、`λ` 等超参数虽称对结果不敏感，但未给出详细的稳定性分析或调优范围。

（完）
