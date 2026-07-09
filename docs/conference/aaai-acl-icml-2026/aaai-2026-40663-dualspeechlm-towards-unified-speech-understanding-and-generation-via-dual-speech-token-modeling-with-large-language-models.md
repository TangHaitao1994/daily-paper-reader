---
title: "DualSpeechLM: Towards Unified Speech Understanding and Generation via Dual Speech Token Modeling with Large Language Models"
title_zh: DualSpeechLM：通过双语音Token建模构建统一语音理解与生成大模型
authors: "Yuanyuan Wang, Dongchao Yang, Yiwen Shao, Hangting Chen, Jiankun Zhao, Zhiyong Wu, Helen Meng, Xixin Wu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40663/44624"
tags: ["query:speech-model"]
score: 10.0
evidence: 使用双语音Token的统一语音理解与生成模型
tldr: 针对构建统一语音理解与生成模型面临的模态鸿沟和不同任务对信息粒度需求差异的挑战，本文提出DualSpeechLM，采用双语音Token建模，分别捕捉高层语义与细节声学特征，解耦理解与生成。实验表明该模型在ASR和TTS任务上均取得优异性能，迈出了语音全功能大模型的重要一步。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 统一语音理解与生成困难，模态鸿沟大，不同任务信息需求冲突。
method: 设计双语音Token分别服务于理解和生成，解耦声学与语义表示。
result: DualSpeechLM在语音识别与合成上均表现优异。
conclusion: 双Token建模为语音通用大模型提供了有效方案，促进了语音交互统一范式的进步。
---

## Abstract
Extending pre-trained text Large Language Models (LLMs)’s speech understanding or generation abilities by introducing various effective speech tokens has attracted great attention in the speech research community. However, building a unified speech understanding and generation model still faces the following challenges: (1) Due to the huge modality gap between speech and text tokens, extending text LLMs to unified speech LLMs relies on large-scale paired data for fine-tuning, and (2) Generation and understanding tasks prefer information at different levels, e.g., generation benefits from detailed acoustic features, while understanding favors high-level semantics. This divergence leads to difficult performance optimization in one unified model. To solve these challenges, in this paper, we present two key insights in speech tokenization and speech language modeling. Specifically, we first propose an Understanding-driven Speech Tokenizer (USTokenizer), which extracts high-level semantic information essential for accomplishing understanding tasks using text LLMs. In this way, USToken enjoys better modality commonality with text, which reduces the difficulty of modality alignment in adapting text LLMs to speech LLMs. Secondly, we present DualSpeechLM, a dual-token modeling framework that concurrently models USToken as input and acoustic token as output within a unified, end-to-end framework, seamlessly integrating speech understanding and generation capabilities. Furthermore, we propose a novel semantic supervision loss and a Chain-of-Condition (CoC) strategy to stabilize model training and enhance speech generation performance. Experimental results demonstrate that our proposed approach effectively fosters a complementary relationship between understanding and generation tasks, highlighting the promising strategy of mutually enhancing both tasks in one unified model.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义  
- **研究动机**：预训练文本大语言模型（LLM）在自然语言处理上表现卓越，扩展到语音领域以实现统一的理解与生成是当前热点。但现有方法面临两大难题：  
  - **模态鸿沟巨大**：语音与文本模态差异显著，将文本 LLM 适配为语音 LLM 需要海量对齐的语音-文本对进行微调，成本高昂。  
  - **信息需求冲突**：理解任务（如语音识别）依赖高层语义，生成任务（如语音合成/转换）却需要丰富的声学细节（韵律、情感、说话人特征）。单一 token 化方式难以同时满足这两类需求，导致理解与生成在一体化模型中互为掣肘（基线实验表明改善一者性能往往损害另一者）。  
- **整体含义**：本文旨在通过新型语音 token 化和双分支建模，在统一框架下消解模态鸿沟与信息需求冲突，实现可互相促进的语音理解与生成。

### 2. 论文提出的方法论  
核心思路是 **解耦理解与生成的表征**：用 **理解驱动语音 Token（USToken）** 作为输入，捕获高层语义；用 **声学 Token** 作为输出，保留细节信息，在端到端的 **DualSpeechLM** 中并行建模。

