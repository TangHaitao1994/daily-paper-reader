---
title: Speech-Aware Long Context Pruning and Integration for Contextualized Automatic Speech Recognition
title_zh: 面向上下文语音识别的语音感知长文本剪枝与集成方法
authors: "Yiming Rong, Yixin Zhang, Ziyi Wang, Deyang Jiang, Yunlong Zhao, Haoran Wu, Shiyu Zhou, Bo Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40563/44524"
tags: ["query:speech-model"]
score: 9.0
evidence: 语音感知长文本剪枝提升基于领域知识的上下文ASR
tldr: 针对自动语音识别在需要长上下文领域知识的场景（如会议）中难以利用稀疏关键信息的问题，本文提出SAP^2方法，通过语音驱动的注意力池化在两个阶段动态剪枝并集成相关上下文关键词，有效压缩了冗长上下文，显著提升了上下文场景下对专业术语等的识别准确率。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: ASR难以利用长文本领域知识，上下文窗口受限且信息稀疏。
method: 提出SAP^2，利用语音驱动注意力池化分两阶段剪枝和集成上下文关键词。
result: 在需要领域知识的上下文语音识别任务中显著提升准确率。
conclusion: 该框架有效扩展了ASR的上下文利用能力，增强了实用性。
---

## Abstract
Automatic speech recognition (ASR) systems have achieved remarkable performance in common conditions but often struggle to leverage long-context information in contextualized scenarios that require domain-specific knowledge, such as conference presentations. This challenge arises primarily due to constrained model context windows and the sparsity of relevant information within extensive contextual noise. To solve this, we propose the SAP^2 method, a novel framework that dynamically prunes and integrates relevant contextual keywords in two stages. Specifically, each stage leverages our proposed Speech-Driven Attention-based Pooling mechanism, enabling efficient compression of context embeddings while preserving speech-salient information. Experimental results demonstrate state-of-the-art performance of SAP^2 on the SlideSpeech and LibriSpeech datasets, achieving word error rates (WER) of 7.71% and 1.12%, respectively. On SlideSpeech, our method notably reduces biased keyword error rates (B-WER) by 41.1% compared to non-contextual baselines. SAP^2 also exhibits robust scalability, consistently maintaining performance under extensive contextual input conditions on both datasets.

---

## 论文详细总结（自动生成）

## 1. 论文核心问题与整体含义
- **背景**：自动语音识别（ASR）在通用场景中已接近人类水平，但在会议演示、讲座等需要领域知识的上下文丰富场景中，识别性能仍然受限。
- **挑战**：上下文（如幻灯片OCR文本）往往长达数千词，其中真正与当前语音相关的关键词（如专业术语、人名）极度稀疏；模型上下文窗口有限，且大量噪声信息会引入干扰。
- **目标**：设计一种能够从长文本上下文中动态筛选并压缩关键信息、同时有效融入ASR的框架，以提升对领域特定实体和短语的识别准确率。

## 2. 论文方法论
- **整体框架 SAP^2（Speech-Aware Context Pruning with Speech-Driven Attention-based Pooling）**：分为两个阶段，均基于语音驱动注意力池化。
  - **阶段一（Speech-Aware Context Pruning）**：利用SpeechLLM（如Qwen2-Audio），根据语音内容从长关键词列表中筛选出真正相关的高频核心关键词，实现**动态剪枝**。
  - **阶段二（Contextualized ASR）**：将剪枝后的精简关键词与语音拼接，送入同一SpeechLLM完成上下文敏感的语言识别。
- **核心机制：Speech-Driven Attention-based Pooling**
  - 计算语音嵌入 ( h_x ) 与上下文关键词嵌入 ( h_z ) 之间的注意力分数。
  - 以该分数加权关键词特征，并在固定窗口内进行**池化压缩**，在保留语音相关信息的同时大幅缩短上下文序列长度（例如，窗口大小为2时长度减半）。
- **训练与优化**：
  - 将原始数据扩充为四元组 ( (X, Y, Z, \tilde{Z}) )，其中 ( \tilde{Z} = Z \cap Y ) 是真正出现在转录中的关键词。
  - 总体目标分解为两个子目标：最大化剪枝模型 ( p_\psi(\tilde{Z}|X,Z) ) 的条件概率，与最大化ASR模型 ( p_\theta(Y|X,\tilde{Z}) ) 的条件概率。
  - 两个阶段可联合训练，实际采用Qwen2-Audio-7B，冻结语音编码器，通过LoRA微调多模态投影层和LLM主干。

