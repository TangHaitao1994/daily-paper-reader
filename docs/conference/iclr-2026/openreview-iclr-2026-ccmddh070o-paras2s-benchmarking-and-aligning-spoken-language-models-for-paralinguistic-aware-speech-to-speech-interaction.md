---
title: "ParaS2S: Benchmarking and Aligning Spoken Language Models for Paralinguistic-aware Speech-to-Speech Interaction"
title_zh: ParaS2S：评估与对齐面向副语言感知语音到语音交互的语音语言模型
authors: "Shu-wen Yang, Ming Tu, Andy T. Liu, Xinghua Qu, Hung-yi Lee, Lu Lu, Yuxuan Wang, Yonghui Wu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=CcmDDh070o"
tags: ["query:speech-model"]
score: 9.0
evidence: 利用强化学习实现副语言感知的语音到语音交互，优化对话系统
tldr: 语音对话系统缺乏对情感、语调等副语言线索的感知与适当回应。ParaS2S提出强化学习框架，直接在波形层面评估并优化回复内容和说话风格，并构建ParaS2SBench基准。实验表明模型在自然度和表现力上显著提升，推进了情感化语音交互发展。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有S2S模型难以处理副语言线索，缺乏高质量表达性数据。
method: 设计ParaS2SBench基准，用RL在波形层面联合优化内容和风格。
result: 模型回复的自然度和风格适配性显著优于基线条统。
conclusion: 为副语言感知的语音交互提供基准和对齐方法，提升对话体验。
---

## Abstract
Speech-to-Speech (S2S) models have shown promising dialogue capabilities, but their ability to handle paralinguistic cues—such as emotion, tone, and speaker attributes—and to respond appropriately in both content and style remains underexplored. Progress is further hindered by the scarcity of high-quality and expressive demonstrations. To address this, we introduce a novel reinforcement learning (RL) framework for paralinguistic-aware S2S, ParaS2S, which evaluates and optimizes both response content and speaking style directly at the waveform level. We first construct ParaS2SBench, a benchmark that evaluates the naturalness of input–output pairs in terms of content and speaking style using expressive and challenging queries. For the automatic judge, we propose a PolyTone training strategy and a multi-stage framework, preventing the style hallucination of end-to-end audio LLM judging. Our judge correlates well with human preferences and is scalable, enabling the model to interact and learn from unlabeled speech via RL. Experiments show that existing S2S models fail to respond appropriately to paralinguistic attributes, performing no better than pipeline-based baselines. Our RL approach (ParaS2SAlign) achieves an 10% relative improvement in the appropriateness of response content and speaking style on ParaS2SBench over supervised fine-tuning (SFT), surpassing all prior models while requiring substantially fewer paired demonstrations than pure SFT. Our findings highlight the need for a scalable and accurate automatic evaluator for speech-to-speech interaction.

---

## 论文详细总结（自动生成）

# 论文《ParaS2S: Benchmarking and Aligning Spoken Language Models for Paralinguistic-aware Speech-to-Speech Interaction》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有的语音到语音（Speech-to-Speech, S2S）对话模型虽然在一般对话能力上展现出潜力，但严重缺乏对副语言线索（paralinguistic cues）——如情感、语调、说话风格、说话人属性——的感知与恰当响应能力。模型往往只能机械地回复文本内容，无法在**说话风格**和**情绪表现**上做出与上下文匹配的回应。
- **研究动机**：
  - **能力缺口**：当前 S2S 系统在理解并生成带有表现力的语音方面存在明显短板，回复内容与风格的**自然度**和**适配性**远未达到实用水平。
  - **数据稀缺**：高质量、富含表达性的语音对话演示数据严重不足，制约了监督学习方法的效果。
  - **评估困难**：缺乏能够精确、可扩展地评价语音交互中“风格适当性”与“内容适当性”的自动评估指标，从而限制了模型的迭代优化。
- **整体含义**：该工作旨在通过建立自动化、可扩展的评估范式和对齐方法，推动副语言感知的语音交互系统发展，使得语音助手能够像人类一样感知到对方的情绪、语气，并用合适的语调和风格进行回应，提升对话的真实感与情感共鸣。

## 2. 论文提出的方法论
- **整体思路**：提出一个名为 **ParaS2S** 的强化学习（RL）框架，首次在**波形层面**直接联合优化回复的**文本内容**和**说话风格**，以对齐人类对副语言自然度的偏好。
- **关键技术细节**：
  - **ParaS2SBench 基准构建**：收集一系列**具有挑战性和高度表达性**的查询，并构造输入-输出对。基准侧重评估在内容和说话风格两个维度的自然度与适配性。
  - **可扩展的自动裁判（Judge）**：
    - 提出 **PolyTone 训练策略** 和**多阶段框架**，用于训练一个能够评判音频响应的端到端音频大语言模型（Audio LLM）。
    - 该策略防止模型产生“风格幻觉”（style hallucination），即错误地将评委自身的风格倾向强加于被评测音频。
    - 自动裁判与人类偏好具有高相关性，且可以大规模运行，从而支持利用**无标注语音**进行 RL 训练。
  - **强化学习对齐（ParaS2SAlign）**：
    - 使用自动裁判给出的风格与内容恰当性分数作为奖励信号。
    - 直接在原始波形上对 S2S 模型进行 RL 微调（可能是基于策略梯度类算法，摘要未明确公式，但流程为：模型生成回复波形 → 自动裁判评分 → 更新模型参数以最大化奖励）。
    - 该方法相比纯粹监督微调（SFT）需要**更少的配对演示数据**（substantially fewer paired demonstrations）。

