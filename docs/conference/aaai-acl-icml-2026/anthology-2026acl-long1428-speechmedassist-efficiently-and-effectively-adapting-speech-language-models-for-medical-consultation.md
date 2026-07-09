---
title: "SpeechMedAssist: Efficiently and Effectively Adapting Speech Language Models for Medical Consultation"
title_zh: SpeechMedAssist：高效有效地将语音语言模型适配至医疗咨询
authors: "Sirry Chen, Jieyi Wang, Wei Chen, Zhongyu Wei (魏忠钰)"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1428.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 适应语音语言模型用于多轮语音医学咨询
tldr: 医疗咨询天然适合语音交互，但现有工作多为文本形式，且医学语音数据稀少、直接微调语音模型成本高。本文提出SpeechMedAssist，一种原生支持多轮语音交互的语音语言模型。通过解耦训练为知识注入和指令微调两个阶段，高效地实现领域适配。在医疗对话任务上的实验表明，该方法能有效处理患者语音查询并生成恰当回复，为语音助手在医疗等专业领域的应用提供了实用范式。该方法减少对大规模标注数据的依赖，加速了语音对话系统的垂直领域迁移。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 803, \"height\": 842, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1647, \"height\": 591, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 796, \"height\": 360, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 797, \"height\": 485, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1648, \"height\": 497, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 796, \"height\": 343, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 789, \"height\": 479, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 789, \"height\": 482, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 802, \"height\": 752, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 796, \"height\": 1086, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 802, \"height\": 872, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 792, \"height\": 942, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1428/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 493, \"height\": 942, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 796, \"height\": 436, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1663, \"height\": 743, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 800, \"height\": 223, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 806, \"height\": 446, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 812, \"height\": 339, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 809, \"height\": 331, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 806, \"height\": 394, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 811, \"height\": 643, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 813, \"height\": 643, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 500, \"height\": 245, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1428/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 811, \"height\": 696, \"label\": \"Table\"}]"
motivation: 医疗咨询本应语音驱动，但现有研究多基于长文本交互，且医学语音数据稀缺、微调低效。
method: 利用SpeechLM架构特性，将单阶段训练解耦为知识注入与指令微调的两阶段范式。
result: SpeechMedAssist能够进行多轮语音交互，在医学问答等任务上高效适配并取得良好效果。
conclusion: 该工作为语音模型在垂直领域的部署提供了高效适配方案，推动口语对话系统落地医疗场景。
---

## Abstract
Medical consultations are intrinsically speech-centric. However, most prior works focus on long-text-based interactions, which are cumbersome and patient-unfriendly. Recent advances in speech language models (SpeechLMs) have enabled more natural speech-based interaction, yet the scarcity of medical speech data and the inefficiency of directly fine-tuning on speech data jointly hinder the adoption of SpeechLMs in medical consultation. In this paper, we propose SpeechMedAssist, a SpeechLM natively capable of conducting speech-based multi-turn interactions with patients. By exploiting the architectural properties of SpeechLMs, we decouple the conventional one-stage training into a two-stage paradigm consisting of **(1) Knowledge Capability Injection via Text** and **(2) Modality Re-alignment with Limited Speech Data**, thereby reducing the requirement for medical speech data to only **10k** synthesized samples. To evaluate SpeechLMs for medical consultation scenarios, we design a benchmark comprising both single-turn question answering and multi-turn simulated interactions. Experimental results show that our model outperforms all baselines in both effectiveness and robustness in most evaluation settings.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将基于提供的论文内容，以 Markdown 格式，使用中文对该论文进行结构化、深入且客观的总结。

### 1. 论文核心问题与整体含义

*   **研究动机：**
    *   **领域痛点：** 医疗咨询（Medical Consultation）本质上是语音驱动的，但现有的大型语言模型（LLM）医疗系统主要依赖文本交互，对老年人、读写或打字能力受限的患者造成了显著的访问障碍。
    *   **现有方案不足：** 通过级联自动语音识别（ASR）、LLM和文本到语音（TTS）模块的管道系统虽然能实现语音交互，但存在延迟累积、ASR错误传播以及丢失咳嗽等副语言信息的问题，降低了医疗咨询的有效性。
    *   **技术鸿沟：** 尽管端到端的语音语言模型（SpeechLM）提供了原生支持语音多轮交互的有希望的方案，但其在医疗领域的适配面临三大挑战：缺乏领域特定的医学知识、缺乏医生的临床问诊技能（如主动追问、安全意识）、以及医学语音数据的稀缺性导致直接微调效率低下。

