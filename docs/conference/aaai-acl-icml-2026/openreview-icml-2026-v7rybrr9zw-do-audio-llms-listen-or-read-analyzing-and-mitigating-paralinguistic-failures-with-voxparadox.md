---
title: Do Audio LLMs Listen or Read? Analyzing and Mitigating Paralinguistic Failures with VoxParadox
title_zh: 音频大语言模型是在听还是在读？利用VoxParadox分析并缓解副语言理解失败
authors: "Jiacheng Pang, Ashutosh Chaubey, Mohammad Soleymani"
date: 2026-04-30
pdf: "https://openreview.net/pdf/7febf94f612efbd80b1e3057212935b4cacd9951.pdf"
tags: ["query:speech-model"]
score: 7.0
evidence: 分析并缓解音频大语言模型的副语言理解失败，服务于对话理解
tldr: 为了系统评估音频大模型在理解副语言信息（如情感、语调）方面的不足，本文创建了VoxParadox对抗基准，通过合成语音制造文本与风格矛盾以测量真实理解。评估发现模型过度依赖文本而非声学真相，进而提出改进训练策略，为构建更自然的语音对话AI提供了分析工具和改进路径。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有音频LLM对副语言信息（情感、语气等）理解不足，缺乏系统量化基准。
method: 构建VoxParadox对抗基准，通过风格与文本矛盾的合成语音测量副语言理解，并进行层级分析。
result: 发现模型普遍依赖语言信号而非声学真相，并揭示了注意力分配偏差。
conclusion: 指出了当前语音对话AI的副语言缺陷，并提供了改进训练策略的方向。
---

## Abstract
Audio large language models (Audio LLMs) demonstrate strong performance on speech understanding tasks, yet their ability to understand paralinguistic information remains limited. To systematically quantify this issue, we introduce **VoxParadox**, an adversarial benchmark with 2,000 verified examples, spanning 10 paralinguistic tasks, created with controlled speech synthesis to intentionally mismatch transcript claims and speaking style, enabling direct measurement of speech paralinguistic understanding. Evaluation of a diverse set of Audio LLMs reveals consistently low accuracy on acoustic ground truth and a strong tendency to follow language-implied (incorrect) answers. To understand the cause of this gap, we perform layer-wise probing and find that (i) paralinguistic cues can degrade in deeper encoder layers and at the encoder--LLM interface, and (ii) even when such cues are available in audio tokens, the language model frequently ignores them. To address these problems, we propose **Prompt-Conditioned Layer Mixer (PCLM)**, which adaptively combines information from multiple audio layers based on the input prompt, and pair it with **Direct Preference Optimization (DPO)** to explicitly prefer acoustically supported options over language-implied alternatives. These methods substantially improve Audio LLM paralinguistic understanding, improving Audio Flamingo 3 from **17.40%** to **65.20%** on VoxParadox, and from **37.74%** to **54.78%** on MMSU paralinguistic subset. Our project page is available at https://voxparadox.github.io/.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：当前音频大语言模型（Audio LLMs）虽然在语音理解任务上表现出色，但对副语言信息（如情感、语势、说话风格等非文字内容）的理解能力显著不足，这成为语音对话 AI 走向自然交互的关键瓶颈。
- **核心问题**：如何系统化地量化 Audio LLMs 的副语言理解缺陷，并揭示其“在读而非在听”的失败根源——即模型过度依赖文本语义，而忽略声学信号所携带的真实副语言线索。
- **整体含义**：论文指出现有模型在面对文本声称与说话风格相矛盾时会偏向文本，这违背了副语言理解的本质，需要通过对抗式基准和新的训练策略来纠正这种偏差，从而推动更自然的语音 AI。

### 2. 论文提出的方法论
- **VoxParadox 对抗基准**：
  - 通过可控语音合成，刻意制造转录文本声明与发音风格之间的矛盾（例如，用愤怒的语气说“我很平静”）。
  - 包含 2,000 个经人工验证的样本，覆盖 10 类副语言任务，用于直接度量声学真相理解能力，而非表面文字推断。
