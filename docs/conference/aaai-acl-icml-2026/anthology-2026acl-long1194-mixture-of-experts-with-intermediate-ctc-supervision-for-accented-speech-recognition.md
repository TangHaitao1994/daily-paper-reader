---
title: Mixture-of-Experts with Intermediate CTC Supervision for Accented Speech Recognition
title_zh: 带中间CTC监督的混合专家模型用于口音语音识别
authors: "Wonjun Lee, Hyounghun Kim, Gary Geunbae Lee"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1194.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 混合专家模型结合中间CTC监督，提升口音语音识别精度
tldr: 针对口音语音识别性能下降的问题，提出带有中间CTC监督的混合专家模型MoE-CTC，通过训练时口音感知路由促进专家专业化，推理时无需口音标签，在多个口音数据集上显著提升识别准确率。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1194/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1429, \"height\": 924, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1194/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 682, \"height\": 718, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1660, \"height\": 658, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 812, \"height\": 333, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 816, \"height\": 355, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 803, \"height\": 293, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 811, \"height\": 315, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 799, \"height\": 315, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 802, \"height\": 665, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 803, \"height\": 327, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 799, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 810, \"height\": 273, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 812, \"height\": 236, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1194/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 809, \"height\": 298, \"label\": \"Table\"}]"
motivation: 现有ASR模型对高资源英语变体过拟合，导致对重口音或未见口音识别性能严重下降。
method: 提出MoE-CTC架构，利用中间CTC监督和逐步从口音感知路由过渡到无标签路由，促使专家学习特定口音模式。
result: 在多个口音英语数据集上，MoE-CTC相比基线模型显著降低了词错误率。
conclusion: 该方法有效提升了口音语音识别的鲁棒性和泛化能力，为低资源口音提供解决方案。
---

## Abstract
Accented speech remains a persistent challenge for automatic speech recognition (ASR), as most models are trained on data dominated by a few high-resource English varieties, leading to substantial performance degradation for other accents. Accent-agnostic approaches improve robustness yet struggle with heavily accented or unseen varieties, while accent-specific methods rely on limited and often noisy labels. We introduce MoE-CTC, a Mixture-of-Experts architecture with intermediate CTC supervision that jointly promotes expert specialization and generalization. During training, accent-aware routing encourages experts to capture accent-specific patterns, which gradually transitions to label-free routing for inference. Each expert is equipped with its own CTC head to align routing with transcription quality, and a routing-augmented loss further stabilizes optimization. Experiments on the MCV-Accent benchmark demonstrate consistent gains across both seen and unseen accents in low- and high-resource conditions, achieving up to 29.3% relative WER reduction over strong FastConformer baselines.

---

## 论文详细总结（自动生成）

### 1. 论文核心问题与研究动机

*   **核心问题**：口音仍然是自动语音识别（ASR）系统的一个主要挑战。大多数模型在由少数高资源英语变体主导的数据上训练，导致对其他口音的识别性能显著下降。
*   **背景与挑战**：
    *   **口音无关（Accent-agnostic）方法**：虽能提升鲁棒性，但在重度口音或未见过的口音上表现不佳。
    *   **口音特定（Accent-specific）方法**：依赖于有限且通常有噪声的口音标签，推理时也需要标签，可扩展性和泛化能力受限。
*   **研究动机**：需要一种既能学习口音特定模式以实现专业化，又能在没有口音标签的情况下进行推理以实现泛化的方法。

### 2. 方法论：MoE-CTC架构

MoE-CTC（Mixture-of-Experts with Intermediate CTC Supervision）的核心思想是将混合专家模型与中间CTC监督相结合，通过两阶段训练实现专家专业化和推理泛化。

*   **整体架构**：在FastConformer编码器的层间插入MoE模块，每个模块包含多个专家（前馈网络），并通过一个序列级路由器将整个话语分配给少数专家。
*   **关键技术细节**：
    *   **序列级路由**：对输入帧进行平均池化，路由网络输出选择Top-K专家的概率。
    *   **口音感知路由（Accent-Aware Routing）**：训练时，通过一个偏置项（\(\tilde{L}_{i,j} = L_{i,j} + \alpha \cdot \mathbf{1}[j = a_i]\)）将每个口音与一个指定专家对齐，并辅以口音分类辅助损失（\(L_{\text{accent}}\)），引导专家专业化。
    *   **专家级CTC监督（MoE-CTC）**：每个专家配备独立的CTC头（CTC-HEAD），产生辅助CTC损失（\(L_{\text{CTC}}^{(\ell,j)}\)）。将CTC头输出的logits投影后，根据路由权重组合并残差回输入。这使路由与转录质量对齐。
    *   **路由增强损失（\(L_{\text{local}}\)）**：局部目标将专家路由概率与其CTC损失加权求和，鼓励路由器高权重分配给转录损失更低的专家。
    *   **两阶段训练**：
        1.  **第一阶段（专业化）**：使用口音感知路由（偏置项+口音分类损失）训练。
        2.  **第二阶段（泛化）**：去除口音特定信号，仅使用全局CTC损失和专家级CTC监督（\(L_{\text{local}}\)）进行微调，让路由器自主选择专家。
    *   **总损失函数**：\(L = L_{\text{CTC}} + \beta L_{\text{local}} + \gamma L_{\text{accent}}\)。

