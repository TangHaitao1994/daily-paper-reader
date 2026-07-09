---
title: Multimodal In-context Learning for ASR of Low-resource Languages
title_zh: 面向低资源语言语音识别的多模态上下文学习
authors: "Zhaolin Li, Jan Niehues"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.findings-acl.1239.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: 多模态上下文学习提升未见低资源语言的语音识别
tldr: 自动语音识别（ASR）对世界语言覆盖率不足，尤其低资源语言数据稀缺。本文研究如何通过多模态上下文学习（MICL）利用语音大语言模型（LLM）提升未见语言的语音识别能力。在Phi-4和Qwen3-Omni两个语音LLM上，对三种濒危语言进行实验，发现MICL能有效结合语音和文本模态实现跨领域学习，且跨语言迁移进一步提升了效率。这项工作证明了语音LLM的少样本泛化潜力，为低资源语言ASR的快速发展提供了新思路，对全球语言保护和人机交互具有积极意义。该方法无需大量标注数据，即可快速适配新语言。
source: ACL-2026-Findings
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1239/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1638, \"height\": 500, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-findings/anthology-2026.findings-acl.1239/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1627, \"height\": 484, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 783, \"height\": 173, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 988, \"height\": 499, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1225, \"height\": 1040, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 469, \"height\": 212, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 719, \"height\": 539, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1063, \"height\": 211, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 707, \"height\": 175, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 706, \"height\": 208, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 790, \"height\": 174, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 413, \"height\": 178, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 446, \"height\": 823, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1231, \"height\": 462, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1150, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1465, \"height\": 927, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-findings/anthology-2026.findings-acl.1239/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1253, \"height\": 171, \"label\": \"Table\"}]"
motivation: 现有语音识别主要覆盖高资源语言，低资源语言因数据稀缺发展受限，上下文学习多局限于文本单模态。
method: 探索语音LLM的多模态上下文学习（MICL），利用语音和文本模态提升低资源语言ASR，并引入跨语言迁移。
result: 在三种濒危语言上，MICL有效提升ASR性能，跨语言迁移进一步提高效率。
conclusion: 多模态上下文学习为低资源语言语音识别提供了数据高效的新路径，展示了语音LLM的强大泛化能力。
---

## Abstract
Automatic speech recognition (ASR) still covers only a small fraction of the world’s languages, mainly due to supervised data scarcity. In-context learning (ICL) with large language models (LLMs) addresses this problem, but prior work largely focuses on high-resource languages covered during training and text-only settings. This paper investigates whether speech LLMs can learn unseen languages with multimodal ICL (MICL), and how this learning can be used to improve ASR. We conduct experiments with two speech LLMs, Phi-4 and Qwen3-Omni, on three diverse endangered languages. Firstly, we find that MICL is effective for unseen languages, leveraging both speech and text modalities. We further show that cross-lingual transfer learning improves MICL efficiency on target languages without training on them. Moreover, we analyze attention patterns to interpret MICL mechanisms, and we observe layer-dependent preferences between audio and text context, with an overall bias towards text. Finally, we show that prompt-based ASR with speech LLMs performs poorly on unseen languages, motivating a simple ASR system that combines a stronger acoustic model with a speech LLM via MICL-based selection of acoustic hypotheses. Results show that MICL consistently improves ASR performance, and that cross-lingual transfer learning matches or outperforms corpus-trained language models without using target-language data. Our code is publicly available

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **研究动机**：全球7000余种语言中，自动语音识别（ASR）仅覆盖一小部分，主要瓶颈在于低资源语言缺乏有监督数据。
- **问题背景**：现有大语言模型（LLM）的上下文学习（ICL）大多聚焦于训练中已覆盖的高资源语言和纯文本场景，未见语言的语音识别尚未充分探索。
- **研究目标**：探究语音大语言模型（speech LLM）能否通过**多模态上下文学习（MICL）** 掌握训练中未出现的低资源语言，并验证该能力能否用来提升ASR性能。
- **整体意义**：为低资源语言ASR开辟了一条无需大量标注数据、利用语音和文本双模态快速适配的新路径，展示了语音LLM的少样本跨语言泛化潜力。

