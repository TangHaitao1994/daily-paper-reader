---
title: "Bridging the Temporal Gap in Multimodal LLMs: Deeply Stacking Temporal Tokens for Audio-Visual Speech Recognition"
title_zh: 弥合多模态LLM中的时序差距：为音视频语音识别深度堆叠时序Token
authors: "Liyong Wang, Junliang Xing, Tianyu Hu, Jianfei Jiang, Jihuai Zhao, Huimin Ma"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.1381.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: 音视频语音识别，用时序Token弥合时序差距
tldr: 针对当前多模态LLM在音视频语音识别中时序建模不足的问题，本文提出在编码与解码阶段深度堆叠时序Token的框架，通过时序感知注意力和时序旋转位置编码精确捕捉唇部运动动态，弥合时序差距。实验证明该方法显著提升噪声环境下的语音识别鲁棒性，为多模态ASR提供了新的技术路径。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 774, \"height\": 649, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 783, \"height\": 653, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1639, \"height\": 502, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 794, \"height\": 505, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1663, \"height\": 538, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1381/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1635, \"height\": 1308, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1381/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1582, \"height\": 1450, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1381/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 803, \"height\": 210, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1381/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 803, \"height\": 241, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1381/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 837, \"height\": 431, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1381/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 703, \"height\": 235, \"label\": \"Table\"}]"
motivation: 多模态LLM存在时序差距，视觉编码器时序建模弱，LLM解码层语义退化。
method: 在视觉编码器引入时序感知注意力与旋转位置嵌入，解码端堆叠时序Token。
result: 在音视频语音识别中，框架有效提升噪声下的鲁棒性。
conclusion: 通过深度时序Token堆叠，本工作增强了多模态LLM的时序建模能力，推动了音视频ASR发展。
---

## Abstract
Audio-Visual Speech Recognition enhances speech recognition robustness in noisy conditions by leveraging visual cues. However, current Multimodal LLMs suffer from a fundamental temporal gap. This gap is characterized by limited fine-grained temporal modeling in vision encoders and progressive temporal semantic degradation throughout the deep layers of LLM decoders. To bridge this gap, we propose a novel framework that deeply stacks temporal tokens across both the encoding and decoding stages. Specifically, we enhance the vision encoder with a temporal-aware attention module and temporal rotary positional embeddings to precisely capture the sequential evolution and dynamics of lip movements. Furthermore, we stack hierarchical temporal tokens that incorporate temporally enriched features into multiple layers of the LLM decoder in a bottom-up manner. Extensive experiments on the LRS2 and LRS3 benchmarks demonstrate that our approach achieves high efficiency and firm performance, outperforming existing supervised, self-supervised, and LLM-based methods by 6.1% on LRS2 and 7.8% on LRS3.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有基于多模态大语言模型（MLLM）的音视频语音识别（AVSR）系统存在根本性的“时序差距”（Temporal Gap）。
  - 视觉编码器缺乏对唇部运动序列演化的细粒度时序建模，仅将动态唇语序列视为静态前缀信息。
  - 在 LLM 解码器的深层自回归解码过程中，输入层的细粒度时序语义会逐步退化或被语言模型的强先验知识稀释，导致仅靠视觉输入的语音识别（VSR）性能显著低于传统自监督或监督模型。
- **整体含义**：本文旨在弥合上述时序差距，提升多模态 LLM 在 AVSR 尤其是 VSR 任务上的鲁棒性与准确率，使其在利用强大语言推理能力的同时不丢失视觉动态信息。

### 2. 论文提出的方法论：核心思想、关键技术细节
核心思想是在**编码**与**解码**两个阶段深度堆叠时序 Token，增强模型对动态视觉信息的捕捉与维持。

- **时序感知注意力 (Temporal-Aware Attention, TAA) 模块**
  - 作用于融合后的音视频序列，通过时序注意力捕捉帧间依赖。
  - 采用**时序旋转位置嵌入 (Temporal Rotary Positional Embeddings, T‑RoPE)**：为同一时间戳的视频帧和音频段分配统一的位置 ID，并通过缩放保证跨模态的时间对齐；在查询和键向量中乘入欧拉公式项作为相对位置编码，以建模帧的先后顺序。

