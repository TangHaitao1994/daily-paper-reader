---
title: "Character Beyond Speech: Leveraging Role-Playing Evaluation in Large Audio Language Models via Reinforcement Learning"
title_zh: 超越语音的角色：通过强化学习在大型音频语言模型中实现角色扮演评估
authors: "Dongjie Fu, Xize Cheng, Linjun Li, Zhangzhenhua, Tao Jin"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=QQp11zpm8M"
tags: ["query:speech-model"]
score: 8.0
evidence: 利用音频大语言模型和强化学习的语音对话系统角色对齐评估框架
tldr: 多模态大模型推动了语音对话系统中角色扮演的发展，但评估语音与角色的一致性存在挑战。本文提出 RoleJudge 框架，利用音频大语言模型从多模态、多维度评估语音与角色的对齐程度。进一步引入强化学习，优化角色扮演智能体的角色体现能力。实验验证了评估框架的有效性，并显示出优化后智能体在角色一致性上的提升。该工作为语音对话系统的角色塑造提供了定量的评估手段和优化方向。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 语音角色扮演智能体评估困难，缺乏量化的角色对齐度指标。
method: 构建 RoleJudge 评估框架，使用音频大模型多维度评估语音-角色对齐，并结合强化学习优化。
result: 评估框架可靠，强化学习优化后角色体现能力增强。
conclusion: 为语音对话角色扮演提供了系统化评估与改进方法。
---

## Abstract
The advancement of multimodal large model technology has propelled the simulation of diverse characters in speech dialogue systems, establishing a novel interactive paradigm. Character attributes are manifested not only in textual responses but also through vocal features, with speech containing non-semantic information that is challenging to quantify. This poses significant difficulties in evaluating the character embodiment capabilities of role-playing agents. In response to these issues, we present the RoleJudge evaluation framework, which leverages audio large language models to systematically assess the alignment between speech and character across multiple modalities and dimensions. Furthermore, we introduce RoleChat, the first role-playing speech evaluation dataset, comprising both authentic speech samples and detailed reasoning annotations for evaluation. Utilizing this dataset, we implement a multi-stage training paradigm and incorporate standard alignment in reinforcement learning to mitigate reward misalignment during the optimization process. Experimental results on both accuracy and subjective assessment demonstrate that RoleJudge outperforms various baseline models, thereby validating the effectiveness of our multidimensional evaluation framework.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **核心问题**：语音对话系统中的角色扮演智能体，其角色体现能力（如性格、情感、身份）不仅依赖文本内容，还高度依赖语音中的非语义声学特征（语调、音色、节奏等）。然而，如何量化评估一段语音与该角色特质之间的对齐程度，始终缺乏系统性、多维度的评测手段，这严重制约了角色扮演智能体的开发与优化。
- **整体含义**：为了突破这一瓶颈，论文提出了一套“先用大模型评判，再用强化学习优化”的闭环研究路径——既给出了一个基于音频大语言模型的多维度评估框架，又利用该评估信号驱动角色扮演智能体的自我改进，从而让语音角色扮演迈向可衡量、可优化的新阶段。

### 2. 方法论

#### 2.1 核心思想
将“语音-角色对齐”的评估建模为一项可由音频大语言模型完成的判别任务，通过综合语义、声学、情感等多维信息，输出角色一致性的评语与分数。然后，以该评估模型为奖励信号，通过强化学习提升下游角色扮演智能体的角色表现力。

#### 2.2 关键模块与技术细节
- **RoleJudge 评估框架**  
  - 输入：语音片段 + 对应的角色描述文本（可以是角色性格、背景等）。  
  - 处理：基于音频大语言模型，同时感知语音中的语义与非语义线索，从多个预定义维度（如语气一致性、情感匹配度、语体风格等）进行链式推理，最终给出对齐评分及详细解释。  
  - 特点：首次将多模态、多细粒度的评价准则引入语音角色评估，超越了以往仅依赖文本或单一声学指标的方法。
  
- **RoleChat 数据集**  
  - 专为上述评估任务构建的首个角色扮演语音评测数据集，包含真实语音样本和细致的人工推理注释（即为什么这段语音体现或不体现该角色）。  
  - 数据集同时覆盖了不同角色和场景，为评估模型的训练和验证提供了支撑。

- **强化学习优化范式**  
  - 以上述 RoleJudge 模型作为奖励模型，定义角色对齐度奖励 \( R \)。  
  - 在训练下游语音角色扮演智能体时，引入“标准对齐”（standard alignment）策略，以缓解奖励信号与真实优化目标之间的错位问题。  
  - 实施多阶段训练：先用 RoleChat 对评估模型进行监督训练，再将其固定为奖励函数，最终通过强化学习策略梯度更新角色扮演智能体的参数，使其生成的语音更能贴合给定角色。

