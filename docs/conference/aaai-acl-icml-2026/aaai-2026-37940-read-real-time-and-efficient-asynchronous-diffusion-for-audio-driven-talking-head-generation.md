---
title: "READ: Real-time and Efficient Asynchronous Diffusion for Audio-driven Talking Head Generation"
title_zh: READ：面向音频驱动说话人头部生成的实时高效异步扩散框架
authors: "Haotian Wang, Yuzhe Weng, Jun Du, Haoran Xu, Xiaoyan Wu, Shan He, Bing Yin, Cong Liu, Jianqing Gao, Qingfeng Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37940/41902"
tags: ["query:speech-model"]
score: 8.0
evidence: 提出实时扩散Transformer框架，通过压缩视频潜在空间和SpeechAE实现音频-视觉对齐
tldr: 扩散模型在说话人头部生成中取得进展，但推理速度极慢。本文提出READ框架，通过时序VAE学习高度压缩的视频潜在空间以减少Token数量，并设计预训练SpeechAE生成压缩语音潜在码以增强音频-视觉对齐，实现了实时高效的异步扩散生成，大幅提升了推理速度。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散模型在说话人头部生成中推理速度慢，难以实用。
method: 使用时序VAE压缩视频潜在空间，并引入SpeechAE进行音频-视觉对齐。
result: READ实现了实时生成，显著加速了扩散推理。
conclusion: 通过压缩和对齐实现了高效实时的音频驱动视觉生成。
---

## Abstract
The introduction of diffusion models has brought significant advances to the field of audio-driven talking head generation. However, the extremely slow inference speed severely limits the practical implementation of diffusion-based talking head generation models. In this study, we propose READ, a real-time diffusion-transformer-based talking head generation framework. Our approach first learns a spatiotemporal highly compressed video latent space via a temporal VAE, significantly reducing the token count to accelerate generation. To achieve better audio-visual alignment within this compressed latent space, a pre-trained Speech Autoencoder (SpeechAE) is proposed to generate temporally compressed speech latent codes corresponding to the video latent space. These latent representations are then modeled by a carefully designed Audio-to-Video Diffusion Transformer (A2V-DiT) backbone for efficient talking head synthesis. Furthermore, to ensure temporal consistency and accelerated inference in extended generation, we propose a novel asynchronous noise scheduler (ANS) for both the training and inference processes of our framework. The ANS leverages asynchronous add-noise and asynchronous motion-guided generation in the latent space, ensuring consistency in generated video clips. Experimental results demonstrate that READ outperforms state-of-the-art methods by generating competitive talking head videos with significantly reduced runtime, achieving an optimal balance between quality and speed while maintaining robust metric stability in long-time generation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：扩散模型（diffusion models）在音频驱动说话人头部生成（audio-driven talking head generation）中取得了显著进展，但其推理速度极慢（通常需数十到数百秒生成5秒视频），严重阻碍了实际应用。
- **研究动机**：现有方法普遍使用无时序压缩的变分自编码器（VAE）来保留唇音同步精度，但导致输入token数巨大、计算成本高；同时去噪步数多、长视频生成需要额外网络或后处理来维持片段一致性，进一步增加了延迟。因此，亟需一种能实时生成、且保持高视听质量的框架。
- **整体含义**：提出首个端到端、实时、基于扩散Transformer的说话人头部生成框架READ，通过大幅压缩视频潜在空间、同步压缩语音特征、异步噪声调度等创新，在推理速度、视频质量、唇形同步和长期一致性之间实现最优平衡。

### 2. 论文提出的方法论
#### 2.1 总体架构
READ由三个核心组件构成：
- **时序VAE（Temporal VAE）**：采用LTX-VIDEO的VAE，每token对应32×32×8像素的高时空压缩比，将视频从 `X(0) ∈ R^{H×W×F×D_v}` 压缩为 `Z(0) ∈ R^{h×w×f×d_v}`，大幅减少扩散模型处理的token数量。
- **音频自编码器（SpeechAE）**：用于将原始语音特征同步压缩到与视频潜变量相同的时间压缩比。其结构包含冻结的Whisper-tiny编码器提取特征 `S_{1:F}`，随后通过可训练的线性维度变换和1D因果卷积的时序下采样/上采样模块进行压缩与重建。通过自监督预训练（MSE损失和帧级对比损失）使得压缩后的语音潜码 `C` 保有原始时序信息并与视频潜空间对齐。
- **A2V-DiT（Audio-to-Video Diffusion Transformer）**：在DiT块中融合自注意力、3D全注意力（处理文本描述）和帧级2D交叉注意力（处理音频条件）。音频条件采用 `SpeechAE` 输出的帧对齐潜码 `C`，通过交叉注意力实现精确的唇形同步。

#### 2.2 异步噪声调度器（Asynchronous Noise Scheduler, ANS）
- **异步加噪前向过程（训练）**：对视频潜变量不同位置施加不同强度的噪声。将第一帧作为“运动帧”，参考帧 `z_R` 拼接在前，运动帧和后续帧使用不同的噪声时间步 `t = [0, t1, ..., t2]`，且 `t1 < t2`。噪声时间步从偏移逻辑正态分布采样。加噪公式为 `Z(t) = (1 - t) ⊙ Z(0) + t ⊙ ϵ`，可形成带有部分信息残留的运动帧。
- **异步运动引导反向过程（推理）**：将长视频划分为具有单帧重叠的多个片段。首个片段自由推理（所有帧施加相同强度噪声），后续片段的第一个帧被替换为前一片段的最后一帧，并对该帧施加较低噪声强度（`t_{i+1}`），其余帧噪声强度更高（`t_i`），从而利用前一帧的运动信息引导当前片段的生成，确保片段间时序一致性，且无需额外网络。
- 推理时还可采用 **联合/分离无分类器引导（Joint-CFG / Split-CFG）** 来平衡质量与速度。Split-CFG 对参考图像和音频条件分别应用引导，可取得更优的唇形同步，但略增推理时间。

