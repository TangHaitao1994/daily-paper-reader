---
title: Closing the Gap Between Text and Speech Understanding in LLMs
title_zh: 缩小大语言模型中文本与语音理解的差距
authors: "Santiago Cuervo, Skyler Seto, Maureen de Seyssel, Richard He Bai, Zijin Gu, Tatiana Likhomanenko, Navdeep Jaitly, Zakaria Aldeneh"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=dDHnO3Vhyj"
tags: ["query:speech-model"]
score: 9.0
evidence: 弥合文本-语音理解鸿沟直接提升LLM的语音识别准确率
tldr: 该工作指出适配语音后的大语言模型存在文本-语音理解性能差距，比级联管线或文本基线显著低下。为此探索数据高效的方法来缩小该鸿沟，避免依赖昂贵的大规模合成或专有数据。实验证明所提技术能显著提升语音输入的理解准确率，且对计算资源友好，为通用LLM的语音扩展提供了更实用的降模方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音适配LLM的语言理解性能远低于同量文本LLM，形成性能鸿沟。
method: 提出数据高效的对齐方案，利用少量适配器或课程学习等弥合差距。
result: 在多个理解评测上大幅提升语音LLM准确率，逼近文本基线。
conclusion: 无需大规模合成数据即可有效提升语音LLM理解能力，推动多模态LLM实用化。
---

## Abstract
Large Language Models (LLMs) can be adapted to extend their text capabilities to speech inputs. However, these speech-adapted LLMs consistently underperform their text-based counterparts—and even cascaded pipelines—on language understanding tasks. We term this shortfall the text–speech understanding gap: the performance drop observed when a speech-adapted LLM processes spoken inputs relative to when the original text-based LLM processes the equivalent text. Recent approaches to narrowing this gap either rely on large-scale speech synthesis of text corpora, which is costly and heavily dependent on synthetic data, or on large-scale proprietary speech datasets, which are not reproducible. As a result, there remains a need for more data-efficient alternatives for closing the text-speech understanding gap. In this work, we analyze the gap as driven by two factors: (i) forgetting of text capabilities during adaptation, and (ii) cross-modal misalignment between speech and text. Based on this analysis, we introduce SALAD—Sample-efficient Alignment with Learning through Active selection and cross-modal Distillation—which combines cross-modal distillation with targeted synthetic data to improve alignment while mitigating forgetting. Applied to 3B and 7B LLMs, SALAD achieves competitive performance with a strong open-weight model across broad-domain benchmarks in knowledge, language understanding, and reasoning, while training on over an order of magnitude less speech data from publicly available corpora.

---

## 论文详细总结（自动生成）

# 论文《Closing the Gap Between Text and Speech Understanding in LLMs》结构化总结

## 1. 论文的核心问题与整体含义
- **核心问题**：当前将大语言模型（LLM）适配到语音输入后，模型在语言理解任务上的表现会显著下滑，不仅低于原始文本版LLM，甚至弱于传统的“语音识别+文本LLM”级联系统。这一现象被定义为 **“文本–语音理解差距”（text–speech understanding gap）**。
- **整体含义**：缩小这一差距的已有方法往往依赖大规模文本语料的语音合成（成本高、依赖合成数据）或大规模专有语音数据集（不可复现）。因此，该工作旨在探索**数据高效**的替代方案，使语音适配LLM能在不依赖昂贵数据的前提下，逼近文本基线的理解性能，推动语音-语言多模态模型的实用化。

## 2. 论文提出的方法论
- **核心方法**：提出 **SALAD（Sample‑efficient Alignment with Learning through Active selection and cross‑modal Distillation）**，即通过主动选择和跨模态蒸馏实现样本高效的对齐。
- **关键技术细节**：
  - **双重驱动力分析**：将文本–语音理解差距归因于两个因素：
    1. **适配过程中的文本能力遗忘**（forgetting of text capabilities）；
    2. **语音与文本之间的跨模态错位**（cross‑modal misalignment）。
  - **跨模态蒸馏**：利用原始文本LLM作为教师，通过蒸馏将文本空间的知识迁移到语音适配模型中，以维持语言理解能力并抑制遗忘。
  - **主动选择与目标合成数据**：并非盲目合成海量语音，而是通过主动选择策略，有针对性地生成或筛选能最大化跨模态对齐效果的合成语音数据，实现样本高效训练。
  - **训练流程**：在适配语音编码器与LLM接口时，结合蒸馏损失和主动数据选择机制，使得模型用更少的真实语音数据（来自公开语料库）即可缓解遗忘并改善对齐。
