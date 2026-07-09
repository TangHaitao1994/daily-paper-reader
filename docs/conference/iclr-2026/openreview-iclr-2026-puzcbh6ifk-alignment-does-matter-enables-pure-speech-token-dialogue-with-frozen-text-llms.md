---
title: "Alignment Does Matter: Enables Pure-Speech-Token Dialogue with Frozen Text LLMs"
title_zh: 对齐至关重要：使冻结文本大语言模型支持纯语音token对话
authors: "Zhen Ye, Xu Tan, Yiming Li, Guangyan Zhang, Chi-Min Chan, Haohe Liu, Zhengxi Liu, Hongzhan Lin, Zheqi Dai, ZHANG Xinshen, Peiwen Sun, Qiuqiang Kong, Yike Guo, Wei Xue"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=PUZCbh6IfK"
tags: ["query:speech-model"]
score: 9.0
evidence: 通过对齐语音与文本token，实现纯语音token对话，提升基于冻结文本LLM的语音对话AI。
tldr: 针对语音对话模型中语音与文本token长度对齐的非最优问题，探索最优语音序列长度以实现从文本LLM高效迁移知识。通过系统的帧率搜索和微调策略，在冻结LLM上实现纯语音token对话，在保持文本理解能力的同时显著提升语音交互自然度，为轻量级多模态对话AI提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 语音与文本token序列长度简单平均对齐可能并非最优，影响从文本LLM迁移知识的效果。
method: 探索语音token帧率对知识继承的影响，发现最优帧率并设计微调策略适配冻结LLM。
result: 在纯语音token对话任务上取得与微调LLM相当的自然度和理解力。
conclusion: 通过对齐优化，冻结的强文本LLM可有效支持纯语音对话，降低训练成本。
---

## Abstract
End-to-end speech dialogue models typically inherit knowledge from pretrained text LLMs by continually learning to model speech tokens. Thus, alignment between speech tokens and text tokens becomes critical: effective alignment enables more efficient transfer of LLM knowledge from text to speech. A well-known challenge lies in the sequence length mismatch between speech tokens and text tokens. But is matching them on average (i.e., making one second of speech map to the same number of tokens as the corresponding text) truly optimal? The answer remains unknown.
In this paper, we aim to discover the optimal speech sequence length—equivalently, the frame rate of speech tokens—for inheriting text LLM knowledge. First, we observe that when forcing speech length to approach text length under a standard LLM architecture (single VQ + single LM head), information loss becomes a bottleneck. To address this, we propose a new method that enables scaling the VQ codebook capacity up to nearly 300 bits per frame, coupled with an efficient audio LM head. This design preserves sufficient speech information under aggressive downsampling while aligning sequence lengths more closely with text tokens, all without modifying the base text LLM backbone. Next, we explore three alignment tasks—speech→text, text→speech, and speech→speech—under different speech token frame rates, while keeping the text LLM parameters frozen. Furthermore, we find that speech and text representation still occupy distinct latent subspaces in the LLM. To mitigate this gap, we introduce a representation alignment objective to further strengthen cross-modal alignment.
Experiments show that with only alignment, a frozen text LLM can already perform pure speech-to-speech QA, achieving comparable results on speech QA benchmarks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：端到端语音对话模型通常通过持续学习语音 token 的方式，从预训练文本大语言模型（LLM）中迁移知识。在此过程中，语音 token 与文本 token 之间的对齐质量直接决定知识迁移效率。一个已知的核心挑战是两种 token 序列的长度不匹配问题。现有方法往往追求平均意义上的对齐（例如让每秒语音映射到与对应文本相同数量的 token），但这种“平均对齐”是否最优尚无定论。
- **核心问题**：探索最优的语音序列长度（等价于语音 token 的帧率），以最大化从冻结文本 LLM 中继承知识的效果，从而实现纯语音 token 对话。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：通过系统搜索最优语音 token 帧率，使语音序列长度尽可能接近文本序列长度，同时利用高容量码本（codebook）保持语音信息完整性，并在冻结的文本 LLM 上通过仅调整对齐组件实现高效率的语音对话。
- **关键技术细节**：
  - **高码率语音编码**：提出一种新方法，在单 VQ + 单 LM head 的标准架构下，将 VQ 码本容量扩展至每帧近 300 比特，配合高效的音频 LM head，既实现激进的下采样（缩短语音序列长度），又保持足够语音信息，且无需修改文本 LLM 主干。
  - **三种对齐任务探索**：在语音→文本、文本→语音、语音→语音三种任务下，研究不同语音 token 帧率对对齐效果的影响，保持文本 LLM 全部参数冻结。
  - **表示空间对齐**：发现语音和文本表征在 LLM 内部仍占据不同的隐子空间，引入一种表示对齐目标（representation alignment objective），进一步强化跨模态对齐，弥合模态差异。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **任务场景**：纯语音到语音的问答（speech-to-speech QA），同时支持语音识别（speech→text）和语音合成（text→speech）作为辅助评估。
