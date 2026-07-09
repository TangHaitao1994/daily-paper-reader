---
title: "Hearing More with Less: Multi-Modal Retrieval-and-Selection Augmented Conversational LLM-Based ASR"
title_zh: 以少听多：面向对话式大模型语音识别的多模态检索增强
authors: "Bingshen Mu, Hexin Liu, Hongfei Xue, Kun Wei, Lei Xie"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40528/44489"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用多模态检索选择相关上下文提升对话语音识别准确率
tldr: 对话语音识别中，固定数量上下文或全部历史会引入噪声和计算负担。本文提出多模态检索增强的LLM-ASR方法，通过检索与当前语音语义相关的历史上下文进行选择，有效提升识别准确率。实验表明，该方法在复杂对话场景下优于现有方法，实现了更高效和准确的语音识别。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法使用固定长度或全部历史上下文导致语音识别混乱和计算开销。
method: 提出多模态检索与选择模块，动态获取与当前语音相关的历史对话上下文。
result: 在对话ASR基准上提高了识别准确率并减少了无关上下文干扰。
conclusion: 该方法为对话语音识别提供了智能化上下文利用范式，提升了实用性能。
---

## Abstract
Automatic Speech Recognition (ASR) aims to convert human speech content into corresponding text. In conversational scenarios, effectively utilizing context can enhance its accuracy. Large Language Models' (LLMs) exceptional long-context understanding and reasoning abilities enable LLM-based ASR (LLM-ASR) to leverage historical context for recognizing conversational speech, which has a high degree of contextual relevance. However, existing conversational LLM-ASR methods use a fixed number of preceding utterances or the entire conversation history as context, resulting in significant ASR confusion and computational costs due to massive irrelevant and redundant information. This paper proposes a multi-modal retrieval-and-selection method named MARS that augments conversational LLM-ASR by enabling it to retrieve and select the most relevant acoustic and textual historical context for the current utterance. Specifically, multi-modal retrieval obtains a set of candidate historical contexts, each exhibiting high acoustic or textual similarity to the current utterance. Multi-modal selection calculates the acoustic and textual similarities for each retrieved candidate historical context and, by employing our proposed near-ideal ranking method to consider both similarities, selects the best historical context. Evaluations on the Interspeech 2025 Multilingual Conversational Speech Language Model Challenge dataset show that the LLM-ASR, when trained on only 1.5K hours of data and equipped with the MARS, outperforms the state-of-the-art top-ranking system trained on 179K hours of data.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：在对话语音识别中，如何高效利用历史上下文来提升当前语句的识别准确率，同时避免无关冗余信息带来的干扰和高昂计算开销。
- **研究动机**：大语言模型基座语音识别（LLM-ASR）具备长上下文理解能力，但现有方法要么固定取前若干句、要么取全部历史作为上下文，这会导致大量与当前语句无关的填充词、话题漂移等内容混入，引起识别混乱并浪费算力。对话中真正相关的历史上下文往往是动态分布在远处的，并非固定在前几句（如图1所示）。
- **整体含义**：提出多模态检索与选择方法MARS，在完整历史中智能地检索并挑选出与当前语句最相关的一条历史上下文，从而以更少的信息获得更优的识别效果。

### 2. 论文提出的方法论
- **核心思想**：构建一个对话历史数据库，通过多模态（语音和文本）检索得到一组候选上下文，再通过多模态选择从中决出最佳历史上下文，与当前话语一同送入LLM-ASR进行转录。
- **关键技术与流程**：
  - **数据库构建**：使用在目标数据上微调的Whisper-large-v3，为每个历史语句存储三元组（ID、语音嵌入、识别假设文本）。
  - **多模态检索模块**：
    - *语音模态检索*：利用FastDTW计算帧级声学相似度，同时通过池化后余弦相似度计算语句级相似度，加权求和得到语音检索相似度，取Top-K作为语音候选。
    - *文本模态检索*：使用Qwen3-Embedding模型计算当前语句假设与历史假设的句子级语义相似度，取Top-K作为文本候选。
  - **多模态选择模块（近理想排序）**：
    - 为所有2K个候选上下文补齐语音和文本两种相似度。
    - 对相似度进行归一化：\(sr_i = \frac{sw_i}{\sqrt{\sum sw_j^2}},\ tr_i = \frac{tw_i}{\sqrt{\sum tw_j^2}}\)。
    - 定义理想解（两种相似度均取最大值）和负理想解（均取最小值）。
    - 计算每个候选到理想解与负理想解的欧氏距离 \(d_i^+, d_i^-\)。
    - 相对贴近度 \(c_i = d_i^-/(d_i^+ + d_i^-)\)，选择 \(c_i\) 最大的候选作为最佳历史上下文。
  - **自适应上下文训练与解码**：训练时以50%概率随机掩码最佳历史上下文，防止模型过度依赖。推理提供三种策略：直接解码、MARS解码、两遍解码（先用直接解码得到初步假设，构建新数据库，再用MARS解码提升质量）。

