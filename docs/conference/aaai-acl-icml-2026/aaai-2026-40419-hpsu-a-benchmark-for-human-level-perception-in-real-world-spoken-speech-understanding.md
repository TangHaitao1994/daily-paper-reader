---
title: "HPSU: A Benchmark for Human-Level Perception in Real-World Spoken Speech Understanding"
title_zh: HPSU：真实世界口语理解中人类级别感知的基准测试
authors: "Chen Li, Peiji Yang, Yicheng Zhong, Jianxing Yu, Zhisheng Wang, Zihao Gou, Wenqing Chen, Jian Yin"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40419/44380"
tags: ["query:speech-model"]
score: 7.0
evidence: 评估语音理解中人类级别感知的基准，涵盖意图与情感
tldr: 针对现有语音大模型在真实口语理解中是否达到人类级听觉感知尚不明确的问题，本文提出HPSU基准，包含超2万条中英文专家验证样本，用于全面评估语音大模型对人类隐含意图和隐性情感的理解能力。该基准为衡量语音理解模型的进展提供了标准化工具，推动实现更自然的语音交互。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 语音大模型虽在ASR等任务上进展显著，但能否达到人类级听觉感知仍缺乏评估。
method: 构建包含超2万条中英文样本的HPSU基准，覆盖多种口语理解任务。
result: 基准可全面评估语音大模型的感知与理解能力，发现当前模型在意图与情感理解上仍有差距。
conclusion: HPSU为语音大模型的人类级理解评估提供了新标准，推动研究向深层语义理解发展。
---

## Abstract
Recent advances in Speech Large Language Models (Speech LLMs) have led to great progress in speech understanding tasks such as Automatic Speech Recognition (ASR) and Speech Emotion Recognition (SER). However, whether these models can achieve human-level auditory perception, particularly in terms of their ability to comprehend latent intentions and implicit emotions in real-world spoken language, remains underexplored. To this end, we introduce the Human-level Perception in Spoken Speech Understanding (HPSU), a new benchmark for fully evaluating the human-level perceptual and understanding capabilities of Speech LLMs. HPSU comprises over 20,000 expert-validated spoken language understanding samples in English and Chinese. It establishes a comprehensive evaluation framework by encompassing a spectrum of tasks, ranging from basic speaker attribute recognition to complex inference of latent intentions and implicit emotions. To address the issues of data scarcity and high cost of manual annotation in real-world scenarios, we developed a semi-automatic annotation process. This process fuses audio, textual, and visual information to enable precise speech understanding and labeling, thus enhancing both annotation efficiency and quality. We systematically evaluate various open-source and proprietary Speech LLMs. The results demonstrate that even top-performing models still fall considerably short of human capabilities in understanding genuine spoken interactions. Consequently, HPSU will be useful for guiding the development of Speech LLMs toward human-level perception and cognition.

---

## 论文详细总结（自动生成）

好的，作为一名资深学术论文分析助手，我将使用中文、以 Markdown 形式，对您提供的论文进行结构化、深入且客观的总结。

### 1. 核心问题与整体含义

-   **研究背景**：近期，语音大模型在自动语音识别等基础任务上取得了巨大进展，其目标已从简单转录迈向深层理解，包括对说话人属性、潜在意图和复杂情感状态的全面把握。
-   **核心问题**：现有基准测试主要关注粗粒度任务或纯文本推理，忽略了构成人类级别理解所需的综合感知能力，并且数据往往局限于非交互式、单一语言来源。这导致语音大模型是否能达到人类水平的听觉感知能力，特别是对真实口语中**潜在意图**和**内隐情感**的理解，仍是未知数。
-   **整体含义**：本文旨在解决评估方法上的关键缺口，通过提出 HPSU 基准，为全面评估语音大模型的人类级感知与认知能力建立一个标准化、高标准的工具，从而指导未来的研究方向。

### 2. 方法论

