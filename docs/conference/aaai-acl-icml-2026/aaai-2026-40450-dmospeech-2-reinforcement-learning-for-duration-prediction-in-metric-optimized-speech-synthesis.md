---
title: "DMOSpeech 2: Reinforcement Learning for Duration Prediction in Metric-Optimized Speech Synthesis"
title_zh: DMOSpeech 2：基于强化学习的度量优化语音合成中的时长预测
authors: "Yinghao Aaron Li, Xilin Jiang, Fei Tao, Cheng Niu, Kaifeng Xu, Juntong Song, Nima Mesgarani"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40450/44411"
tags: ["query:speech-model"]
score: 10.0
evidence: 强化学习优化时长预测以实现度量优化的语音合成
tldr: 针对扩散式文本到语音合成中时长预测模块未优化的问题，本文提出DMOSpeech 2，通过强化学习中的群组相对偏好优化（GRPO）将说话人相似度和词错误率作为奖励信号，训练时长策略网络，从而将度量优化扩展到整个合成流程，最终实现更完整的感知质量提升。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有度量优化语音合成未优化时长预测模块，限制了整体感知质量。
method: 提出基于强化学习的时长策略框架，利用群组相对偏好优化和说话人相似度/词错误率奖励。
result: 实现了更完整的度量优化，进一步提升了合成语音的感知质量。
conclusion: 将度量优化扩展至时长预测，推动了语音合成整体质量提升。
---

## Abstract
Diffusion-based text-to-speech (TTS) systems have made remarkable progress in zero-shot speech synthesis, yet optimizing all components for perceptual metrics remains challenging. Prior work with DMOSpeech demonstrated direct metric optimization for speech generation components, but duration prediction remained unoptimized. This paper presents DMOSpeech 2, which extends metric optimization to the duration predictor through a reinforcement learning approach. The proposed system implements a novel duration policy framework using group relative preference optimization (GRPO) with speaker similarity and word error rate as reward signals. By optimizing this previously unoptimized component, DMOSpeech 2 creates a more complete metric-optimized synthesis pipeline. Additionally, this paper introduces teacher-guided sampling, a hybrid approach leveraging a teacher model for initial denoising steps before transitioning to the student model, significantly improving output diversity while maintaining efficiency. Comprehensive evaluations demonstrate superior performance across all metrics compared to previous systems, while reducing sampling steps by half without quality degradation. These advances represent a significant step toward speech synthesis systems with metric optimization across multiple components.

---

## 论文详细总结（自动生成）

**DMOSpeech 2：基于强化学习的度量优化语音合成中的时长预测**

---

### 1. 论文的核心问题与整体含义

- **研究背景**：扩散式零样本文本到语音（TTS）系统已能合成几乎与人声无异的语音，但在端到端优化感知质量指标（如说话人相似度、可懂度）方面仍存在瓶颈。
- **核心问题**：先前的 **DMOSpeech** 利用分布匹配蒸馏对语音生成模块进行了直接的度量优化，但其 **时长预测器（duration predictor）** 仍独立于优化回路，导致整体管线无法实现完整的端到端感知优化。
- **整体含义**：DMOSpeech 2 旨在将度量优化延伸至时长预测这一“最后一公里”，通过强化学习使时长预测直接服务于最终的感知指标，从而构建更完备的度量驱动合成框架。

---

### 2. 论文提出的方法论

#### 基础框架：流匹配与蒸馏的延续
- 基于 **F5‑TTS** 教师模型，采用改进的分布匹配蒸馏（DMD²）训练一个仅需 **4 步采样** 的学生生成器。
- 学生生成器直接输出梅尔谱图，并通过预训练声码器合成波形；训练中结合多模态对抗训练与直接度量优化（对词错误率 WER 和说话人相似度 SIM 的梯度回传）。

#### 时长预测的强化学习方案
- **时长预测器架构**：编解码 Transformer，输入文本和语音提示帧，输出“剩余帧数”的概率分布（最大 30 秒，每类 100ms）。初始以交叉熵进行监督预训练。
- **策略建模**：将时长预测器视为随机策略 ⇡₀(L|x, p)，定义选择总时长 L 的动作。
- **群组相对策略优化（GRPO）**：
  1. 对每个输入采样 K 个时长 Lₖ（论文中 K=16）。
  2. 用 4 步学生模型分别生成语音。
  3. 奖励函数：`rₖ = log P_CTC( text | ASR( yₖ ) ) + β · cos( e_prompt , e_yₖ )`，β=3。
  4. 组内标准化得优势 Aₖ，计算比率 Rₖ，采用带截断的损失 `min( Aₖ·Rₖ, Aₖ·clip(Rₖ, 1±ε) )`，并加入与参考策略的 KL 散度正则项（β=0.04）。
- **优化目标**：仅更新时长预测器参数，生成器冻结，大幅降低 RL 计算开销。
- **质量控制**：使用温度采样鼓励探索；跳过组内奖励差异过小的批次。