- **层次化原因诊断**：
  - 对 Audio LLM 的编码器各层以及编码器-大语言模型接口进行逐层探查（layer-wise probing），定位副语言线索在深层网络中的衰减点以及语言模型对可用声学信息的忽视行为。
- **改进方法：Prompt-Conditioned Layer Mixer (PCLM) + DPO**：
  - **PCLM**：根据输入提示（prompt）自适应地混合来自音频编码器不同深度的多层信息，保留对副语言任务有益的浅层声学表征，避免其被后期语义主导的层所压制。
  - **训练策略**：结合直接偏好优化（DPO），显式地让模型偏好声音支持的选项，而非单纯由语言暗示的选项，从而强化学到的副语言对齐。

### 3. 实验设计
- **基准数据集与评估场景**：
  - 自建基准 **VoxParadox**（10 个副语言任务，2,000 样本）：评估模型在语言与声学冲突下的副语言判断准确率。
  - 现有基准 **MMSU 的副语言子集**：用于测试泛化能力。
- **对比模型与方法**：
  - 评估了一组多样化的 Audio LLMs（文中提到 Audio Flamingo 3 等），比较它们在对抗设置下的表现。
  - 消融实验中对比了有无 PCLM、有无 DPO 的配置。

### 4. 资源与算力
- 论文所提供的摘要及元数据中 **并未明确说明** 使用的 GPU 型号、数量、训练时长或推理算力消耗。此类细节可能需查阅原论文完整版或附录。

### 5. 实验数量与充分性
- **实验组数**：
  - 多个 Audio LLMs 在 VoxParadox 上的基准评估（10 个任务 × 约 2,000 样本）。
  - MMSU 副语言子集的跨基准测试。
  - 层次探查实验（逐层分析多个模型）。
  - PCLM 和 DPO 的消融研究（至少比较基础模型、+PCLM、+DPO、两者结合的变体）。
- **充分性与公平性**：
  - 任务覆盖较广，对抗构造严谨，探查实验能从模型内部解释失败原因，方法论自洽。
  - 通过统一合成语音生成流程消除其他变量干扰，评估客观。但对比方法仅限于自身模型变体，未广泛对比同期其他 Audio LLM 训练范式，这是常见的限制。

### 6. 论文的主要结论与发现
- Audio LLMs 在 VoxParadox 上的声学真实答案准确率整体很低，强烈倾向于跟随语言暗示的错误答案，证实其“阅读”而非“聆听”倾向。
- 副语言线索在编码器深层及编码器-LLM 接口处会显著退化；即便音频令牌中保留了线索，LLM 组件也经常忽略它们。
- 提出的 PCLM 通过多层自适应融合有效保留了副语言信息，结合 DPO 能大幅提升副语言理解能力：Audio Flamingo 3 在 VoxParadox 上从 17.40% 提升至 65.20%，在 MMSU 副语言子集上从 37.74% 提升至 54.78%。

### 7. 优点
- **问题洞察深刻**：首次系统地区分 Audio LLM 的“听”与“读”行为，用对抗式基准量化偏差。
- **基准设计巧妙**：通过可控冲突合成语音直接测量真正声学理解，避免文本先验作弊。
- **层次探查解释充分**：从表征角度揭示了副语言失败的内在机理，为方法设计提供依据。
- **轻量且有效的改进**：PCLM 仅需调整特征混合方式，DPO 聚焦偏好对齐，无需大规模重训，提升显著。

### 8. 不足与局限
- **合成语音的生态效度**：基准使用合成语音，可能和自然语音的副语言表现存在分布差异，模型改进在现实人声录音上的泛化性待进一步验证。
- **任务范围**：集中在 10 种副语言类别，尚未覆盖更复杂的交互性、多模态副语言维度（如视觉副语言）。
- **模型多样性有限**：主要改进在 Audio Flamingo 3 上进行，虽然基准评估了多个模型，但方法对其他架构（如非 Flamingo 类）的移植效果未报告。
- **算力信息缺失**：无法评估方法的计算成本与实用性，可能涉及额外的训练开销。
- **可解释性局限**：虽然探查了层效力，但未深入分析 PCLM 混合权重的语言学或声学意义，解释仍停留在性能层面。

（完）