*   **整体含义：**
    *   本文旨在提出一种高效且有效的方法，将一个通用的语音语言模型适配到专业性极强的医疗咨询领域，使其能够原生支持与患者进行语音多轮对话。核心思想是**解决垂直领域中语音数据稀缺和微调成本高昂的“最后一公里”问题**，从而推动语音交互系统在医疗等专业场景的落地。

### 2. 方法论

论文提出了 **SpeechMedAssist**，一个专为语音多轮医疗咨询定制的 SpeechLM。其核心是一个解耦的两阶段训练范式，替代了传统的仅使用语音数据的单阶段训练。

*   **核心思想：**
    *   利用 SpeechLM 的“编码器-适配器-LLM核心-解码器”架构特性，即语音和文本在模型内部被映射到**共享的语义空间**。这意味着从文本模态获取的知识和能力，理论上可以被语音模态所复用。
    *   基于领域自适应理论，将适配过程解耦为：（1）从丰富的文本数据中注入领域知识与能力；（2）用有限的语音数据重新对齐被干扰的模态。

*   **关键技术细节与流程：**

    *   **阶段一：通过文本注入知识与能力（Knowledge & Capability Injection via Text）**
        *   **操作：** 冻结所有语音相关模块（语音编码器、适配器、生成器、声码器），将 SpeechLM 退化为一个纯粹的 LLM 核心 + 文本模块。
        *   **数据：** 使用大规模、高质量的医学文本数据集 **TextMedDataset**（405k 样本，通过改写和过滤构建）进行训练。
        *   **目标：** 将医学知识、诊断能力、临床问诊工作流（如循序渐进的症状披露、主动追问、基于证据的决策）和安全约束注入 LLM 核心。
        *   **理论解释：** 主要优化了 LLM 核心在文本域上的误差 ϵt(f)，使其变得很小。

    *   **阶段二：用有限语音数据重新对齐模态（Modality Re-alignment with Limited Speech Data）**
        *   **操作：** 解冻所有模块，但仅使用少量的医学语音对话数据进行训练。
        *   **数据：** 使用 **SpeechMedDataset**（198k 样本，但实验证明仅需 10k 样本即可）。该数据集是通过一个考虑患者年龄、性别的 pipeline 合成的。
        *   **目标：** 重新对齐在阶段一中被扰动的语音-文本模态，将文本域的能力迁移到语音域。根据领域自适应理论，此阶段主要为了最小化文本与语音嵌入之间的分歧度 dH∆H(Dt, Ds)。
        *   **具体步骤：** 阶段二分为两步：(a) 联合训练语音适配器和 LLM 核心，使用 `<语音输入, 文本回复>` 对；(b) 仅训练语音解码器，使用 `<语音输入, 语音回复>` 对，以提升生成语音的质量。

### 3. 实验设计

*   **数据集与场景：**
    *   **训练集：**
        *   **TextMedDataset (405k样本):** 通过统一的重写和合成管道构建，包含来自医学考试、百科、书籍、诊断对话、安全约束等多源数据。
        *   **SpeechMedDataset (198k, 高效仅需10k):** 从 TextMedDataset 的对话中提取患者信息，匹配说话人属性（年龄、性别），并使用 CosyVoice2 合成语音。
    *   **评估基准 SpeechMedBench：**
        *   **单轮问答：** 评估医学知识掌握（CMB、CME 选择题）和语音交互下的医学百科全书问答（Ency）与安全性（Safety）。
        *   **多轮对话：** 构建了一个由 LLM 驱动的虚拟医疗咨询环境，包含患者模拟器、待评估的实习医生模型和主考官裁判模型。基于真实的医患对话（MedDG）和病例（AIHospital）进行交互。
        *   **真实场景测试（Wild）：** 收集了 20 组来自真实临床环境且带有背景噪声的患者语音问题，由 5 名医学专业人士进行投票。
        *   **语音质量评估：** 评估自然度（UTMOS）、内容一致性（ASR-CER）和延迟（Latency）。

*   **对比方法（Baselines）:**
    *   **ASR + LLMs + TTS级联模型:** 包括 HuatuoGPT2、DISC-MedLLM、Zhongjing、Baichuan2 等文本医疗 LLM，外接 ASR 和 TTS 模块。
    *   **通用 SpeechLMs:** 包括 Kimi-Audio、Qwen2-Audio、GLM4-Voice、SpeechGPT2、StepAudio2-mini 等。
    *   **多模态全栈模型（OmniLMs）:** 包括 Qwen2.5-Omni、BaichuanOmni-1.5、MiniCPM-o 2.6，以及医疗多模态模型 ShizhenGPT-Omni。

### 4. 资源与算力

