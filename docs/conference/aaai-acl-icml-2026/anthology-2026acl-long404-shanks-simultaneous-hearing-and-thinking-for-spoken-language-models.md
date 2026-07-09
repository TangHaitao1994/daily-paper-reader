---
title: "Shanks: Simultaneous Hearing and Thinking for Spoken Language Models"
title_zh: SHANKS：语音语言模型的同步聆听与思考
authors: "Cheng-Han Chiang, Xiaofei Wang, Linjie Li, Chung-Ching Lin, Kevin Lin, Shujie Liu, Zhendong Wang, Zhengyuan Yang, Hung-yi Lee, Lijuan Wang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.404.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 使语音语言模型在聆听时进行思考，实现低延迟语音交互
tldr: 现有语音语言模型在用户说完后才开始思考，导致交互延迟。受人类边听边想启发，SHANKS框架让模型在接收语音片段时同步生成非发声的思维链推理，利用遮蔽语音预测损失进行流式训练。实验表明，该方法显著降低响应延迟，提升了对话的自然度和连贯性。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.404/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 783, \"height\": 501, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.404/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1620, \"height\": 202, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.404/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1604, \"height\": 1878, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.404/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1616, \"height\": 971, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.404/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 713, \"height\": 541, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.404/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 814, \"height\": 339, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.404/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 808, \"height\": 237, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.404/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1658, \"height\": 338, \"label\": \"Table\"}]"
motivation: 解决语音模型因等待用户结束而造成的响应延迟和缺乏实时交互问题。
method: 设计流式输入固定时长语音块并同步生成思维链的推理框架，结合流式训练。
result: 在多个对话场景中实现了低延迟响应，交互流畅性大幅提高。
conclusion: SHANKS为语音交互模型提供了更贴近人类的实时推理能力，革新了语音对话体验。
---

## Abstract
Current large language models (LLMs) and spoken language models (SLMs) begin thinking and taking actions only after the user has finished their turn. This prevents the model from interacting with the user during the user’s turn and can lead to high response latency when the model is thinking. To address this issue, we draw inspiration from the “think while listening” behavior of humans. In this paper, we propose SHANKS, a general inference framework that enables SLMs to generate unspoken chain-of-thought reasoning while listening to user input. SHANKS streams input speech in fixed-duration chunks and, as soon as a chunk is received, generates unspoken reasoning based on all previous speech and reasoning, while the user continues speaking. SHANKS uses unspoken reasoning to determine whether to interrupt the user and make tool calls to complete the task. We demonstrate that SHANKS enhances real-time user–SLM interaction in two scenarios: (1) SHANKS can listen to the user’s speech and interrupt when the user makes a mistake. (2) In a tool-augmented dialogue scenario, SHANKS can complete 56.9% of the tool calls before the user ends their turn. Overall, SHANKS is a step toward models that keep thinking throughout the conversation, not only after a turn ends. Demos can be found on the project page: https://d223302.github.io/SHANKS/.

---

## 论文详细总结（自动生成）

好的，这是对论文《SHANKS: Simultaneous Hearing and Thinking for Spoken Language Models》的结构化总结。

### 1. 论文的核心问题与整体含义

*   **研究问题**：当前的大语言模型（LLM）和语音语言模型（SLM）采用**回合制交互模式**，即只能在用户完全结束发言后才开始思考和行动。这导致了两个关键问题：
    *   **高响应延迟**：模型所有的推理、工具调用等过程都发生在用户停顿之后，用户需要等待较长时间。
    *   **缺乏实时互动**：模型无法在用户发言过程中进行干预，例如纠正错误或提前准备，这与人类“边听边想”的交流模式相去甚远。
*   **整体含义**：本文的核心在于赋予SLM**实时思考与行动的能力**，使模型能在聆听用户流式语音的同时，进行内部推理、决定打断时机或调用外部工具，从而显著降低响应延迟并提升交互的自然度和实时性。

### 2. 论文提出的方法论

