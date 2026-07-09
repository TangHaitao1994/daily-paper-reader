---
title: Detecting Hallucinations in SpeechLLMs at Inference Time Using Attention Maps
title_zh: 利用注意力图检测语音大语言模型在推理时的幻觉
authors: "Jonas Waldendorf, Bashar Awwad Shiekh Hasan, Evgenii Tsymbalov"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.2147.pdf"
tags: ["query:speech-model"]
score: 8.0
evidence: 基于注意力的SpeechLLM幻觉检测，用于自动语音识别和语音翻译
tldr: 随着语音大语言模型在自动语音识别等任务中的广泛应用，其产生的幻觉问题严重制约可靠性。本文提出利用注意力图提取音频一致性等四类量化特征，并基于此训练逻辑回归模型进行推理时检测。该方法不依赖金标准，在Qwen-2-Audio等模型上验证有效，为保障语音模型输出质量提供了轻量级实用工具。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2147/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1641, \"height\": 547, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2147/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 773, \"height\": 643, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.2147/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1644, \"height\": 1641, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 817, \"height\": 360, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 816, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 814, \"height\": 739, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 813, \"height\": 736, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 817, \"height\": 357, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 816, \"height\": 677, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 816, \"height\": 676, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 768, \"height\": 306, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 849, \"height\": 364, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 570, \"height\": 383, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 657, \"height\": 408, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.2147/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 659, \"height\": 412, \"label\": \"Table\"}]"
motivation: 语音大语言模型易产生幻觉，现有检测方法依赖金标准且忽略音频特有信号。
method: 提取音频与文本注意力图衍生指标，训练轻量级逻辑回归分类器进行推理时检测。
result: 在ASR与翻译任务上有效识别幻觉，不依赖金标准，计算开销小。
conclusion: 所提基于注意力的幻觉检测方法增强了语音模型的可靠性与实用性。
---

## Abstract
Hallucinations in Speech Large Language Models (SpeechLLMs) pose significant risks, yet existing detection methods typically rely on gold-standard outputs that are costly or impractical to obtain. Moreover, hallucination detection methods developed for text-based LLMs do not directly capture audio-specific signals. We investigate four attention-derived metrics: AudioRatio, AudioConsistency, AudioEntropy, and TextEntropy, designed to capture pathological attention patterns associated with hallucination, and train lightweight logistic regression classifiers on these features for efficient inference-time detection. Across automatic speech recognition and speech-to-text translation tasks, evaluations on Qwen-2-Audio and Voxtral-3B show that our approach outperforms uncertainty-based and prior attention-based baselines on in-domain data, achieving improvements of up to +0.23 PR-AUC, and generalises to out-of-domain ASR settings. We further find that strong performance can be achieved with approximately 100 attention heads, improving out-of-domain generalisation compared to using all heads. While effectiveness is model-dependent and task-specific training is required, our results demonstrate that attention patterns provide a valuable tool for hallucination detection in SpeechLLMs

---

## 论文详细总结（自动生成）

## 论文总结

### 1. 核心问题与整体含义

语音大语言模型（SpeechLLMs）在自动语音识别（ASR）和语音到文本翻译（S2TT）等任务中会产生幻觉（Hallucinations），即输出流畅但偏离输入音频内容的错误内容，可能带来严重风险。现有幻觉检测方法大多依赖金标准输出进行比对，获取成本高且不适合在线部署；而面向纯文本LLM的检测方法无法直接捕捉音频特有的信号。本论文旨在利用模型内部的注意力图（Attention Maps）设计无需金标准的轻量级推理时幻觉检测器，以在线过滤错误输出、降低标注成本，提升语音LLM的可靠性。

### 2. 方法论

核心思想是通过量化解码时注意力分配的异常模式来识别幻觉。作者观察到：
- 正确生成时，音频与文本之间会形成对角注意力对齐；
- 产生幻觉时，注意力往往坍缩到音频序列的开头，并失去对角结构。

基于此，提出四种注意力衍生指标，对每一解码步、每一层和每一头计算，然后对各步取平均得到每个头的一个标量值，最后将所有层的头特征拼接为向量，用于训练逻辑回归分类器。

具体指标如下：
- **AudioRatio**：音频输入部分的注意力占音频与自回归文本前缀总注意力的比例。
- **AudioConsistency**：连续解码步之间音频注意力分布的皮尔逊相关系数，意图捕获注意力塌陷到开头的现象。
- **AudioEntropy**：对音频注意力权重重新归一化后计算的熵，度量对输入的不确定性。
- **TextEntropy**：对文本输入端注意力权重计算熵，度量文本侧不确定性。

