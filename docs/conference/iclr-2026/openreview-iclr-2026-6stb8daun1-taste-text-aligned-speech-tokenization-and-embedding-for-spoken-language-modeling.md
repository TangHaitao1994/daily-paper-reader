---
title: "TASTE: Text-Aligned Speech Tokenization and Embedding for Spoken Language Modeling"
title_zh: TASTE：面向口语语言建模的文本对齐语音分词与嵌入
authors: "Liang-Hsuan Tseng, Yi-Chang Chen, Kuan Yi Lee, Da-shan Shiu, Hung-yi Lee"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=6STb8DauN1"
tags: ["query:speech-model"]
score: 8.0
evidence: 引入文本对齐的语音分词作为语音语言模型的数据预处理步骤，直接优化数据管道。
tldr: 针对语音语言模型中语音token与文本对齐不足的问题，提出TASTE方法，在分词阶段通过注意力聚合机制将语音token与对应文本对齐。在保留语音重建能力的同时实现跨模态语义对齐，实验表明该方法显著提升了联合文语建模的性能，为语音对话模型的数据预处理提供了有效方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音token与文本之间存在模态鸿沟，难以直接用于联合文语建模。
method: 在语音分词阶段，通过注意力聚合对齐语音与文本，以语音重建为训练目标。
result: 在多项联合建模任务中表现优异，有效提升口语语言模型的跨模态理解能力。
conclusion: 文本对齐的语音分词是提升语音大模型性能的关键预处理步骤。
---

## Abstract
Recent efforts target spoken language models (SLMs) that not only listen but also speak for more natural human-LLM interaction. Joint text-speech modeling is a promising direction to achieve this. However, the effectiveness of recent speech tokens for joint modeling remains under-explored. To address this, we introduce Text-Aligned Speech Tokenization and Embedding (TASTE), a method that directly addresses the modality gap by aligning speech token with the corresponding text transcription during the tokenization stage. We propose a method that can achieve this through a attention-based aggregation mechanism and with speech reconstruction as the training objective. We have conducted extensive experiments to demonstrate that TASTE can preserve essential paralinguistic information while dramatically reducing the token sequence length. Moreover, TASTE enables straightforward joint spoken language modeling by using Low-Rank Adaptation on the pre-trained text LLM. Our experimental results show that joint modeling with TASTE outperforms other pre-trained SLMs in tasks such as speech continuation and likelihood-based next-speech selection, showcasing its effectiveness. To our best knowledge, TASTE is the first end-to-end approach that utilizes a reconstruction objective to learn a joint tokenization and embedding tailored for text-speech spoken language modeling.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **核心问题**：构建能听会说的口语语言模型（Spoken Language Models, SLMs），实现更自然的人机语音交互，需要将语音与文本进行联合建模。然而，当前语音分词器产生的语音 token 与文本 token 之间存在明显的模态鸿沟，导致直接使用这些语音 token 与文本进行联合训练时效果不佳。
- **整体含义**：本工作旨在从数据预处理（分词）阶段直接解决这一对齐问题。通过提出文本对齐的语音分词与嵌入方法 TASTE，将语音 token 在预训练阶段就与对应的文本转录进行对齐，为后续的联合文语建模提供一座“语义桥梁”，从而从根本上提升口语语言模型的跨模态理解与生成能力。

## 2. 论文提出的方法论

- **核心思想**：在语音分词的训练过程中引入文本作为指导，通过注意力机制将语音片段聚合为与文本单元对齐的 token，使得语音 token 不仅能够重建原始语音，还能在语义上与对应文本保持一致。
- **关键技术细节**：
  - **注意力聚合机制**：以语音编码器的输出和文本转录作为输入，利用基于注意力的聚合模块，将语音帧特征动态合并到与文本序列对应的离散语音 token 上，实现强制对齐。
  - **训练目标**：以**语音重建**作为主要训练目标，即通过解码器从对齐后的语音 token 恢复原始波形或声学特征。这一目标确保所学的 token 保留了足够的副语言信息（如音色、情感、韵律），而非仅坍缩为文本语义。
  - **端到端学习**：整个分词器与嵌入模块是端到端训练的，首次在重建目标下学习专门服务于文语联合建模的联合分词与嵌入表示。
  - **下游联合建模**：在预训练好的文本大语言模型（LLM）上，利用**低秩适配**（Low-Rank Adaptation, LoRA）将 TASTE 的语音嵌入直接接入，无需修改模型架构即可实现文语联合微调。