*   论文中没有明确给出具体的 GPU 型号、数量和总训练时长。文中仅提及了训练的超参数，例如阶段一的 batch size 为 8，学习率为 5e-5；阶段二的 batch size 为 1，学习率为 1e-5。计算资源得到复旦大学 CFFF 平台的支持。

### 5. 实验数量与充分性

*   **实验数量：** 实验设计全面，覆盖了多个维度。
    *   **主实验（Main Results）：** 在单轮问答（4个指标）、多轮对话（2个场景，6个维度）、真实世界测试上进行大规模横向对比。
    *   **消融实验：** 严格验证了两阶段训练的必要性（仅阶段一、仅阶段二 vs. 完整流程）。
    *   **效率分析：** 定量分析了模态重对齐阶段所需的**最少语音数据量**（10k samples），并对比了训练效率。
    *   **鲁棒性测试：** 测试了模型在不同强度噪声下的表现和对咳嗽等副语言信号的感知能力。
    *   **泛化性验证：** 在另一种 SpeechLM 架构（OpenS2S）上验证了训练策略的通用性。
    *   **公正性验证：** 使用 Qwen、LLaMA、DeepSeek 三种不同的 LLM 作为多轮对话的裁判模型，以消除单一裁判的偏差。

*   **充分性与客观公平性评价：** 实验设计**非常充分、客观且公平**。它不仅在多个维度和场景下进行了综合评估，还通过更换基座模型、裁判模型、以及详尽的消融实验来验证方法的稳健性和普适性，有力地支撑了论文提出的每一个主张。

### 6. 主要结论与发现

1.  **高效领域适配：** 提出的两阶段训练策略极其高效，仅需 **10,000 个合成语音样本**即可完成模态重对齐，使模型获得医学领域能力，大幅降低了对稀缺医学语音数据的依赖。
2.  **性能全面领先：** 在涵盖医学知识、诊断能力、安全性和真实世界表现的 **SpeechMedBench** 基准上，**SpeechMedAssist 在绝大多数指标上均优于所有基线模型**，包括级联模型和通用的全栈多模态模型。
3.  **优越的交互体验与鲁棒性：** 模型能生成简洁、符合临床习惯的回复，具备更低的延迟，并对环境噪声有很强的鲁棒性。同时能有效感知并利用咳嗽等副语言信息进行诊断推理。
4.  **模态对齐的关键性：** 单纯的文本训练（阶段一）会轻微干扰语音-文本的共享空间，损害多轮对话性能。经过阶段二的重对齐后，性能不仅恢复，还在文本域能力的基础上显著提升。
5.  **方法具有通用性：** 该两阶段训练策略不仅在 LLaMA-Omni2 上有效，在 OpenS2S 模型上同样取得了显著的性能提升，证明了其作为一种适配范式的普适性。

### 7. 优点

*   **方法创新且高效：** 创新性地将 SpeechLM 的领域适配解耦为文本知识注入和语音模态重对齐两个阶段，极大地降低了数据和计算成本，是一种优雅且实用的解决方案。
*   **数据构建 Pipeline：** 提出了一套可扩展的、包含改写和合成的多轮医学语音对话数据构建流程，并考虑了患者属性，确保了合成数据的多样性和合理性。
*   **评估基准全面坚实：** 构建的 **SpeechMedBench** 综合了客观题、模拟多轮交互、真实世界测试和语音质量评估，并从多个维度评价模型能力，为该领域提供了有价值的评估框架。特别是引入了副语言信号（咳嗽）的感知能力评估，具有前瞻性。
*   **实验严谨性高：** 大量的消融实验、效率分析、鲁棒性测试以及通过更换基座模型和裁判模型进行的泛化性/公正性验证，使得结论极具说服力。

### 8. 不足与局限

*   **模态单一：** 医疗诊断通常依赖于多模态信息（如影像、文本报告）。本文仅聚焦于文本和语音，未融合其他模态信息，限制了其在复杂诊断场景中的应用。
*   **语言局限：** 实验和数据集均基于中文（Mandarin），虽然通过随机音色等技术增强了泛化性，但其在多语言和多方言环境下的有效性仍有待验证。
*   **合成数据依赖：** 训练所用的语音数据集完全由 TTS 合成。虽然降低了成本，但与真实临床环境中充满情感、犹豫和各种复杂副语言信号的自然语音可能存在分布差异（Domain Gap）。
*   **安全与伦理风险：** 与其他医疗 LLM 一样，该模型仍可能存在“幻觉”问题，生成不准确的医疗建议。论文也明确指出，该模型仅应在专业监督下使用，实际部署需要额外的安全防护。

（完）
