---
title: "Two Heads Are Better Than One: Audio-Visual Speech Error Correction with Dual Hypotheses"
title_zh: 双头胜一头：基于双假设的视听语音纠错
authors: "Sungnyun Kim, Kangwook Jang, Sungwoo Cho, Joon Son Chung, Hoi-Rin Kim, Se-Young Yun"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.26.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用大语言模型组合ASR和VSR假设进行视听语音识别的生成式纠错
tldr: 本文针对视听语音识别中如何有效融合多模态证据提升识别精度的问题，提出生成式纠错框架DualHyp。该框架利用大语言模型集成ASR和VSR的独立N-best假设，并设计RelPrompt机制提供模态时间可靠性提示，引导模型动态切换关注点进行准确纠正。实验结果显示该方法在噪声环境下显著优于单一模态和传统融合方法，推动了音视觉语音识别性能提升。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.26/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1629, \"height\": 522, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.26/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 803, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.26/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1650, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.26/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 794, \"height\": 423, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.26/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1599, \"height\": 909, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 799, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1646, \"height\": 697, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1650, \"height\": 1138, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 801, \"height\": 294, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 799, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 799, \"height\": 228, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 796, \"height\": 262, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 796, \"height\": 346, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1644, \"height\": 2358, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1645, \"height\": 2358, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1645, \"height\": 2301, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1647, \"height\": 231, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1642, \"height\": 487, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1650, \"height\": 1070, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.26/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 796, \"height\": 346, \"label\": \"Table\"}]"
motivation: 现有视听语音识别模型未能有效利用模态特异性证据在语言空间进行推理，限制了识别准确率。
method: 提出DualHyp框架，利用大语言模型组合独立的ASR和VSR模型的N-best假设，并引入RelPrompt噪声感知引导机制动态调整模态置信度以纠正错误。
result: 实验表明DualHyp显著提升了不同噪声条件下的识别准确率。
conclusion: 通过大语言模型整合多模态假设进行生成式纠错，为视听语音识别提供了新范式。
---

## Abstract
This paper introduces a new paradigm for generative error correction (GER) framework in audio-visual speech recognition (AVSR) that reasons over modality-specific evidences directly in the language space. Our framework, **DualHyp**, empowers a large language model (LLM) to compose independent N -best hypotheses from separate automatic speech recognition (ASR) and visual speech recognition (VSR) models. To maximize the effectiveness of DualHyp, we further introduce **RelPrompt**, a noise-aware guidance mechanism that provides modality-grounded prompts to the LLM. RelPrompt offers the temporal reliability of each modality stream, guiding the model to dynamically switch its focus between ASR and VSR hypotheses for an accurate correction. Under various corruption scenarios, our framework attains up to 57.7% error rate gain on the LRS2 benchmark over standard ASR baseline, contrary to single-stream GER approaches that achieve only 10% gain. To facilitate research within our DualHyp framework, we release the code and the dataset comprising ASR and VSR hypotheses at https://github.com/sungnyun/dualhyp.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- 现有基于大语言模型（LLM）的生成式纠错（GER）框架大多只利用单一模态（通常是音频）的 ASRN-best 假设进行转录纠正，在噪声环境下性能大幅下降。
- 虽然视听语音识别（AVSR）可利用视觉信息（如唇语）提升鲁棒性，但现有方法往往在特征层进行早期融合，容易因单模态噪声导致跨模态污染，且仍依赖单一识别模型的假设集。
- 本文希望回答：**能否通过显式保持模态特异性通路，在语言空间中利用 LLM 对独立生成的音频和视觉假设进行组合推理，从而显著提升噪声下的纠错能力？**

### 2. 论文提出的方法论

#### 2.1 核心思想
- 提出 **DualHyp** 框架，将模态融合延迟到语言空间：利用独立的 ASR 和 VSR 模型各自生成 N-best 假设列表 \( \mathcal{H}_{\text{asr}} \) 和 \( \mathcal{H}_{\text{vsr}} \)，再组合成双流假设 \( \mathcal{H}_{\text{dual}} \)，由 LLM 在文本层面进行组合推理和纠正。
- 进一步引入 **RelPrompt**，一种噪声感知引导机制：通过轻量预测器为每个模态流预测时间维度的可靠性标签（Clean/Noisy/Mixed），并将这些可靠性 mask 以文本提示（如 [C][N][N][M]…）的形式注入 LLM 输入，指导模型动态切换对 ASR 和 VSR 假设的关注。

#### 2.2 关键技术细节
- **假设生成**：ASR 用 Whisper-large-v3，VSR 用 BRAVEn-large（或针对数据集微调的版本），均采用 beam search 生成 5-best（或更多）假设。
- **LLM 微调**：在 TinyLlama (1.1B)、Phi-2 (2.7B)、Llama-3.2 (3B) 等模型上使用 LoRA（秩 r=16）进行高效微调。
- **可靠性预测器**：基于 1D 卷积网络，使用 ASR/VSR 编码器的中间特征，按 0.4 秒片段预测每个片段的离散噪声标签。
- **训练**：将可靠性 mask 序列与双假设一并输入 LLM，直接优化下式：
  
  \[
  \hat{y} = \arg\max_{y} P(y \mid \mathcal{H}_{\text{dual}}, m^a, m^v; \theta_{\text{LLM}})
  \]

  m^a, m^v 分别为音频和视频的可靠性 mask 序列。

