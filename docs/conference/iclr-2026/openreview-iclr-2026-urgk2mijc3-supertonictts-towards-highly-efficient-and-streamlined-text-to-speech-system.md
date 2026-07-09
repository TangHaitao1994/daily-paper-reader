---
title: "SupertonicTTS: Towards Highly Efficient and Streamlined Text-to-Speech System"
title_zh: SupertonicTTS：迈向高效精简的文本到语音系统
authors: "Hyeongju Kim, Jinhyeok Yang, Yechan Yu, Seunghun Ji, Jacob Richard Morton, Frederik Bous, Joon Byun, Juheon Lee"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=urGk2mIJc3"
tags: ["query:speech-model"]
score: 9.0
evidence: 高效TTS系统，低维隐空间，流匹配，字符级输入，消除音素转换和外部对齐器
tldr: 为解决语音合成系统高效性和简洁性问题，SupertonicTTS采用低维隐空间、流匹配和字符级文本输入，构建了一个轻量级TTS流水线。通过ConvNeXt模块和跨注意力机制实现文本-语音对齐，省去了G2P和外部对齐器，并在批次推理中采用上下文共享策略进一步加速。实验表明该系统在保证音质的同时显著提升了合成速度和推理效率，对实时语音合成具有推进意义。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 构建高效且精简的TTS系统，减少模块和计算开销。
method: 基于低维隐空间自编码器、流匹配文本到隐变量映射、ConvNeXt和字符级输入。
result: 实现轻量级架构，合成速度显著提升。
conclusion: 高效TTS设计为实时语音合成提供了可行方案。
---

## Abstract
We introduce SupertonicTTS, a novel text-to-speech (TTS) system designed for efficient and streamlined speech synthesis. SupertonicTTS comprises three components: a speech autoencoder for continuous latent representation, a text-to-latent module leveraging flow-matching for text-to-latent mapping, and an utterance-level duration predictor. To enable a lightweight architecture, we employ a low-dimensional latent space, temporal compression of latents, and ConvNeXt blocks. The TTS pipeline is further simplified by operating directly on raw character-level text and employing cross-attention for text-speech alignment, thus eliminating the need for grapheme-to-phoneme (G2P) modules and external aligners. In addition, we propose context-sharing batch expansion that accelerates loss convergence and stabilizes text-speech alignment with minimal memory and I/O overhead. Experimental results demonstrate that SupertonicTTS delivers performance comparable to contemporary zero-shot TTS models with only 44M parameters, while significantly reducing architectural complexity and computational cost.

---

## 论文详细总结（自动生成）

# SupertonicTTS：迈向高效精简的文本到语音系统 — 论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前主流文本到语音（TTS）系统普遍依赖复杂的模块化流水线（如音素转换、外部时长对齐器、高维表征），导致系统架构臃肿、计算开销大，难以在资源受限场景下实现高效、实时的语音合成。
- **整体含义**：该工作旨在通过精简流水线、压缩表征空间和采用更轻量的网络设计，构建一个**极高效且结构极其简单的零样本 TTS 系统**，从而推动 TTS 技术在实时、边缘设备等场景中的实际落地。

## 2. 论文提出的方法论

- **总体架构**：SupertonicTTS 由三个核心组件构成：
  1.  **语音自编码器**：将语音波形压缩至连续的低维隐空间，并采用时间维度的压缩来减少序列长度。
  2.  **文本到隐变量模块**：基于 **流匹配（flow‑matching）** 的生成模型，直接在隐空间中完成从文本表征到声学隐变量的映射。
  3.  **语句级时长预测器**：一次性预测整个语句的时长分配，替代逐帧或逐音素的对齐。

- **关键技术细节**：
  - **轻量化设计**：使用 **低维隐空间** 和 **ConvNeXt 模块** 构建语音编解码器，大幅减少参数量和计算量。
  - **端到端字符级输入**：文本输入直接采用原始字符（无需音素转换），利用 **交叉注意力（cross‑attention）** 机制在解码过程中自动实现文本与语音的对齐，彻底移除了传统的 **G2P 模块** 和 **外部对齐器**。
  - **上下文共享批次扩展（Context‑Sharing Batch Expansion）**：一种训练策略，在批次内共享上下文信息，加速损失收敛、稳定文本‑语音对齐，并且几乎没有增加内存和 I/O 开销。

