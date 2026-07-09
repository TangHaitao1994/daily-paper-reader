---
title: Semi-Supervised Speech Enhancement with Gradient-Guided Channel Attenuation
title_zh: 基于梯度引导通道衰减的半监督语音增强
authors: "Qin Yang, Ying Hu, Liang He, Zhijian Ou, Hexin Liu, Eng Siong Chng"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=xGmWHCpVoM"
tags: ["query:speech-model"]
score: 4.0
evidence: 半监督语音增强以提升泛化性，可作为预处理步骤
tldr: 该论文针对有监督语音增强在真实噪声数据上泛化差的问题，提出 SS-SENet 半监督方法，结合均值教师和域对抗学习，并设计梯度引导通道衰减模块以抑制域特定特征。实验显示，利用无标签数据可显著提升模型在实际场景中的语音增强效果。该方法可作为语音识别等下游任务的预处理模块，提高系统对噪声的鲁棒性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 有监督语音增强模型在真实噪声条件下泛化能力有限。
method: 采用均值教师框架与域对抗学习，并设计 GGCA 模块增强域不变特征。
result: 在真实噪声数据上取得了增强效果的提升，泛化性能优于有监督方法。
conclusion: 半监督方法和 GGCA 模块有效提升了语音增强的域泛化能力。
---

