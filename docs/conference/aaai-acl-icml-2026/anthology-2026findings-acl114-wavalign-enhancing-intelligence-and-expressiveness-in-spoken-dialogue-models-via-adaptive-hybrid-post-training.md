---
title: "WavAlign: Enhancing Intelligence and Expressiveness in Spoken Dialogue Models via Adaptive Hybrid Post-Training"
title_zh: WavAlign：通过自适应混合后训练增强语音对话模型的智能与表现力
authors: "Yifu Chen, Shengpeng Ji, Qian Chen, Tianle Liang, Yangzhuo Li, Ziqing Wang, Wen Wang (王雯), Jingyu Lu, Haoxiao Wang, Xueyi Pu, Fan Zhuo, Zhou Zhao"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.114.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 通过自适应混合后训练增强语音对话模型的智能和表现力
tldr: 端到端语音对话模型在智能和表现力上不及预期，直接应用在线强化学习存在困难。本文分析奖励建模和采样中的难点，提出自适应混合后训练方法，结合模仿学习与偏好优化，在共享参数更新下协调密集语音生成与稀疏偏好监督。实验显示，模型自然度和对话质量显著提升。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1273, \"height\": 805, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1663, \"height\": 493, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1145, \"height\": 681, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1166, \"height\": 560, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1150, \"height\": 536, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.114/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1293, \"height\": 790, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1648, \"height\": 655, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 800, \"height\": 604, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 800, \"height\": 281, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 796, \"height\": 211, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1658, \"height\": 208, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1658, \"height\": 212, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1056, \"height\": 1119, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 649, \"height\": 251, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.114/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 608, \"height\": 251, \"label\": \"Table\"}]"
motivation: 现有开源语音对话模型在智能和表现力方面表现不足，强化学习迁移面临挑战。
method: 分析奖励建模与采样障碍，提出结合模仿学习和偏好优化的自适应混合后训练。
result: 模型在智能和表现力指标上获得显著提升，优于原有方法。
conclusion: WavAlign为语音对话模型的后训练提供了有效策略，推动了语音交互技术的发展。
---

## Abstract
End-to-end spoken dialogue models have garnered significant attention because they offer a higher potential ceiling in expressiveness and perceptual ability than cascaded systems. However, the intelligence and expressiveness of current open-source spoken dialogue models often remain below expectations. Motivated by the success of online reinforcement learning(RL) in other domains, one might attempt to directly apply preference optimization to spoken dialogue models, yet this transfer is non-trivial. We analyze these obstacles from the perspectives of reward modeling and rollout sampling, focusing on how sparse preference supervision interacts with dense speech generation under shared-parameter updates. Based on the analysis, we propose a modality-aware adaptive post-training recipe that makes RL practical for spoken dialogue: it constrains preference updates to the semantic channel and improves acoustic behavior via explicit anchoring, while dynamically regulating their mixture from rollout statistics to avoid unreliable preference gradients. We evaluate the method across multiple spoken dialogue benchmarks and representative architectures, and observe consistent improvements in semantic quality and speech expressiveness.

---

## 论文详细总结（自动生成）

### 一、 论文的核心问题与整体含义

端到端语音对话模型直接在语音信号上运行，理论上能减少级联系统的误差传播与信息损失，并实现语义与副语言信息的联合建模。然而，当前开源端到端模型在语义能力、自然度及表现力上尚未稳定超越级联基线。核心挑战在于：**如何在单一模型中同时提升语义对话质量（智能，IQ）与语音自然度/表现力（表现力，EQ），避免二者相互牺牲。**

受文本与视觉领域强化学习（RLHF/RLAIF）成功的启发，直接对混合文本-语音输出应用序列级偏好优化（如GRPO/DPO）存在非平凡障碍。论文分析表明，直接迁移会导致三个耦合问题：
- **跨模态权衡与不一致**：语义偏好目标可能提升，但语音质量常出现退化（声学漂移、自然度下降）。
- **梯度能量严重失衡**：语义（文本）梯度在共享参数更新中占主导，而密集语音token获得的偏好监督信号弱且方差高。
- **奖励/信号稀释**：稀疏的偏好反馈分散在过长的语音token序列上，导致信用分配困难，且声学奖励噪声大、易被“奖励黑客”利用。

