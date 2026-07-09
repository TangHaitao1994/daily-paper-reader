---
title: "UltraVoice: Scaling Fine-Grained Style-Controlled Speech Conversations for Spoken Dialogue Models"
title_zh: UltraVoice：面向口语对话模型的细粒度风格控制语音对话扩展
authors: "Wenming Tu, Guanrou Yang, Ruiqi Yan, Wenxi Chen, Ziyang Ma, Yipeng Kang, Kai Yu, Xie Chen, Zilong Zheng"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=UrWdRcLINM"
tags: ["query:speech-model"]
score: 9.0
evidence: 面向口语对话模型细粒度风格控制的大规模语音对话数据集
tldr: 当前口语对话模型缺乏精细的语音风格控制能力，限制了人机交互的自然度。本文发布 UltraVoice 数据集，包含超过830小时的语音对话，覆盖情感、语速、音量、口音、语言和组合风格六种风格维度。在 SLAM-Omni 和 VocalNet 等模型上微调后，模型的细粒度风格控制能力显著增强。该数据集填补了语音对话风格控制数据的空白，为构建更具表现力的语音助手提供了关键资源。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有口语对话模型忽略语音风格控制，无法满足人类交互的多样性需求。
method: 构建多维度风格标注的大规模语音对话数据集，并提供指令进行微调。
result: 微调后的模型在多项风格控制指标上大幅提升，交互更自然。
conclusion: UltraVoice 为口语对话系统的风格化交互奠定了数据基础。
---

## Abstract
Spoken dialogue models currently lack the ability for fine-grained speech style control, a critical capability for human-like interaction that is often overlooked in favor of purely functional capabilities like reasoning and question answering. To address this limitation, we introduce \textbf{UltraVoice}, the first large-scale speech dialogue dataset engineered for multiple fine-grained speech style control. Encompassing over 830 hours of speech dialogues, UltraVoice provides instructions across six key speech stylistic dimensions: emotion, speed, volume, accent, language, and composite styles. Fine-tuning leading models such as SLAM-Omni and VocalNet on UltraVoice significantly enhances their fine-grained speech stylistic controllability without degrading core conversational abilities. Specifically, our fine-tuned models achieve improvements of 29.12-42.33\% in Mean Opinion Score (MOS) and 14.61-40.09 percentage points in Instruction Following Rate (IFR) on multi-dimensional control tasks designed in the UltraVoice. Moreover, on the URO-Bench benchmark, our fine-tuned models demonstrate substantial gains in core understanding, reasoning, and conversational abilities, with average improvements of +10.84\% on the Basic setting and +7.87\% on the Pro setting. Furthermore, the dataset's utility extends to training controllable Text-to-Speech (TTS) models, underscoring its high quality and broad applicability for expressive speech synthesis.

---

## 论文详细总结（自动生成）

# UltraVoice 论文详细总结

## 1. 论文的核心问题与整体含义
- **研究动机**：当前的口语对话模型（Spoken Dialogue Models）过于侧重功能性的问答和推理能力，严重忽视了对语音风格的精细控制，这阻碍了人机交互的自然度和表现力。
- **核心问题**：如何赋予口语对话模型细粒度的语音风格控制能力，使生成的语音能在情感、语速、音量、口音、语言等多个维度上进行灵活调节。
- **整体含义**：通过提供首个大规模、多维度风格标注的语音对话数据集，为下一代更具人类表现力的语音助手和对话系统奠定数据基础。

## 2. 论文提出的方法论
- **核心思想**：将风格控制问题转化为数据驱动问题，构建一个包含丰富风格指令的大规模语音对话数据集，并通过对主流模型进行指令微调，使其学会遵循风格指令生成相应语音。
- **关键技术细节**：
  - **UltraVoice 数据集**：超过 830 小时的语音对话数据，每条对话配有细粒度风格控制指令。
  - **风格维度（六种）**：情绪（emotion）、语速（speed）、音量（volume）、口音（accent）、语言（language）以及组合风格（composite styles）。组合风格指同时控制多个风格维度。
  - **模型微调**：选取两款代表性的口语对话模型——SLAM-Omni 和 VocalNet，利用 UltraVoice 中的指令进行监督式微调，使模型输出风格受控的语音。
- **流程说明**：无特定公式，本质是标准的有监督微调流程，损失函数通常为语音生成目标与风格标签条件化的交叉熵或扩散损失（文中未详述）。