-   **核心思想**：构建一个模拟人类多模态认知的半自动标注流程，以高效、高质量地创建一个大规模的、包含深层语义理解任务的评估基准。
-   **关键技术细节：标注流程**
    该流程分为三个步骤：
    1.  **数据收集与预处理**：从电影片段、社交媒体视频等开放域场景收集音视频数据（如 CelebV-HQ, MELD）。通过音频质量评估工具和语音增强模型进行降噪。使用转录模型（如 Whisper）生成文本，确保音、视、文三模态数据对齐。
    2.  **信息提取与交叉验证**：
        *   **先验知识推断**：用大语言模型基于转录文本推断可能的说话人状态（如情绪、意图）。
        *   **单模态描述**：分别将音频和视频输入对应的音、视觉模型，提取其表现的说话人状态描述。
        *   **交叉验证**：将视觉描述送入音频模型，反之亦然，以评估跨模态信息的一致性和逻辑连贯性，从而得到互补的、经过验证的语义描述。
    3.  **信息融合与专家验证**：
        *   **分层融合**：使用 QWQ 推理模型将验证过的信息合成为多维度的开放式描述，构建**核心层**（主属性）、**细节层**（支撑线索）和**背景层**（上下文元素）。
        *   **数据集划分**：融合后，80% 的数据构成语音描述数据集（**HPSC**，约 5 万对），20% 的数据用于构建基准（**HPSU**）。
        *   **基准生成与人工验证**：使用 Gemini 2.5 Pro 将用于基准的多层描述结构化为评估三元组（问题，正确选项，干扰项），再由至少三位训练有素的标注员独立审核，只有全票通过的数据才被保留，最终接受率为 81.26%。
-   **基准任务设计**：
    HPSU 包含一个两层的任务层次结构，覆盖 16 个具体任务：
    *   **基础感知**：包括社会属性（年龄、性别、口音）、情绪识别（主情绪、多重情绪、情绪强度）。
    *   **复杂推理**：包括情绪推演（情绪转换易/难、内外情绪不一致易/难）、非言语行为（语音风格、可见行为、综合描述判断）、语篇语境（意图、对话场景、潜台词）。
    *   **对抗性评估设计**：为评估模型鲁棒性，每个任务实例包含三种提示变体：标准无偏查询、正面诱导提示、负面误导提示。

### 3. 实验设计

-   **数据集/场景**：评估主要基于构建的 **HPSU 基准**，该基准包含 **11,580 个英文**和 **8,786 个中文**专家验证样本，覆盖了从基本感知到复杂推理的 16 个任务。数据源自真实世界的视频语料，确保了评估的生态效度。
-   **Benchmark**：HPSU 自身即是作为新的基准提出的，旨在与 MMAU、AIR-Bench 等现有基准形成互补，弥补它们在评估深层、内隐、动态口语理解上的不足。
-   **对比方法**：共评估了 **13 个领先模型**，包括：
    *   **11 个开源模型**：Audio Flamingo 2/3/3.5， Kimi-Audio-Instruct， Qwen-Audio-Chat， Qwen2-Audio-Instruct， SALMONN， Soundwave， Baichuan-Omni-1.5， Qwen2.5-Omni， Audio-Reasoner。
    *   **2 个专有模型**：Gemini 2.5 Flash 和 Pro。
    *   **3 个基线**：
        1.  **人类基线**：由 10 位双语母语者回答 300 个分层抽样问题。
        2.  **随机猜测**：提供随机选择的理论下限。
        3.  **级联系统**：Whisper（转录） + GPT-4o（文本推理），用于分离声学信息的贡献。

### 4. 资源与算力

-   论文未提及为构建基准或运行评估所需的 **GPU 型号、数量或具体的训练/推理时长**。
-   论文主要贡献是**基准的构建和评估**。对于其提出的 **HPSC 数据集**，作者提到使用该数据对开源语音大模型进行监督微调可以增强模型能力，但并未报告 SFT 实验的具体算力开销。

### 5. 实验数量与充分性