*   **核心思想**：借鉴人类“边听边想”（Thinking while listening）的认知过程，提出并实现一种同步聆听与思考的SLM框架。
*   **关键技术细节：SHANKS框架**
    *   **流式输入处理**：用户的语音输入以**固定时长**（\(t_{\text{chunk}}\)，如4秒）的音频块（chunks）为单位进行流式传输和编码。
    *   **同步推理生成**：模型在接收到第 \(i\) 个语音块 \(S_i\) 后，会立即生成一段**内部的、非语音的思维链推理**文本块 \(R_i\)。这段推理基于所有已接收的语音块（\(S_1...S_i\)）和此前生成的推理块（\(R_1...R_{i-1}\)）。该生成过程与用户正在说出下一个语音块 \(S_{i+1}\) 的过程**同步进行**。
    *   **专用令牌**：使用特殊令牌来标记上下文：
        *   `[EOPA]`：追加在每个非末尾语音块后，表示“部分音频结束”。
        *   `[EOA]`：追加在最后一个语音块后，表示“整体音频结束”。
        *   `<think>` 和 `</think>`：包裹内部的思维链推理文本。
        *   `[INTERRUPT]`：在推理文本中生成，表示模型决定打断用户。
    *   **训练数据构造**：通过构造“语音块-推理块”交替序列的训练数据，并使用标准的语言模型交叉熵损失进行微调，让模型学会这种交替生成模式。推理块内容由GPT-4o等教师模型根据部分上下文生成。
    *   **推理时的应用**：
        *   **打断用户**：当推理块 \(R_k\) 中生成 `[INTERRUPT]` 令牌时，模型立即停止聆听并开始输出回复 \(O\)，打断用户。
        *   **提前工具调用**：推理块 \(R_i\) 的内容可以是API调用指令，模型在聆听过程中即可发出调用，并利用用户仍在发言的时间等待返回结果。

### 3. 实验设计

论文在两个场景下评估SHANKS框架的有效性：

*   **场景一：打断用户**
    *   **数据集与Benchmark**：
        *   **评估集**：基于GSM8K数学题数据集自建。由LLM生成正确和错误的解题过程，并使用TTS模型合成为语音。包含1280个“正确解题”样本和1140个“错误解题”样本。
        *   **训练集**：使用Tulu3-Persona-Math-Grade数据集中的题目构造训练数据。
    *   **对比方法**：
        *   `SHANKS-E2E`：基于端到端SLM Qwen-2.5-Omni实现的SHANKS。
        *   `SHANKS-Cascade`：基于ASR+LLM（Whisper + Qwen-2.5-7B）的级联版SHANKS。
        *   `No-thinking`：基于Qwen-2.5-Omni的消融基线，不经过推理，直接在每个语音块后预测是否打断。
    *   **评估指标**：打断率、有效打断率、打断延迟。

*   **场景二：提前工具调用**
    *   **数据集与Benchmark**：
        *   **评估集**：ComplexFuncBench，一个评估多步API调用的旅行规划对话数据集。将文本查询由TTS合成为语音。共500个测试样本。
        *   **训练集**：使用ComplexFuncBench的另一半数据训练。
    *   **对比方法**：
        *   `SHANKS-E2E`、`SHANKS-Cascade`。
        *   `Call-after-listen`：基线模型，在听完用户完整语音后再进行所有API调用。
        *   `SHANKS + Call-after-listen`：集成方法，聆听时用SHANKS调用，发言结束后由`Call-after-listen`补全未完成的调用。
    *   **评估指标**：调用准确率、任务成功率、响应延迟。

### 4. 资源与算力

*   **硬件**：训练使用了**8块A100 GPU**。
*   **训练时长**：**未明确说明总训练时长**。只提到微调训练进行了**2个epoch**（针对打断场景）或**10个epoch**（针对工具调用场景），后者的训练集更小。
*   **模型规模**：主要使用的模型为7B/8B参数级别（如Qwen-2.5-7B， Qwen-2.5-Omni）。

### 5. 实验数量与充分性

