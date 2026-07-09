---
title: "Pseudo2Real: Task Arithmetic for Pseudo-Label Correction in Automatic Speech Recognition"
title_zh: Pseudo2Real：基于任务算法的伪标签纠正在自动语音识别中的应用
authors: "Yi-Cheng Lin, Yu-Hsuan Li Liang, Hsuan Su, Tzu-Quan Lin, Shang-Tse Chen, Yun-Nung Chen, Hung-yi Lee"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.59.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 使用参数空间算术进行伪标签纠正，提升域迁移下的ASR
tldr: 针对域迁移下伪标签训练ASR会引入系统性偏差的问题，本文提出Pseudo2Real方法，在源域通过真实标签与伪标签训练的两个模型计算参数差异向量，用来纠正目标域伪标签模型的偏差。实验显示该方法无需目标域真实标签即可有效提升不同口音场景下的识别准确率，为半监督域适应提供了轻量参数级解决方案。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.59/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 803, \"height\": 541, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.59/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 806, \"height\": 406, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.59/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 778, \"height\": 673, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.59/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 768, \"height\": 414, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1663, \"height\": 713, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1352, \"height\": 988, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 660, \"height\": 270, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1164, \"height\": 337, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1638, \"height\": 590, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 774, \"height\": 283, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1485, \"height\": 212, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 765, \"height\": 465, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.59/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1635, \"height\": 1054, \"label\": \"Table\"}]"
motivation: 伪标签在域迁移下引入系统性偏差，过滤方法难以修正。
method: 在源域计算伪标签偏差向量，对目标模型进行参数空间纠正。
result: 无需目标域真值，有效提升跨域ASR性能。
conclusion: 参数空间纠正为半监督域适应ASR提供了一种简单有效的方法。
---

## Abstract
Robust ASR under domain shift is crucial because real-world systems encounter unseen accents and domains with limited labeled data. Although pseudo-labeling offers a practical workaround, it often introduces systematic, accent-specific errors that filtering fails to fix. We ask: How can we correct these recurring biases without target ground truth? We propose a simple parameter-space correction: in a source domain containing both real and pseudo-labeled data, two ASR models are fine-tuned from the same initialization, one on ground-truth labels and the other on pseudo-labels, and their weight difference forms a correction vector that captures pseudo-label biases.When applied to a pseudo-labeled target model, this vector enhances recognition, achieving up to a 35% relative Word Error Rate (WER) reduction on AfriSpeech-200 across ten African accents with the Whisper tiny model.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在自动语音识别（ASR）的无监督域适应中，目标域缺乏真实标注，常使用伪标签。但伪标签会继承教师模型的系统性偏差（例如对特定口音的规律性错误识别），简单的置信度过滤无法根本纠正这些结构化的错误模式。论文关注：**如何在没有目标域真值标注的情况下，自动检测并减轻ASR伪标签中的系统性错误？**
- **整体含义**：论文提出一种基于参数空间校正的方法，利用源域中“真实标签”与“伪标签”训练出的模型之间的参数差异向量，去修正目标域伪标签模型中的偏差。本质是将伪标签“噪声”的方向从参数空间中识别出来，并跨域转移，从而提升目标域ASR的鲁棒性。

### 2. 方法论：核心思想、关键技术细节与算法流程

**核心思想**：在参数空间中学习一个可迁移的“校正向量”，该向量表征从伪标签到真实标签的性能提升方向。

**Pseudo2Real（单校正向量）**：
- 从同一个预训练模型θ<sub>pre</sub>出发，在**源域**微调两个ASR模型：
  - θ<sub>s</sub><sup>real</sup>：使用真实转录对（S<sub>s</sub>, T<sub>s</sub>）训练。
  - θ<sub>s</sub><sup>pseudo</sup>：使用教师模型生成的伪标签对（S<sub>s</sub>, Ť<sub>s</sub>）训练。
- 计算校正向量：**τ = θ<sub>s</sub><sup>real</sup> − θ<sub>s</sub><sup>pseudo</sup>**。
- 在**目标域**，先使用目标域伪标签微调得到θ<sub>t</sub><sup>pseudo</sup>，然后应用校正：
  **θ<sub>t</sub><sup>corrected</sup> = θ<sub>t</sub><sup>pseudo</sup> + λτ**，其中λ为缩放因子（通过源域验证集搜索，如0.1~1.0）。

**Pseudo2Real-SC（子群校正向量）**：
- 考虑到源域内部不同说话人群体的伪标签偏差可能不同（如不同口音），提出：
  - 使用ECAPA-TDNN提取说话人嵌入，用k-means将源域语音划分成C个说话人子群。
  - 对每个子群c，分别计算校正向量τ<sub>c</sub>。
  - 最终校正向量为所有子群向量的平均：τ<sub>avg</sub> = (1/C) Σ τ<sub>c</sub>。
  - 目标模型应用：θ<sub>t</sub><sup>corrected</sup> = θ<sub>t</sub><sup>pseudo</sup> + λ τ<sub>avg</sub>。

**关键细节**：方法基于线性模式连通性（linear mode connectivity）假设，即从同一预训练模型微调得到的模型处于参数空间的共享低损失盆地中，其差值代表有意义的编辑方向。校正向量的计算仅需源域同时拥有真实和伪标签，目标域完全无需真实标签。