因此，工作旨在设计一种实用的后训练方案，使强化学习对语音对话模型可行，以实现**智能与表现力的协同增强**。

### 二、 方法论

论文提出了一种**单阶段、模态感知的自适应混合后训练框架（WavAlign）**，核心思想是将不同模态的优化角色解耦：用监督微调（SFT）构建和维持声学可行性与自然度，用偏好优化（PO）精炼语义行为，并通过动态门控机制调节二者混合比例。

**关键技术细节与流程：**

1.  **模态分工与损失设计**
    -   **文本-偏好优化（\( \mathcal{L}_{\text{GRPO}}^{(T)} \)）**：仅对文本token（语义通道）应用GRPO偏好更新，利用其更可靠、更一致的语义偏好信号。通过token掩码限制梯度范围，避免偏好信号干扰密集的语音分布。
    -   **全token-监督微调（\( \mathcal{L}_{\text{SFT}} \)）**：对所有token（文本+语音）执行SFT，作为分布锚点。它提供密集的token级约束，稳固语音token分布，防止声学漂移，并实现比局部偏好优化更大的全局分布迁移。

2.  **动态混合门控**
    -   **目标**：避免在采样轨迹质量低、判别性差时引入不可靠的偏好梯度。
    -   **门控制器**：据每一训练步骤的采样组（rollout group）的奖励统计，动态计算混合权重 \( \lambda_t \)。混合损失为：
        \[
        \mathcal{L}_{\text{hybrid}} = (1 - \lambda_t) \mathcal{L}_{\text{SFT}} + \lambda_t \mathcal{L}_{\text{GRPO}}^{(T)}
        \]
    -   **权重计算**：\( \lambda_{\text{raw}} = \lambda_{\max} \cdot g(R) \cdot g(V) \)，其中：
        -   \( g(R) \) 为**方向门**，当组内最高奖励 \( R_{\max} \) 超过中立阈值时才激活，确保存在可接受样本。
        -   \( g(V) \) 为**信息门**，基于归一化奖励方差 \( v_t \)，确保样本具有充分判别度。
    -   **平滑**：对 \( \lambda_t \) 应用指数移动平均（EMA），减少步间振荡，增加训练稳定性。

3.  **算法流程**：
    -   从当前策略采样一组 \( G \) 条语音回复。
    -   将语音解码为音频，直接输入奖励模型获取标量奖励。
    -   计算组内奖励统计量，通过动态门控机制得到平滑的 \( \lambda_t \)。
    -   据 \( \lambda_t \) 计算混合损失，更新模型参数。

### 三、 实验设计

**1. 数据集与训练细节**
- **训练集**：从公开和自建来源混合构建，共13.5k条音频指令样本，覆盖推理、知识、指令跟随、安全、副语言控制等任务。通过重复采样与评判模型打分构建偏好对。
- **骨干架构**：为验证通用性，选择两种代表性端到端语音对话架构：
    -   **VITA-Audio**（交织式生成文本和语音token）
    -   **KimiAudio**（并行式设计）

**2. 基准测试（Benchmarks）与评估指标**
- **IQ（智能）评估**：使用**VoiceBench**（含AlpacaEval、IFEval等9个子任务）和**OpenAudioBench**（含常识、推理等5个子任务），依据官方流程，通过GPT-4o等文本评判模型对语音转录的文本质量进行打分。
- **EQ（表现力）评估**：使用**VStyle**基准，包含声学属性、风格指令跟随、角色扮演、共情四个维度，通过Gemini-2.5-Pro根据官方流程对语音输出评分。

**3. 对比方法**
- **标准基线**：基础模型（Base）、纯SFT（教师强制）。
- **DPO系列**：全Token DPO、仅文本Token DPO。
- **RL系列**：统一全Token RL（GRPO）、统一仅文本Token RL（GRPO）、两阶段SFT→RL。
- **本文方法**：动态门控混合（Ours）。

### 四、 资源与算力