## 2. 论文提出的方法论
### 2.1 多模态上下文学习任务形式化
- 给定一组上下文演示 \( \mathcal{C} = \{c_i\}_{i=1}^N \) 和目标音频 \( a^* \)，模型预测转录 \( t^* \)。
- 预测过程建模为条件概率分解：\( p_\theta(t^*|a^*, \mathcal{C}) = \prod_{j=1}^T p_\theta(w_j^*|w_{<j}^*, a^*, \mathcal{C}) \)。

### 2.2 提示（Prompt）模态设计
通过四种设置隔离各模态贡献：
- **T-ICL**：仅有文本演示（无目标音频），测量纯文本上下文能带来的改进。
- **ICL**：文本演示 + 目标音频，评估文本示例对音频转写的引导作用。
- **MICL**：配对音频–文本演示 + 目标音频，测试多模态示例是否提供额外增益。
- **ASR基线**：无上下文演示，直接预测目标音频转录。

### 2.3 示例选择策略
- 使用多语言多模态嵌入模型SONAR，将候选样本和目标的预测转录映射到同一空间，按余弦相似度选择Top-N最相关示例。

### 2.4 微调策略
- **ASR-FT**：标准ASR微调，无上下文示例。
- **TFT（目标语言微调）**：使用目标语言的音频和上下文示例微调。
- **XFT（跨语言微调）**：使用143种其他语言（不含目标语言）的MICL数据进行微调，测试多语言迁移对未见语言的泛化能力。
- 训练时仅在目标转录token上计算损失，上下文部分作为条件。微调采用LoRA，解冻decoder，冻结其余参数。

### 2.5 基于MICL的ASR假设选择系统
- 先用更强的声学模型（MMS）对输入音频生成N-best候选列表 \( \mathcal{H} = \{h^{(k)}\} \)。
- 对每个假设计算：（1）声学模型得分；（2）利用MICL的语言模型得分（对数似然）。
- 最终输出：\( \hat{h} = \arg\max_{h \in \mathcal{H}} [\text{Acoustic\_score}(h) + \text{LM\_score}_{\text{MICL}}(h)] \)。
- 该方法结合了强声学识别能力与语音LLM的上下文理解能力。

## 3. 实验设计
### 3.1 数据集
- **目标语言**：三种来自不同语系、不同类型的濒危语言——**Khinalug**（东北高加索语系）、**Kichwa**（克丘亚语系）、**Mboshi**（班图C区）。
- **数据规模**：各语言训练集2–4小时，测试集约0.5小时不等（详见Table 1）。
- **跨语言微调数据**：ML-SUPERB 2.0的143种语言（严格不包含上述三种目标语言）。
- **预处理**：文本小写化并移除标点。

### 3.2 模型与对比方法
- **主测模型**：两个开源语音LLM——**Phi-4** 和 **Qwen3-Omni**。
- **对比上下文设置**：T-ICL、ICL、MICL、ASR，并在不同示例数量（1-100）及微调策略（Base、XFT、TFT）下测试。
- **ASR假设选择对比**：
  - 基线：仅声学模型（MMS）
  - 文本LM：N-gram LM、Transformer LM、Llama-3-8B-Instruct
  - 多模态LLM：Phi-4（ASR-FT、XFT、TFT）和Qwen3-Omni
  - Oracle：理论最优

### 3.3 评价指标
- **上下文学习评估**：困惑度（Perplexity），仅计算目标转录token上的概率，数值越低表示模型对正确转录的置信度越高。
- **ASR评估**：词错误率（WER）。

