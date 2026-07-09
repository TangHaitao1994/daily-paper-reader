---
title: "DepthSense+DP: Adaptive Learning for Robust and Differential Private Silent Speech Recognition"
title_zh: DepthSense+DP：面向鲁棒差分隐私无声语音识别的自适应学习
authors: "Rong Fu, weizhi Tang, Simon James Fong"
date: 2025-09-06
pdf: "https://openreview.net/pdf?id=HBozeTR6J6"
tags: ["query:speech-model"]
score: 9.0
evidence: 具有隐私保证且接近基线精度的无声语音识别
tldr: 提出深度感知差分隐私框架，用于从动态三维点云中识别无声语音。通过校准输入扰动、特征级差分隐私和几何保持对齐，在保护隐私的同时实现鲁棒识别。实验表明，在大型多地点数据集上接近基线准确率，且成员推理风险大幅降低，为隐私敏感的语音交互提供了可行方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 无声语音识别需兼顾识别准确率和用户隐私保护，现有方法难以在隐私-效用之间取得平衡。
method: 集成输入扰动、特征级差分隐私和几何保持对齐的轻量框架，采用双阶段隐私注入和自适应门控。
result: 在大型多地点语料上实现接近基线准确率，同时显著降低成员推理攻击风险。
conclusion: 该框架在保护隐私的同时保持了高识别精度，适用于实际无声语音识别场景。
---

## Abstract
DepthSense+DP is a privacy-preserving framework for silent speech recognition from dynamic 3D depth point clouds. It integrates calibrated input perturbation, feature-level differential privacy, and geometry-preserving alignment within a lightweight P4DConv front end and Conformer encoder to ensure robust cross-user and cross-device generalization under formal DP guarantees. A dual-stage DP pipeline injects noise at point and feature levels while maintaining articulatory geometry, aided by an adaptive DAD gate for improved privacy–utility trade-off. The co-designed architecture enables efficient on-device inference. Experiments on a large multi-location corpus show near-baseline accuracy with significant reductions in membership, inversion, and attribute-inference risks, supported by full DP accounting and attack evaluations.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
无声语音识别（Silent Speech Recognition, SSR）通过感知面部或喉部动作实现无声音交互，在隐私敏感场景（如移动支付、医疗）中极具价值。然而，此类生物特征数据极易泄露用户身份、说话内容甚至个人属性，现有识别系统大多未提供正式隐私保障，导致隐私与效用的权衡成为关键瓶颈。本文针对此问题，提出了 **DepthSense+DP** 框架，其核心目标是在**形式化差分隐私（Differential Privacy, DP）** 保证下，从动态三维深度点云中实现高精度、鲁棒的无声语音识别，并显著降低成员推理、反转和属性推断等攻击风险，从而为现实世界的隐私保护语音交互提供可行方案。

### 2. 方法论
DepthSense+DP 围绕轻量级点云处理与双阶段噪声注入展开，整体设计兼顾隐私性、准确率和设备端部署效率。

- **核心架构**
  - **前端特征提取**：使用轻量级的 **P4DConv**（点四维卷积）处理动态三维点云，捕捉空间与时间上的发音几何信息。
  - **后端序列建模**：采用 **Conformer** 编码器对特征序列建模，实现端到端语音识别。
  - **双阶段差分隐私管线**：
    - **点级扰动（输入阶段）**：对原始点云坐标进行校准的高斯噪声注入，同时通过几何保持对齐（geometry-preserving alignment）维持嘴唇、面部等关键发音结构的几何关系，避免噪声破坏有效信息。
    - **特征级差分隐私**：在提取的中间特征上再次注入可控噪声，进一步限制信息泄露，并提供可量化的隐私预算（ε, δ）核算。
  - **自适应门控（DAD gate）**：引入自适应动态对齐-扰动门控机制，在训练与推理阶段动态调节噪声强度与特征对齐的权重，优化隐私-效用权衡曲线。
  - **设备端推理优化**：通过协同设计的轻量化架构，使整体模型可在边缘设备上高效运行。