- **算法流程简述**：
  1.  语音经自编码器的编码器得到时序压缩的隐变量序列。
  2.  原始字符序列通过文本编码器提取特征，与隐变量序列进行交叉注意力，完成文本‑语音软对齐。
  3.  时长预测器依据对齐信息生成语句级时长，驱动解码器（或流匹配模块）从文本特征预测目标隐变量序列。
  4.  最终由自编码器的解码器将隐变量恢复为波形。

## 3. 实验设计

- **数据集与场景**：论文摘要未明确列出所使用的语音数据集。通常此类零样本 TTS 模型会在 **多说话人、大量语音数据集**（如 LibriTTS、VCTK 等）上进行训练与评测，但此处无具体信息。
- **Benchmark 与对比方法**：实验对比了 **当代零样本 TTS 模型**（摘要未指出具体模型名称，可能涵盖如 VALL‑E、NaturalSpeech 系列等 SOTA 系统）。评估指标应包含合成音质（如 MOS）、推理速度和参数量。
- **主要结论**：仅 44M 参数量的 SupertonicTTS 取得了 **与当代零样本 TTS 模型相媲美的性能**，同时显著降低了架构复杂度和计算成本。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量及训练时长。从“高效”“轻量”的设计理念推测，其训练成本可能远低于主流大模型，但摘要及提供的元数据中未给出具体算力细节。

## 5. 实验数量与充分性

- **实验组数评估**：摘要仅报告了整体性能对比和参数量（44M），未提及消融实验（如移除 G2P、不同隐空间维度、ConvNeXt vs. 其他主干网络等）或不同数据集下的鲁棒性分析。
- **充分性与公平性**：从摘要看，实验能证明模型在极低参数量下可达到先进水平，支撑了“高效精简”的核心主张。但若缺乏消融实验来量化各设计（字符级输入、低维隐空间、上下文共享批次扩展）的独立贡献，论证的完整性会打折扣。同样，若对比的零样本模型在参数量和训练数据量级上与 SupertonicTTS 存在巨大差异，则需注意对比的公平性。

## 6. 论文的主要结论与发现

- SupertonicTTS 成功构建了一个 **仅需 44M 参数** 的完整零样本 TTS 流水线。
- 通过 **低维隐空间、流匹配、字符级端到端学习** 以及 **移除 G2P 和外部对齐器**，在保持高合成质量的前提下，大幅压缩了模型体量和计算复杂度。
- 提出的 **上下文共享批次扩展** 有效提升了训练稳定性和效率。
- 总体验证了 **极其精简的 TTS 架构** 能够与大规模零样本模型性能看齐，为实时语音合成提供了可行方案。

## 7. 优点（方法或实验设计的亮点）

- **极致的系统精简**：砍掉 G2P、外部对齐器，使用原始字符作为输入，大幅简化工程流水线。
- **轻量高效的架构**：ConvNeXt 主干、低维隐空间与时间压缩，显著降低参数量和计算负担（仅 44M）。
- **优雅的文本‑语音对齐**：完全依赖交叉注意力机制从数据中自动学习对齐，无需人工标注或额外的对齐模型。
- **创新的训练策略**：上下文共享批次扩展在几乎不增加开销的情况下加速收敛并提升对齐鲁棒性。
- **实用导向**：强调“高效”与“精简”，对于实时、边缘计算等部署场景具有直接价值。

## 8. 不足与局限

- **实验细节缺失**：摘要中未公布训练数据集、评测协议、具体的对比模型以及详细的消融实验设计，难以评估结论的普适性和稳健性。
- **泛化性未知**：未提及对多语言、带噪文本、情感或风格控制等场景的支持能力，应用边界不清晰。
- **性能上限可能受限**：极简架构和低维压宿对于超高保真度录音或复杂口音的建模能力能否媲美大型模型，尚需进一步验证。
- **对比公平性存疑**：若对比的零样本模型使用了大规模预训练或海量数据，而 SupertonicTTS 仅使用相对小的数据，则“性能相当”的结论需要谨慎解读；反之亦然。
- **流匹配的稳定性**：虽然流匹配是高效生成模型，但在 TTS 中的训练稳定性和长文本生成时的对齐鲁棒性，摘要未予讨论。

（完）