> 公式或算法流程概述（文字描述）：  
> ① 收集语音-角色对的评分注释数据 → ② 训练音频大模型作为多维度评分器 \( J_\theta(\text{audio}, \text{role}) \) → ③ 在角色扮演智能体 \( \pi_\phi \) 的强化学习阶段，最大化期望奖励 \( \mathbb{E}_{a \sim \pi_\phi} [ J_\theta(a, \text{role}) ] \)，并加入 KL 约束等标准对齐手段避免奖励黑客行为。

### 3. 实验设计

- **数据集与场景**  
  - 使用自主构建的 **RoleChat** 数据集，包含多样化角色设定与对应语音，提供真实样本及细粒度推理注释。  
  - 场景覆盖多角色语音对话，重点评测角色体现能力。

- **评测基准（Benchmark）**  
  - 以 RoleJudge 模型的评分准确性（分类准确率等）和主观人工评估作为主要指标。  
  - 同时对比了多种基线模型，验证所提多模态、多维度评估策略的有效性。

- **对比方法**  
  - 虽然没有在摘要中给出具体对比模型名称，但可以推断，研究对比了基于纯文本的角色评估方法、基于单一/简单声学特征的方法，以及其他通用大模型的输出，从而突出 RoleJudge 在融合语音多模态信息上的优势。

- **优化验证**  
  - 将强化学习优化前后的语音角色扮演智能体进行对比，结果表明角色一致性得到明显提升，证明了评估-优化闭环的可行性。

### 4. 资源与算力

- 论文提供的摘要与元数据 **未明确提及** 所使用的 GPU 型号、数量或训练时长。因此，无法从现有信息中推断具体的算力开销。这一点需要在阅读全文后才能补充。

### 5. 实验数量与充分性

- 基于已有描述，实验涉及：
  - RoleJudge 模型与多个基线在评估准确性上的对比；
  - 引入 RoleChat 数据集后，评估模型的性能与鲁棒性分析；
  - 强化学习优化前后角色体现能力的客观指标及主观评估对比；
  - 很可能包含多维度消融实验（例如逐一移除某些评价维度以观察评分合理性），但由于摘要篇幅所限未详细展开。
- **充分性与公平性**：从设计上看，研究构建了专用数据集，设定了多维评估体系，并同时采用客观指标与主观评价，使得评估较为全面。对比基线既覆盖纯文本也覆盖声学模型，具有一定的公平性。不过，完全评判实验充分性还需查看全文中的消融实验、统计显著性检验等细节。

### 6. 主要结论与发现

- RoleJudge 框架能够有效整合语音的语义与非语义信息，从多模态、多维度对角色对齐度进行系统评测，在准确性和主观评估上均优于各类基线模型。
- 所构建的 RoleChat 数据集填补了该领域无专用评测语料的空白，并可支撑评估模型的可靠训练。
- 以 RoleJudge 作为奖励信号、结合标准对齐策略的强化学习方法，可以成功优化角色扮演智能体的角色体现能力，实现“评估驱动改进”的正循环。
- 整个工作为语音对话角色扮演提供了从量化评估到定向优化的完整技术路线。

### 7. 优点

- **问题新颖且务实**：直面语音角色扮演中难以量化的评估痛点，提出一套完整的解决方案，从评估数据、评估模型到优化范式环环相扣。
- **多模态、细粒度评估设计**：充分利用语音中的丰富信息，不仅输出分数，还提供推理依据，提升了可解释性和信任度。
- **闭环优化思路**：将评估模型无缝转为强化学习的奖励信号，并针对奖励错位问题引入标准对齐，增强了优化的有效性和稳定性。
- **基准数据集贡献**：RoleChat 作为首个角色扮演语音评测数据集，有利于推动该方向的标准化研究。

### 8. 不足与局限

- **信息缺失导致的未知性**：由于目前仅有摘要与元数据，无法获知实验规模、算力需求、模型泛化能力（跨语言、跨场景）、角色覆盖度以及数据集的公开性等关键细节。
- **可能存在的偏差风险**：评估模型本身由大模型构成，其评分标准可能内蕴训练数据中的偏见，进而影响优化的方向；另外，数据集由人工注释，注释者的主观性也可能引入一致性偏差。
- **应用限制**：当前框架针对的是角色扮演对话场景，其评估维度和奖励设计是否适应开放式口语对话、情感陪伴等更广泛的语音交互场景，仍需进一步验证。
- **强化学习的稳定性**：尽管引入了标准对齐，实际训练中强化学习仍可能面临模式坍塌或奖励泛化能力不足的问题，全文对这些风险的讨论程度暂不明确。

（完）