- **GPU**：所有训练运行使用 **4×A100 GPU**。
- **超参数**：RL采用KL正则化系数 \( \beta_{\text{text}}=\beta_{\text{speech}}=0.01 \)，采样组大小 \( G=4 \)，采样温度 \( T=0.9 \)，top-p \( p=0.9 \)，学习率 \( 1\times10^{-6} \)，批量大小1，最大序列长度2048。奖励模型使用Gemini-2.5-Pro。
- **训练时长**：论文未明确说明总训练时长或步数。

### 五、 实验数量与充分性

论文实验设计较为详尽，主要体现在：
1.  **主体实验**：在**2种不同架构**上，分别于**3大基准**（合计超18个子任务）上进行了全面对比，涉及至少7种不同的训练方法配置。结果以主表（Table 1 & Table 2）呈现。
2.  **消融研究**：专门设置了消融实验（Table 3），探究优化范围（全token vs 仅文本token）、加权策略（固定 vs 动态）以及EMA平滑的作用，清晰地展示了各组件的贡献。
3.  **分析性实验**：通过系列观察（Observations 1-4），从分布迁移、评判一致性、梯度几何、输出多样性等角度，深入剖析了混合模态偏好优化的失败机理，为方法设计提供了坚实依据。包含人类评估与奖励模型一致性分析、梯度能量与余弦相似度分析等。
4.  **人类主观评估**：开展了并列对比（SBS）人类研究，对模型在“有用性”和“自然度”两个轴向上进行盲评，并报告了统计显著性，增强了结论的可信度。

综合来看，实验覆盖广泛、对比公平、分析与消融深入，实验充分性较高。

### 六、 主要结论与发现

1.  **故障机理**：揭示并刻画了统一偏好优化在混合模态语音对话输出上的关键失败模式，主要源于弱跨模态耦合、梯度能量失衡及嘈杂的声学奖励。
2.  **方法有效性**：提出的动态混合后训练方案能够稳定提升模型性能。将偏好优化聚焦于语义通道，以监督学习锚定声学通道，并通过基于采样可靠性的动态门控调节混合强度，实现了智能与表现力的协同增益（在IQ与EQ上均取得最优）。
3.  **架构通用性**：该方案在交织式和并行式两种不同骨干架构上均表现出一致的有效性。
4.  **消融结论**：优化“范围”（仅文本）远比“加权策略”本身关键；动态门控（尤其是结合EMA平滑）相较于固定权重，能更好地权衡IQ-EQ帕累托前沿，显著提升整体结果。

### 七、 优点

1.  **问题分析深刻**：没有简单套用RL方法，而是从奖励建模、采样分布、梯度特性等角度，系统性地诊断了混合模态偏好优化的根本障碍，为后续研究提供了有价值的见解（Observation 1-4）。
2.  **方法设计巧妙且原则性强**：基于分析结论，提出了“模态分工”：语义通道做偏好优化，声学通道做分布锚定。解决了跨模态干扰问题，设计思路清晰合理。
3.  **自适应机制优雅**：动态门控利用采样组自身的统计量（最高分、方差）来调节混合权重，使模型能自动判断何时信任偏好信号，避免了手动调节固定权重的繁琐和次优风险，算法轻量且有效。
4.  **验证全面扎实**：覆盖了两种主流模型架构、多个维度的评估基准（IQ+EQ）、丰富的对比与消融实验，并辅以人类主观评估，论据极具说服力。

### 八、 不足与局限

1.  **奖励信号粒度**：研究聚焦于序列级奖励。论文指出，若能为语音token提供更可靠、更密集的指导（例如，PPO与token级或帧级反馈），可能进一步提升语音质量与稳定性，但受限于资源未能开展相关实验。
2.  **声学评委可靠性**：声学/副语言评判模型的校准和可靠性目前尚未与语义/文本评判模型持平。论文的动机观察和最终结果可能会因为更精准的声学评委的出现而发生变化。当前方法对声学奖励噪声的处理（即尽可能避免直接对其优化）是一种规避策略，而非根本解决。
3.  **训练分布外的影响**：论文展示了在特定基准上的提升，但在更开放、长尾的真实场景对话中，动态门控机制是否能始终保持稳健，缺乏进一步讨论。
4.  **计算开销描述缺失**：未提及动态混合训练相对于纯SFT或RL基线在训练时间或内存上的额外开销，这在实际部署中是一个考量点。

（完）
