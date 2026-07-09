---
title: Hierarchical Semantic-Acoustic Modeling via Semi-Discrete Residual Representations for Expressive End-to-End Speech Synthesis
title_zh: 基于半离散残差表示的层次化语义-声学建模用于富有表现力的端到端语音合成
authors: "Yixuan Zhou, Guoyang Zeng, Xin Liu, Xiang Li, Renjie Yu, Ziyang Wang, Runchuan Ye, Weiyue Sun, Jiancheng Gui, Kehan Li, Zhiyong Wu, Zhiyuan Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=h5KLpGoqzC"
tags: ["query:speech-model"]
score: 10.0
evidence: 层次化语义-声学建模用于富有表现力的端到端语音合成
tldr: 针对语音合成中离散令牌稳定但缺乏表现力、连续信号表现力强但误差累积的问题，提出层次化语义-声学建模框架，引入半离散残差表示和可微量化瓶颈，实现语义级联和声学细节保留，在端到端富有表现力语音合成中取得突破。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音合成在离散令牌的稳定性和连续信号的表现力之间存在根本取舍，且多级管道造成语义-声学割裂。
method: 提出层次化语义-声学建模，通过半离散残差表示和可微量化瓶颈实现语义级联与声学建模的协同。
result: 框架实现了稳定且富有表现力的端到端语音合成。
conclusion: 该方法统一了稳定性与表现力，为端到端语音合成提供了新范式。
---

## Abstract
Generative models for speech synthesis face a fundamental trade-off: discrete tokens ensure stability but sacrifice expressivity, while continuous signals retain acoustic richness but suffer from error accumulation due to task entanglement. This challenge has driven the field towards multi-stage pipelines that rely on pre-trained discrete speech tokenizers, but these create a semantic-acoustic divide, limiting holistic and expressive speech generation.  We resolve these dilemma through hierarchical semantic-acoustic modeling with semi-discrete residual representations.Our framework introduces a differentiable quantization bottleneck that induces natural specialization: a Text-Semantic Language Model (TSLM) generates semantic-prosodic plans, while a Residual Acoustic Model (RALM) recovers fine-grained acoustic details.This hierarchical semantic-acoustic representation guides a local diffusion-based decoder to generate high-fidelity speech latents. 
Critically, the entire architecture is trained end-to-end under a simple diffusion objective, eliminating dependency on external discrete speech tokenizers. Trained on over 1 million hours of speech, our 0.5B-parameter model achieves state-of-the-art zero-shot TTS performance among open-source systems, demonstrating that our approach delivers expressive and stable synthesis. Audio samples are available at: https://voxcpm.github.io/VoxCPM-demopage/.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心矛盾**：语音生成的生成模型面临一个根本性取舍：
  - 离散令牌（discrete tokens）编码稳定，易于自回归建模，但会牺牲语音的声学细节与表现力。
  - 连续语音信号保留丰富的声学信息，但因任务纠缠（语义、韵律、音色、环境等）容易导致误差累积，合成不稳定。
- **现有方案的局限**：主流的多阶段流水线依赖预训练的离散语音分词器（tokenizer），先将语音压缩为离散单元，再通过语言模型生成。这种做法造成**语义-声学割裂**，难以进行整体、富有表现力的端到端语音生成。
- **本文目标**：打破离散稳定性与连续表现力之间的权衡，实现一种既能稳定建模语义-韵律又能保留精细声学细节的**端到端富有表现力的语音合成**系统。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：层次化语义-声学建模（Hierarchical Semantic-Acoustic Modeling），通过**半离散残差表示（semi-discrete residual representations）**与**可微量化瓶颈（differentiable quantization bottleneck）**，让语义建模与声学恢复在统一框架中自然分工。
- **模型架构与流程**：
  - **文本-语义语言模型（TSLM）**：接收文本输入，生成语义-韵律规划（semantic-prosodic plans），提供高层结构。
  - **残差声学语言模型（RALM）**：在 TSLM 规划的基础上，恢复细粒度的声学细节（如音色、能量、精细频谱变化）。这两个模型共同构成层次化半离散表示。
  - **局部扩散解码器（Local Diffusion-based Decoder）**：以层次化表示作为条件，通过扩散过程生成高保真的语音潜变量（speech latents），最终合成波形。
