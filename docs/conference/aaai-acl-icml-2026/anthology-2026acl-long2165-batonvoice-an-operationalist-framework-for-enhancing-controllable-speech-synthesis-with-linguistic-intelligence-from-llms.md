---
title: "BatonVoice: An Operationalist Framework for Enhancing Controllable Speech Synthesis with Linguistic Intelligence from LLMs"
title_zh: BatonVoice：一种利用大语言模型语言学智能增强可控语音合成的操作主义框架
authors: "Yue Wang, Ruotian Ma, Xingyu Chen, Zhengliang Shi, Morunliu Yang, Wanshun Chen, Huang Liu, Jiadi Yao, Xin He, Qu Yang, Qingxuan Jiang, Fanghua Ye, Juntao Li, Zhaopeng Tu, Xiaolong Li, Liefeng Bo, Min Zhang"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.2165.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 将大语言模型用作指挥者实现可控文本到语音合成
tldr: 现有大语言模型（LLM）集成的语音合成方法未能充分挖掘LLM的指令遵循能力，限制了文本到语音（TTS）的可控性。受操作主义启发，本文提出BatonVoice框架，将指令理解与语音生成解耦，让LLM扮演指挥角色，理解用户指令并规划显式声学特征。这种解耦设计提升了可控语音合成的准确性和灵活性。实验表明，BatonVoice能执行复杂的文本指令生成高质量语音，拓展了LLM在语音合成中的应用前景，为交互式语音界面提供了有力的技术支撑。该工作展示了如何有效利用LLM的语言智能来改进语音生成质量。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2165/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1499, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2165/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 766, \"height\": 375, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2165/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 797, \"height\": 558, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.2165/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 791, \"height\": 795, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1646, \"height\": 695, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 754, \"height\": 720, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 796, \"height\": 389, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 666, \"height\": 533, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 556, \"height\": 219, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 794, \"height\": 298, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.2165/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 566, \"height\": 219, \"label\": \"Table\"}]"
motivation: 现有语音合成方法未能充分利用LLM的指令遵循能力，限制了文本指令控制TTS的灵活性。
method: 提出操作主义新范式，解耦指令理解与语音生成，LLM作为指挥理解用户指令并生成文本计划——显式声学特征。
result: BatonVoice实现了高效的可控语音合成，能够根据复杂文本指令生成符合要求的语音。
conclusion: 该框架为TTS系统提供了更强的可控性和通用性，推动了LLM与语音生成的有效融合。
---

## Abstract
The rise of Large Language Models (LLMs) is reshaping multimodel models, with speech synthesis being a prominent application. However, existing approaches often underutilize the linguistic intelligence of these models, typically failing to leverage their powerful instruction-following capabilities. This limitation hinders the model’s ability to follow text instructions for controllable Text-to-Speech (TTS). To address this, we propose a new paradigm inspired by operationalism that decouples instruction understanding from speech generation. We introduce BatonVoice, a framework where an LLM acts as a conductor, understanding user instructions and generating a textual plan – explicit vocal features (e.g., pitch, energy). A separate TTS model, the orchestra, then generates the speech from these features. To realize this component, we develop BatonTTS, a TTS model trained specifically for this task. Our experiments demonstrate that BatonVoice achieves strong performance in controllable and emotional speech synthesis, outperforming strong open- and closed-source baselines. Notably, our approach enables remarkable zero-shot cross-lingual generalization, accurately applying feature control abilities to languages unseen during post-training. This demonstrates that objectifying speech into textual vocal features can more effectively unlock the linguistic intelligence of LLMs.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的结构化、深入、客观的中文总结。

### **1. 论文的核心问题与整体含义**

- **研究动机**： 现有基于大语言模型（LLM）的文本到语音合成系统通常将LLM作为骨架模型（Backbone），但其强大的**指令理解与执行能力**并未得到充分利用。若要实现可控TTS（如情感表达），仍需依赖昂贵且难以标注的大规模指令-语音配对数据，这限制了模型的灵活性与泛化能力。
- **核心问题**： 如何更有效地解锁LLM的语言智能，使其能根据文本指令实现对语音的精细化控制。
- **整体含义**： 本文提出了一种受“操作主义”启发的新范式，核心思想是将复杂的指令理解与语音生成过程解耦。通过将语音“操作化”为可量化的文本声学特征（如音高、能量），LLM仅需作为“指挥”生成这些特征，而由专用的TTS模型（“乐团”）负责合成最终语音。该方法旨在不依赖人工标注指令数据的前提下，显著提升可控语音合成的性能和泛化能力。

### **2. 论文提出的方法论**

- **核心思想：操作主义范式**
    - **解耦设计**： 将TTS系统分为“指挥”（LLM）和“乐团”（BatonTTS）两部分。
    - `指挥`： 一个外部LLM（如Gemini 2.5 Pro），负责理解用户的开放式文本指令，并将其转化为一个结构化的文本“计划”，即包含时间粒度的显式声学特征集合（Fv），例如每个词或片段的音高均值/斜率、能量RMS/斜率、频谱质心等。
    - `乐团`： 一个专门训练的TTS模型（BatonTTS），接收原始文本（X）和声学特征计划（Fv），合成符合要求的语音。

- **关键技术细节：BatonTTS模型与训练**
    - **模型架构**： 采用CosyVoice2的框架，由**LLM骨干网络**（如Qwen3-1.7B）和固定的**预训练语音解码器**（流匹配模型 + HiFi-GAN声码器）组成。LLM负责将文本和声学特征自回归地转换为离散语音令牌。
    - **三阶段训练流程**：
        1.  **预训练**： 在大型语料库上训练LLM，使其具备基础的文本到语音令牌生成能力。
        2.  **监督微调**： 训练模型根据文本和显式声学特征（Fv）生成语音令牌。通过特征提取工具从解码器重建的语音中获取Fv，将其文本化后与语音令牌进行序列拼接，进行下一个令牌的预测。
        3.  **偏好优化**： 构建偏好数据集，让模型学习偏好根据特征计划生成的、高清晰度（低WER）和正常语速的语音（`Sw`），而拒绝未按计划生成的或质量较低的语音（`Sl`）。使用APO-down算法进行微调，以优化控制精度和输出质量。