## 3. 实验设计
- **数据集/场景**：
  - 构造的 **ParaS2SBench** 基准，包含表达性、具有挑战性的查询，用于评估模型在副语言感知上的表现（可能涵盖多种情绪、语调变化场景）。
  - 训练过程中使用未命名的无标注语音（unlabeled speech）作为 RL 探索数据。
- **对比方法**：
  - **现有 S2S 模型**：调研中的其他语音到语音模型。
  - **基于管道的基线（Pipeline-based baselines）**：即传统的“语音识别→文本生成→语音合成”级联系统。
  - **监督微调（SFT）版本模型**：仅使用配对演示进行微调。
  - **ParaS2SAlign(Rl)**：本论文提出的 RL 对齐方法。
- **评估指标**：
  - 主要关注回复**内容适当性**和**说话风格适当性**在 ParaS2SBench 上的得分（由自动裁判给出，也可能有人工评估验证其与人类相关性）。

## 4. 资源与算力
- 摘要及提供的元数据中**未明确提及**所使用的 GPU 型号、数量、训练时长等具体算力信息。只能推断需要训练端到端音频 LLM 裁判和 RL 微调 S2S 模型，属于较大规模计算，但无具体数据支撑。

## 5. 实验数量与充分性
- **实验数量**：摘要中未给出精确实验组数，但从描述可推断至少包含以下几类核心实验：
  - 多种 SOT 模型与管道基线的对比实验（验证现有模型能力不足）。
  - 自动裁判与人类偏好的相关性验证实验（证明裁判可靠性）。
  - SFT 与 RL 对齐（ParaS2SAlign）在 ParaS2SBench 上的性能对比实验。
  - 可能还有关于所需演示数据量的消融实验（文中提到 RL 所需配对数据远少于 SFT）。
- **充分性与公平性**：
  - 实验设计**较充分**，既验证了问题的存在（现有模型不佳），又证明了所提裁判的有效性，再到最终 RL 对齐的增益，形成闭环。
  - 对比条件公平：所有方法在同一基准上评估，且自动裁判经过人类校准，避免了指标赛马。
  - 但受限于摘要，具体消融维度（如不同 RL 奖励构成、不同基础 S2S 模型、不同标注数据量）的深入程度未知。

## 6. 论文的主要结论与发现
- **现有模型短板**：目前 S2S 模型在副语言回应上表现糟糕，其性能甚至不优于传统的级联系统（语音识别 + 文本模型 + 语音合成），说明端到端模型并未真正理解或生成合适的表达风格。
- **RL 对齐优势**：提出的 ParaS2SAlign（RL 方法）在 ParaS2SBench 上的回复内容和风格适当性指标上，相比监督微调（SFT）实现了**10% 的相对提升**，并且超越了所有先前的模型。
- **数据效率**：RL 方法在取得更优性能的同时，所需的配对演示数据量**远少于纯 SFT**，显示出从无标注语音中学习副语言风格的能力。
- **评估器的关键作用**：一个可扩展、与人类对齐的自动裁判对于推进语音交互系统至关重要，本工作的 PolyTone 多阶段裁判为这一难题提供了有效解决方案。

## 7. 优点
- **问题新颖且实用**：首次明确定义并系统解决 S2S 对话中的副语言感知与对齐问题，直指情感化语音交互落地的关键瓶颈。
- **方法论创新**：
  - 在**波形级别**进行端到端的风格+内容联合优化，不同于之前将风格作为后处理的做法。
  - 提出的 **PolyTone 训练与多阶段评判框架**，设计精巧，有效抑制风格幻觉，使自动裁判真正可用。
  - **RL 与无标注数据结合**，突破了表达性语音数据稀缺的局限，具有很高的数据效率。
- **基准贡献**：构建了 ParaS2SBench，为社区提供了急需的评测工具，推动该方向的研究。
- **实验说服力**：与多类基线对比，并用 SFT 和 RL 的对比清晰证明了 RL 对齐在这一任务中的价值，结论扎实。

## 8. 不足与局限
- **评估裁判的依赖性**：整个 RL 框架的性能上限取决于自动裁判的质量，尽管裁判与人类偏好相关，但其本身可能在细粒度风格分类或长尾场景中存在偏差，可能被模型“欺骗”或过度拟合。
- **实验覆盖范围不明**：摘要未提及模型在多语言、多说话人、强噪声环境下的表现，泛化性有待验证。且缺乏对 RL 训练稳定性、样本效率的具体分析。
- **安全与伦理**：RL 可能使模型学习出过分夸张、不恰当甚至冒犯性的回应风格，论文未讨论安全对齐机制。
- **算力与复现细节缺失**：由于未提供资源消耗细节和更细粒度的训练参数，复现该框架的门槛较高。
- **仅基于摘要的局限**：本分析仅依据论文摘要和元数据，对于模型架构细节、消融实验范围、人类评估的具体指标等深度信息无法获知，因此对某些结论的强度和局限性判断可能不够充分。

（完）
