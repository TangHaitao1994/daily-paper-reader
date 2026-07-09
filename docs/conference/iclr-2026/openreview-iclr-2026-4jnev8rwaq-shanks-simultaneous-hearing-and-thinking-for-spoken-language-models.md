---
title: "Shanks: Simultaneous Hearing and Thinking for Spoken Language Models"
title_zh: Shanks：语音语言模型的边听边思考
authors: "Cheng-Han Chiang, Xiaofei Wang, Linjie Li, Chung-Ching Lin, Kevin Lin, Shujie LIU, Zhendong Wang, Zhengyuan Yang, Hung-yi Lee, Lijuan Wang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=4Jnev8rWaq"
tags: ["query:speech-model"]
score: 9.0
evidence: 让语音语言模型边听边推理的推理框架，实现实时语音交互
tldr: 针对语音交互中模型在听完后思考导致高延迟的问题，Shanks提出一种推理框架，允许语音语言模型在监听用户输入时生成未发声的思维链推理。通过流式输入固定时长语音块，模型提前进行推理，从而在交互轮次中实现低延迟响应。实验证明该方法能显著提升语音交互的实时性和流畅度，对语音对话AI设计有重要贡献。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 解决语音交互中模型等待完整输入后才思考导致的延迟问题。
method: 提出Shanks推理框架，在流式输入过程中提前执行思维链推理。
result: 有效降低语音交互延迟，实现实时对话。
conclusion: 仿生边听边思考使语音交互更自然高效。
---

## Abstract
Current large language models (LLMs) and spoken language models (SLMs) begin thinking and taking action only \textit{after} the user has finished their turn.
This can create a high latency for waiting until the model ends the thinking process.
Consequently, thinking \textit{after} receiving the full input is not suitable for speech-to-speech interaction, where real-time and low-latency interaction is important.
We address the above issue by drawing inspiration from the fact that humans can naturally \textit{``think while listening''}.
In this paper, we propose \textbf{Shanks}, a general inference framework that enables SLMs to generate unspoken chain-of-thought reasoning when listening to the user input.
Shanks streams the input speech in fixed-duration chunks and, as soon as an input chunk is received, generates an unspoken reasoning based on all previous speech and reasoning; in the meantime, the user is still speaking.
Shanks uses unspoken reasoning to perform intermediate calculations, make API calls to complete the task, and determine whether to interrupt the user.
We demonstrate that Shanks enhances the real-time user-SLM interaction in two scenarios:
(1) When the user is presenting their solution to a math problem, Shanks can listen to and reason over the user's speech and make interruption when the user makes a mistake.
Shanks interrupts the user 37.1% more accurately compared with a baseline that interrupts the user without thinking.
(2) In a task-oriented dialogue setting, where the user's request needs to be completed by calling hotel and flight booking APIs, Shanks can complete 63.2% of the API calls before the user even ends their turn.

---

## 论文详细总结（自动生成）

# Shanks：语音语言模型的边听边思考

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前的大语言模型（LLM）与语音语言模型（SLM）普遍采取“听完再想”的串行范式，即模型在用户完整结束一个话轮后才开始内部思考并生成响应。这一过程在语音交互中会引入显著延迟，破坏实时对话的流畅性。
- **研究动机**：受人类能够自然“边听边想”的认知特性启发，作者希望改变模型的推理时序，将思考阶段提前至用户说话期间，从而根本性地降低语音交互的响应延迟。
- **整体含义**：论文提出一种通用的推理框架，赋予 SLM 在流式收听用户语音时同步生成未发声的思维链推理的能力，让语音对话 AI 从被动响应走向主动、实时、低延迟的交互模式。

## 2. 论文提出的方法论
- **核心思想**：通过将语音输入切分为固定长度的流式语音块，每收到一个语音块就立即触发一次内部推理生成，使“聆听”与“思考”两过程在时间上重叠，实现并行处理。
- **关键技术细节**：
  - **流式分块机制**：将用户持续的语音流按固定时间间隔切割为音频块，以流式方式逐一馈入模型。
  - **即时增量推理**：每当模型收到一个新的音频块，便联合所有历史语音块与历史推理内容，生成一个当前时刻的内部思维链步骤。该推理仅用于模型内部状态更新和决策，不直接向用户输出。
  - **多用途内部推理**：生成的未发声推理可用于：
    - 进行中间计算（例如数学题目的分步验算）；
    - 调用外部 API（如酒店、航班预订接口）提前完成任务；
    - 判断是否需要打断用户（例如发现用户口述解题出错时即时提醒）。
  - **框架通用性**：Shanks 作为纯推理阶段的框架，可与不同 SLM 架构配合使用，不限制底层模型的训练方式（推测为基于现有 SLM 的推理策略修改）。
