---
title: Hierarchical Policy Optimization for Simultaneous Translation of Unbounded Speech
title_zh: 面向无界语音同声传译的分层策略优化
authors: "Siqi Ouyang, Shuoyang Ding, Oleksii Hrinchuk, Vitaly Lavrukhin, Brian Yan, Boris Ginsburg, Lei Li"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.80.pdf"
tags: ["query:speech-model"]
score: 7.0
evidence: 策略优化后训练方法用于同声传译模型，提升质量和效率
tldr: 针对同声传译任务中合成训练数据质量不高和计算开销大的问题，提出分层策略优化（HPO）方法对预训练模型进行后训练，在保持效率的同时提升翻译质量。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 799, \"height\": 519, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1641, \"height\": 573, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1653, \"height\": 1292, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1651, \"height\": 431, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1648, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 555, \"height\": 440, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1660, \"height\": 1276, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 811, \"height\": 701, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1634, \"height\": 272, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.80/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 443, \"height\": 97, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.80/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1665, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.80/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1645, \"height\": 301, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.80/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1653, \"height\": 1138, \"label\": \"Table\"}]"
motivation: 现有同声传译方法依赖合成的多轮对话数据，质量无法保证，且LLM计算开销大。
method: 提出分层策略优化（HPO），对基于不完美监督微调数据的模型进行后训练，包含高层和低层策略优化。
result: 实验显示，HPO显著提高了同声传译质量，同时借助KV缓存复用降低了计算成本。
conclusion: HPO为低资源同声传译任务提供了一种有效且高效的后训练方案。
---

## Abstract
Simultaneous speech translation (SST) generates translations while receiving partial speech input. Recent advances show that large language models (LLMs) can substantially improve SST quality, but at the cost of high computational overhead. To reduce this cost, prior work reformulates SST as a multi-turn dialogue task, enabling full reuse of the LLM’s key–value (KV) cache and eliminating redundant feature recomputation. However, this approach relies on supervised fine-tuning (SFT) data in dialogue form, for which few human annotations exist, and existing synthesis methods cannot guarantee data quality. In this work, we propose a Hierarchical Policy Optimization (HPO) approach that post-train models trained on imperfect SFT data. We introduce a hierarchical reward that balances translation quality and latency objectives. Experiments on English to Chinese/German/Japanese demonstrate improvements of over +7 COMET score and +1.25 MetricX score at a latency of 1.5 seconds. Comprehensive ablation studies further validate the effectiveness of different quality rewards, hierarchical reward formulations, and segmentation strategies.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景**：同声传译（SST）在接收部分语音流的同时生成翻译，对实时交互场景至关重要。近年来，大语言模型（LLM）虽显著提升了 SST 的翻译质量，却带来了高昂的计算开销。
- **现有方案及其不足**：为降低开销，已有工作将 SST 重构为多轮对话任务，以复用 LLM 的键值（KV）缓存，避免重复计算。然而，该方法依赖对话形式的监督微调（SFT）数据，但：
  - 高质量的人工标注数据极其匮乏；
  - 现有的合成数据方法（如基于词对齐或 LLM 模拟口译员）无法保证轨迹的质量，可能导致翻译时机不当或生成无效的读-写轨迹。
- **核心问题**：如何在计算效率高（复用缓存）但训练数据质量不完美的情况下，进一步提升无界语音同声传译模型的翻译质量与延迟的权衡？
- **整体含义**：本文提出了一种**分层策略优化（HPO）后训练方法**，专门用于纠正因不完美合成训练数据而引入的错误行为，在不牺牲推理效率的前提下，显著提升翻译质量。

## 2. 论文提出的方法论

- **基本架构**：采用 InfiniSST（IWSLT 2025 低延迟赛道最优方案），由流式语音编码器和大语言模型翻译器组成。语音按固定时长分块，编码器增量编码，LLM 以交错输入（语音特征、已生成译文）的方式解码翻译。
- **数据合成（SFT 阶段）**：从 YODAS 英文长语音中提取片段，用 ASR 获取时间戳并翻译，再通过 SimAlign 词对齐工具构建多轮交错轨迹，用于 SFT 训练。
- **分层策略优化（HPO）**：
  - **整体流程**：基于 GRPO（Group Relative Policy Optimization），对每个长语音段采样多个翻译假设，分别评估其质量与延迟，用分层奖励优化模型参数。
  - **关键步骤**：
    1. **分段与对齐**：用 SEGALE（结合 spaCy 句子分割与嵌入对齐）而非传统的 mwerSegmenter，鲁棒地将假设译文与参考译文切分为句子，并对齐，可识别过译与漏译（空对齐）。
    2. **分层奖励设计**：
       - 对每个对齐句对计算质量分数 \( q_{j,k} \)（如 MetricX）和延迟分数 \( l_{j,k} \)（基于 LAAL）。
       - **门控机制**：若质量分数低于预设阈值 \( q_{\text{thres}} \)，则将延迟分数强制设为最大值 \( l_{\text{max}} \)，即 `if q < threshold then l = l_max`，以此**防止模型为了追求低延迟而牺牲翻译质量**。
       - 对每个假设的所有句子求平均质量和延迟分数。
       - **组归一化**：将一批样本内所有假设的平均质量分数和平均延迟分数分别进行组归一化，再按权重 \( \lambda \) 相加得到最终奖励 \( r_j = \bar{q}_j - \lambda \cdot \bar{l}_j \)。
    3. **优化目标**：采用 GRPO 的裁剪奖励与重要性采样比率，并加入 KL 散度惩罚项，防止策略偏离参考模型过远。
  - **公式概要**：\( J(\theta) \) 对多个采样轨迹的归一化奖励求期望，单步奖励 \( R_{jt} \) 包含重要性采样比、GRPO 裁剪后的奖励和 KL 项。