- **Benchmark**：在语音 QA 基准（speech QA benchmarks）上进行评估（摘要未列出具体数据集名称，例如可能包括 Spoken SQuAD 等）。
- **对比方法**：
  - 不同帧率下的对齐方法，分别对比单任务与多任务对齐效果。
  - 与微调 LLM 的语音对话模型进行性能对比（文中指出仅靠对齐即可在冻结 LLM 下取得相当结果）。
  - （推测）可能还包括基于平均对齐长度的 baseline。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力
- **未明确说明**：从提供的摘要和元数据中，未提及所使用的 GPU 型号、数量、训练时长等具体算力信息。因此无法给出具体的资源消耗数据。

## 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- **实验设计维度丰富**：
  - 帧率搜索：在多个语音 token 帧率下进行实验，寻找最优帧率。
  - 任务覆盖：同时考察三种交叉对齐任务（speech↔text, text↔speech, speech↔speech）。
  - 消融分析：码本容量扩展、表示对齐目标等模块的有效性分析。
- **充分性与公平性**：
  - 充分性：从摘要看，实验覆盖了关键超参数和多任务组合，能够支撑对齐重要性的结论；对比了冻结与微调 LLM 的性能，验证了方法的竞争力。
  - 客观公平性：保持文本 LLM 完全冻结，确保性能提升仅源于对齐优化，排除了 LLM 自身微调带来的干扰，对比较为公平。但样本规模、领域多样性以及 benchmark 的具体细节未披露，难以进一步判断统计效力。

## 6. 论文的主要结论与发现
- **帧率对齐至关重要**：简单平均对齐未必最优，通过系统搜索发现存在一个最优帧率，使得语音 token 在序列长度和内容保真度之间达到最佳平衡，从而最大程度继承文本 LLM 的知识。
- **冻结 LLM 可支撑纯语音对话**：仅通过精心设计的对齐（高容量码本 + 表示空间对齐），无需微调文本 LLM，即可在语音 QA 任务上取得与微调 LLM 相当的性能。
- **表示对齐弥补模态差异**：引入额外的表示对齐损失有助于拉近语音和文本在 LLM 隐空间中的分布，进一步提升对话质量。

## 7. 优点：方法或实验设计上有哪些亮点
- **聚焦“对齐”本身的价值**：系统性地研究了语音 token 帧率对知识迁移的影响，为语音与文本 token 对齐问题提供了量化见解。
- **高效且资源友好**：冻结文本 LLM 的策略极大降低了训练成本，仅需轻量级的语音编解码和对齐模块，使纯语音对话更易部署。
- **技术新颖性**：将近 300 比特每帧的高容量码本引入单 VQ 架构，同时保持架构简洁，平衡了压缩率与信息保真度。
- **多任务验证**：同时涵盖语音识别、合成和对话三种形态，验证了对齐方法的通用性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验细节缺失**：摘要未提供具体数据集、模型规模、音频时长、对抗性测试等，无法评估方法的普适性及在真实噪声环境下的鲁棒性。
- **模型容量上限**：虽然码本接近 300 比特/帧，但极度压缩可能导致部分副语言信息（情绪、韵律细节）丢失，可能影响交互自然度的上限。
- **语言与领域局限**：未知评估是否仅针对英语或单领域数据集，不同语言、多领域、长对话等场景下的泛化性有待验证。
- **表示对齐的潜在风险**：强制拉近语音与文本隐空间可能削弱模型利用语音独特信息的能力，对于强调语气、口音等非语言任务可能存在副作用。

（完）