## 3. 实验设计
- **数据集与场景**
  - **SlideSpeech**：会议演讲场景，上下文为幻灯片OCR提取的关键词；使用单个幻灯片（1 slide）和扩展五页幻灯片（5 slides）两种设置。
  - **LibriSpeech**：通用阅读语音，按前人设定动态构建长度为100、500、1000的偏置列表（biasing list）。
- **评估指标**：WER、B-WER（偏置词错误率）、U-WER（非偏置词错误率）、Recall（关键词召回率）。
- **对比方法**
  - SlideSpeech：无上下文基线（Speech model only）、MaLa-ASR、Qwen2-Audio、CPP、LCB-net 等。
  - LibriSpeech：DB-RNNT+DB-LM、Attention-based DB + BPB beam search、Biasing fusion + SpeechLLM、CTC-Assisted LLM-Based ASR 等。

## 4. 资源与算力
- **训练**：在 SlideSpeech L95 5-slide 设置下，使用 **7 块 NVIDIA A100 40GB** GPU 进行训练。
- **推理**：在 15-slide 测试集上使用 **1 块 40G A100** GPU。
- **训练时间**：SAP^2-TPI 阶段一约 38268 秒，阶段二约 29609 秒；作为对比，直接拼接方法的 Qwen2-Audio-PC 约 60135 秒。
- **推理时间**：SAP^2-TPI 阶段一约 1564 秒，阶段二约 5076 秒（15 slides），总推理时间与 PC 方法相近，但阶段一因池化显著加速。

## 5. 实验数量与充分性
- **主要结果**：在 SlideSpeech 的两个子集（S95、L95）和 LibriSpeech 的 test-clean/test-other 上，对比了全部已有方法，共涉及 **6 种以上模型**，覆盖有无上下文、不同偏置长度。
- **消融实验**：
  - 三种集成策略对比：**PC**（直接拼接）、**JPI**（联合剪枝识别）、**TPI**（提出的两阶段）。
  - 语音驱动注意力池化的**有无**对比（Qwen2-Audio 与 SAP^2 变体）。
  - 池化**窗口大小**（2、4、8）的影响。
- **泛化实验**：在 SlideSpeech 上测试训练数据为 5 slides 时，对 1 至 25 slides 的泛化能力。
- **充分性与公平性**：实验设计完善，覆盖了主流基线、多组消融、多种上下文长度，指标全面（WER、U-WER、B-WER、Recall），结果具有说服力，对比公平。

## 6. 论文的主要结论与发现
- SAP^2 **在 SlideSpeech 和 LibriSpeech 上均取得最优性能**，WER 分别低至 7.71% 和 1.12%。
- 在 SlideSpeech 上，相较于无上下文基线，**B-WER 相对降低 41.1%**，同时关键词召回率高达 95.59%。
- **两阶段剪枝集成优于直接拼接或联合训练**，有效过滤噪声、避免任务干扰。
- **语音驱动注意力池化**显著压缩上下文并提升抗噪能力，尤其在上下文较长时优势更明显。
- 框架具备**极强的上下文长度鲁棒性**：从 5 slides 扩展到 15 slides 时，WER 甚至略有下降，未出现性能饱和。

## 7. 优点
- **创新性强**：将主动剪枝与注意力池化结合，模拟人类基于先验知识筛选关键信息的过程，有效解决长上下文中的信息稀疏问题。
- **方法简洁有效**：复用同一 SpeechLLM 完成剪枝和识别，无需额外的复杂偏置结构，易于实现。
- **性能显著**：在多个数据集和多种上下文长度下均实现 SOTA，尤其在大噪声长文本条件下增益突出。
- **可扩展**：训练和推理开销可控，注意力池化机制在加速的同时保持性能，适合实际部署。
- **分析透彻**：通过消融和泛化实验清晰验证了各模块的贡献及鲁棒性。

## 8. 不足与局限
- **上下文粒度单一**：目前仅将上下文分割为单个单词（single words），未利用短语级别的语义信息，可能丢失上下文关联。论文自身已指出未来需探索语义短语上下文。
- **模态局限**：仅使用文本上下文，未融合视觉特征（如幻灯片布局、图像），多模态信息可能进一步改善识别。
- **数据集依赖**：实验仅在 SlideSpeech 和 LibriSpeech 上进行，缺少更多真实复杂场景（如医疗、法律、直播）的验证。
- **模型与算力要求**：基于 7B 参数的 SpeechLLM，训练和推理仍需要一定规模的 GPU 资源，对轻量级场景的适配有待验证。
- **联合训练的挑战**：JPI 策略在实验中表现不佳，说明剪枝与识别任务的联合端到端学习存在干扰，两阶段方法虽优但增加了推理步骤。

（完）