## 3. 实验设计

- **训练数据**：YODAS 英语语音的 5k 小时子集，合成长语音段（≤67.2 秒），并自动翻译为中文、德文、日文。最终获得 En–Zh 1592h、En–De 1622h、En–Ja 1018h。
- **评估数据集**：
  - **ACL 60/60 dev set**：5 个学术演讲，每段 10–20 分钟，涵盖英语→中文/德语/日语三个方向。
  - **RealSI**：10 个自发演讲，约 5 分钟/个，仅英语→中文。
- **对比方法（Baseline）**：
  - **离线 ST**：使用完整语音翻译，分为 utterance-level（单句）和 long-form（最长 67.2 秒，并条件历史译文）两种推理模式。
  - **InfiniSST（SFT）**：基于合成轨迹训练的强基线 SST 模型，HPO 的初始化点。
  - **HPO**：本文提出的后训练方法。
- **评价指标**：
  - **延迟**：StreamLAAL（结合 SEGALE 分段，空对齐逐惩罚为 10 秒）。
  - **翻译质量**：BLEU、BLEURT-20、COMET、MetricX、LLM-as-Judge（Gemini-3.1），空对齐赋予最差分。

## 4. 资源与算力

- **硬件配置**：HPO 训练使用了**三台各配备 8 块 NVIDIA H100 GPU 的节点**（共 24 块 H100）。
- **角色分配**：一台节点专用于奖励计算，另外两台节点用于并置的模型训练、推理和轨迹展开生成。
- **训练时长**：一个 HPO 训练运行约需要 **20 小时（对应 500 步优化）**。文中提到训练最多 700 步，并选取验证质量奖励最高的检查点。

## 5. 实验数量与充分性

- **主要对比实验**：在 ACL 60/60 的 3 个语言方向和 RealSI 的 1 个语言方向上，分别绘制了 4 种质量指标随延迟变化的曲线，对比 HPO、InfiniSST 和两种离线 ST，实验**覆盖面广**。
- **消融实验**：
  - **质量奖励函数**：对比了 6 种不同奖励（包括 MetricX、COMET、MQM、VIP-LLM 等），并在 4 种自动评估指标和人类评价（VIP 协议）下交叉验证。
  - **分层奖励设计**：对比了“仅归一化”、“归一化+截断（SeqPO）”、“文档级分层”和“句子级分层（HPO）”。
  - **分割策略**：对比 SEGALE 与 mwerSegmenter，并展示奖励黑客案例。
  - **超参数敏感性**：对质量阈值 \( q_{\text{thres}} \)、最大延迟 \( l_{\text{max}} \)、延迟权重 \( \lambda \) 各自取 3–4 个值，并报告了 5 次运行的平均结果。
- **充分性与公平性**：实验设计严谨，消融全面，覆盖了关键模块和超参数，结果具有统计稳健性（多次运行取平均）。对比基准包括当前最优的 InfiniSST，并考虑了离线上下界，具有较好的公平性。

## 6. 论文的主要结论与发现

- **质量-延迟权衡显著提升**：在约 1.5 秒平均延迟下，HPO 相较于 InfiniSST（SFT）基线，COMET 提升超 7 分，MetricX 提升超 1.25 分，BLEURT 提升超 4 分，且在多数指标上表现出最佳权衡。HPO 甚至在某些情况下超越了离线高延迟翻译的质量。
- **奖励机制有效性**：分层奖励中的门控机制（质量不达标时禁用延迟优化）是关键，有效避免了模型牺牲质量换取低延迟。MetricX 作为质量奖励在所有测试函数中与人类判断最吻合。
- **分割方式的重要性**：SEGALE 能处理空对齐、防止奖励黑客（如模型生成无意义文本来骗取高分），对训练稳定性至关重要。
- **潜在风险**：Gemini 评估提示，基于神经指标（如 MetricX）的优化仍可能存在奖励黑客行为，模型偶尔会通过省略细节来追求“流畅高分”。

## 7. 优点

- **创新性方法论**：首次将基于 GRPO 的强化学习后训练引入无界语音同传任务，解决了 SFT 数据不完美导致的次优行为。
- **精巧的奖励设计**：提出了“质量门槛”的分层奖励，直接解决 RL 中延迟目标易于优化而损害质量的问题，设计简洁且有效。
- **工程鲁棒性**：引入 SEGALE 鲁棒分段与对齐，并配合组归一化，增强了训练的稳定性和对长文本的适应能力。
- **实验验证扎实**：包含多语言、多数据集、多指标、人类评估、消融研究和超参数敏感性分析，结论可信度高。

## 8. 不足与局限

- **架构与方法的通用性有限**：仅基于 InfiniSST 单一架构进行验证，且数据合成仅依赖于一种词对齐工具，对其它架构或合成流程的泛化性尚未探讨。
- **语言覆盖范围窄**：源语言仅为英语，目标语言仅包括中、德、日三个，缺乏对资源更匮乏或语言差异更大的语种的验证。
- **奖励模型的不透明性**：测试发现神经质量奖励（MetricX）仍有奖励黑客倾向（如导致省略），表明自动奖励函数的可靠性仍是瓶颈，可能限制性能的上限。
- **离线对比的不完全公平**：离线 ST 模型未采用类似的 RL 优化，使得与 HPO 的质量比较存在一定的不对等性。
- **应用场景的潜在风险**：案例研究表明 HPO 可能增加翻译省略的风险，在实际部署中需谨慎评估。

（完）
