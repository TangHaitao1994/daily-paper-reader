---
title: "BatonVoice: An Operationalist Framework for Enhancing Controllable Speech Synthesis with Linguistic Intelligence from LLMs"
title_zh: BatonVoice：基于操作主义框架利用大语言模型增强可控语音合成
authors: "Yue Wang, Ruotian Ma, Xingyu Chen, Zhengliang Shi, Wanshun CHEN, Huang Liu, Jiadi Yao, Xin He, Qu Yang, Qingxuan Jiang, Fanghua Ye, Juntao Li, Min Zhang, Zhaopeng Tu, Xiaolong Li, Liefeng Bo"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=YsVQBe0HNA"
tags: ["query:speech-model"]
score: 9.0
evidence: 利用大语言模型生成声学特征计划的可控文本转语音框架，提升合成质量
tldr: 目前基于大模型的语音合成未能充分利用其指令跟随能力，可控性有限。本文提出 BatonVoice 框架，借鉴操作主义思想，将指令理解与语音生成解耦：LLM 充当指挥，理解用户文本指令并生成包含显式声学特征的计划，再由语音生成器执行。实验表明，该框架显著提升了 TTS 的控制精度与合成质量，实现了更自然、更具表现力的可控语音合成。该研究为利用大语言模型的智能提升语音合成交互性开辟了新路径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有大模型语音合成未能充分挖掘 LLM 的指令跟随潜力，导致可控性差。
method: 提出解偶指令理解与语音生成的框架，LLM 生成声学特征计划以控制合成。
result: 提升了文本转语音的指令跟随精度和自然度，合成质量更高。
conclusion: BatonVoice 为可控 TTS 提供了新范式，充分发挥了 LLM 的语言智能。
---

## Abstract
The rise of Large Language Models (LLMs) is reshaping multimodel models, with speech synthesis being a prominent application. However, existing approaches often underutilize the linguistic intelligence of these models, typically failing to leverage their powerful instruction-following capabilities. This limitation hinders the model's ability to follow text instructions for controllable Text-to-Speech~(TTS). To address this, we propose a new paradigm inspired by operationalism that decouples instruction understanding from speech generation. We introduce BatonVoice, a framework where an LLM acts as a conductor, understanding user instructions and generating a textual plan -- explicit vocal features (e.g., pitch, energy). A separate TTS model, the orchestra, then generates the speech from these features. To realize this component, we develop BatonTTS, a TTS model trained specifically for this task. Our experiments demonstrate that BatonVoice achieves strong performance in controllable and emotional speech synthesis, outperforming strong open- and closed-source baselines. Notably, our approach enables remarkable zero-shot cross-lingual generalization, accurately applying feature control abilities to languages unseen during post-training. This demonstrates that objectifying speech into textual vocal features can more effectively unlock the linguistic intelligence of LLMs.

---

## 论文详细总结（自动生成）

# BatonVoice 论文总结

## 1. 论文的核心问题与整体含义
- **研究背景**：大语言模型（LLMs）正在重塑多模态模型，语音合成是其中的重要应用。但当前主流方法常常未能充分释放 LLM 的语言智能，尤其在指令跟随能力方面表现不足。
- **核心问题**：如何让 LLM 的指令理解能力真正服务于可控文本转语音（TTS），突破现有模型在精细控制（如情感、韵律、跨语言）上的局限。
- **整体含义**：提出一种操作主义（operationalism）的新范式，将指令理解与语音生成解耦，使 LLM 像指挥家那样生成明确的声学特征计划，再由专门的 TTS 模型执行，从而实现更精准、更自然的可控语音合成，并展示出出色的零样本跨语言泛化能力。

## 2. 方法论
- **核心思想**：借鉴“操作主义”哲学，将抽象的语音指令转化为可操作的、文本化的声学特征计划，以此作为 LLM 理解指令和 TTS 生成语音之间的桥梁。
- **框架构成**：
  - **LLM 指挥家（Conductor）**：以文本形式接收用户指令（如“以高兴的语气阅读”或“提高音调”），输出一个**文本计划**，该计划明确描述了声学特征，如音高（pitch）、能量（energy）、语速等的时间变化。
  - **BatonTTS 乐团（Orchestra）**：专为此任务训练的 TTS 模型，接收音素序列和上述声学特征计划，生成最终波形。该 TTS 模型并不是从文本一步生成语音，而是按照计划“演奏”语音。