#### 2.3 训练流程
两阶段训练：
1. **SpeechAE 预训练**：自监督重建原始语音特征，使用 MSE + 帧级对比损失。
2. **A2V-DiT 端到端训练**：使用 Flow Matching 目标，基于 ANS 异步加噪过程训练网络预测向量场 `v = ϵ - Z(0)`，损失为 `L_FM = E ||v - S_θ(Z(t), C, z_R, t)||^2`。

### 3. 实验设计
- **数据集**：HDTF（高分辨率音视频）和 MEAD（情感丰富多说话人），随机划分 95% 训练、5% 测试，无身份重叠。
- **基准方法**：对比了多种最先进的开放式方法，包括端到端扩散模型（Sonic, EchoMimic, Hallo, FantasyTalking, AniPortrait）和运动空间扩散模型（AniTalker, Ditto）。所有模型在相同硬件、相同测试数据、相同视频长度（4.84秒/121帧）下评估，确保公平。
- **评估指标**：视觉质量（FID, FVD）；唇音同步（Sync-C 置信度、Sync-D 距离）；表情逼真度（E-FID）；效率（扩散主干网络平均运行时间 Runtime）。
- **用户调研**：18名参与者对 6 个模型生成的 12 组样本从唇音同步、运动平滑度、真实感、视频质量四个维度评分（1-5分）。

### 4. 资源与算力
- **GPU 使用**：训练和评估均在 NVIDIA A100 GPU 上进行，但未明确提及 GPU 数量、单卡还是多卡训练、训练总时长或迭代次数，仅说明训练和评估在同一设备完成。
- **推理配置**：默认采用 8 步采样、Split-CFG（α=2.0, β=6.0）。推理窗口长度与训练长度匹配（121帧），潜空间运动重叠为1帧。

### 5. 实验数量与充分性
实验设计较为系统，包括：
- **整体对比实验**：在 HDTF 和 MEAD 两个数据集上与 7 种 SOTA 方法对比（表1），涵盖多种指标。
- **消融实验**：
  - SpeechAE 有效性：三种配置（完整/无预训练/无 SpeechAE）验证其对唇音同步的影响（表2）。
  - ANS 有效性：定性可视化对比（图2），展示片段间一致性的改善。
  - 长视频生成稳定性：生成121帧、457帧、1017帧三种长度，评估指标稳定性（表3）。
  - 性能与运行时的权衡：测试不同推理步数（4~10步）和 CFG 策略（Split/joint），绘制曲线（图3）。
- **案例研究**：选取 HDTF 样本与其他方法可视化对比（图4）。
- **用户调研**：主观评估（表4）。
整体实验覆盖了客观量化、主观评价、消融、效率权衡等多个维度，对比方法均为近期高水平模型，且在同等条件下比较，实验较为充分且公平。

### 6. 论文的主要结论与发现
- READ 实现了实时端到端扩散生成，主干网络运行时间仅 4.421 秒（生成4.84秒视频），速度远超其他对比方法（Sonic 83.584s，Hallo 212.002s 等），基本达到1:1的实时比例。
- 在 HDTF 和 MEAD 上，READ 在 Sync-C 指标上全面超越所有方法，并在 FID、FVD、Sync-D、E-FID 上达到或接近最优，尤其在情感丰富的 MEAD 数据集上展现最佳表情生成保真度。
- 消融证实 SpeechAE 预训练及模块本身对于同步压缩和唇音对齐至关重要；ANS 能有效消除片段间的不连续性，并在长时间生成中保持指标稳定。
- 通过调节 CFG 策略和推理步数，可在质量与速度之间灵活权衡（Split-CFG 质量更优，Joint-CFG 速度更快）。

### 7. 优点
- **高效实时**：首次实现扩散模型在说话人生成任务上的实时推理，通过时序 VAE 与 SpeechAE 大幅减少 token 数和对齐成本。
- **创新的音频对齐**：设计 SpeechAE 与视频压缩同步的语义对齐机制，配合自监督预训练，在高度压缩的潜空间中仍保持高精度唇音同步。
- **异步噪声调度**：提出 ANS，在无需额外网络或重叠融合的情况下实现长视频片段间平滑过渡和一致性，同时支持运动引导生成，方法优雅且计算开销低。
- **系统化实验验证**：包含多数据集、多指标、消融、效率权衡、用户调研，论证充分且相互印证。
- **开源基线对比公平**：所有评测在相同硬件、相同测试数据下进行，增强了结果的可信度。

### 8. 不足与局限
- **资源未详细说明**：未提供训练所需的 GPU 数量、总墙钟时间、模型参数量等关键复现信息，不利于资源评估。
- **身份泛化性与偏置风险**：仅测试了 HDTF 和 MEAD 两个数据集，未在野生（in-the-wild）视频或更多样的说话人上验证，可能存在过拟合或对人种/表情分布偏置。
- **长视频生成未量化一致性指标**：尽管展示了长视频指标稳定性和定性差异图，但缺乏片段间一致性的直接量化指标（如光学流平滑度或监督对比分数）。
- **异步调度参数敏感**：异步时间步设计的超参数（如采样分布）如何选取未有详细分析，实际部署时可能需要额外调参。
- **应用限制**：目前仅为人脸说话生成，未涉及全身或复杂背景场景；此外，实时生成虽达到1:1，但仍需 A100 级别 GPU，消费级设备上可能无法完全实时。

（完）