### 3. 实验设计：数据集、场景、对比方法

- **数据集**：AfriSpeech-200，200小时非洲英语口音语音数据。选取了具有完整训练/验证/测试划分的10种口音（如Igbo、Swahili、Hausa、Zulu、Twi、Yoruba、Ijaw等），分为两组折叠交叉验证：Fold 1和Fold 2互为源域和目标域。
- **模型与尺度**：Whisper系列（Tiny、Base、Small、Medium、Large-v2），从39M到1.55B参数。
- **对比方法（baselines）**：
  - **无适应**：θ<sub>pre</sub>（预训练模型直接评估）、θ<sub>s</sub><sup>real</sup>（仅源域真实标签微调）。
  - **伪标签基线**：θ<sub>t</sub><sup>pseudo</sup>（目标域伪标签微调）。
  - **标签空间方法**：置信度过滤（Conf.）、错误纠正（EC，用T5模型修正伪标签）。
  - **权重空间方法**：指数移动平均（EMA）。
  - **上界**：目标域真实标签微调（topline）。
- **评价指标**：词错误率（WER），使用贪心解码。

### 4. 资源与算力

- **GPU**：单张NVIDIA V100。
- **总训练时长**：约500 GPU小时。
- **训练细节**：AdamW优化器，学习率3×10⁻⁵，权重衰减0.1，最多40k更新步，批量大小16，混合精度FP16。评估每50步进行一次。任务算术使用全部模型参数。

### 5. 实验数量与充分性

实验覆盖了较为全面的场景，包括：
- **跨口音域适应**：两组交叉验证，10种口音。
- **不同模型尺度**：Tiny、Base、Small、Medium、Large，以及多种教师-学生配对。
- **消融与参数分析**：
  - 缩放因子λ的影响（0.0至0.5的曲线）。
  - Pseudo2Real-SC中聚类数目k的影响（1,2,4,8）。
  - Pseudo2Real-SC与单校正向量的对比。
- **定性分析**：通过案例分析展示了系统性错误的纠正效果。
- **公平性**：所有对比方法均在相同的无监督域适应设置下进行，使用相同的伪标签质量。超参数λ仅在源域验证集上调优，避免了利用目标域信息。实验设计合理，比较充分，但部分对比方法（如EC、EMA）的实现细节可能影响效果，但已尽力保持一致。

### 6. 主要结论与发现

- **Pseudo2Real显著降低WER**：在Whisper Tiny上，相对伪标签微调平均降低35%的WER；Small上从47.2降至45.0，接近上界。
- **有时可超越目标域真值训练的上界**：在某些口音（如Igbo、Twi）上，Pseudo2Real甚至低于topline WER，分析发现主要是由于校正向量引入了额外的抗幻觉正则化，显著减少了插入错误。
- **方法在多种教师-学生大小下有效**，但增益幅度依赖于配对和口音。例如Large→Tiny平均相对提升21.6%，而Small→Tiny为8.4%。
- **缩放因子λ存在最佳区间**：通常在0.2-0.3，过大会导致过校正和性能下降。
- **Pseudo2Real-SC通过说话人聚类平均可进一步提升性能**，特别是在教师容量大、偏差更异质时（如Large→Small得到6.1%相对提升），且随着聚类数k增加到8，WER逐步降低。
- **定性案例**：校正向量有效修复了教师模型产生的词汇混淆和幻觉词汇（如将“survived”误识别为“as a vif”或错误插入“full stop”）。

### 7. 优点：方法或实验设计的亮点

- **无需目标域真值**：完全遵循无监督域适应设定，仅依赖源域的有标注数据。
- **简单高效**：仅需向量加减运算，无需额外模型或复杂迭代训练，可直接应用于现有微调流程。
- **可解释性强**：参数空间中的方向直观对应“伪→真”的改善，并与任务算术领域呼应。
- **扩展性好**：通过子群聚类进一步捕捉组内偏差，且聚类数增加带来连续增益，无性能饱和迹象。
- **实验全面**：覆盖多种口音、多个Whisper尺度、多种教师-学生配对、详尽的消融与对比，评估扎实。
- **意外发现**：有时校正后的模型由于跨域正则化效果能超越目标域完全监督训练，揭示了一种新的迁移学习范式。

### 8. 不足与局限

- **依赖源域监督量**：需要源域同时具备真实标签和伪标签，若源域太小或与目标域差异过大，校正向量可能欠拟合或不匹配。
- **偏差转移假设**：假设源域的伪标签偏差模式在目标域重复出现；若目标域偏差由完全不同的条件引起（如信道、噪声），校正可能无效甚至有害。
- **λ需手动调节**：虽然仅在源域搜索，但仍引入超参数，其最优值可能随域迁移而变化。
- **源域组成未充分研究**：实验仅采用固定的双折分割，对不同源域组合（如更多折叠、不同语言家族混合）的鲁棒性未深入探索。
- **语言与场景局限**：仅验证了英语非洲口音的数据集和阅读/临床语音，未测试多语种、自发对话或远场等场景。
- **聚类计算开销**：Pseudo2Real-SC需要为每个说话人子群训练两个模型，k较大时训练成本增加（论文实验中k=8已表现出计算负担）。
- **教师-学生配对敏感**：Large→Small等容量悬殊时效果不稳定，甚至个别口音退化，适用条件需进一步明确。

（完）