- **关键设计**：
  - 解耦指令理解与语音生成，使得 LLM 的泛化能力可以单独评估和优化。
  - 声学特征以文本形式离散化表达，便于 LLM 处理和生成。
- **算法流程**：
  1. 用户输入文本和风格/控制指令。
  2. LLM 根据指令生成包含声学特征序列的计划（文本形式）。
  3. BatonTTS 接收文本和该计划，合成语音。
- **训练**：BatonTTS 专门训练以遵循这类文本化声学特征计划，并在训练数据中覆盖多语言和多种情感、韵律条件。

## 3. 实验设计
- **数据集与场景**：
  - 用于训练和评估的情感/可控 TTS 数据集（文中未详细列出具体名称，但从描述可知涉及 emotion TTS 和可控属性如音高、能量）。
  - 零样本跨语言泛化实验：测试模型在训练后未见语言上的表现。
- **Benchmark 与对比方法**：
  - 与现有的**开源和闭源最强基线**进行对比，包括已有的基于 LLM 的语音合成方法。
  - 评测指标：指令跟随精度（控制准确性）、合成自然度（可能通过 MOS 或类似指标）、情感表达准确度等。
- **主要实验场景**：
  - 可控语音合成（多维度特征控制）。
  - 情感语音合成。
  - 零样本跨语言控制泛化（实现未见语言的精确特征控制）。

## 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量或训练时长等具体算力信息。基于提供的元数据和摘要，没有提及硬件配置或训练成本。
- 这一点需要读者参考论文正文获取详细信息，当前摘要与元数据未覆盖。

## 5. 实验数量与充分性
- **实验组数**：至少包含以下几组：
  - 与多个强基线在可控 TTS 上的对比实验。
  - 情感合成专项评估。
  - 跨语言零样本泛化实验。
  - 消融实验（验证框架各组件贡献，如指挥家 LLM 的作用、文本计划的有效性等，虽未明确列出但方法论中隐含设计对比）。
- **充分性与公平性**：
  - 对比方法包括开源和闭源的强基线，覆盖竞争性方案。
  - 实验设计覆盖多种维度（控制精度、自然度、情感、跨语言），较为全面。
  - 零样本泛化实验进一步验证了方法的稳健性，避免了在单一数据集上的过拟合。
- **潜在不足**：当前信息未提供用户主观评估细节或大规模众包测试的具体设计，无法评判统计显著性。

## 6. 主要结论与发现
- **性能提升**：BatonVoice 在可控和情感语音合成上超越了强基线，指令跟随精度更高，合成语音更自然。
- **框架有效性**：将语音通过文本声学特征“客体化”（objectifying speech into textual vocal features）能更有效地释放 LLM 的语言智能。
- **跨语言泛化**：BatonVoice 展现出惊人的零样本跨语言控制能力，即使后训练阶段未见某语言，也能在其上精确应用特征控制，说明该框架具备强大的语言泛化性。
- **新范式**：为可控 TTS 开辟了一种 LLM 作为指挥、TTS 作为执行者的新范式，提高交互性和表现力。

## 7. 优点
- **创新性思想**：将操作主义哲学引入 TTS，提出指令理解与生成解耦，相比现有把 LLM 直接融入声学模型的方法更优雅。
- **显式且可解释的控制**：声学特征计划以文本形式呈现，使得控制过程透明、可审计，便于调整和调试。
- **强大的泛化能力**：零样本跨语言泛化表现突出，证明模型学到的是语言无关的声学控制知识。
- **实用性强**：框架模块化，LLM 和 TTS 可独立升级，未来可替换更强大的 LLM 或更高保真的 TTS 后端。

## 8. 不足与局限
- **算力信息缺失**：摘要未提及训练资源需求，无法判断实际可行性和成本。
- **数据集细节不详**：没有提供训练/测试所用语料库规模、语言种类、情感类别数量等，难以评估覆盖范围。
- **主观评估不确定性**：虽然报告自然度提升，但缺少详细的 MOS 测试设置、置信度区间等统计学信息，结论可靠性受限。
- **控制粒度限制**：声学特征计划是离散的文本形式，可能有量化误差，极端细微的韵律变化（如细微的颤音、个性化音色）的控制能力未讨论。
- **实际应用偏差风险**：零样本跨语言实验可能基于特定相近语系，对远距离语言（如完全不同的声调语言）的控制效果需进一步验证；且模型在真实嘈杂环境或长文本上的鲁棒性未经检验。

（完）
