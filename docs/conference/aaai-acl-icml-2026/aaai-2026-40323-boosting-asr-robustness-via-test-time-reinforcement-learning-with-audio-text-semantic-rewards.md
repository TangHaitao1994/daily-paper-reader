---
title: Boosting ASR Robustness via Test-Time Reinforcement Learning with Audio-Text Semantic Rewards
title_zh: 通过测试时强化学习与音频文本语义奖励增强ASR鲁棒性
authors: "Linghan Fang, Tianxin Xie, Li Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40323/44284"
tags: ["query:speech-model"]
score: 9.0
evidence: 测试时强化学习使ASR适应噪声和口音，无需标签
tldr: 为了提升自动语音识别系统在真实场景中面对噪声、口音等分布偏移时的鲁棒性，本文提出测试时强化学习方法ASR-TRA。与依赖伪标签的适应策略不同，该方法利用音频文本语义对齐作为奖励，引导模型在无真值条件下进行优化，有效抑制确认偏差，在多种分布外测试集上取得了显著的性能提升。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: ASR模型对噪声和口音敏感，现有测试时适应易陷入确认偏差。
method: 提出ASR-TRA，在测试时利用强化学习和音频文本语义对齐奖励进行无标签适应。
result: 在噪声和口音场景下显著提升识别准确率，优于传统伪标签方法。
conclusion: 测试时强化学习避免确认偏差，为鲁棒自适应ASR开辟了新路径。
---

## Abstract
Recently, Automatic Speech Recognition (ASR) systems (e.g., Whisper) have achieved remarkable accuracy improvements but remain highly sensitive to real-world unseen data (data with large distribution shifts), including noisy environments and diverse accents. To address this issue, test-time adaptation (TTA) has shown great potential in improving the model adaptability at inference time without ground-truth labels, and existing TTA methods often rely on pseudo-labeling or entropy minimization. However, by treating model confidence as a learning signal, these methods may reinforce high-confidence errors, leading to confirmation bias that undermines adaptation.
To overcome these limitations, we present ASR-TRA, a novel Test-time Reinforcement Adaptation framework inspired by causal intervention. More precisely, our method introduces a learnable decoder prompt and utilizes temperature-controlled stochastic decoding to generate diverse transcription candidates. These are scored by a reward model that measures audio-text semantic alignment, and the resulting feedback is used to update both model and prompt parameters via reinforcement learning.
Comprehensive experiments on LibriSpeech with synthetic noise and L2 Arctic accented English datasets demonstrate that our method significantly outperforms existing state-of-the-art (SOTA), including SUTA and SGEM, in both accuracy and inference speed. Ablation studies further confirm the effectiveness of combining audio and language-based rewards, highlighting our method's enhanced stability and interpretability. Overall, our approach provides a practical and robust solution for deploying ASR systems in challenging real-world conditions.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：自动语音识别系统（如 Whisper）在实际部署中，面对噪声环境、口音等分布外数据时性能急剧下降。现有测试时适应（TTA）方法多依赖伪标签或熵最小化，将模型置信度作为学习信号，易强化高置信度错误，陷入确认偏差，导致适应失效。
- **整体含义**：论文提出将测试时适应重新定义为强化学习驱动的奖励决策过程，通过外部音频-文本语义对齐奖励替代内部置信信号，从根本上避免确认偏差，实现无真值监督下的鲁棒、可解释、高效适应。

### 2. 论文提出的方法论
- **核心思想**：以结构因果模型为指导，将解码器提示注入视为因果干预，利用温度控制随机解码生成反事实转录假设，再用音频-文本语义对齐模型 CLAP 作为奖励源，通过策略梯度更新提示和模型参数。
- **关键技术细节**：
  - **可学习解码器提示（Prompt Injection）**：在解码器输入前端插入长度$L=4$的可训练向量 $\mathbf{p} \in \mathbb{R}^{L\times d}$，作为因果干预 $do(\mathbf{p})$ 直接调制每一时间步的预测。
  - **反事实采样与奖励评估**：
    - 通过带有温度参数 $T$ 的随机采样生成 $K$ 个转录候选。
    - 用 CLAP 模型计算每个候选与原始音频的余弦相似度作为序列级奖励 $r^{(i)} = R(\hat{\mathbf{y}}^{(i)})$。
    - 还可加入大语言模型（LLM）奖励作为补充信号。
  - **策略梯度优化**：采用 REINFORCE 算法，以批量奖励均值 $\bar{r}$ 为基线计算优势值 $r^{(i)}-\bar{r}$，更新提示和 Whisper 参数 $\theta$，梯度估计为：
    \[
    \nabla_{\theta,\mathbf{p}}\mathcal{L} = \frac{1}{N}\sum_{i=1}^{N}\nabla_{\theta,\mathbf{p}}\log P(\hat{\mathbf{y}}^{(i)})\cdot(r^{(i)}-\bar{r})
    \]
  - **单样本在线适应**：每个测试样本独立优化，完成后恢复模型原始状态，避免跨样本干扰。