### 3. 实验设计
- **数据集与场景**：Interspeech 2025 MLC-SLM挑战赛数据，覆盖11种语言（英语多个方言区、法、德、意、葡、西、日、韩、俄、泰、越），共约1500小时双人自然对话。
- **评估指标**：MER（混合错误率）、WER（单词错误率，用于拉丁字母语言）、CER（字符错误率，用于日/韩/泰）。
- **对比方法**：
  - Vanilla Whisper-large-v3。
  - 全微调Whisper-large-v3（相同1.5K小时数据）。
  - 语音大模型Qwen2-Audio。
  - 挑战赛最优系统TEA-ASLP（在179K小时多语言数据上预训练，再在1.5K小时数据上微调）。
  - 使用固定上下文的其他对话LLM-ASR方法（Bi-context利用前后文、Seewo利用前两句）。
  - 自身构件的消融对照（无上下文、仅语音检索、仅文本检索、多模态检索无选择、简单求和排序等）。

### 4. 资源与算力
- **GPU资源**：使用6块NVIDIA A800 GPU训练。
- **训练配置**：最大批次可容纳30秒语音，梯度累积设为6；优化器为Adam，学习率采用warm-up调度峰值0.0001，训练3个epoch，最终平均所有检查点进行推理。文中未给出具体总训练时长，但明确了硬件型号与数量。

### 5. 实验数量与充分性
- **实验组数**：包含多个维度对比，总计约6–8组主要实验，涵盖主流基线、挑战赛最优系统、不同上下文利用策略、消融实验（模块移除、检索类型与数量、选择方法）以及两遍解码策略。可视化分析也提供了支持。
- **充分性与公平性**：实验设计较为全面，在统一数据集和评测工具（MeetEval）下进行对比。消融实验清晰地验证了各模块的贡献，并与强基线（如179K小时数据训练的系统）公平比较，充分证明了方法的有效性。

### 6. 论文的主要结论与发现
- MARS能够从完整对话历史中高效提取最相关的单一上下文，避免冗余信息干扰。
- 在仅使用1.5K小时训练数据的条件下，所提方法超越了在179K小时数据上训练的SOTA系统，展示了极高的数据效率。
- 近理想排序方法比直接求和或仅用单模态检索更能选出优质上下文。
- 随机掩码训练和两遍解码策略对最终性能有显著提升。

### 7. 优点
- **智能上下文利用**：突破固定窗口限制，动态检索最相关的一句话作为上下文，减少无关信息干扰和计算量。
- **多模态融合**：同时利用声学和语义两个层面的相似度，互为补充，提升检索质量。
- **近理想排序**：提出了一种不依赖单一相似度尺度的多指标综合选择方法，合理且有效。
- **训练与解码策略**：随机掩码训练增强泛化，两遍解码利用更准确的假设进一步提升了识别率。
- **高数据效率**：在较少训练数据下实现超越大规模预训练系统的结果，降低了数据依赖。

### 8. 不足与局限
- **依赖前馈系统**：数据库构建和假设生成依赖Whisper模型，若其转录质量不佳会传播误差，且未探索迭代式改善。
- **数据集单一**：仅在MLC-SLM挑战赛数据上验证，泛化到其它对话风格、噪声环境或更多语言的实际效果尚不明确。
- **推理速度权衡**：虽然选择了单一上下文，但多模态检索和选择过程依然引入了额外计算延迟（如FastDTW和嵌入计算），对实时性要求高的场景可能有挑战。
- **LLM基座单一**：仅基于Qwen2.5-7B进行实验，未在不同规模或架构的大模型上进行对比。
- **文本检索依赖转录**：文本模态相似度依赖当前语句的识别假设，若假设本身错误，可能造成检索偏差。

（完）
