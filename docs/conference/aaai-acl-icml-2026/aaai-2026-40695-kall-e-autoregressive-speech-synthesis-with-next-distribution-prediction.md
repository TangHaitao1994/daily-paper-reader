---
title: "KALL-E: Autoregressive Speech Synthesis with Next-Distribution Prediction"
title_zh: KALL-E：基于下一分布预测的自回归语音合成
authors: "Kangxiang Xia, Xinfa Zhu, Jixun Yao, Wenjie Tian, Wenhao Li, Lei Xie"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40695/44656"
tags: ["query:speech-model"]
score: 9.0
evidence: 通过下一分布预测实现自回归语音合成，消除扩散组件以提升质量
tldr: 现有TTS方法多依赖扩散模型或离散语音标记，KALL-E提出一种新的自回归框架，利用Flow-VAE提取连续潜在表示，直接用AR Transformer预测语音分布，避免了扩散组件，通过KL散度优化实现高质量语音合成，实验表明合成质量优越。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有TTS方法依赖扩散组件或离散标记，存在局限。
method: 采用Flow-VAE提取连续语音表示，AR Transformer预测下一分布。
result: 实验证明KALL-E合成质量优越，且能适应连续语音建模。
conclusion: 自回归分布预测为TTS提供了一种高效且高质量的方案。
---

## Abstract
We introduce KALL-E, a novel autoregressive (AR) language model for text-to-speech (TTS) synthesis that operates by predicting the next distribution of continuous speech frames. Unlike existing methods, KALL-E directly models the continuous speech distribution conditioned on text, eliminating the need for any diffusion-based components. Specifically, we utilize a Flow-VAE to extract a continuous latent speech representation from waveforms, instead of relying on discrete speech tokens. A single AR Transformer is then trained to predict these continuous speech distributions from text, optimizing a Kullback–Leibler divergence loss as its objective. Experimental results demonstrate that KALL-E achieves superior speech synthesis quality and can even adapt to a target speaker from just a single sample. Importantly, KALL-E provides a more direct and effective approach for utilizing continuous speech representations in TTS.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：主流语音合成（TTS）模型多将语音离散化（如使用神经编解码器得到离散标记），再用自回归语言模型进行下一个标记预测。离散化过程存在信息损失、高帧率导致的序列长、生成幻觉等问题。同时，连续表示的方法常依赖复杂的后处理模块（如扩散模型）或过简化的回归损失。
- **核心问题**：如何在不依赖离散语音标记和扩散组件的情况下，直接、高效地从文本建模连续语音分布，从而提升合成质量与推理速度。
- **整体含义**：提出 **KALL-E**，一种直接预测连续语音帧的“下一分布”的自回归框架，统一了高保真、低帧率、说话人可控与多样化合成。

### 2. 方法论
- **核心思想**：利用 *下一分布预测* 替代传统的下一标记预测，用 KL 散度作为训练目标直接建模连续语音帧的均值和方差。
- **关键技术细节**：
  - **Flow-VAE**：在变分自编码器（VAE）中加入归一化流（normalizing flow），把简单先验（高斯）映射为更复杂的潜在分布。编码器（下采样扩张卷积残差块）提取语音的均值与方差；解码器（上采样转置卷积残差块）重构波形。训练时使用重建损失（ℓ1 mel谱）、多周期/多分辨率判别器损失、特征匹配损失和 KL 散度损失的加权和。
    - 潜在表示维度 512，帧率 12.5 Hz。
  - **自回归语言模型**：基于 Llama3.2-1B 的因果 Transformer 解码器。输入：文本嵌入、从前一步采样的分布 $Z_t$ 投影、说话人嵌入（来自 ECAPA-TDNN）。每步输出均值和方差预测，自回归采样形成序列。
    - 训练目标：每一时间步的预测分布与目标分布（由 Flow-VAE 编码得到）间的 KL 散度，加上终止分布的 KL 散度。
  - **说话人声音分布建模**：用 ECAPA-TDNN 提取说话人嵌入，经线性层映射至潜在空间并施加 KL 正则化。推理时可直接使用参考音频的说话人潜在向量，实现确定性克隆；也可从标准高斯采样 $S̃$，获得可重复的随机声音。
  - **测试时训练 (TTT)**：利用一条提示语音通过 Flow-VAE 提取分布，重复采样多个潜在序列构成微调数据集，仅微调语言模型（冻结说话人编码器），KL 散度损失不包括终止帧。适应不同说话人的风格、口音等细节。