- **USTokenizer（理解驱动的语音分词器）**  
  - 结构：预训练 Whisper 编码器 → 下采样编码器 → 矢量量化（VQ，单码本）→ 上采样解码器。  
  - 关键点：通过一个**适配器**将 VQ 后的离散码映射至冻结文本 LLM 的输入空间，并用 **理解驱动损失** 优化 VQ 码本。该损失定义为以语音 prompt 为条件、生成目标文本响应的负对数似然：  
    \[
    L_{\text{Under}} = -\sum_{t=1}^{L} \log p(S_t \mid m, S_{<t}; \theta)
    \]  
    其中 \(m\) 为 USTokenizer 提取的语音特征，\(\theta\) 为文本 LLM 参数。  
  - 总损失：\(L_{\text{USTokenizer}} = \alpha L_{\text{commit}} + \beta L_{\text{Under}} + \gamma L_{\text{reconstruction}}\)，重建损失为 MSE（\(\hat{E}\) 与原始 Whisper 特征 \(E\) 之间）。  
  - 效果：通过文本 LLM 的理解信号反向传播优化语音 token，使其与文本模态天然对齐，减小模态差距。

- **DualSpeechLM（双 Token 建模框架）**  
  - **理解路径**：输入语音经 USTokenizer 得到 USToken，拼接任务提示后送入文本 LLM，以交叉熵损失预测文本答案。  
  - **生成路径**：文本 LLM 先根据输入语音的 USToken 和文本 prompt 自回归预测目标语音的 USToken 序列，再利用 **AcousticGPT** 模块将这些语义 token 转化为声学 token，最终由语音解码器恢复波形。  
    - 语义监督损失：\(L_{\text{semantic}} = -\frac{1}{L}\sum_{t=1}^{L} \log p(U_t^{\text{tar}} \mid U_{<t}^{\text{tar}}, P, U^{\text{in}}; \theta)\)，稳定 USToken 预测。  
    - 声学损失：\(L_{\text{acoustic}} = -\sum_{t=1}^{L} \log p(A_t^{\text{tar}} \mid A_{<t}^{\text{tar}}, S, \text{Spk}; \theta)\)，其中条件信号 \(S\) 采用 **链式条件（CoC）** 策略——训练时从三种来源等概率随机选择：提示隐藏状态 \(S_p\)、预测的 USToken \(U^{\text{tar}}\) 或两者的拼接 \([S_p; U^{\text{tar}}]\)；推理时固定使用拼接。CoC 作为正则化手段，防止过拟合单一条件，并减轻 USToken 预测偏差的负面影响。  
    - 总生成损失：\(L_{\text{generation}} = \lambda L_{\text{semantic}} + \xi L_{\text{acoustic}}\)。  
  - 整体采用 LoRA 高效微调文本 LLM，AcousticGPT 整合在 LLM 内实现端到端联合训练。

### 3. 实验设计  
- **训练数据**：  
  - USTokenizer：LibriSpeech（ASR）、IEMOCAP（SER）、SALMONN 中的 SQA 数据集（基于 LibriSpeech 用 ChatGPT 生成的问答对）。  
  - DualSpeechLM：约 **4,500 小时**语音数据，覆盖 8 类任务：ASR（LibriSpeech）、语音翻译 S2TT（CoVoST2 En2De）、SER（IEMOCAP）、SQA；文本到语音 TTS（LibriTTS‑R）、语音到语音翻译 T2ST（CVSS Es→En 和 Fr→En）、语音转换 VC（LibriHeavy 子集 + VCTK 评估）、语音对话 SC（用火山引擎 TTS 合成 SQA 语音）。  
- **对比方法**：SpeechGPT、SpiritLM、Mini‑Omni2、VITA、Moshi、Qwen2.5‑Omni，以及两个自建基线：**Baseline‑Acoustic**（输入输出均为 WavTokenizer 声学 token）、**Baseline‑Semantic**（输入输出均为 HuBERT token）。  
- **评估指标**：  
  - 理解：WER（ASR）、BLEU‑4（S2TT, SQA）、准确率（SER）、ChatGPTScore（SQA）。  
  - 生成：说话人相似度 SIM、WER、DNSMOS（TTS/VC）、BLEU‑4（T2ST/SC）、ChatGPTScore（SC）。  
  - 主观：QMOS（语音质量）和 SMOS（说话人相似度），针对 TTS、VC。  
