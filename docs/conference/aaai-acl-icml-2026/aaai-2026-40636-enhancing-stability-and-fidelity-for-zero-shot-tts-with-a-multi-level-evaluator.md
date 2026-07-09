---
title: Enhancing Stability and Fidelity for Zero-Shot TTS with a Multi-Level Evaluator
title_zh: 基于多级评估器的零样本文本到语音稳定性与保真度增强
authors: "Hualei Wang, Na Li, Chuke Wang, Shu Wu, Zhifeng Li, Dong Yu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40636/44597"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用多级评估器纠正错误语音片段并对齐偏好以提升TTS质量
tldr: 针对零样本文本到语音合成中存在的发音错误、噪声和质量退化问题，本文提出Vox-Evaluator多级评估器，能够定位错误片段边界并提供整体质量评估，通过指导错误纠正和偏好对齐提升合成的稳定性和保真度，实验表明所提方法有效增强了TTS模型的鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 零样本TTS仍存在稳定性与保真度不足，如发音错误和可闻噪声。
method: 提出Vox-Evaluator多级评估器，识别错误片段并指导纠正，进行偏好对齐。
result: 实验表明有效提升零样本TTS的稳定性与保真度。
conclusion: 多级评估器为提升TTS鲁棒性提供了有效途径。
---

## Abstract
Recent advances in zero-shot text-to-speech (TTS), driven by language models, diffusion models and masked generation, have achieved impressive naturalness in speech synthesis. Nevertheless, stability and fidelity remain key challenges, manifesting as mispronunciations, audible noise, and quality degradation. To address these issues, we introduce Vox-Evaluator, a multi-level evaluator designed to guide the correction of erroneous speech segments and preference alignment for TTS systems. It is capable of identifying the temporal boundaries of erroneous segments and providing a holistic quality assessment of the generated speech. Specifically, to refine erroneous segments and enhance the robustness of the zero-shot TTS model, we propose to automatically identify acoustic errors with the evaluator, mask the erroneous segments, and finally regenerate speech conditioning on the correct portions. In addition, the fine-gained information obtained from Vox-Evaluator can guide the preference alignment for TTS model, thereby reducing the bad cases in speech synthesize. Due to the lack of suitable training datasets for the Vox-Evaluator, we also constructed a synthesized text-speech dataset annotated with fine-grained pronunciation errors or audio quality issues. The experimental results demonstrate the effectiveness of the proposed Vox-Evaluator in enhancing the stability and fidelity of TTS systems through the speech correction mechanism and preference optimization.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：基于语言模型、扩散模型与掩码生成的零样本文本到语音合成（Zero-shot TTS）在自然度上已取得长足进步，但稳定性和保真度仍是关键短板，常表现为发音错误、可闻噪声、异常停顿及整体质量下降。
- **核心问题**：现有错误修正方法依赖复杂的 ASR、MFA 等外部工具，且难以定位低音频质量片段；偏好对齐方法受限于昂贵的细粒度偏好标注和复杂的反馈收集流程。
- **整体含义**：本文提出统一的多级评估器 Vox-Evaluator，能够同时定位错误片段的时间边界并进行整体质量评估，既能指导错误语音段的纠正，又能为偏好对齐提供细粒度奖励信号，从而提升零样本 TTS 的稳定性和保真度。

## 2. 论文提出的方法论
### 2.1 Vox-Evaluator 多级评估器架构
- **语音编码器**：基于 wav2vec2.0 的 Transformer 结构，提取语音隐特征。
- **单元编码器**：采用 BART 编码器架构，接收双模态输入（语音编码器输出的语义 token + 文本音素 token），通过双向自注意力实现跨模态对齐。
- **文本解码器**：BART 解码器，自回归生成目标文本序列，用于检测发音内容错误。
- **时间戳预测器**：两层 Bi‑LSTM + 线性层 + Sigmoid，预测错误语音片段的时间范围（逐帧分类）。
- **质量评分预测器**：三层 MLP（LN + GeLU），输出整体质量评分（1‑10）。
- **训练策略**：多任务学习，总损失为：
  - 逐帧焦点损失（focal loss）——时间戳预测；
  - 均方误差（MSE）——质量评分；
  - 词级别交叉熵损失——文本解码。

### 2.2 合成语音校正（Speech Correction）
- **错误检测**：Vox-Evaluator 输出预测文本 \(\hat T\) 和错误片段时间范围 \(\hat S\)，然后利用动态时间规整（DTW）将预测文本与目标文本对齐，定位内容错配。
- **掩码与再生**：基于 \(\hat S\) 生成语音掩码，对错误区域及其安全余量进行遮蔽；利用支持语音编辑的 TTS 模型（如 F5‑TTS、VoiceCraft），根据正确部分和文本提示重新生成被遮蔽片段。
- **评估筛选**：仅对综合评分（质量分数 + WER）较低的样本进行修正，控制计算开销；迭代 2 次（实验显示该次数最佳）。

### 2.3 细粒度偏好对齐（Fine‑grained Preference Alignment）
- **偏好对构建**：从同一条件下生成的两个语音样本中，由 Vox-Evaluator 判定高保真样本为“获胜” \(x_w\)，含错误或低质量样本为“失败” \(x_l\)。
- **细粒度 DPO 损失**：不再对整句计算损失，而是利用 Vox-Evaluator 的时间戳标注，仅对错误片段所在部分计算式(3)中的去噪偏好损失，避免全句信息冗余与过优化。