## 4. 资源与算力
- **推理硬件**：Phi-4使用NVIDIA RTX 6000 Ada（48GB显存），Qwen3-Omni使用Nvidia RTX PRO 6000 Blackwell（96GB显存）。
- **内存与速度**：以Phi-4在Khinalug数据集上进行假设选择为例，模型加载需11GB，假设选择阶段额外需要11GB，共22GB显存；单样本平均推理时间约3秒。
- **训练**：采用LoRA微调解码器（参数约4.6亿），优化器为adamw_torch，学习率1e-5，weight decay 0.01，warm up 500步，batch size 8。**论文未明确报告训练总时长或总GPU小时数**，但提及受限于计算资源。

## 5. 实验数量与充分性
- **实验组数**：在3种语言上，针对2个模型、4种上下文类型、多种示例数量（0–100）、3种微调策略，进行了系统的困惑度评测；此外还进行了完整的ASR假设选择实验、注意力分析、以及多项消融实验（语言覆盖度、注意力干预、示例选择策略、微调模块、微调用例数量等）。
- **充分性评价**：实验设计多层次且相互印证，对比基线合理（包含文本LM、纯声学模型等），消融研究详实，能够支撑主要结论。评估指标包含困惑度和WER，虽不直接一一映射，但实证表明趋势一致。整体实验较为充分、客观。

## 6. 论文的主要结论与发现
- **MICL有效性**：语音LLM能通过多模态上下文学习掌握训练中未见过的语言，且同时受益于语音和文本两种模态。
- **多语言迁移**：跨语言指令微调（XFT）显著提升目标语言的MICL效率，甚至能在不使用目标语言数据的情况下匹敌或超越专门训练的文本语言模型。
- **注意力机制解释**：语音LLM在多模态上下文学习中呈现**层依赖的模态偏好**，早期和后期层更关注音频，中间层更关注文本；整体上模型更加依赖文本示例。
- **提示ASR局限性**：仅通过提示直接进行ASR在未见语言上效果极差，WER居高不下。
- **假设选择有效性**：将强声学模型与MICL驱动的假设选择相结合，能稳定提升ASR性能；其中跨语言微调后的模型甚至超过了用目标语言数据训练的传统语言模型。
- **注意力与实际学习**：注意力分配反映了模型对上下文样本的真实使用情况，替换关键样本可大幅改变注意力分布和预测结果。

## 7. 优点
- **新颖性**：首次系统研究语音LLM在多模态上下文学习中对少见语言的ASR能力，填补了从纯文本ICL到语音+文本MICL的空白。
- **方法实用**：提出的假设选择系统无需昂贵训练，利用现成声学模型和LLM即可提升低资源ASR，并开源代码。
- **可解释性分析**：通过层级别注意力分析揭示了模态利用的层级化模式，为模型设计提供洞见。
- **严谨的跨语言研究**：选择三种语系完全不同、文献类型各异的濒危语言，且跨语言微调严格排除目标语言，增强了结论的普适性。
- **多角度验证**：同时使用困惑度和WER评估，辅以消融和注意力干预实验，论证扎实。

## 8. 不足与局限
- **语言与任务覆盖有限**：仅测试三种语言（虽具多样性），且只在ASR任务上评估，未拓展到语音翻译、语言理解等多任务。
- **模型受限**：仅实验了两个开源语音LLM，结论可能不适用于GPT-4o、Gemini等闭源超大模型。
- **注意力分析非因果**：注意力权重仅作为相关性诊断信号，不能视为模型行为的因果解释；存在注意力下沉等已知artifact的干扰。
- **计算与规模约束**：推理需较大显存（22GB+），训练时长未明确；跨语言微调使用的语言和示例数量仍较有限，未充分探索大规模数据下的潜力。
- **假设选择vs联合解码**：虽然假设选择提升明显，但文中对比显示，传统的ngram LM联合解码在低资源场景下仍明显优于重排序方法；如何在联合解码中高效融入MICL仍是挑战。
- **示例选择依赖外部模型**：选择策略依赖SONAR嵌入和初步转录，其质量会影响最终效果。

（完）