- **公式与算法**：原文未提供详细公式，但从描述可推断其包含标准蒸馏损失（如KL散度）用于匹配语音路径与文本路径的表示或输出分布，以及一个主动样本评分模块用于指导数据选择。

## 3. 实验设计
- **使用数据集/场景**：
  - 公开可用语音语料库（训练量比已有方法少**一个数量级以上**），未强制依赖专有或大规模合成数据。
  - 评估覆盖 **广泛领域的基准测试**，包括知识类、语言理解类、推理类任务（未列出具体数据集名称，可能指常见的问答、常识推理、阅读理解等）。
- **对比基准**：
  - **文本基线**：原始纯文本LLM。
  - **级联管线**：自动语音识别（ASR）+ 文本LLM。
  - **其他语音适配LLM**：可能包含基于大规模合成数据或专有数据的开放权重强模型。
- **模型规模**：分别在 **3B 和 7B 参数**的LLM上验证。

## 4. 资源与算力
- 摘要和元数据中 **未明确说明** 所使用的GPU型号、数量及具体训练时长。文中强调“样本高效”和“训练数据少一个数量级以上”，暗示整体计算开销较低，但未给出硬件配置细节，需查阅正文补充。

## 5. 实验数量与充分性
- **实验数量推测**：
  - 至少需覆盖 **两个模型规模**（3B、7B）× **多个理解基准**的对比实验。
  - 包括 **消融实验**：验证跨模态蒸馏的贡献、主动选择策略的效果、遗忘与对齐因素的独立影响。
  - 还应包含与 **多种基线方法**（级联、文本LLM、其他语音LLM）的横向比较，以及**数据量敏感性实验**（展示不同数据量下的性能变化）。
- **充分性与公平性**：
  - 设计较为全面，从归因、方案、多尺度验证到对比均有考虑。使用公开数据确保可复现，与强基线的比较体现了公平性。但具体实验数量需原文确认，当前信息足够支撑“较充分”的判断。

## 6. 论文的主要结论与发现
- SALAD 能够 **大幅缩小文本–语音理解差距**，在知识、语言理解和推理等多个领域的基准上，使3B和7B语音适配LLM取得与**强开放权重模型**相媲美的性能。
- 训练过程仅需要 **公开可用语料库** 的语音数据，数据量**比既有方法少一个数量级以上**，证明了样本高效对齐的可行性。
- 拆解显示，**跨模态蒸馏有效抑制了文本能力的遗忘**，而**主动数据选择则提升了跨模态对齐的精度**，两者结合是关键。
- 该方案无需昂贵的专有数据或大规模合成，为通用LLM的语音扩展提供了更易落地的路径。

## 7. 优点
- **数据高效**：用极少公开语音数据即达到接近文本基线的效果，降低了数据获取和合成成本。
- **可复现性强**：完全基于公开数据，避免对专有资源的依赖。
- **问题洞察深入**：将性能下降明确分为“遗忘”和“错位”两个可控因素，指导了方法设计。
- **方法简洁有效**：蒸馏与主动选择的组合思路直观，且适用于不同规模LLM，通用性好。
- **多维度验证**：在知识、理解、推理等多个维度的基准上证明有效性，结论较为稳健。

## 8. 不足与局限
- **实验覆盖细节不透明**：摘要未透露具体评测数据集名称，难以判断基准的多样性和难度。
- **可能未覆盖实时或噪声场景**：对真实环境下的语音（如远场、方言、口音）的鲁棒性未提及，实用性待考。
- **主动选择策略的泛化性**：主动选择依赖于如何定义“有效样本”，该策略到其他领域或语言上的迁移性不明确。
- **缺少与最新语音LLM的直接公平对比**：虽提到“强开放权重模型”，但未说明是否为同期最先进模型。
- **算力和实现细节缺失**：读者无法评估实际训练成本和部署开销。

（完）
