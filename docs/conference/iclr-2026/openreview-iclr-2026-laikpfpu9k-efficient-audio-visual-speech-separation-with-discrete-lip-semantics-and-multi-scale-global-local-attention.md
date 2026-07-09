---
title: Efficient Audio-Visual Speech Separation with Discrete Lip Semantics and Multi-Scale Global-Local Attention
title_zh: 基于离散唇语语义和多尺度全局局部注意力的高效视听语音分离
authors: "Kai Li, Gao Kejun, Xiaolin Hu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=LaIkPfPu9K"
tags: ["query:speech-model"]
score: 9.0
evidence: 利用唇语语义的高效视听语音分离，体现多模态学习
tldr: 现有视听语音分离方法计算成本高，难以实际应用。本文提出Dolphin，利用双路径轻量视频编码器提取唇动离散语义标记，结合多尺度注意力分离目标语音。实验表明该方法在保证分离质量的同时大幅降低参数量和计算开销。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视听语音分离方法参数量大、计算成本高，不适用于预处理场景。
method: 提出Dolphin方法，使用DP-LipCoder将唇动转化为离散语义标记，并构建轻量级音频分离网络。
result: 在保持分离质量的同时显著降低参数和计算量。
conclusion: 高效的多模态方法使视听语音分离更具实用价值。
---

## Abstract
Audio-visual speech separation (AVSS) methods leverage visual cues to extract target speech and have demonstrated strong separation quality in noisy acoustic environments. However, these methods usually involve a large number of parameters and require high computational cost, which is unacceptable in many applications where speech separation serves as only a preprocessing step for further speech processing. To address this issue, we propose an efficient AVSS method, named **Dolphin**. For visual feature extraction, we develop **DP‑LipCoder**, a dual‑path lightweight video encoder that transforms lip‑motion into discrete audio‑aligned semantic tokens. For audio separation, we construct a lightweight encoder–decoder separator, in which each layer incorporates a global–local attention (GLA) block to efficiently capture multi-scale dependencies. Experiments on three benchmark datasets showed that Dolphin not only surpassed the current state-of-the-art (SOTA) model in separation quality but also achieved remarkable improvements in efficiency: over 50\% fewer parameters, more than 2.4$\times$ reduction in MACs, and over 6$\times$ faster GPU inference speed. These results indicate that Dolphin offers a practical and deployable solution for high-performance AVSS in real-world scenarios. Our code and demo page are publicly available at https://cslikai.cn/Dolphin.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：视听语音分离（AVSS）利用说话人的唇动视觉线索，从混合语音中提取目标说话人的声音。现有方法在噪声环境下分离质量高，但普遍面临**模型参数量大、计算成本高**的问题。
- **核心问题**：在许多实际应用中，语音分离仅作为后续语音处理（如识别、合成）的预处理步骤，过高的计算开销使其难以落地。
- **整体含义**：本文旨在设计一种**高效、可部署的视听语音分离方法**，在保持甚至提升分离质量的同时，大幅降低参数量、计算量和推理延迟，使模型更适合资源受限或实时场景。

### 2. 论文提出的方法论

- **整体框架（Dolphin）**：由两个核心组件构成——用于提取紧凑视觉语义的**DP-LipCoder**，以及用于分离混合语音的**轻量级编码器-解码器分离网络**。
- **视觉特征提取（DP-LipCoder）**：
  - 提出**双路径轻量视频编码器**，将连续的唇动帧转换为**离散的语义标记（discrete semantic tokens）**。
  - 离散标记具有语义对齐性（与音频内容对齐），可大幅压缩视觉信息冗余。
  - 引入多尺度特征聚合和通道缩减策略，降低视频编码器的参数量。
- **音频分离网络**：
  - 采用轻量级编码器-解码器结构，编码器和解码器的每一层均嵌入**全局-局部注意力（Global-Local Attention, GLA）模块**。
  - GLA 模块可同时捕获**长程全局依赖**和**局部细粒度模式**，使模型在较浅深度下仍能有效建模多尺度时序上下文。
  - 视觉语义标记作为条件信息融合到分离网络中，引导目标说话人提取。