### 3. 实验设计

*   **数据集**：MCV-Accent基准（源自CommonVoice英语部分），包含100小时和600小时两个训练集，以及开发集和测试集。测试集包含5种**已见**口音（澳大利亚、加拿大、英格兰、苏格兰、美国）和9种**未见**口音，用于评估跨口音泛化。所有模型先在LibriSpeech-960h上预训练，再在MCV-Accent上微调。
*   **基线方法**：
    *   **FastConformer**（Small、Medium、Large多种尺寸）。
    *   **Inter-CTC**：带有中间CTC损失的FastConformer。
    *   **标准MoE**：无口音监督的序列级MoE。
    *   **Accent-MoE**：仅口音感知路由，无专家CTC头。
    *   **先前基准**：Conformer、MTL、DAT、Prabhu et al. (2023; 2024)，以及HuBERT等。
*   **评估指标**：词错误率（WER），包括已见、未见口音的平均值及加权平均值（Seen ALL / Unseen ALL）。

### 4. 资源与算力

*   论文明确指出：所有实验使用NeMo框架，在**8块NVIDIA A100 GPU（每块80GB）**上训练。
*   全局批量大小为1024，使用混合精度（FP16）训练。
*   预训练与微调均进行最多500个epoch，第二阶段微调额外进行20个epoch。未明确给出总训练时长。

### 5. 实验数量与充分性

*   **实验组数**：进行了多维度的大量实验，覆盖：
    *   **主实验**：在100小时数据上对比4种模型尺寸（Small, Medium, 46M, 76M, Large）下的FastConformer、Inter-CTC、MoE、Accent-MoE、MoE-CTC，共计约20余组结果。
    *   **基准比较**：在100h和600h数据上与先前发表的多个模型结果（加权WER）对比。
    *   **消融分析**：深入分析了训练阶段、“泛化训练”的影响、路由器性能、Oracle设置。
    *   **架构分析**：研究了专家数量（5 vs 8）、CTC头共享策略、MoE模块插入位置等变体。
*   **充分性与客观性**：实验设计充分且客观。模型参数被控制以确保公平比较（如46M和76M变体专门匹配参数规模），提供了详细配置表。评估同时报告已见和未见口音，区分简单平均和按样本数加权平均。结论由消融实验和可视化有力支撑。

### 6. 主要结论与发现

*   **性能提升**：MoE-CTC在所有设置下均取得最低WER。在Large尺寸下，MoE-CTC相对FastConformer基线在已见口音上实现29.3%的相对WER下降，在未见口音上实现27.8%的相对下降。
*   **泛化能力**：随模型容量增大，对未见口音的增益更显著，表明该方法泛化能力强。
*   **训练阶段必要性**：口音感知训练后接口音无关微调的两阶段策略，对MoE-CTC的提升尤为关键，使其在保留口音敏感性的同时提升泛化性。
*   **路由行为**：即使经过第二阶段的无监督微调，路由器仍能学到有意义的口音组织（如加拿大语音常路由至美国专家），这种自适应重分布有利于泛化。
*   **Oracle边界**：Oracle实验表明，在推理时使用真实口音标签可达到最佳性能（5.2% WER），但两阶段方法在没有标签的情况下（5.5%）已非常接近上界。

### 7. 优点

*   **创新性强**：巧妙地将MoE、中间CTC监督和分阶段训练策略结合，解决了口音ASR中专业化与泛化的矛盾。专家级CTC头直接对齐路由与识别质量是关键创新。
*   **推理高效**：最终模型在推理时无需任何口音标签，通过自学习路由达到强泛化，实用价值高。
*   **实验严谨全面**：对比了大量基线，覆盖多种模型尺寸和训练数据量，消融实验深入（训练阶段、专家数量、头共享、模块位置等），分析详实（包括路由混淆矩阵）。
*   **表现优异**：在已见和未见口音上均大幅超越强基线，且优势随模型规模扩展。
*   **成本明确**：给出了具体计算资源（8×A100 80GB），便于复现参考。

### 8. 不足与局限

*   **泛化边界假设**：序列级路由假设话语内有离散的口音边界，可能不适用于混合口音或语码转换的语音。
*   **训练依赖标签**：第一阶段训练需要口音标签，限制了其在完全无监督场景下的应用。
*   **计算成本**：尽管稀疏激活，额外的MoE模块和CTC头增加了训练成本和推理延迟（文中提及是局限性之一）。
*   **评估范围**：实验仅限于MCV-Accent上的英语口音，尚未在更广泛的多语种或多任务上进行验证，专家的可解释性分析也未深入展开。

（完）