- **算法流程**（文字说明）：
  1. 固定温度 $T=0$ 生成确定性基线转录，计算初始奖励。
  2. 注入随机提示，采样多个温度 $t_i \sim \text{Uniform}(0.4,0.6)$，生成多个候选转录并计算奖励。
  3. 计算各候选的优势值，求和得到策略梯度损失。
  4. 同时更新模型参数和提示参数（提示学习率比模型大 100 倍）。
  5. 以当前优化后的模型生成最终转录输出，之后恢复参数。

### 3. 实验设计
- **数据集与场景**：
  1. **噪声鲁棒性**：LibriSpeech test-other 叠加 MS-SNSD 噪声集的 8 种噪声（空调、机场广播、嘈杂人声等），信噪比固定为 10 dB。
  2. **口音鲁棒性**：L2-Arctic 数据集，涵盖 6 种母语背景的非英语母语者英语语音。
- **Benchmark 方法**：
  - Whisper-Tiny 原始模型（无适应）
  - SUTA（基于伪标签和高置信度熵最小化）
  - SGEM（序列级广义熵最小化）
- **评价指标**：词错误率（WER）和推理延迟（秒）。

### 4. 资源与算力
- 所用模型为 Whisper-Tiny（约 39M 参数）和 Whisper-Base（74M），提示引入额外 1536 个参数。
- 所有实验在**单块 NVIDIA RTX 6000 Ada GPU** 上完成，未提及训练时长。解码温度随机采样范围为 $[0.4, 0.6]$，每样本生成 4 个候选。

### 5. 实验数量与充分性
- **主要对比实验**：两组（噪声集 8 种条件 + 口音集 6 种 L1 背景），分别与 3 个基准比较 WER 和延迟，共约 14 种独立设置下的对比。
- **消融实验**：系统探究提示调优、模型微调、奖励模型类型（无、CLAP、LLM、CLAP+LLM）在 Whisper-Tiny 和 Whisper-Base 上的效果，共 2×6=12 组配置，并汇报延迟。
- **置信度子集分析**：选取高斯噪声下置信度最高的 100 个样本，单独评估。
- **相关性分析**：计算 CLAP 分数与真实 WER 的 Spearman 相关系数。
- **充分性**：实验覆盖两种主要分布偏移、不同模型大小、多种策略消融，对比公平，指标统一，能有效支撑结论。

### 6. 论文的主要结论与发现
- ASR-TRA 在噪声场景下平均 WER 降至 28.64%（原始 32.71%），延迟仅 0.720 秒，均优于 SUTA 和 SGEM。
- 对口音鲁棒性提升显著，平均 WER 从 32.11% 降至 28.21%，尤其在阿拉伯语和越南语母语者上改善明显。
- 高置信度子集中，基线 WER 高达 83.61%，SUTA 恶化至 122.37%，ASR-TRA 降至 45.17%，证实模型存在“盲目自信”，而外部奖励可有效避免该类错误。
- 消融实验表明提示调优与参数微调互补，CLAP 提供高效基础奖励，LLM 奖励进一步降低 WER 且混合奖励可兼顾精度与速度。
- CLAP 分数与真实 WER 呈中等负相关（$\rho = -0.431$），证明其作为语义奖励的合理性。

### 7. 优点
- **因果框架创新**：将测试时适应建模为因果干预，通过 do-操作注入提示，解释性强。
- **机制简单高效**：仅需少量额外参数和并行采样，延迟接近无适应基线，适合资源受限部署。
- **鲁棒性强**：不依赖置信度，通过外部语义奖励有效缓解确认偏差，在极端噪声和高置信度错误样本中表现稳定。
- **实验全面**：覆盖多噪声、多口音，包含效率分析、消融实验和深度分析，评估体系客观。
- **可扩展性**：支持插入不同奖励模型（CLAP/LLM），为后续多模态或对话级适应提供基础。

### 8. 不足与局限
- **语言限制**：CLAP 目前主要支持英语音频-文本对齐，多语言评估不足，可能影响跨语言鲁棒性验证。
- **延迟与奖励成本**：引入 LLM 奖励虽提升精度，但推理延迟增加 7–9 倍，制约极低延迟场景应用。
- **单句适应限制**：仅针对孤立语句进行独立适应，未扩展到流式或对话上下文，连续适应和上下文利用有待探索。
- **温度与采样数固定**：温度采样范围 $[0.4,0.6]$ 和候选数 $K=4$ 凭经验设定，缺少自适应机制和更广范围的消融。
- **模型规模局限**：仅验证了 Tiny 和 Base 规模，对大模型或其他编码器-解码器结构的泛化性未覆盖。

（完）