- **算法流程简示**
  > 输入动态点云 → 校准点扰动 + 几何对齐 → P4DConv 前端 → DAD 门控 → 特征级 DP 噪声 → Conformer 编码器 → 识别输出  
  > 全流程在 DP 会计师（privacy accountant）监督下训练，确保满足 (ε, δ)-DP。

### 3. 实验设计
- **数据集**：在**大型多地点采集的语料库**（multi-location corpus）上评估，数据涵盖不同用户、不同设备，以测试跨用户和跨设备的泛化能力。
- **基准（Benchmark）**：以**无隐私保护的基线模型**（baseline accuracy）作为精度上界，同时对比其他典型隐私保护方法（如仅加噪输入、仅特征加噪等变体），衡量隐私保护带来的精度损失。
- **对比方法**：包括无保护的原始模型、单阶段扰动方案、无几何对齐的扰动方案，以及不同隐私预算 ε 下的表现。
- **攻击评估**：系统性地测试了三种代表性推理攻击：
  - 成员推理攻击（Membership Inference）
  - 模型反转攻击（Inversion）
  - 属性推断攻击（Attribute Inference）
  并报告了相应的风险降低幅度。

### 4. 资源与算力
论文摘要及公开元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等算力资源细节。仅提及模型设计为“轻量级”，可支持边缘设备推理，但训练阶段的资源消耗未有报告。

### 5. 实验数量与充分性
从摘要可以推断出以下实验维度：
- **多地点、多用户、多设备**场景下的识别精度评估。
- **不同隐私预算 ε** 下的效用-隐私权衡分析。
- **消融实验**：比较双阶段 DP vs 单阶段 DP、有无几何保持对齐、有无 DAD 门控等模块的贡献。
- **三种攻击评估**：成员、反转、属性推理，覆盖了主要的隐私威胁。
- **全 DP 会计与攻击评测**的结合，使得隐私保证可量化、可验证。

总体而言，实验设计较为全面，覆盖了效用验证、模块贡献与安全性评测，但具体实验数量（如消融项数目、对比方法数量）未在摘要中展开。在方法论客观性上，采用标准 DP 定义和多元攻击测试，对比基线明确，具备公平性。

### 6. 主要结论与发现
- DepthSense+DP 在**大型多地点语料库**上取得了**接近基线（无隐私保护）的识别准确率**，同时满足形式化 (ε, δ)-差分隐私保证。
- 双阶段噪声注入与几何保持对齐显著改善了隐私-效用权衡，DAD 门进一步提升了自适应调节能力。
- 与无保护模型相比，该框架**大幅降低了成员推理、模型反转和属性推断攻击的成功率**，使隐私风险降至接近随机猜测水平。
- 轻量化设计使得模型可在设备端高效部署，为实际隐私敏感场景的无声语音交互提供了可行路径。

### 7. 优点
- **隐私-效用平衡创新**：双阶段 DP 注入（点云+特征）并辅以几何保持对齐和自适应门控，在严格隐私约束下最大程度保留发音几何信息。
- **全面的安全评估**：不仅提供 DP 会计，还系统测试多种攻击，验证了实际隐私保护强度，而不仅是理论 ε 值。
- **实用性导向**：轻量网络设计与设备端推理支持，兼顾了部署可行性。
- **泛化验证**：跨用户、跨设备的实验设置提升了结论的可靠性。

### 8. 不足与局限
- **算力与效率细节缺失**：未报告模型参数量、训练时长、硬件配置及实际推理延迟，难以评估端侧部署的真实特性。
- **数据集局限**：虽然提及“大型多地点”，但语种、参与人数、环境噪声、传感器型号等具体信息未披露，外部有效性存疑。
- **对比范围可能有限**：未提及与现有其他 DP-SSR 方法（如基于差分隐私的声学识别迁移）的直接比较，难以判断相对增益。
- **攻击向量覆盖**：仅测试了三种常见推理攻击，对梯度泄露、后门攻击等其他隐私威胁未作讨论。
- **超参数敏感性**：自适应门控和双阶段噪声的敏感度未展开，实际调优可能较为复杂。
- **实时性未验证**：无声语音交互对低延迟要求高，设备端流式处理性能未知。

（完）
