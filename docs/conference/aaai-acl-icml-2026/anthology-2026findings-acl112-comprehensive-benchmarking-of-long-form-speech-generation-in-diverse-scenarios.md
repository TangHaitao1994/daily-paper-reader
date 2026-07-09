---
title: Comprehensive Benchmarking of Long-Form Speech Generation in Diverse Scenarios
title_zh: 面向多场景的长文本语音生成综合评估基准
authors: "Changhao Pan, Rui Yang, Han Wang, Zhuan Zhou, Xuming He, Wenxiang Guo, Ziyue Jiang, Ruiqi Li, Yu Zhang, Chenyuhao Wen, Ke Lei, Xiang Yin, Jingyu Lu, Zhiyuan Zhu, Zhou Zhao"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.112.pdf"
tags: ["query:speech-model"]
score: 5.0
evidence: 长文本语音生成质量的多维评估基准
tldr: 现有长文本语音合成评估局限于特定领域且忽略连贯性等关键长文本因素。本文提出LFSBench，这是一个覆盖多场景、解耦长文本质量维度的综合基准，包含丰富语音场景与针对性指标，能够系统衡量长语音生成模型的整体表现，从而指导未来的质量优化方向。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1662, \"height\": 599, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1645, \"height\": 735, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 776, \"height\": 468, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1641, \"height\": 737, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1608, \"height\": 805, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1643, \"height\": 824, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1606, \"height\": 721, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.112/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1653, \"height\": 2337, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 813, \"height\": 318, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1656, \"height\": 855, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1657, \"height\": 675, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 801, \"height\": 456, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 797, \"height\": 322, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 800, \"height\": 1045, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1644, \"height\": 398, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1649, \"height\": 543, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 617, \"height\": 769, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 707, \"height\": 413, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 799, \"height\": 437, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 802, \"height\": 454, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1658, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1648, \"height\": 1629, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.112/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1649, \"height\": 1160, \"label\": \"Table\"}]"
motivation: 长文本语音合成评估场景有限，缺乏针对一致性、连贯性等长文本质的标准化基准。
method: 构建LFSBench，解构长文本语音质量维度，设计多场景测试集与对应指标。
result: 为长文本语音生成模型提供了系统化的评估框架，揭示了现有模型的薄弱环节。
conclusion: 该基准将促进长语音合成领域评价体系完善与技术进步。
---

## Abstract
Recent advances in speech generation have enabled high-fidelity synthesis, yet systematic evaluation of models under long-context conditions remains largely underexplored. A comprehensive evaluation benchmark for long-form speech is indispensable for two reasons: 1) existing test scenarios are often confined to limited domains, creating a significant gap with the diverse downstream applications; 2) existing metrics overlook critical long-text factors such as consistency and coherence, failing to generalize reliably. To this end, we propose LFSBench, a comprehensive benchmark that decomposes “long-form speech quality” into specific, disentangled dimensions. LFSBench has three key properties. 1) Rich speech scenarios: Focusing on long-form speech generation and multi-speaker dialog generation, LFSBench covers acoustics, semantics, and expressiveness challenges, and consists of 1,101 samples spanning 17 common speech scenarios; 2) Comprehensive evaluation dimensions: Along the acoustics, semantics, and expressiveness axes, LFSBench defines an automated evaluation protocol with seven metrics to provide a comprehensive, accurate, and standardized assessment; 3) Valuable Insights: Through extensive experiments, we reveal that current models still struggle in highly expressive scenarios and exhibit a notable gap in consistency and hierarchy compared to real recordings.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文内容，以下是详细的中文结构化总结。

### 1. 论文的核心问题与整体含义

*   **核心问题**：当前的长文本（Long-form）语音生成模型发展迅速，但缺乏一个系统、全面且能反映人类感知的评估基准。
*   **研究动机与背景**：
    *   **场景局限**：现有的评估测试集多局限于单一领域或单人场景，与真实世界中多样的下游应用（如播客、有声书、直播）存在巨大差距。
    *   **指标不足**：现有的语音质量指标（如词错误率WER）在长文本下趋于饱和，且无法有效衡量长文本特有的、关键的连贯性（coherence）和一致性（consistency）问题，与人类主观感知的相关性较差。
    *   **整体含义**：本研究旨在填补这一空白，提出一个综合性的评估基准，以系统性地衡量和推动长文本语音生成技术的进步。

### 2. 论文提出的方法论

*   **核心思想**：将“长文本语音质量”解耦为声学、语义和表达力三个核心挑战，设计分层评估协议，并使用与人类感知高度对齐的自动化指标来评估模型。
*   **关键技术细节**：
    *   **数据集构建（LFSBench）**：
        *   **多源数据整合**：从在线文本语料库、在线音频媒体和大型语言模型生成三个渠道收集数据。
        *   **数据精炼流程**：经过语义去重、质量评估、隐私与伦理过滤、人工审核等步骤，最终构建包含1，101个样本、覆盖17个下游场景的测试集。
    *   **七项解耦评估指标**：
        1.  **音色一致性**：计算公式：对音频应用滑动窗口，提取说话人嵌入，然后计算所有不同窗口嵌入对之间余弦相似度的平均值。对于对话，首先通过强制对齐获取每位说话人的段落，再分别计算并取均值。
        2.  **混响一致性**：计算公式：对音频应用滑动窗口，计算每个窗口的语音-混响调制能量比（SRMR），然后取该序列的标准差。值越低，表示混响越稳定。
        3.  **声音保真度**：计算公式：使用非侵入式、无参考的SQUIM-PESQ指标进行评估。
        4.  **内容准确性**：计算公式：利用ASR模型转录合成音频，计算其与标准文本的词错误率（WER）或字符错误率。
        5.  **韵律连贯性**：计算公式：使用在长文本韵律任务上微调过的SpeechJudge模型进行打分（1-5分）。
        6.  **表达力丰富度**：计算公式：将音频切分为10秒的片段，利用大型音频-语言模型（LALM）对每个片段打分（1-5分），取所有片段的平均分。
        7.  **表达力层次感**：计算公式：直接对整个长音频，使用大型音频-语言模型进行全局性评估（1-5分），关注情感和动态的变化。
    *   **人工感知对齐验证**：通过用户研究，证明所提出的自动化指标（如韵律、表达力）与人类评分具有高相关性（斯皮尔曼相关系数分别为0.82和0.71/0.62）。