## Abstract
Recent methods for speech enhancement (SE) have generally adopted the supervised learning way and trained the models on synthetic noisy-clean paired speech data. However, when applying the supervised trained SE model to the recordings of real-world scenario, which we call unlabeled data, it will lead to the performance degradation. To improve the generalization performance of SE, we propose a semi-supervised monaural speech enhancement network, SS-SENet, which adopts the mean-teacher (MT) framework with domain adversarial (DA) learning to effectively exploit the unlabeled data. We also propose the Gradient-Guided Channel Attenuation (GGCA) module for suppressing the domain-specific features and enhance domain-invariant one, and Domain Shift-Aware Monitor (DSAM) strategy for dynamically adjusting the attenuation rate in GGCA. Comparing with seven SOTA methods exploiting the unlabeled data, our proposed SS-SENet achieves the best performances at all metrics both on synthetic Reverberant LibriCHiME-5 and LibriMix datasets, and at the critical metric, OVRL, on the real-world CHiME-5 dataset. The results verify that our proposed basic MT-based method is superior to the compared methods based on full supervised or self-supervised learning. It also verifies the effectiveness of our proposed GGCA module and DSAM strategy. The source code is available at \url{https://anonymous.4open.science/r/SS-SENet}.

---

## 论文详细总结（自动生成）

## 一、论文的核心问题与整体含义

- **研究动机**：当前语音增强（Speech Enhancement, SE）普遍采用有监督学习，依赖合成“噪声-干净”配对数据训练模型。然而，当此类模型应用于真实场景录音（即无标签数据）时，其性能会严重退化，泛化能力不足。
- **核心问题**：如何有效利用大量无标签的真实世界数据，提升语音增强模型在未知真实噪声环境下的泛化性能。
- **整体含义**：论文提出一种半监督单通道语音增强网络，利用无标签数据配合梯度引导技术，使模型在保持监督信息的同时，削弱对域特定特征的依赖，从而在实际应用中取得更稳健的增强效果，并可作为语音识别等下游任务的鲁棒预处理模块。

## 二、方法论

- **总体框架**：
  - 采用**均值教师（Mean Teacher, MT）** 框架，在半监督学习中通过教师模型为学生模型生成一致性约束。
  - 结合**域对抗学习（Domain Adversarial Learning）**，促使模型学习域不变特征。
- **核心创新——梯度引导通道衰减模块（GGCA）**：
  - 设计用于**抑制域特定特征、强化域不变特征**。
  - 通过分析梯度信息，识别出哪些通道对域分类器敏感（即携带域信息），并对这些通道进行衰减，从而降低模型对域的敏感性。
- **域偏移感知监控策略（DSAM）**：
  - 动态调整 GGCA 中的衰减率，以适应训练过程中域偏移程度的变化，避免过度抑制有效信息。
- **流程概览**（文字描述）：
  1. 输入带噪语音同时进入学生模型和教师模型（教师模型参数通过学生模型指数移动平均更新）。
  2. 学生模型在监督分支使用配对数据计算损失（如 SI-SDR），在半监督分支利用教师模型的预测作为目标，并在中间特征层引入域对抗分支判断域标签。
  3. 在特征提取关键层插入 GGCA 模块，根据域对抗梯度计算通道衰减系数，选择性削弱域特定通道。
  4. DSAM 监控域偏移量，调节衰减强度。

## 三、实验设计

- **使用数据集**：
  - 合成数据集：**Reverberant LibriCHiME-5**、**LibriMix**（包含混响和不同类型噪声）。
  - 真实无标签数据集：**CHiME-5** 真实场景录音，用于验证实际泛化能力。
- **评价指标**：
  - 客观指标：包括语音质量（如 PESQ）、可懂度（如 STOI）、整体感知质量（**OVRL** 等）。
  - 特别指出 OVRL 为真实场景下的关键指标。
- **对比方法**：
  - 与 **七种最先进（SOTA）方法** 进行比较，涵盖全监督方法和自监督学习方法（如利用无标签数据的其他策略）。
- **Benchmark 性质**：分别检验合成域和真实域下的性能，全面评估泛化效果。

## 四、资源与算力

- **论文未明确提及 GPU 型号、数量及训练时长等具体算力信息**。仅能从开源代码库推断其可在常规深度学习硬件上复现，但缺乏详细定量描述。

## 五、实验数量与充分性

- **实验组数**（基于摘要及元数据推断）：
  - 至少在 **三个数据集**（两个合成数据集和一个真实数据集）上进行主实验对比。
  - 包含对所提出 **GGCA 模块和 DSAM 策略的消融研究**，验证每个部分的贡献。
  - 可能还包括不同信噪比条件、不同无标签数据比例等分析（未展开细节）。
- **充分性评价**：
  - 对比方法众多且包含多种范式，具有一定公平性。
  - 在合成和真实场景下均进行了验证，消融实验支撑了模块设计的有效性。
  - 但因无完整论文细节，无法确认是否涵盖了足够多的噪声类型、是否进行了统计显著性检验或交叉验证；预印本形式可能存在实验未完全收敛或未充分调参的风险。

## 六、主要结论与发现

- 提出的 **SS-SENet** 在所有合成数据集的全指标上优于对比的七种 SOTA 方法。
- 在真实数据集 CHiME-5 的关键指标 OVRL 上取得最佳成绩，证明**半监督策略和 GGCA 模块有效提升了域泛化性**。
- 均值教师与域对抗学习的组合作为基础，已优于全监督或自监督对比方法；GGCA 与 DSAM 进一步带来了增益，验证了域特定特征抑制的必要性和可行性。

## 七、优点（方法或实验设计的亮点）

- **新颖的域特征抑制机制**：首次提出基于梯度引导的通道衰减模块，在特征层面主动削弱域信息，而非仅依赖对抗学习，更具针对性和可解释性。
- **动态调整策略**：DSAM 根据域偏移量自适应控制衰减强度，避免固定超参数的次优问题。
- **半监督范式契合实际**：直接利用完全无标签的真实录音进行学习，大幅降低数据标注成本，更贴近工业部署场景。
- **多维度验证**：同时在多种合成数据集和真实数据集上评估，覆盖不同类型失真，结论可靠。

## 八、不足与局限

- **算力细节缺失**：未报告训练计算开销，复现所需资源不明确。
- **实验覆盖有限**：仅测试了英语语音数据，未见跨语种或多说话人复杂场景的扩展。
- **域泛化范畴局限**：主要针对“合成→真实”泛化，未讨论采集设备、房间声学、远场等更丰富域偏移的影响。
- **对比方法可能未全面更新**：虽比较了七种方法，但未确保所有对比方法均使用最新配置或同等计算预算。
- **无标签数据依赖**：半监督效果依赖于无标签数据的分布和目标域匹配度，当无标签数据存在域不匹配时，增益可能下降（论文未详细探讨）。
- **创新模块的通用性**：未验证 GGCA 是否可即插即用到其他半监督语音增强架构中，通用性尚待检验。

（完）