## 3. 实验设计
- **使用的数据集**：自建的 UltraVoice 数据集（用于训练风格控制）；URO-Bench 基准（用于评估核心对话能力）。
- **评估基准与指标**：
  - **多维度控制任务**：采用平均意见得分（MOS）和指令遵循率（IFR）来衡量模型在情感、语速、音量等单维度及组合维度上的风格控制质量。
  - **URO-Bench**：一个用于衡量语音对话模型理解、推理和对话能力的标准基准，分为 Basic 和 Pro 两种难度设置。
- **对比方法**：将微调后的模型与未微调的原始基座模型进行直接对比。
- **扩展应用**：验证数据集对可控文本转语音（TTS）模型训练的有效性，进一步说明数据的普适性。

## 4. 资源与算力
- **文中提及情况**：摘要及现有信息**未明确说明**所使用的 GPU 型号、数量、训练时长及总计算开销。关于模型微调和数据集构建所需的具体算力资源，论文正文可能有所披露，但当前提供的摘要中阙如，无法总结。

## 5. 实验数量与充分性
- **已进行的主要实验**：
  1. 分别在 SLAM-Omni 和 VocalNet 两个模型上进行微调，并评估风格控制能力（MOS、IFR）。
  2. 在 URO-Bench 上评估微调前后模型的核心对话能力（Basic 和 Pro 设置）。
  3. 训练可控 TTS 模型，验证数据集对语音合成任务的泛化价值。
- **充分性评价**：
  - **客观性与公平性**：在同一基座模型上对比微调前后，并采用公开基准 URO-Bench，对比方式较为公平。
  - **覆盖范围**：实验覆盖了风格控制、核心能力保留、跨任务迁移（TTS）三个关键维度，能较全面地支撑论文主张。但未被提及是否有消融实验（例如不同风格维度的贡献、数据量影响）或对更多口语模型的验证，依据摘要无法判断其全面性，初步看来实验设计合理且具有说服力。

## 6. 论文的主要结论与发现
- 使用 UltraVoice 微调后，模型在细粒度语音风格控制上取得**显著提升**：
  - MOS 提升 **29.12% ～ 42.33%**（相对于基线）。
  - IFR 绝对提升 **14.61 ～ 40.09 个百分点**。
- 增强风格控制的同时**不损害核心对话能力**，在 URO-Bench 上反而实现：
  - Basic 设置平均提升 **+10.84%**，Pro 设置平均提升 **+7.87%**。
- UltraVoice 数据集不仅能训练口语对话模型，还能有效训练可控 TTS 模型，证明其**高标准和广泛适用性**。

## 7. 优点与亮点
- **首个大规模细粒度风格控制语音对话数据集**：填补了领域空白，风格维度覆盖全面（含组合风格）。
- **巨大性能提升**：MOS 和 IFR 提升幅度惊人，且基准测试上的增益体现对模型整体能力的正向作用。
- **保持甚至提升核心能力**：与常见的“风格控制可能损伤语义”不同，证明了精心设计的数据集能同时强化风格表达与内容质量。
- **多任务范式验证**：通过口语对话和 TTS 两种任务验证数据集质量，增强了可信度与实用价值。
- **模型无关性**：对两个不同架构的模型均有效，表明方法的通用性。

## 8. 不足与局限
- **算力与资源信息缺失**：未能提供训练成本，难以评估方法的资源可行性与可复现门槛。
- **验证模型数量有限**：仅在 SLAM-Omni 和 VocalNet 上验证，未覆盖其他流行架构（如 LLaMA-style 语音模型、SpeechGPT 等），结论的泛化性有待进一步考察。
- **评估主观性**：MOS 依赖人工评分，其一致性、样本量以及评分者背景未在摘要中披露；IFR 的评判标准（人工或自动化）亦不明确，可能引入偏差。
- **数据集细节未知**：830 小时数据的来源、说话人数量、风格分布、组合风格的复杂程度等信息缺失，难以评估数据的多样性及潜在偏向。
- **应用限制**：真实场景下的风格控制粒度与鲁棒性（如对非指令性输入的默认行为）尚待验证，数据集微调的迁移效果能否在完全零样本条件下维持也未被充分讨论。
- **语言与口音覆盖**：虽包含口音和语言维度，但具体涵盖的语种与口音种类未说明，可能集中在中英等高资源语言。

（完）