- **变体设置**：DualSpeechLM‑USToken（本文最终方案）、DualSpeechLM‑Hubert（输入使用 HuBERT token）、Baseline‑Acoustic/Semantic。

### 4. 资源与算力  
论文**未明确给出** GPU 型号、数量、训练时间等算力细节。仅提及 Baseline‑Acoustic 模型在多任务设置下需训练 **160k 步**收敛，其他模型约 **60k 步**收敛，但缺乏对应硬件和 wall‑clock 时间的说明。

### 5. 实验数量与充分性  
- **主要实验**：  
  - 表 1–3：多种理解与生成任务的客观及主观评估，涵盖 6‑7 种任务、多个公开模型和基线。  
  - 表 4–5：更换 LLM 骨干（Phi‑3.5‑3B、Vicuna‑7B）后的性能，验证跨架构泛化性。  
  - 表 6–7：消融实验，分别移除理解驱动损失、重建损失、语义损失、CoC 策略，分析对理解和生成任务的影响。  
  - 图 1：在不同训练数据量比例下，对比基线与本文方法在生成/理解上的趋势。  
- **充分性与公平性评价**：  
  - 实验设计较为全面，覆盖多任务、多骨干、组件消融和数据量敏感性。  
  - 对比基线包括全量微调的大数据模型和同条件的 LoRA 微调模型，设置公平（统一数据量、训练步骤）。  
  - 主观评测进一步验证生成质量。  
  - 存在一定局限：**未测试更大规模数据**（如 10 万小时级），且生成任务的消融（CoC）仅针对 TTS 和 VC，对 T2ST/SC 的影响未报告。

### 6. 论文的主要结论与发现  
- **USTokenizer 有效缓解模态鸿沟**：通过理解驱动损失直接对齐文本 LLM，使语音 token 获得更强的语义一致性，在理解和翻译任务上大幅优于 HuBERT token。  
- **双 Token 建模成功解耦信息粒度**：DualSpeechLM 在仅 4.5k 小时数据和 LoRA 微调下，同时达到高水平的理解与生成性能，部分指标甚至好于全量微调、更大数据量的现有模型。  
- **语义监督与 CoC 关键**：语义损失对确保高质量 USToken 预测至关重要；CoC 作为条件正则化提升了生成稳定性和准确度。  
- **理解与生成可互相促进**：随着某一类型训练数据增加，模型在该类任务上提升的同时，另一类任务性能不降反升，表明一体化框架能带来协同增益。

### 7. 优点  
- **创新性强**：首次将**理解需求直接映射到 VQ 码本优化**，实现了与文本 LLM 深度对齐的 USToken；并设计了输入/输出分离的 dual‑token 架构，自然兼顾语义和声学。  
- **方法实用**：仅需**小规模数据**和**参数高效微调**（LoRA）即可超越部分大资源方案，降低落地门槛。  
- **实验严谨**：多任务、多 backbone、消融与主客观结合，验证全面；CoC 策略增强鲁棒性。  
- **协同效应突出**：数据增量实验清晰显示统一模型能实现理解与生成的双赢。

### 8. 不足与局限  
- **算力未披露**：缺少训练所用 GPU 资源与时长，难以评估实际部署成本。  
- **数据规模局限**：训练仅使用 4.5k 小时英文为主的数据，未扩展到多语言、大规模（如 10 万小时级）场景，泛化性有待检验。  
- **生成任务覆盖有限**：CoC 消融及部分生成指标仅评估 TTS/VC，未涵盖 T2ST 和 SC 的消融分析；主观评测只做了 TTS 和 VC。  
- **模型复杂度增加**：双分支设计和 AcousticGPT 引入额外模块，可能带来推理延迟和资源消耗，论文未讨论实时性。  
- **对齐风险**：USTokenizer 依赖 Whisper 和冻结 LLM 的理解信号，若 LLM 本身存在偏见或错误，可能传递至语音 token 的表征空间。

（完）