### 3. 实验设计

#### 3.1 数据集与噪声场景
- **主数据集**：LRS2（英式英语，BBC 视频），扩充实验增加 LRS3（TED/TEDx 视频）和 LRS2 预训练集（HR）。
- **多语言评估**：MuAViC（涵盖 8 种语言），添加多语言人声噪声（babble）。
- **噪声协议**：系统性添加音频噪声（人声、音乐、自然噪声、多人交谈声，信噪比 -10 dB 到 10 dB 不等）和视觉破坏（目标遮挡、手部遮挡、像素化、模糊），构成多种组合场景。

#### 3.2 对比方法
- **单模态基线和早期融合方法**：
  - GER（标准单流 N-best 纠错）。
  - RobustGER（针对音频噪声鲁棒设计的 GER 变体）。
  - LipGER（通过适配器注入视觉特征，但仍用单一 ASR 假设流）。
  - GER w/ Auto-AVSR（使用早期融合 AVSR 模型生成的假设）。
- **无 LLM 后处理**：ROVER（基于假设合并投票）。
- **消融变体**：
  - DualHyp 无 LLM（仅 ROVER）。
  - RelPrompt 用于单模态流（无 DualHyp）。
  - DualHyp 与 DualHyp + RelPrompt。
- **预言机（Oracle）**：N-best oracle 和组合 oracle 提供理论上限。

#### 3.3 评估指标
- 词错误率（WER），并计算相对 WER 降低（WERR）。

### 4. 资源与算力

- 训练硬件：单张 NVIDIA A6000 GPU。
- 训练时长：完整 DualHyp + RelPrompt 模型约 8 小时。
- LLM 参数量：TinyLlama 约 1.1B，Phi-2 约 2.7B，Llama-3.2 约 3.2B。
- 可靠性预测器：约 1.1M 参数，轻量级。
- 训练数据量：LRS2 训练集约 45k 样本，高资源场景额外使用 LRS3（59 小时）或 LRS2 预训练集（195 小时）。

### 5. 实验数量与充分性

- **实验组数**：进行了约十余组对照实验，覆盖不同噪声类型、不同模态损坏条件、不同数据集、不同 LLM 大小、多语言场景、数据规模消融等。
- **充分性分析**：
  - 系统性覆盖了多种音频和视觉损坏组合，评估维度全面。
  - 包含详细的消融实验（去除 LLM 的 ROVER 对比、RelPrompt 单独使用、使用 AVSR 头替代 VSR 头、不同 top-k 假设等）。
  - 对比方法涵盖当前主流的单流和特征融合 GER 方法，基线公平（均在相同噪声数据上重新训练至收敛）。
  - 提供了 oracle 分析表明双流假设的潜力上限以及实际性能差距。
- **客观性**：所有方法在相同噪声协议下训练和评估，且公开代码和假设数据集，利于复现。

### 6. 论文的主要结论与发现

- 在语言空间中组合独立的 ASR 和 VSR 假设，能够显著提升噪声环境下的纠错性能，相对 ASR 基线 WER 降低最高达 57.7%（LRS2 视觉噪声场景）。
- 单流 GER 方法在低信噪比时几乎无增益，而 DualHyp 可有效利用视觉信息避免性能崩溃。
- RelPrompt 进一步增强了动态模态选择能力，尤其在低信噪比下带来额外增益。
- 早期融合的 AVSR 假设过分依赖音频，在音频噪声严重时效果甚至不如独立的 VSR 假设，验证了模态解耦的重要性。
- 框架具有良好的可扩展性：更大 LLM、更多训练数据均能进一步提升性能。
- 在多语言实验中，VSR 模型质量成为瓶颈；当 VSR 本身很差时，双假设方法提升有限甚至可能变差。

### 7. 优点

- **范式创新**：首次在视听 GER 中显式采用模态解耦、语言空间晚期融合，避免了特征层融合的跨模态污染。
- **可解释性与可控性**：RelPrompt 提供了时间维度的可靠性信号，既增强性能又使模型决策更透明。
- **模块化设计**：可灵活接入现成的 ASR、VSR 和 LLM，无需重新设计端到端模型。
- **实验深度**：覆盖多种噪声类型、多数据集、多语言，包含 oracle 基准和丰富的消融分析，论证扎实。
- **社区贡献**：公开代码和假设数据集，降低后续研究门槛。

### 8. 不足与局限

- **依赖上游识别器质量**：当 ASR 或 VSR 模型表现极差时（如某些多语言场景），框架纠错能力受限。
- **计算延迟**：虽可并行处理音视频，但 LLM 纠错步骤为串行，且包含多个模型模块，实时部署存在挑战。
- **仅限英语为主的验证**：多语言实验中 VSR 模型较弱，结果主要基于英语数据集；缺乏高质量多语言 VSR 模型限制了推广性。
- **噪声模拟的局限性**：虽有多种合成损坏，但仍与真实世界噪声分布有差距。
- **可靠性预测粒度固定**：片段 0.4 秒的固定划分可能不完全匹配所有语速和噪声模式。
- **失败案例分析不足**：虽展示了成功和失败案例，但对 LLM 内部幻觉或错误语义关联的深层机制探讨有限。

（完）