- **关键技术**：
  - **可微量化瓶颈**：促使表示自然分化，一部分趋向语义/韵律编码，另一部份承担残差声学建模，而无需硬性分离或手工特征。
  - **端到端训练**：整个框架（TSLM + RALM + 扩散解码器）在简单的扩散目标下联合优化，**不依赖任何外部预训练的离散语音分词器**。
  - 总参数量：0.5B（5 亿参数）。

### 3. 实验设计：数据集、benchmark 与对比方法

- **训练数据规模**：超过 **100 万小时** 的语音数据。
- **任务场景**：主要评估 **零样本语音合成（zero-shot TTS）**，即对未见过的说话人音色直接合成自然语音。
- **对比基准**：
  - 对比对象为**开源系统**，声称在开源系统中达到最优性能（state-of-the-art）。
  - 由于缺少完整论文文本，未列出具体对比方法名称，但摘要提到其相对于依赖外部离散 tokenizer 的多阶段 pipeline 有明显优势。
- **主要评估维度**：稳定性（避免崩溃或误差累积）与表现力（自然度、韵律等）。在线音频示例页面表明有主观或客观评估。

### 4. 资源与算力

- **算力信息**：
  - **未明确提及** GPU 型号、数量与训练时长。摘要仅给出训练数据规模 > 1M 小时，但未进一步说明所需计算资源。
  - 由于参数规模为 0.5B，训练数据极大，可推断需要大规模分布式训练，但具体配置在所提供的文本片段中缺失。

### 5. 实验数量与充分性

- **实验次数**：
  - 因仅有摘要和元数据，无法准确统计实验组数。通常此类工作的实验包含：
    - 零样本 TTS 的主实验（自然度 MOS、说话人相似度等）
    - 与多个主流基线（如离散 token 方案、连续扩散方案）的比较
    - 消融实验（验证 TSLM/RALM 分层的作用、量化瓶颈的影响等）
- **充分性与公平性**：
  - 评价标准的覆盖：既然在零样本 TTS 上声称开源 SOTA，理应在至少一个公开且公认的 benchmark 上进行对比。
  - 由于摘要未披露具体测试集、评估协议和基线配置，目前难以判断是否完全公平。但论文被 ICLR 2026 接收（分数 10.0），实验充分性应已通过同行评审。

### 6. 论文的主要结论与发现

- **实现了稳定性与表现力的统一**：通过层次化半离散残差表示，在不失去细节的情况下保证语义与韵律生成的稳定性。
- **摆脱了外部离散 tokenizer 的依赖**：端到端训练消除了因预训练 tokenizer 引入的信息瓶颈和语义-声学割裂。
- **0.5B 参数模型在 > 1M 小时数据上训练即取得开源 SOTA**，证明方法的有效性与缩放潜力。
- 提出的框架为高表现力端到端语音合成提供了新范式，展示扩散解码器与层次化表示协同的广阔前景。

### 7. 优点：方法或实验设计上的亮点

- **新颖的表示设计**：半离散残差 + 可微量化瓶颈，让表示自然分化为语义和声学流，构思巧妙。
- **端到端简单高效**：仅使用扩散目标联合训练全部组件，无需复杂的多阶段预训练或强制对齐。
- **颠覆性意味强**：证明可以完全抛开占主导地位的离散语音分词器，为端到端富有表现力语音合成开辟新路。
- **实用性强**：参数规模相对紧凑（0.5B），却有出色的零样本合成能力，有利于实际部署。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **细节缺失**：由于仅凭摘要和元数据，无法知晓：
  - 对比 baselines 的具体配置与是否经过公平调优；
  - 评估指标及统计显著性；
  - 多语言、多场景的泛化性；
  - 对长文本、情感强度、噪声背景等鲁棒性。
- **大规模数据依赖**：训练数据超过 100 万小时，对小数据场景下的适用性未知；数据来源与版权、偏见未提及。
- **计算资源不透明**：训练成本、推理延迟都未披露，可能限制其实时应用。
- **可能存在的偏见**：巨量数据的筛选可能引入人口统计学或语言偏见，需要负责任 AI 评估（论文未提及）。

**（完）**