-   **实验数量**：大规模且全面。
    1.  **主实验**：在 **16 个任务**、**两种语言**上对 **13 个模型**和 **3 个基线**进行了全面评估。结果在表 4 和图 2 中展示，构成了非常庞大的实验矩阵。
    2.  **细粒度分析**：
        *   **对抗鲁棒性测试**：对每个模型在每个任务上的响应，都分析了其在标准、正面、负面三种提示下的表现差异（图 4）。
        *   **分级错误分析**：对于主观题，分析了模型错误选项在”相似“、”中间“、”相反“三个层级的分布，以诊断推理粒度（图 5）。
-   **充分性、客观性与公平性**：
    *   **充分性**：实验设计覆盖了不同规模、不同架构、开源与闭源的模型，并从准确率、跨语言能力、对抗鲁棒性、错误粒度等多个维度进行了细致分析，实验非常充分。
    *   **客观性与公平性**：使用 Gemini 2.5 Pro 作为自动评分器，基于语义等价进行评判，避免了简单字符串匹配的偏差。同时，包含随机猜测基线和级联系统，为模型性能提供了多角度的参照。但使用 Gemini 进行评分可能对同为 Gemini 系列的模型存在潜在偏好风险。

### 6. 主要结论与发现

1.  **任务极具挑战性**：HPSU 对现有模型构成了巨大挑战。最佳模型（Gemini 2.5 Pro）平均准确率仅为 **62.6%**，远低于人类的 **87.3%**，证明了基准的难度和区分度。
2.  **基础与深层能力分化**：模型在性别识别等基础感知任务上接近人类水平，但在`场景理解`、`情绪不一致推断（难）`等深层语义推理任务上表现严重不足。
3.  **开源与闭源模型差距缩小**：在复杂的口语理解任务上，领先的开源模型（如 Qwen2.5-Omni，60.0%）与专有模型（62.6%）的性能差距极小，并在部分子任务上实现了超越。
4.  **级联方案的局限性**：”Whisper+GPT“级联方案在依赖语义的任务中表现有竞争力，但在需要精细声学细节的任务（如综合描述判断、情绪转换推断）上表现不佳，凸显了声学模态信息和先进多模态融合策略的不可或缺性。
5.  **模型易受诱导**：与人类几乎免疫的特性相反，所有评估模型都对对抗性诱导提示高度敏感，尤其在歧义性高、开放性强的任务中表现得更脆弱。具有原生全模态设计的模型（如 Gemini）显示出更强的鲁棒性，而基于纯文本 LLM 微调的模型则表现出更强的”文本偏见“。

### 7. 优点

-   **数据规模与质量**：包含超过 2 万条高质量、经专家多重验证的双语样本，为稳健评估提供了坚实基础。
-   **任务设计的深度与广度**：首次系统性地评估了跟踪情绪动态、推断潜台词等深层感知和认知能力，填补了先前基准的空白。
-   **创新的评估策略**：
    *   引入了”相似-中间-相反“的分级选项，能更精细地诊断模型错误。
    *   内建的对抗性诱导协议，首次从鲁棒性角度量化了模型的脆弱性，揭示了文本偏见等深层问题。
-   **贡献了有价值的辅助资源**：公开了 **HPSC 数据集**（5 万对语音-描述），该数据集不仅能用于 SFT，还有潜力用于可控语音生成等任务，具有催化后续研究的价值。

### 8. 不足与局限

-   **语言与场景受限**：基准仅覆盖中、英两种语言，且数据源自特定类型的视频语料（电影、社交视频），其结论是否能推广到更多语言和文化背景下的日常口语对话中尚待验证。
-   **算力开销未明**：论文未提供基准构建和模型评估时的具体算力需求，不利于后续研究者评估资源成本。
-   **评估主体的潜在偏差**：使用 Gemini 2.5 Pro 作为核心的自动评分器和部分选项的生成器，可能对同为 Gemini 家族的模型评估结果产生微妙的积极影响，尽管做出了去偏差的努力。
-   **静态评估的局限**：基准采用的是离线、单轮的问题回答形式，无法评估模型在实时、多轮、交互式对话中的动态理解能力。
-   **主观任务的客观性挑战**：虽然引入了分级选项，但高层次的语义任务（如潜台词）本身就具有主观性，将其完全转化为客观选择题可能会损失部分细微理解之间的差异评价。

（完）