*   **实验组数量**：论文进行了**两个核心场景**下的多组实验。
    *   **场景一（打断）**：包含3种主要方法（`No-thinking`, `SHANKS-E2E`, `SHANKS-Cascade`）和针对`SHANKS-E2E`的2个消融实验（\(t_{\text{chunk}}=3, 5\)），共约**5组**对比。
    *   **场景二（工具调用）**：包含4种方法（`Call-after-listen`, `SHANKS-E2E`, `SHANKS-Cascade`, `SHANKS+Call-after-listen`），共**4组**对比。
*   **充分性评估**：
    *   **充分**：实验覆盖了端到端和级联两种主流SLM架构，并在两个性质完全不同的任务上验证了框架的有效性和通用性。消融实验考察了关键超参数（\(t_{\text{chunk}}\)）的影响。
    *   **客观公平**：对比基线的选择合理（无思考、听后再做），能够突显SHANKS带来的增益。在工具调用场景，论文承认了SHANKS在成功率上的不足，并提出了有效的集成方案，分析较为客观。
    *   **局限性**：场景一的自建评估集可能存在构造偏差；两个场景的训练集均使用了GPT-4o等大模型合成数据，其质量依赖于这些教师模型，可能存在噪声。

### 6. 论文的主要结论与发现

1.  **SHANKS能实现有效的边听边想**：模型成功学会了在聆听过程中同步生成连贯的、与当前上下文相关的内部推理。
2.  **打断行为更精准**：在错误解题场景中，`SHANKS`的打断率是`No-thinking`基线的**6倍以上**（84.8% vs 13.8%），且有效打断率显著更高（63.9% vs 26.8%），证明思考过程对准确干预至关重要。更强的LLM（Qwen-2.5-7B）带来更好的打断准确率。
3.  **提前工具调用显著降低延迟**：`SHANKS`能够在用户发言结束前，完成**56.9%（E2E）** 的API调用。与`Call-after-listen`集成后，任务成功率得到保持，而响应延迟则降低了**62.3%**（转化为更少的输出令牌数）。
4.  **\(t_{\text{chunk}}\)参数具有鲁棒性**：推理时改变语音块的大小对打断性能影响不大，但会影响最终延迟。

### 7. 优点

*   **立意新颖，直击痛点**：将人类的“边听边想”机制引入SLM，这是口语对话系统迈向更低延迟、更自然交互的关键一步。论文对“思考”的应用（打断、后台工具调用）挖掘深入，场景设计巧妙。
*   **框架简洁通用**：SHANKS是一种推理框架，不依赖于特定的模型结构，可同时应用于端到端和级联SLM，具有良好的推广潜力。
*   **实验设计坚实**：通过两个对比鲜明的应用场景，全面展示了SHANKS在不同维度（互动性与低延迟）上的价值。与“无思考”和“听后再做”的基线对比非常有说服力。
*   **分析深入**：不仅报告性能，还分析了方法的不足（工具调用成功率低）并提出了有效的集成补救方案，分析非常务实。

### 8. 不足与局限

*   **依赖语音长度和结构**：要求用户发言足够长且信息流有序，才能发挥边听边想的优势。对于“今天天气怎么样？”这类短句效果甚微。
*   **固定块延迟**：模型的理解永远比用户当前发言慢一个\(t_{\text{chunk}}\)的时间。
*   **计算成本增加**：边听边想显著增加了推理期间的总计算量，这是一个计算成本与延迟之间的权衡，论文也承认了这点。
*   **工具调用鲁棒性**：在聆听阶段的工具调用失败后，模型缺乏重试机制，导致单用SHANKS的任务成功率较低。
*   **实验环境理想化**：工具调用场景中，API的响应是预先准备好的，没有计算网络延迟；语音也是合成的TTS，非真实的自然语音。
*   **安全与伦理**：论文虽提及模型是否打断用户应由部署者决定，但未深入探讨模型具备打断能力后可能带来的社会伦理风险。
*   **算力信息不完整**：未报告训练时长，难以准确评估资源消耗和复现成本。

（完）