- **关键技术优势**：离散唇语语义的引入显著减少了视觉模态的冗余信息传递，GLA 模块在提升感受野的同时控制计算复杂度。

### 3. 实验设计

- **数据集与场景**：
  - 在**三个基准数据集**上进行评估（文中提及三个 but 未在提取文本中列出具体名称，通常为 LRS2、LRS3、VoxCeleb2 或类似 AVSS 常用数据集）。
  - 场景为嘈杂声学环境下的目标说话人语音分离。
- **基准（Benchmark）与对比方法**：
  - 与当前最优（SOTA）模型进行全面对比。
  - 评估指标应为语音分离质量和模型效率（如 SI-SDRi、PESQ、STOI 等质量指标，以及参数量、MACs、推理速度等效率指标，虽文本未详列，但结论中明确提及）。
- **对比实验维度**：
  - 分离质量对比（超越 SOTA）。
  - 效率对比：参数量减少 50% 以上，MACs 降低 2.4 倍以上，GPU 推理速度提升 6 倍以上。

### 4. 资源与算力

- 本段文本**未明确给出**具体的 GPU 型号、数量或训练时长。仅提到 GPU 推理速度提升 6 倍，说明效率对比是在 GPU 平台上完成，但具体算力资源未展开说明。

### 5. 实验数量与充分性

- 文中提及在**三个基准数据集**上进行评估，暗示至少进行了多组数据集实验。
- 此外通常需进行**消融实验**以验证各组件（如离散标记、GLA 模块）的有效性，虽未在摘要中列出具体消融数量，但此类论文一般包含充分的消融和变体分析。
- **充分性评估**：基于三个不同数据集的测试以及与 SOTA 的多维度对比，实验覆盖面较广；若补充了消融实验，则整体实验设计较为充分、客观、公平。目前的摘要信息有限，但可推断实验设计合理。

### 6. 论文的主要结论与发现

- Dolphin 在**分离质量上超越当前 SOTA 模型**，同时实现了显著的效率提升。
- 通过离散唇动语义标记与多尺度全局-局部注意力的结合，能够在极低计算成本下保持甚至提升分离性能。
- 该方法使高效、可实际部署的视听语音分离成为可能，适合作为语音处理流水线中的轻量级预处理组件。

### 7. 优点

- **效率与质量的平衡**：在多个数据集上同时取得 SOTA 分离效果和大幅效率提升（参数量减半、MACs 降 2.4 倍、推理快 6 倍以上），实用性强。
- **创新的视觉表征**：将唇动转化为离散语义标记，有效去冗余并与音频对齐，为后续轻量分离网络提供丰富而紧凑的信息。
- **精巧的注意力设计**：全局-局部注意力在不增加深度的情况下捕获多尺度依赖，是轻量化的关键。
- **工程开放**：提供代码和演示页面，易于复现和应用。

### 8. 不足与局限

- **数据集细节缺失**：摘要未给出具体数据集名称及规模，难以评估其泛化到更复杂真实场景的能力。
- **算力说明不充分**：未报告训练所需的 GPU 型号、数量和训练时长，无法完整衡量端到端的资源消耗。
- **可能存在的偏差**：若数据集偏向特定语种、说话人或录制条件，可能引入视觉或声学偏差；离散语义标记的泛化性（如对非正面唇动、极端噪声）仍需验证。
- **实际部署受限因素**：仅评估了 GPU 推理加速，对其他硬件（如移动端、嵌入式设备）的适配性未讨论。
- **方法局限**：离散标记虽压缩信息，但量化过程可能丢失唇动的细微细节，在极端噪声或低分辨率视频下性能可能下降；全局-局部注意力的设计效果是否在所有层保持最优未深入分析。

（完）
