---
title: "Self-Guidance: Training VQ-VAE Decoders to be Robust to Quantization Artifacts for High-Fidelity Neural Speech Codec"
title_zh: 自引导：训练VQ-VAE解码器对量化伪影鲁棒以实现高保真神经语音编解码
authors: "Xiang Li, Yixuan Zhou, Jingran Xie, Zhiyong Wu, Yue Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=lCaU7NlZ1I"
tags: ["query:speech-model"]
score: 8.0
evidence: 提升VQ-VAE语音编解码器对量化误差的鲁棒性，改善语音模型预处理分词质量。
tldr: 针对VQ-VAE神经语音编解码器因量化误差导致重建保真度受限的问题，提出自引导训练机制，通过让解码器从连续嵌入中学习来增强对量化伪影的鲁棒性。在不增加码本大小或层次的前提下显著提高重建质量，为语音大语言模型提供更高保真的离散token，平衡压缩率和建模难度。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VQ-VAE编解码器的量化误差限制重建保真度，增大码本又会增加下游建模负担。
method: 利用解码器对连续嵌入的优质输出作为自引导信号，训练解码器抵抗量化噪声。
result: 在相同复杂度下显著提升重建音质，优于传统VQ-VAE和层次化方案。
conclusion: 自引导训练为高保真、低复杂度语音编解码器提供了有效方法，利于语音大模型发展。
---

## Abstract
Neural speech codecs, predominantly based on Vector-Quantized Variational Autoencoders (VQ-VAEs), serve as fundamental audio tokenizers for speech large language models (SLLMs). However, their reconstruction fidelity is limited by quantization errors introduced during latent space discretization. Existing solutions typically increase model complexity through larger codebooks or hierarchical quantization, which subsequently intensify the modeling challenge for downstream SLLMs. Inspired by the key insight that the codec decoder produces superior output from continuous pre-quantize embeddings, we propose a novel self-guided training mechanism that addresses this problem by enhancing decoder robustness rather than modifying the quantization process. Our method introduces an additional training objective that aligns the decoder's intermediate features when processing both quantized tokens and continuous pre-quantized embeddings through a feature-mapping loss. Extensive experiments on XCodec2 demonstrate that self-guidance consistently improves reconstruction quality across various codebook sizes and quantization techniques (FSQ, SimVQ, multi-codebook VQ), achieving state-of-the-art performance for low-bitrate speech codecs. The method requires minimal additional training cost and no inference-time modifications, offering an efficient solution for high-fidelity neural audio coding. Remarkably, our approach enables a 4× reduction in codebook size while maintaining comparable fidelity. Downstream text-to-speech experiments confirm that this reduction significantly improves LLM-based synthesis performance by simplifying the token modeling space.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

*   **核心问题**：神经语音编解码器（尤其是 VQ-VAE）作为语音大语言模型（SLLM）的基础音频分词器，其重建保真度受限于潜在空间离散化过程中引入的**量化误差**。
*   **现有方案的局限**：增大码本尺寸或引入层次化量化虽然能缓解误差，但会增加模型复杂度，进而加大下游 SLLM 的建模难度，导致压缩率与下游任务性能之间的矛盾。
*   **本文思路**：不改变量化过程本身，而是从**增强解码器对量化伪影的鲁棒性**入手，利用解码器在输入连续嵌入（量化前）时能产生优质输出的特性，设计自引导训练机制，使解码器在输入量化后的离散 token 时也能逼近高质量输出，从而在不增加码本规模的前提下提升重建音质。

### 2. 论文提出的方法论

*   **核心思想**：**自引导（Self-Guidance）**。解码器在处理量化前的连续嵌入时，其输出质量明显优于处理量化后的 token。作者利用这一优质输出作为“教师信号”，引导解码器在使用量化 token 时也能复现类似的高保真输出。
*   **关键技术细节**：
    *   引入一个额外的训练目标：将解码器处理**量化 token** 时的中间层特征，与处理**连续预量化嵌入**时的中间层特征进行对齐。
    *   通过一个**特征映射损失（feature-mapping loss）** 来最小化两种输入路径下解码器中间特征表达之间的差异。
    *   该方法仅增加训练阶段的损失项，不改变模型推理结构，无推理时延开销。
