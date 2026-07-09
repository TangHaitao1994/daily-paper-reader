---
title: Efficient and Adaptive Simultaneous Speech Translation with Fully Unidirectional Architecture
title_zh: 高效自适应同声传译的全单向架构
authors: "Biao Fu, Donglei Yu, Minpeng Liao, Chengxi Li, Xinjie Chen, Yidong Chen, Kai Fan, Xiaodong Shi"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40330/44291"
tags: ["query:speech-model"]
score: 8.0
evidence: 提出了用于训练同声传译模型的多延迟数据管理策略
tldr: 现有基于大语言模型的同声传译方法存在计算开销大或策略固定等问题。本文提出EASiST框架，采用完全单向架构和多延迟数据管理策略，训练时生成不同延迟的翻译数据，推理时根据源语音动态调整读写策略。实验表明，该方法在保持翻译质量的同时显著降低计算复杂度和响应延迟，为语音到文本的实时翻译提供了高效方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 解决基于LLM的同声传译中重复编码和固定策略导致的效率低下问题。
method: 提出完全单向架构的EASiST，采用多延迟数据管理策略和自适应读写策略。
result: 在保证翻译质量的同时减少了计算开销，实现了更低延迟的同声传译。
conclusion: EASiST为实时语音翻译提供了高效、自适应的解决方案，推动了语音模型在翻译任务中的应用。
---

## Abstract
Simultaneous speech translation (SimulST) produces translations incrementally while processing partial speech input. 
Although large language models (LLMs) have shown strong capabilities in offline translation tasks, applying them to SimulST poses notable challenges. 
Existing LLM-based SimulST approaches either incur significant computational overhead due to repeated encoding of bidirectional speech encoder, or they depend on a fixed read/write policy, limiting the efficiency and performance.
In this work, we introduce Efficient and Adaptive Simultaneous Speech Translation (EASiST) with fully unidirectional architecture, including both speech encoder and LLM. 
EASiST includes a multi-latency data curation strategy to generate semantically aligned SimulST training samples and redefines SimulST as an interleaved generation task with explicit read/write tokens. 
To facilitate adaptive inference, we incorporate a lightweight policy head that dynamically predicts read/write actions. 
Additionally, we employ a multi-stage training strategy to align speech-text modalities and optimize both translation and policy behavior. 
Experiments on both in-domain (MuST-C) and out-of-domain (Europarl-ST) En-De and En-Es datasets demonstrate that EASiST offers superior latency-quality trade-offs compared to several strong baselines.

---

## 论文详细总结（自动生成）

好的，我们开始对这篇论文进行结构化梳理。

### 1. 论文的核心问题与整体含义

*   **研究背景与动机**：同声传译（SimulST）要求在接收不完整语音输入时逐步生成译文，需要在翻译质量和延迟之间取得平衡。近来，大语言模型（LLM）在离线翻译任务中表现优异，但将其应用于SimulST面临挑战。
*   **现有方法的局限**：
    *   **级联方法**：依赖自动语音识别（ASR）结果，存在错误传播和重复编码历史输入导致的高延迟。
    *   **端到端LLM方法**：一些方法采用训练时注意力掩码（如FASST）来模拟流式场景，但固定策略限制了性能；另一些方法（如InfiniSST）将SimulST重构为多轮对话，但其数据构造依赖于词对齐工具，可能引入领域不匹配和对齐错误。
*   **核心问题**：如何设计一个高效、自适应的端到端LLM同声传译框架，既能避免重复计算，又能根据输入语义动态调整读写策略，从而优化延迟-质量权衡。

### 2. 论文提出的方法论

论文提出EASiST框架，包含三个核心部分：**多延迟数据管理**、**全单向模型架构**和**多阶段训练策略**。

*   **SimulST数据管理**
    *   **核心思想**：利用大语言模型（如GPT-4）将离线语音翻译数据转换为语义对齐、单调的块级SimulST数据。
    *   **流程**：
        1.  **生成SimulMT数据**：给定离线语音翻译三元组（语音s，转录x，译文y），LLM在多种延迟设置下，将源文x分割成语义独立的块，并同时生成对应的单调译文块，形成对齐的SimulMT数据 `cmt = [(cx1, cy1), ..., (cxI, cyI)]`。
        2.  **语音-文本对齐**：使用Montreal Forced Aligner将文本块 `[cx1, ..., cxI]` 与原始语音序列 `s` 进行时间对齐，生成对齐的语音块 `[cs1, ..., csI]`，最终得到SimulST数据 `cst = [(cs1, cy1), ..., (csI, cyI)]`。
    *   **数据质量**：构造的数据具有更低的单调性分数，同时保持较高的CometKiwi分数，表明其更适配SimulST任务。