- **算法流程简述**：
  1. 初始化对话上下文（语音历史与推理历史为空）。
  2. 循环处理每个音频块 `chunk_i`：
     - 将 `chunk_i` 追加到语音历史；
     - 以当前语音历史和此前所有内部推理为条件，让 SLM 生成一个推理片段 `r_i`；
     - 将 `r_i` 存入推理历史（不输出用户）；
     - 依据 `r_i` 判断是否满足打断条件或任务完成条件；若满足则发起打断或提前输出行动。
  3. 用户话语结束时，利用积累的完整推理链快速合成最终语音回复或行动。

## 3. 实验设计
- **应用场景与数据集**：
  - **场景 1：数学辅导口语交互**。用户口头叙述一道数学题的解答过程，Shanks 需实时推理并在用户出错时打断。具体数学口语数据集在摘要中未详细列举，推测为内部构造的数学解答口语数据集。
  - **场景 2：任务导向对话**。用户提出需调用酒店和机票预订 API 的复合请求，模型需边听边进行 API 调用以服务用户。
- **对比基准**：
  - **打断实验**：对比了“不进行内部思考，仅基于表面语音信号直接判断打断”的基线模型。Shanks 的打断准确率相对基线的提升为 37.1%。
  - **提前 API 调用实验**：对比了传统“听完整个话语后才开始调用 API”的模式。Shanks 在用户结束话轮之前已完成了 63.2% 的必要 API 调用。
- **评价指标**：打断准确率、提前完成 API 调用的比例、交互延迟降低程度（定性提及）。

## 4. 资源与算力
- 提供的论文摘要和元数据中**未明确提及**训练或推理所使用的 GPU 型号、数量、训练时长或总计算开销。该部分信息在现有材料中缺失。

## 5. 实验数量与充分性
- **实验规模**：从现有描述看，实验覆盖了两个代表性场景（数学打断、任务对话），每个场景内进行了关键指标的基准对比。
- **充分性分析**：
  - 已进行的实验能够支撑核心主张——边听边想可以同时提升打断准确率和任务执行的提前量。
  - 然而，现有信息未展示对不同语音时长、语速变体、背景噪声、不同 SLM 骨干网络、推理深度（内部思维链长度）等因素的消融研究或鲁棒性测试。
  - 实验场景相对有限，缺少在多轮复杂对话、情感密集型交互等更多维度下的验证。
  - 因此，实验作为**概念验证**是充分的，但全面性和普适性证据目前仍显不足。

## 6. 论文的主要结论与发现
- Shanks 框架成功使得 SLM 具备边听边思考的能力，大幅压缩了语音交互中的感知等待时间。
- 在数学辅导场景中，利用增量内部推理可在用户犯错时更准确地实施打断，打断准确性相较无思考基线提高 37.1%。
- 在任务型对话中，模型能在用户话轮结束前提前完成超六成的 API 调用，显著减少任务完成总时延。
- 核心结论：模仿人类“边听边想”的并行处理范式能够让语音对话 AI 的交互更实时、更主动，对下一代语音交互系统的设计具有重要指引意义。

## 7. 优点
- **创新性强**：首次将人类层级的“同时聆听与思考”认知机制系统性引入语音语言模型，提出通用推理框架，区别于简单的工程加速。
- **实时性提升显著**：从架构层面将思考与听音并行化，根本性降低了端到端延迟，特别适合需即时反馈的场景。
- **场景适用性广**：框架不限定特定应用，可扩展至教学、客服、智能家居等众多需要主动介入或预行动的语音交互领域。
- **模型解耦设计**：Shanks 作为推理层框架，可灵活搭配不同 SLM 实现，具有较强的技术迁移潜力。

## 8. 不足与局限
- **固定分块粒度敏感**：流式语音块的固定时长作为关键超参数，可能需针对不同说话速率或任务类型精细调节，否则可能破坏推理的连贯性。
- **错误累积风险**：早期不完整输入上的推理错误可能随增量流持续放大，文中未探讨错误检测与自纠机制。
- **计算开销未量化**：每接收一个音频块便触发一次推理生成，可能增加整体计算负担；在成本与延迟收益之间的权衡未给出分析。
- **实验覆盖偏窄**：仅展示了两个特定场景，未涉及多轮复杂对话、抗噪声能力、多语言、用户情感应对等实际部署中的关键挑战。
- **安全与副作用未讨论**：如过早打断可能引发不礼貌感，提前 API 调用若有误可能造成错误预订等负面后果，论文未涉及相关防护设计。

（完）