### 3. 实验设计

*   **数据集/场景**：使用自建的 **LFSBench**，其包含3大核心挑战下的 **17个具体语音场景**（如有声书、播客、新闻、直播、戏剧等），共 **1,101个测试样本**。
*   **基准模型**：广泛对比了超过20个先进模型，包括：
    *   **10个开源长文本语音模型**（如ZipVoice、Spark-TTS、CosyVoice系列、MegaTTS3、VibeVoice等）。
    *   **6个闭源商业系统**（如Gemini-2.5-pro、OpenAI-tts-1-hd、ElevenLabs、Minimax等）。
    *   **6个开源对话模型**和 **4个闭源对话模型**（如MoonCast、SoulX-Podcast等）。
*   **评估角度**：实验从多个视角进行，包括：
    *   **分维度评估**：在所有七个指标上对比模型表现。
    *   **分场景评估**：分析模型在不同核心挑战（声学、语义、表达力）场景下的性能。
    *   **分生成长度评估**：测试模型在输入文本长度变化时的性能表现。

### 4. 资源与算力

*   **文中提及的算力**：
    *   所有开源模型的推理和评估实验在一块搭载 **8张NVIDIA GeForce RTX 4090 GPU** 的服务器上进行。
    *   未提及模型训练所需的算力或时长。
*   **明确说明的缺失**：文中仅说明了推理环境的硬件配置，未提供训练阶段的算力开销信息.

### 5. 实验数量与充分性

*   **实验数量**：实验规模庞大，涵盖了多个层面，至少进行了以下主要组别实验：
    *   长文本语音模型对比（10+模型 × 17场景 × 7指标）。
    *   对话生成模型对比（10+模型 × 多场景 × 7指标）。
    *   多用户对话专项评估（3模型 × 特定测试集）。
    *   消融实验：滑动窗口大小、生成文本长度对性能的影响。
    *   不同评估模型（LALM）对齐人类感知的对比实验。
    *   中英双语分语言评估。
*   **充分性与公平性**：
    *   **充分性**：实验设计非常充分，从模型类型、评估维度、场景覆盖、文本长度等多个角度进行了全面的分析，结论有坚实的数据支撑。
    *   **客观公平性**：实验力求公平，限制了闭源模型的可调参数；为开源模型精选25种参考音色并报告最佳结果；明确指出了评估依赖闭源模型的局限性，体现了客观性。

### 6. 论文的主要结论与发现

*   **整体差距**：当前最好的模型在表达力丰富度和层次感上，与真实录音仍有1分左右的MOS分差，且在声学一致性上存在差距。
*   **场景影响显著**：模型在需要强表达力的场景（如体育解说、演讲）中性能下降明显，表明模型缺乏有效的表达性数据训练。
*   **架构差异**：
    *   **自回归（AR）模型**：韵律建模和表达力更强，但存在长序列下的错误累积问题。
    *   **非自回归（NAR）模型**：生成更鲁棒、高效，但韵律平滑，缺乏动态变化。
    *   **解决方向**：提出未来架构应结合两者优势，走向“从粗到细”的生成框架。
*   **数据质量与数量之争**：单纯增加数据量（scaling laws）会导致表达力同质化。数据中的短文本偏向、声学不稳定性是限制长文本生成能力的关键瓶颈。未来应优先考虑数据质量和时间连续性，采用课程学习策略。

### 7. 优点

*   **场景丰富性**：覆盖17个场景和3大核心挑战，远超现有基准，贴近实际应用。
*   **评估的系统性**：提出了一套解耦、分层的自动化评估指标，特别是针对长文本特点设计的“韵律连贯性”、“表达力层次感”等指标，弥补了传统指标的不足。
*   **人工对齐验证**：通过严谨的用户研究验证了自动化指标与人类感知的一致性，增强了评估结果的可信度。
*   **深刻的洞察**：实验分析深入，揭示了AR/NAR模型的本质差异、数据质量的重要性等问题，为后续研究指明了方向。

### 8. 不足与局限

*   **语言覆盖有限**：基准目前仅限于中文和英文，缺乏对低资源语言的评估能力。
*   **语义理解不足**：对语义维度的评估仍停留在准确性和韵律连贯性，缺乏一个鲁棒的自动化框架来评估基于深层语义理解的情感和风格变化。
*   **评估模型依赖闭源产品**：表达力相关指标依赖Gemini 3 Pro等闭源LALM，其API可能的更新会带来评估结果的不可复现风险。
*   **参考音色偏差风险**：实验使用的参考音色数量有限，而模型性能对此较为敏感，可能引入评估偏差。

（完）