## 3. 实验设计
### 3.1 数据集与场景
- **Vox-Evaluator 训练/验证/测试**：自建的 FGES 合成语音数据集（约 22k 样本），涵盖常见错误（发音、遗漏）、重复问题、标点停顿问题以及异常噪声/停顿；标注了帧级错误时间范围和句子级质量评分（1‑10）。按 20k/1k/1k 划分。
- **TTS 评估基准**：
  - Seed‑TTS test‑en
  - LibriSpeech‑PC test‑clean
  - TTSDS2（稳定性测试）
- **对比方法**：
  - 评估器对比：fine‑tuned wav2vec2.0、SenseVoice、Whisper
  - TTS 模型对比：CosyVoice、Llasa‑1B、VoiceCraft、F5‑TTS、VoiceBox、MaskGCT等；并与经过 VoiceCraft refine、F5‑TTS refine 的版本比较。

### 3.2 评价指标
- **评估器**：错误片段定位用 IOU 和正确样本的 MSE；质量评分用 utt‑PCC 和 sys‑SRCC；转写准确率用 WER。
- **TTS 质量**：客观指标 WER、说话人相似度 SIM‑o；自动 MOS（UTMOS）；主观 CMOS。

## 4. 资源与算力
- 文中仅给出 Vox-Evaluator 的训练细节：batch size 24，50 epochs，Adam 优化器，学习率 1e‑4，长音频切割为 30 秒片段。
- **未明确说明**使用的 GPU 型号、数量及实际训练时长。无其他 TTS 模型训练或推理的算力报告。

## 5. 实验数量与充分性
- **实验组数丰富**：
  - Vox-Evaluator 各模块性能对比表（表 2，含无预训练版本）。
  - 多个零样本 TTS 模型在多个数据集上的校正效果对比（表 3）。
  - 消融实验：单独/组合错误检测与质量评估对故障率和 UTMOS 的影响（表 4）。
  - 迭代次数对故障率下降的影响曲线（图 5）。
  - 细粒度偏好对齐在训练步数增加下 WER 和 SIM‑o 的变化曲线（图 3）。
- **充分性与公平性**：涵盖自回归与非自回归模型，使用多个公开基准，对比主流方法；消融细致，迭代次数选择有依据。但实验仅限于英文数据，跨语言泛化未验证；部分指标依赖自动模型，可能存在偏差。

## 6. 论文的主要结论与发现
- Vox-Evaluator 在错误片段定位（IOU 0.782）和质量评分预测（utt‑PCC 0.541）上优于基线，且以较少参数量获得更优的转写 WER（2.64%）。
- 语音校正流程能显著降低零样本 TTS 的错误率：F5‑TTS 在 Seed‑TTS 上 WER 从 1.73% 降至 1.42%，VoiceCraft 从 7.56% 降至 5.11%；在 LibriSpeech‑PC 上也表现出一致的改进。
- 细粒度偏好对齐可进一步压低 WER，提升说话人相似度，但训练步数过多时会因奖励饱和而性能下降。
- 消融实验表明，错误检测对降故障率贡献最大，与质量评估联合使用可稍优于单独使用；迭代 2 次修正效果最佳。

## 7. 优点（方法与实验亮点）
- **统一多级评估器**：一次前向即可同时给出错误片段定位、内容偏差和整体评分，无需拼接 ASR、MFA 等多个工具，简化了修正和反馈通路。
- **轻量且可移植的语音校正**：只需评估器 + 支持编辑的 TTS 模型，无需重新训练原 TTS 模型，即可后处理提升任意零样本 TTS 输出的稳定性。
- **细粒度偏好优化**：利用时间戳信号将偏好损失聚焦于错误区域，避免全句优化的信息冗余，更高效地改善局部质量问题。
- **自建标注数据集**：贡献了细粒度错误与质量标注的合成语音数据集 FGES，为评估器训练提供基础，且错误类型多样。
- **实验设计扎实**：从评估器自身指标到多个 TTS 模型在多个测试集上的端到端表现，以及迭代次数、消融等，论证链条完整。

## 8. 不足与局限
- **语言与数据覆盖**：实验限于英文，数据集和测试集均为英文，跨语言鲁棒性未知；FGES 为合成语音标注，其错误分布可能与实际应用场景有偏差。
- **依赖性**：校正过程高度依赖编辑 TTS 模型的支持，对于不支持蒙版编辑的 TTS 模型无法直接应用。
- **计算开销未量化**：未报告 Vox-Evaluator 推理耗时、校正迭代带来的额外算力成本，实际部署效率缺乏参考。
- **主观评估受限**：大部分质量评估依赖自动指标（UTMOS、WER）和自动 MOS 预测，仅部分 CMOS 主观实验；偏好对齐的效果未通过大规模人类评估验证。
- **潜在的过拟合风险**：FGES 由特定 TTS 模型生成，训练出的评估器可能偏向该模型产生的错误类型，泛化到其他 TTS 的稳定性需更多检验；偏好对齐训练步数敏感，有可能过拟合。
- **未探索其他模态**：仅关注文本和语音，未考虑说话人风格、情绪等更高级保真度维度。

（完）