- **层次化时序 Token 堆叠 (Hierarchical Temporal Token Stacking, HSTT)**
  - 将 LLM 解码器分为**底层块 (D_bottom)** 和**上层块 (D_up)**。
  - 从 TAA 输出的时序增强表示通过多个投影层映射到 LLM 嵌入空间，并以残差连接的方式**逐层注入底层解码器的多个中间层**，而非仅从输入层注入，从而对抗深层时序语义的退化。
  - 上层解码器不参与时序堆叠，专注于高层语义推理和文本生成。

- **Q‑Former 压缩**
  - 使用带有可学习查询 Token 的因果 Q‑Former 对融合后的音视频序列进行压缩，减少计算开销，保留时序结构。

### 3. 实验设计：数据集、基准与对比方法
- **数据集**
  - LRS2（224 小时 BBC 节目）和 LRS3（439 小时 TED/TEDx 演讲）作为主测试集。
  - VoxCeleb2 英文子集（约 1326 小时）用作大规模伪标签扩充训练。
- **评估指标**
  - 词错误率（WER），分别在 ASR、VSR、AVSR 三种模态下评估。
- **对比方法**
  - **监督学习**：Auto‑AVSR、Hyb‑Conformer、MIR‑GAN、Fast Conformer 等。
  - **自监督学习**：AV‑data2vec、AV‑HuBERT、RAVEn、BRAVEn、u‑HuBERT 等。
  - **LLM 基方法**：Llama‑AVSR、MMS‑Llama、MoME 等，区分是否使用 token 压缩。
- **附加实验**
  - 消融实验：逐步添加 TAA、T‑RoPE、HSTT，观察 WER 变化。
  - 注意力块数量对性能/计算量的影响。
  - 不同 Token 压缩率及噪声鲁棒性（SNR 从 ‑7.5 到 12.5 dB）。
  - 错误分析与时序注意力可视化。

### 4. 资源与算力
- 论文提及采用 **RTX 4090 GPU** 训练，使用梯度累积。
- **未明确说明** GPU 数量、总训练时长、batch size 等详细资源消耗。

### 5. 实验数量与充分性
- 实验覆盖 **2 个主要基准数据集**（LRS2、LRS3）和 1 个扩充数据集。
- 对比方法涵盖三大类范式，总数超过 15 种主流模型，比较维度丰富。
- 进行了 **4 组以上消融/控制变量实验**（组件有效性、注意力块数、压缩率、噪声鲁棒性），实验设计较充分，能够支撑核心主张。
- 提供了错误案例定性分析与注意力可视化，增强了可解释性。
- 总体而言，实验**充分且对比公平**，在同等数据量和参数规模下体现了方法的优势。

### 6. 论文的主要结论与发现
- 本文提出的时序感知框架能有效弥合多模态 LLM 在 AVSR 中的时序差距。
- 在 LRS2 和 LRS3 上，方法在 VSR、ASR 和 AVSR 三项任务均超越现有监督、自监督和 LLM 基方法，尤其在 VSR 上相对 MMS‑Llama 分别提升 6.1% 和 7.8% WER。
- 层叠时序 Token 和 T‑RoPE 的设计参数高效，只需少量额外参量即可获得显著增益，并能在噪声环境下保持鲁棒。
- 注意力可视化证实模型能局部且一致地跟踪唇部动态。

### 7. 优点：方法或实验设计上的亮点
- **双阶段时序强化**：首次在编码器和解码器深层同时注入时序信息，系统地解决时序退化问题。
- **参数与计算效率**：TAA 模块轻量，Q‑Former 实现 6 倍压缩，在不牺牲精度的前提下降低 LLM 推理开销。
- **设计巧妙的时序位置编码**：T‑RoPE 统一多模态时间戳，增强跨模态对齐。
- **实验全面**：涵盖三类基线、多种任务、消融、噪声测试及可视化，分析深入。

### 8. 不足与局限（包括实验覆盖、偏差风险、应用限制等）
- **语言单一**：仅在英文数据集上验证，未测试跨语言或不同音素‑视素映射的语言。
- **离线解码限制**：模型针对句子级离线识别设计，未适配实时流式场景，不满足低延迟需求。
- **资源细节缺失**：未报告训练总时长和 GPU 规模，难以完全复现资源需求。
- **视觉模态局限**：错误分析显示模型在视觉同音异形词（homophenes）和短句子/句末仍存在较高错误率，可能受限于视觉上下文不足。
- **训练数据依赖**：依赖伪标签扩充数据，噪音标签可能影响性能上限。

（完）