#### 教师引导采样（Teacher‑Guided Sampling）
- **动机**：分布匹配蒸馏会导致学生模型“模态收缩”——在高概率区过拟合，丧失韵律多样性。
- **方法**：混合采样策略。前若干步由教师模型负责建立韵律和文本‑语音对齐的大尺度结构，达到预设的切换噪声水平 t_switch 后，剩余少量步骤交由学生模型完成声学细节的精炼。
- 在实验中，教师运行 14 步，学生运行 2 步，总步数 16，既恢复了多样性，又比纯教师推理快约 1.8×。

---

### 3. 实验设计

#### 数据集
- **训练**：Emilia 多语种数据集，过滤后约 **95k 小时** 英语与中文数据。
- **评估**：
  - Seed‑TTS test‑en：1088 条（来自 CommonVoice）。
  - Seed‑TTS test‑zh：2020 条（来自 DiDiSpeech）。
- 任务：**跨句零样本合成**，即用提示音频的说话人音色合成不同文本内容。

#### 对比基线
- **内部对比**：
  - 真实录音（Ground Truth）
  - F5‑TTS 教师模型（32步，无专门时长预测器）
  - DMOSpeech 2（学生 4 步，含 RL 时长预测器）
  - DMOSpeech 2 去除时长预测 RL（4步）
  - 教师引导采样变体（16步）
- **与外部 SOTA 对比**：CosyVoice 2、Spark‑TTS、MaskGCT、LLaSA‑8B 等，均使用官方发布的模型。

#### 评估指标
- **客观指标**：WER（英）/ CER（中）、说话人相似度 SIM、实时因子 RTF、基频变异系数 CV_f0 衡量韵律多样性。
- **主观指标**：比较平均意见分（CMOS）分别在自然度和相似度上以 DMOSpeech 2（4步）为锚点进行评分，共 320 个样本，统计显著性检验。

---

### 4. 资源与算力

- **GPU**：使用 **8 张 NVIDIA H100**。
- **训练量（步数）**：
  - F5‑TTS 教师：2M 步。
  - 学生模型 DMD 阶段：额外 200K 步。
  - 时长预测器监督预训练：85K 步。
  - GRPO 微调：1.5K 步（组大小 16）。
- 批次大小约 0.91 小时音频（教师阶段），优化器为 AdamW。

---

### 5. 实验数量与充分性

- **实验组合**：至少涉及 **5 种内部配置** 和 **5 种外部 SOTA 模型**，在英、汉两个测试集上完整报告了主、客观指标。
- **消融分析**：明确对比了 **有无 RL 时长优化**、**有无教师引导采样** 的影响，并通过 F0 分布图和变异系数定量分析多样性恢复。
- **统计检验**：CMOS 主观实验给出 p 值标记，保证结论显著性。
- **公平性**：所有模型同采样率、同评估协议、同测试集，外部模型使用官方权重，RTF 在同硬件上测量，设计较为严谨充分。

---

### 6. 论文的主要结论与发现

1. **时长预测 RL 有效**：DMOSpeech 2 将英/中 **WER/CER** 和 **SIM** 显著提升，甚至在某些条件下优于教师模型和真实录音。
2. **端到端度量优化闭环**：将时长预测纳入优化后，系统在自然度和相似度 CMOS 上相较无 RL 版本和教师模型均取得统计显著优势。
3. **教师引导采样恢复多样性**：在不牺牲速度的前提下，将 F0 多样性从学生模型的 0.464 恢复到 0.593（接近教师的 0.666），改善了韵律表现力。
4. **效率与质量兼得**：4 步学生采样 RTF 仅 0.032，较教师加速 5.3×，且各项客观/主观指标优于多数大参数 SOTA 模型。

---

### 7. 优点

- **创新性**：首次将 RL 应用于非自回归 TTS 的时长预测，通过直接奖励感知指标解决“分离式优化”痛点。
- **高效 RL 设计**：仅对时长预测器进行策略梯度更新，学生生成器冻结，生成只需 4 步，最大限度降低 RL 训练开销。
- **教师引导采样**：巧妙复用教师的前景（韵律结构）和后景（声学细节分工），同时缓解蒸馏模型多样性损失，兼具柔性与效率。
- **评估全面**：覆盖多语种、多模型、主客观指标和多样性分析，并提供开源代码和模型的承诺。

---

### 8. 不足与局限

- **奖励设计较简**：仅依赖 WER 和 SIM，未纳入韵律自然度、情感表达或听感舒适度等更细人类感知维度。
- **教师模型依赖**：教师引导采样需同时存储教师和学生模型，参数量翻倍（0.6B），且效果受限于教师性能。
- **训练数据单一**：仅使用 Emilia 数据集（虽有 95k 小时），模型在噪声、情感或特定领域文本上的泛化性未验证。
- **多语言有限**：仅评估了英、中两种语言，未涉及其它语种或代码切换场景。
- **伦理未充分展开**：虽提及声音伪造风险，但缺乏具体的对抗措施或检测机制的设计。
- **多样性评估偏粗**：仅用 F0 变异系数和分布图，未量化时长、能量或韵律模式的多维多样性。

---

（完）