训练流程：
- 对每一样本提取四个指标的特征向量，每个指标维度为 L×H（层数×头数）。
- 应用 Min‑Max 缩放（对 Entropy 类指标）。
- 训练 L2 正则化的逻辑回归，也可用 L1 正则化做特征选择（Stable Features），再重新训练 L2 模型。
- 推理时输出幻觉概率，用于样本筛选或报警。

### 3. 实验设计

数据集与任务：
- **ASR 域内**：VoxPopuli（英、德、法、西四种语言，测试集约 7080 句）；**域外**：CALLHOME（英语会话电话录音，3916 句）。
- **S2TT**：FLEURS（英语到德/西/法等多向翻译，测试集约 4613 例）。

模型：
- Qwen-2-Audio 和 Voxtral-3B。

对比基线：
- 不确定性估计：Mean Entropy、Perplexity。
- 之前注意力方法：RAUQ Entropy、Attention Score。
- 随机猜测。

评价指标：F1、精确率、召回率、PR‑AUC、预测拒绝比（PRR）等。

自动标注策略：先用人工标注 1950 例，基于 WER + Semantic Hallucination Score（SHS）设定阈值（高精度 0.979、低召回 0.443），再自动标注大量训练数据。

特征消融与泛化实验：
- 使用全部特征（Combined）、仅 AudioRatio、按系数排序选 Top N 个头、L1 稳定特征选择等。
- 跨域泛化：VoxPopuli→CALLHOME；跨任务泛化：ASR→S2TT。
- 特征数影响分析：头数从 5 到 150 变化，查看 PR‑AUC 曲线。
- 任务特异性分析：比较 ASR 与 S2TT 的 top‑50 头重合度。

### 4. 资源与算力

论文明确指出：推理和特征提取在 8 块 **A100‑40GB GPU** 上完成，处理速度约 **4.5 样本/秒**。单次完整运行（约 57,000 句 ASR + 21,000 句 S2TT）约需 **38.5 GPU 小时**。整个开发与评估过程约进行 6 次完整迭代，加上 XCOMET 评分与 SHS 计算，总 GPU 使用量估算为 **约 300 GPU 小时**。

### 5. 实验数量与充分性

实验覆盖了两种主流 SpeechLLM、两个任务、多个数据集（域内清晰语音、域外嘈杂会话、多语言翻译），并进行了丰富的对照与消融：
- 与 4 种基线方法对比；
- 不同特征组合（全特征、单指标、Top‑N 选头、L1 稳定特征）；
- 特征数缩放分析；
- 跨域和跨任务泛化测试；
- 任务特异性头重合度分析。

整体实验设计全面、客观，对比公平，不仅验证了主方法的有效性，还深入分析了泛化能力、模型依赖和特征重要性，支撑充分。

### 6. 主要结论与发现

- 基于注意力的特征在**域内 ASR** 上显著优于不确定性基线，对 Voxtral‑3B 的 PR‑AUC 提升最高达 +0.23。
- 使用约 **100 个注意力头**即可达到接近全头特征的性能，并能**改善域外泛化**。
- 对于**嘈杂会话数据（CALLHOME）**，不确定性基线表现较强，注意力方法较依赖模型；Voxtral‑3B 的泛化性好于 Qwen‑2‑Audio。
- **ASR 训练的检测器无法直接迁移到 S2TT**，但用 S2TT 数据训练时同样能大幅超越基线，说明注意力模式具有任务特异性。
- 特征组合在翻译任务中更为重要，单指标性能下降明显。
- 头选择的重合度低，进一步表明任务特异性。
- 方法虽然轻量，但需任务特异的训练数据，效果随模型不同而变化。

### 7. 优点

- **无需金标准**：利用模型内部注意力图，完全无参考，适合在线部署。
- **轻量高效**：逻辑回归模型计算开销小，且使用少量头即可保持高性能。
- **可解释性**：四个指标各有明确的物理解释（如对齐、坍陷等），有助于理解模型行为。
- **全面评估**：覆盖多种基线、消融分析、泛化性测试，实验设计扎实。
- **实用价值**：可与不确定性估计等互补信号结合，实现更鲁棒的检测。

### 8. 不足与局限

- **自动标注召回低**（0.443），导致训练信号主要覆盖易检测的严重幻觉，可能遗漏更微妙的错误。
- **仅作二分类**，未区分幻觉的类型或严重程度。
- **跨任务不泛化**，需要为新任务单独训练检测器。
- **效果模型依赖**：Voxtral‑3B 泛化较好，Qwen‑2‑Audio 较差，方法普适性有限。
- **语言覆盖面有限**，仅评估了 4 种语言，且多为资源丰富的语种。
- **推理额外开销**：提取总体注意力图仍需额外计算和内存，虽轻量但对极低延迟场景可能有影响。
- **未与其他类型检测器（如集成、重采样）全面对比**。

（完）