- **公式/算法流程说明**（原摘要未给公式，以文字转述核心流程）：
  1. 语音编码器提取帧级特征；
  2. 对齐模块接收特征与文本 token 序列，用注意力计算每个文本位置对语音帧的聚合权重，生成离散的语音 token；
  3. 语音解码器从这些 token 重建语音，计算重建损失；
  4. 训练收敛后，语音 token 与对应的文本 token 在语义上高度对齐，可直接用于文本 LLM 的输入。

## 3. 实验设计

- **使用的数据集/场景**：摘要中未提供具体数据集名称，但提及任务场景包括**语音续写**（speech continuation）和**基于似然的下一语音段选择**（likelihood-based next-speech selection），这些任务用于评估联合文语建模的跨模态理解与生成能力。
- **benchmark 与对比方法**：以其他预训练 SLM 作为对比基线，验证 TASTE 在联合建模任务上的性能优势。具体对比对象名称未在摘要中列出，但明确表明“优于其他预训练 SLMs”。
- **评估维度**：除了上述任务，还验证了 TASTE 保留副语言信息的能力，以及显著缩短语音 token 序列长度的效果。

## 4. 资源与算力

- **文中未明确说明**：所提供摘要和元数据中，没有提及任何有关 GPU 型号、数量、训练时长或总计算量（如 FLOPs）的信息。无法推断其资源消耗水平。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“进行了大量实验（extensive experiments）”，至少涵盖：
  - 语音重建与副语言信息保留验证；
  - 语音 token 序列长度压缩效果；
  - 语音续写任务；
  - 下一语音段选择任务；
  - 与其他预训练 SLM 的比较；
  - （可能包含）消融实验（如对齐机制的有无、LoRA 的配置等），但具体组数未在摘要中披露。
- **充分性与公平性**：虽然缺少精确数字，但实验设计涵盖了预训练分词的内在质量（重建、信息保留、长度）和下游联合建模任务的表现，且与同类 SLM 直接比较，具备一定的说服力。但无法根据现有信息评估是否存在对比条件不公平或实验覆盖不足的风险。

## 6. 论文的主要结论与发现

- 文本对齐的语音分词是提升语音大模型性能的**关键预处理步骤**。
- TASTE 方法能在保留语音中丰富的副语言信息（如语者、情绪、韵律）的同时，大幅压缩 token 序列长度，实现高效建模。
- 基于 TASTE 的联合文语模型在语音续写、下一语音段选择等任务上，表现优于其他预训练 SLM，验证了跨模态理解能力的显著增强。
- TASTE 是首个以语音重建为目标端到端学习服务于文语联合建模的联合分词与嵌入方法。

## 7. 优点

- **直面模态鸿沟**：在分词阶段就完成语音与文本的对齐，而非在下游模型中去弥补，从根本上解决问题。
- **保留副语言信息**：通过重建目标确保语音 token 不会丢失音色、情感等关键副语言特征，避免语义坍缩。
- **显著的序列压缩**：对齐聚合有效减少了语音 token 的序列长度，降低了后续建模的计算成本。
- **即插即用的联合建模**：借助 LoRA 即可将 TASTE 嵌入直接接入预训练文本 LLM，无需改变架构，简单高效。
- **端到端训练**：首个在重建目标下实现对齐嵌入学习的方法，整体流程一致性强。

## 8. 不足与局限

- **实验细节不透明**：摘要未给出数据集、对比基线模型名称、消融实验设计等关键细节，无法评估实验的全面性与普适性。
- **缺乏算力报告**：难以判断该方法的计算效率或实际落地成本。
- **潜在的应用限制**：
  - 依赖高质量的平行语料（语音-文本对）进行分词器训练，对于低资源语言或缺乏标注的语音数据可能不适用。
  - 仅验证了特定下游任务，其在更复杂的口语对话生成、多轮交互等场景下的效果未知。
  - 对齐机制可能受注意力精度影响，在长语音或噪声条件下对齐性能是否会下降需要进一步检验。

（完）