### 3. 实验设计
- **数据集/场景**：
  - **Flow-VAE 重建评估**：LibriSpeech *test-clean* 子集（2620 句，16 kHz）。
  - **TTS 能力评估**：Seed-TTS 测试集的 *test-zh*（中文）和 *test-en*（英文）；情感感知评估用情绪文本（ChatGPT 生成）和 Emotion2Vec。
  - **训练数据**：Emilia 数据集（中英文，约 96.7k 小时）+ 内部清洗数据（约 3000 小时）。ESD 数据集用于情绪生成训练。
- **对比方法**：
  - **语音标记器**：DAC、Mimi、X-codec2、Stable Audio VAE。
  - **TTS 系统**：Seed-TTS、FireRedTTS、CosyVoice、CosyVoice 2、Llasa（1B / 8B）、F5TTS。
- **评价指标**：
  - 重建质量：STOI、PESQ (WB/NB)、SPK SIM、UTMOS。
  - TTS：WER/CER、说话人相似度 (SIM)、MOS（自然度）、SMOS（说话人相似度）、推理浮点运算量 (GFLOPS)、情感识别混淆矩阵。

### 4. 资源与算力
- **服务器配置**：8 块 NVIDIA A100 GPU。
- **训练对象**：Flow‑VAE 与自回归语言模型均在上述 GPU 上完成训练，但论文未给出具体的训练时长（小时/天）。

### 5. 实验数量与充分性
- **实验组数**：
  1. Flow‑VAE 重建对比（多种离散标记器和连续 VAE，不同维度与帧率）。
  2. 潜在分布可视化（核密度估计）。
  3. TTS 客观评测（中文/英文，含 TTT 变体）。
  4. 主观评测（MOS 与 SMOS，15 名受试者）。
  5. 推理效率对比（GFLOPS，10 秒音频）。
  6. 上下文感知情绪识别（混淆矩阵）。
  7. 消融实验：Flow‑VAE 潜变量维度/帧率影响，Stable Audio VAE 替换，TTT 训练集大小对 SIM 和 CER 的影响。
- **充分性与公平性**：实验覆盖客观、主观、效率、可控性多个维度，消融实验设计合理。对比模型训练数据量不完全一致，但作者已指出最可比的 Llasa‑1B‑160k，且采用 Seed‑TTS 统一测试集，公平性较好。

### 6. 主要结论与发现
- KALL‑E 在 12.5 Hz 极低帧率下实现高效自回归，**CER/WER 优于所有对比系统**（中文 0.96，英文 1.94）。
- 主观自然度 MOS 最高（4.17），显著优于同等规模的 Llasa‑1B，与更大模型 Llasa‑8B 相当。
- Flow‑VAE 的连续表示保留了丰富声学细节，使模型能仅从文本推断说话情绪，体现强上下文感知。
- TTT 能进一步提升说话人相似度，且基本不损失清晰度。
- 推理浮点运算量远低于其他对比模型，效率突出。

### 7. 优点
- **新颖的框架**：直接用 AR Transformer 预测连续分布，无需扩散模型，简化流水线。
- **高效推理**：低帧率（12.5 Hz）大幅减少生成步数，FLOPs 最低。
- **信息保留**：连续表示避免量化损失，Flow‑VAE 设计使分布更宽、方差更大，有利于生成多样性和鲁棒性。
- **统一可控性**：同一框架支持零样本语音克隆和可复现的随机声音合成。
- **开源承诺**：提供代码与模型，推动社区研究。

### 8. 不足与局限
- **训练数据规模与质量**：Emilia 数据集为“野生”数据，虽经二次清洗，仍可能有噪音。使用的内部清洗数据细节未全部公开。
- **说话人相似度**：客观 SIM 分数略低于部分离散标记系统（如 CosyVoice 2），作者认为可能因额外声码器条件导致分数虚高，但主观 SMOS 未见显著优势。
- **TTT 过拟合风险**：TTT 样本数增多时 CER 有回升，提示可能过拟合参考音频中的非理想特征。
- **未提及的局限**：论文未测评长文本或极端噪音环境下的稳定性；Flow‑VAE 的训练细节（如流的具体结构）未详尽说明；未讨论生成的情感是否过于夸张或产生不匹配。
- **对比公平性**：部分对比模型（如 Seed‑TTS）可能使用了更多训练资源，虽已指定最可比基线，但仍可能影响部分结论的强度。

（完）