*   **模型架构**
    *   **全单向设计**：采用流式语音编码器和带因果注意力的LLM，确保所有组件都是单向的，以便在推理时安全地复用KV缓存。
    *   **流式编码器**：使用wav2vec-S，它通过层归一化、绝对正弦位置编码和块级自注意力取代了原始wav2vec2.0中的双向设计，支持增量输入并复用历史KV缓存。
    *   **适配器**：由两个一维卷积层和线性投影层组成，以流式方式压缩语音特征长度并映射到LLM的表示空间。
    *   **交错生成格式**：将SimulST数据格式化为交错序列：`[cs1, <|eor|>, cy1, <|eow|>, ..., csI, <|eor|>, cyI, <|eow|>]`。`<|eor|>` 和 `<|eow|>` 是分别表示“读”和“写”动作切换的特殊令牌。
    *   **策略模块**：一个轻量级二分类器，基于LLM最后层的隐藏状态 `ht` 动态预测读写概率 `pt`，决定模型是继续阅读语音输入还是开始生成译文。

*   **多阶段训练**
    *   **第一阶段：SimulMT预训练**。目标是让LLM学习交错式文本流翻译格式。使用SimulMT和离线MT数据，全参数微调LLM。损失函数为 `LStage-I = LSimulMT + LMT`，对所有Token（包括源语言文本、目标语言文本、特殊令牌）计算交叉熵。
    *   **第二阶段：语音-文本模态对齐**。冻结LLM，只训练流式编码器和适配器。通过离线语音翻译任务，将语音表示与LLM的文本空间对齐。损失函数为 `LStage-II = LST`，只对目标语言文本Token计算交叉熵。
    *   **第三阶段：多任务监督微调**。进一步优化流式翻译能力，并引入策略模块。冻结LLM，微调编码器、适配器和策略模块。
        *   **SimulST任务**：使用构造的SimulST数据，对译文块和特殊令牌计算损失 `LSimulST`。
        *   **策略决策任务**：训练策略模块，对语音块边界（读标签）和译文块内令牌（写标签）计算二分类交叉熵损失 `Lpolicy`。
        *   **离线ST任务**：保留 `LST` 作为正则化项。最终损失为 `LStage-III = LSimulST + LST + λLpolicy`，λ为超参数。

*   **推理**
    *   **无差别的训练-推断**：推理过程与训练格式一致。每接收一个语音块，策略模块预测读写概率。若预测超过阈值τ，则停止读入，生成译文直至输出 `<|eow|>` 令牌。
    *   **可复用缓存**：全单向架构使得编码器和LLM的KV缓存均可安全复用，避免了历史信息的重复计算。

### 3. 实验设计

*   **数据集与场景**：
    *   **域内数据**：MuST-C v1的英语→德语（En→De）和英语→西班牙语（En→Es）数据集，用于训练和评估。测试集使用tst-COMMON。
    *   **域外数据**：Europarl-ST的En→De和En→Es测试集，用于评估泛化能力。
*   **评价指标**：
    *   **翻译质量**：采用SacreBLEU（大小写敏感、去标记化）。
    *   **延迟**：采用LAAL（长度自适应平均延迟）和计算感知延迟LAAL-CA。
*   **对比方法**：
    *   **离线ST基线**：基于wav2vec 2.0编码器、卷积适配器和LLM的级联系统。
    *   **端到端SimulST基线**：wait-k、EDAtt、AlignAtt和InfiniSST。其中，InfiniSST使用其官方发布的权重评估，其余基线基于离线ST模型实现。

### 4. 资源与算力

*   **模型组件**：使用微调过的wav2vec-S-Large作为流式编码器，Llama-3-8B-Instruct作为骨干LLM。
*   **算力配置**：所有实验在8块NVIDIA A100 80G GPU上运行一次（未见多次运行取平均的说明）。
*   **训练策略与成本**：
    *   **优化器**：AdamW，采用余弦学习率调度器。
    *   **批次大小**：全局批次大小为128。
    *   **阶段参数**：
        *   第一阶段：训练LLM 1个epoch，更新8.03B参数，总耗时约81小时。
        *   第二阶段：训练编码器和适配器6个epochs，更新323M参数，总耗时约8小时。
        *   第三阶段：训练1个epoch，更新323M参数，总耗时约7小时。
    *   **总训练时长**：约96小时（相当于4天），论文同时指出这比消融实验中的某些单阶段或两阶段训练方案节省了25%-75%的时间。