*   **工作流程**：
    1.  编码器输出连续嵌入 $z_e$。
    2.  $z_e$ 一路保持连续直接送入解码器，获得中间特征与高质量重建输出（作为自引导参考）。
    3.  $z_e$ 另一路经过量化器得到离散 token 并送入同一解码器，获得中间特征与实际重建输出。
    4.  计算原始重建损失和新增的特征映射损失，联合优化解码器参数，迫使解码器在处理量化噪声输入时保留与连续嵌入相似的特征表示。
*   **公式/算法示意（文字描述）**：
    *   总损失 $L = L_{\text{recon}} + \lambda \cdot L_{\text{align}}$。
    *   $L_{\text{align}}$ 是解码器中若干选定层上，量化路径与连续路径的特征图之间的均方误差或余弦距离等度量。

### 3. 实验设计

*   **基础框架**：实验搭建在 **XCodec2** 这一神经语音编解码器之上。
*   **测试场景与 benchmark**：
    *   验证多种码本尺寸和多种量化技术下的重建性能。
    *   对比的量化方法包括：FSQ（有限标量量化）、SimVQ（简单向量量化）、多码本 VQ 等。
    *   评估指标围绕**低比特率语音编解码的重建质量**，最终取得最先进（state-of-the-art）性能。
    *   还进行了下游**文本到语音（TTS）合成**实验，检验小码本下 token 建模对 LLM 基合成系统的实际影响。
*   **对比方法**：传统 VQ-VAE、层次化量化方案等；同时进行了不同量化技术下的兼容性对比。

### 4. 资源与算力

*   **说明**：原问题所给论文元数据中**未提及**具体使用的 GPU 型号、数量、训练时长等算力信息。仅提到方法“所需训练开销极小”，但无定量数字。

### 5. 实验数量与充分性

*   **实验数量**：根据元数据描述，实验覆盖：
    1.  不同码本尺寸的探究（包括实现 **4 倍码本缩减**而保真度相当的极致实验）。
    2.  多种量化技术（至少 FSQ、SimVQ、多码本 VQ）的一致性验证。
    3.  下游 TTS 任务的应用验证。
    4.  应包含消融实验以验证自引导项的作用（虽未在元数据中逐项列出，但作为被顶会投稿的论文属于标配）。
*   **充分性与公平性**：
    *   **充分性**：涵盖了核心的量化鲁棒性验证、多种量化 backbone 兼容性、以及与下游语音合成的衔接，实验设计较全面。
    *   **客观公平**：在同一基础编解码器 XCodec2 上对比不同量化设置和训练策略，控制变量合理；下游任务用 LLM 合成表现来佐证码本缩减的收益，增加实践说服力。

### 6. 论文的主要结论与发现

*   **自引导训练有效提升重建质量**：在各种码本尺寸和量化技术下，均能一致性地改善重建音质，达到低比特率语音编解码的最优水平。
*   **码本尺寸可大幅缩减**：无需增加码本容量，仅通过增强解码器鲁棒性，即可实现 **4 倍码本尺寸缩减** 而保持相当的保真度。
*   **下游语音大模型受益**：更小的码本简化了 token 的建模空间，在文语合成实验上提升了基于 LLM 的合成性能。
*   **高效实用**：方法训练开销小，推理时无额外计算，是一种高性价比的高保真语音编解码方案。

### 7. 优点

*   **思路新颖**：从“增强解码器抗噪”的视角切入，区别于传统的修改量化器或增大码本的做法，视角独特且有解释性。
*   **兼容性强**：对多种量化方法（FSQ、SimVQ、多码本 VQ）均有效，不依赖特定量化技术架构。
*   **实用导向**：无推理阶段额外开销，且能直接降低下游 LLM 的建模负担，对部署友好。
*   **性价比高**：训练成本增加极少，但能带来明显的性能提升和码本缩减，效果与代价比突出。

### 8. 不足与局限

*   **实验覆盖可更广**：元数据未提及在多语种、多说话人、噪声环境下的鲁棒性验证；是否仅使用单一数据集未说明。
*   **偏倚风险**：所有实验均基于 XCodec2 框架，虽然对多种量化方法兼容，但未展示在其他 VQ-VAE 语音编解码架构（如 SoundStream、EnCodec）上的泛化能力，可能存在架构绑定效应。
*   **应用限制**：方法主要针对语音编解码的保真度问题，对极低延迟或极低复杂度的移动端场景是否适用，未给出明确数据。
*   **特征映射损失的敏感性**：从元数据未看到对损失权重λ、映射层选择等超参数的灵敏度分析，这些可能影响方法的迁移调参成本。
*   **信息缺失**：未提供训练资源需求和详细数据规模，评估完整性受限。

（完）