### **3. 实验设计**

- **数据集与场景**：
    - **TTS清晰度评测**： 使用 **Seed-TTS** 基准测试集，指标为**词错误率**。
    - **情感控制评测**： 基于 **Emotion** 数据集构建测试集，包含喜悦、悲伤、愤怒等5种情感，各100条样本。指标为**情感分类准确率**。
    - **自由形式指令遵循**： 基于 **Social IQa** 构建50个复杂情境与指令的测试集，进行**人工评估**。
    - **跨语言泛化评测**： 使用**中文情感基准测试**，评估模型在未见语言上的零样本迁移能力。
    - **特征控制能力评测**： 使用 **RAVDESS** 数据集，通过声音重建进行控制，指标为**梅尔倒谱失真**和**基频F0均方根误差**。

- **对比方法**：
    - **闭源系统**： Minimax-2.5-HD, Minimax-2.5-Turbo, Minimax-2.0-HD
    - **开源系统**： Spark-TTS, CosyVoice, CosyVoice2, Higgs speech V2

### **4. 资源与算力**

- **算力配置**： 论文明确提及，预训练阶段使用了 **80个GPU**，训练了约 **1天**（历时3个epoch）。
- **软件优化**： 使用了DeepSpeed ZeRO-2进行内存优化，训练时采用序列打包技术提升吞吐量。
- **模型规模**： 实验覆盖了 **0.5B** 和 **1.7B** 两种参数规模的LLM骨干网络。

### **5. 实验数量与充分性**

- **实验数量**： 论文设计了较为全面的实验组合，包括：
    - **主实验**：在英文和中文两种语言的TTS基准上进行性能对比。
    - **组件分析（消融实验）**：验证三阶段训练流程中每个阶段的贡献；探究外部LLM指挥模型能力对最终效果的影响（从1.7B到Gemini Pro）。
    - **特征控制能力实验**：通过剔除单个特征（音高、能量等）进行消融研究，并与定性描述进行对比。
    - **人工评估**：针对自由形式指令跟随进行人工偏好评估。
    - **鲁棒性分析**：使用两种不同的语音情感识别模型交叉验证情感准确率。
- **充分性与公平性**：
    - **充分性**： 实验设计从多角度（清晰度、情感、指令跟随、跨语言、特征控制）验证了方法的有效性，逻辑链条清晰。
    - **公平性**： 对比了多个主流的开源和闭源系统，模型参数规模相近，具有可比性。需要指出的是，在自由形式指令测试中，基线模型实际接收的是简化后的情感标签，这在一定程度上对BatonVoice更为有利，但也正体现了其处理复杂指令的独特优势。论文对此进行了说明，保持了客观性。

### **6. 主要结论与发现**

- **性能卓越**： BatonVoice在情感语音合成上显著超越所有对比的开源和闭源模型。1.7B模型在英文情感基准上达到**57.6%** 的准确率，比最强闭源模型Minimax-2.5-HD高9个百分点。
- **范式有效**： 解耦设计和三阶段训练极其有效。监督微调阶段带来了最大的性能飞跃（准确率从23.2%提升至52.2%），偏好优化进一步提升了效果。
- **解锁LLM能力**： 框架能无损地利用更强大的LLM。将“指挥”模型从Qwen3-1.7B升级为Gemini 2.5 Pro，情感准确率可从29.8%直接提升至**57.6%**，证明了模块化设计的可扩展性。
- **强大的泛化能力**： 模型展现了惊人的**零样本跨语言泛化**能力，仅在英文数据上训练特征控制，就能在中文情感测试上达到56.2%的准确率。
- **精准控制**： 与定性描述相比，量化的数值特征表示能提供更精准的语音控制（更低的MCD和F0 RMSE）。

### **7. 优点**

- **范式创新**： 提出的“操作主义”解耦范式为解决可控多模态生成问题提供了新颖且有效的思路。
- **无需人工标注**： 完全摆脱了对昂贵指令-语音配对数据的依赖，大大降低了开发成本并提升了可扩展性。
- **模块化与可扩展性**： “指挥-乐团”的解耦设计允许独立升级LLM或TTS模型，系统能力可随基座模型的发展而水涨船高。
- **泛化能力强**： 展示了强大的跨语言、跨指令类型的泛化能力，突破了对特定标注数据的限制。
- **实验扎实全面**： 实验设计严谨，覆盖多语言、多基线、多维度指标，并通过消融和鲁棒性分析充分验证了方法的有效性。

### **8. 不足与局限**

- **推理延迟**： “指挥”LLM的加入引入了额外的推理步骤（文中提到生成JSON计划约需20秒），相比端到端系统，两级流水线带来了更高的延迟，不适用于对实时性要求极高的场景。
- **语言覆盖单一**： 目前的训练和评测主要限于英语，虽然在中文上展示了零样本能力，但缺乏更广泛、形态更丰富语言上的充分验证。
- **潜在安全风险**： 论文承认，基于大规模网络数据训练的模型可能生成带有偏见、辱骂性或有害的语音内容，在无安全过滤机制的真实应用中存在滥用风险。
- **训练成本**： 尽管避免了数据标注，但多阶段的大规模模型训练仍需要可观的计算资源，对资源有限的研究者构成一定门槛。

（完）