### 5. 实验数量与充分性

*   **主要实验**：在2个语言方向（En→De, En→Es）×2个数据集（MuST-C, Europarl-ST）上，展示了EASiST及5个基线方法的BLEU-LAAL和BLEU-LAAL-CA曲线。每组曲线包含多个由不同策略阈值τ（0.1到0.6）控制的延迟点，展示了完整的质量-延迟权衡。
*   **效率实验**：对比了各方法的推理速度（毫秒/令牌）。
*   **消融实验**：在En→De MuST-C tst-COMMON集上，进行了系统的消融研究：
    *   **训练阶段消融**：移除第三阶段的策略损失（`w/o Stage III Lpolicy`）、移除第三阶段的离线ST损失（`w/o Stage III LST`）、跳过第二阶段（`w/o Stage II`）、跳过第一阶段（`w/o Stage I`）、同时跳过第一和第二阶段。
    *   **微调模块消融**：探究第三阶段中分别微调不同模块（如只微调解码器、适配器等）对性能的影响。
*   **流畅度评估**：使用外部LLM（DeepSeek-V3-0324）对EASiST不同延迟阈值下的译文流畅度进行评分。
*   **实验充分性与公平性**：
    *   实验覆盖了域内、域外场景，使用了多种延迟和计算感知指标，对比了多个强基线，且包含详细的内部消融，实验设计较为充分。
    *   公平性方面，除InfiniSST使用官方权重外，其余基线均在同一离线ST模型基础上实现，统一的评价框架保障了公平性。但所有实验仅运行一次，未报告方差，结果的稳定性未知。

### 6. 论文的主要结论与发现

*   EASiST在MuST-C和Europarl-ST数据集上均取得了优异的延迟-质量权衡，尤其在低延迟和中延迟区域显著超越wait-k、EDAtt、AlignAtt和InfiniSST等强基线。
*   使用计算感知延迟LAAL-CA评估时，EASiST的优势更加明显，这归因于其全单向架构能够高效复用KV缓存，极大地减少了计算开销。
*   消融实验证明：多阶段训练策略、第三阶段的策略损失与离线ST正则化、以及合理的微调模块选择（编码器+适配器）均对最终性能至关重要。单阶段或跳过特定阶段的训练会显著降低模型性能。
*   所提出的SimulST数据管理方法有效，构造的数据具有更好的单调性和近似的翻译质量，为模型训练奠定了良好基础。

### 7. 优点

*   **架构统一与高效**：提出的全单向架构（编码器+LLM）实现了训练和推理的无缝对接，并可完全复用KV缓存，显著提升了推理效率。
*   **自适应策略**：引入策略模块，使模型能根据语义上下文动态决定读写时机，而非依赖固定的预设规则，增强了模型对复杂输入的适应能力。
*   **数据驱动的构造方法**：利用LLM和强制对齐工具，从离线数据中自动生成多延迟、语义对齐的SimulST训练数据，缓解了流式训练数据匮乏的问题。
*   **训练策略稳健**：分阶段训练逐步引导模型学习交错格式、模态对齐和策略决策，有效降低了训练难度，提升了训练效率和最终性能。

### 8. 不足与局限

*   **数据构造依赖外部工具**：SimulST数据的质量高度依赖于GPT-4的语义分割能力和Montreal Forced Aligner的对齐精度，工具本身的误差可能会影响数据质量。
*   **单次实验的不确定性**：所有实验仅运行一次，未提供多次运行的均值和方差，结果的可重复性和统计显著性有待验证。
*   **语言与领域的局限性**：实验仅在英语→德语和英语→西班牙语两个高资源语言对上进行，且训练数据仅来自MuST-C，其在低资源语言或更多样领域下的泛化能力尚不明确。
*   **策略模块的简洁性**：策略模块是一个简单的单层二分类器，其决策能力可能有限，本文未探讨更复杂的策略网络（如多头注意力等）是否能带来进一步的提升。
*   **长期依赖与鲁棒性**：虽然使用了块级对齐，但在面对需要等待极长距离才能翻译的“重排序”句子时，其块级的读写决策能否很好地捕捉长距离依赖，论文未作深入分析。

（完）
