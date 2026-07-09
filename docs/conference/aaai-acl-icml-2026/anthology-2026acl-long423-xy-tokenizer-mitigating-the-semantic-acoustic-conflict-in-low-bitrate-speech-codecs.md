---
title: "XY-Tokenizer: Mitigating the Semantic-Acoustic Conflict in Low-Bitrate Speech Codecs"
title_zh: XY分词器：缓解低码率语音编解码中的语义-声学冲突
authors: "Yitian Gong, Luozhijie Jin, Kuangwei Chen, Dong Zhang, Ruifan Deng, Xiaogui Yang, Xin Zhang, Zhaoye Fei, Qinyuan Cheng, Shimin Li, Xipeng Qiu (邱锡鹏)"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.423.pdf"
tags: ["query:speech-model"]
score: 7.0
evidence: 低码率语音编解码器将离散语音与文本对齐，用于语音语言模型预处理
tldr: 针对低码率语音编解码中声学保真与语义捕获的冲突，本文提出XY-Tokenizer。该编解码器在约1kbps码率下采用结构化多阶段多任务训练，将离散语音表示与文本对齐，同时保留精细声学细节，有效缓解了语义-声学冲突。实验表明它在语义对齐和重建质量上优于现有低码率方法，为语音大模型提供了更高效的前端分词方案。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.423/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 815, \"height\": 582, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.423/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1315, \"height\": 988, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.423/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1568, \"height\": 532, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.423/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1568, \"height\": 591, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.423/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1574, \"height\": 498, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 731, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1596, \"height\": 1136, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 750, \"height\": 500, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 759, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1071, \"height\": 232, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1211, \"height\": 378, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 813, \"height\": 233, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 603, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 745, \"height\": 539, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 715, \"height\": 200, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 653, \"height\": 245, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 770, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1627, \"height\": 479, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 750, \"height\": 256, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.423/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 781, \"height\": 233, \"label\": \"Table\"}]"
motivation: 现有低码率语音编解码器难以同时保留声学细节和丰富语义信息，限制了其在大语言模型中的应用。
method: 提出多阶段多任务训练的低码率语音编解码器XY-Tokenizer，在约1kbps码率下实现语音离散表示与文本对齐，同时保留精细声学特征。
result: 实验显示XY-Tokenizer在语义对齐和声学重建上均优于现有低码率编解码器。
conclusion: 该分词器为语音语言模型提供了更平衡的声学-语义接口，推动了低码率语音处理发展。
---

## Abstract
Speech codecs provide an important interface between continuous speech signals and large language models. An ideal codec for speech language models should not only preserve acoustic information but also capture rich semantic information. However, existing codecs struggle to balance these objectives at low bitrates. We propose XY-Tokenizer , a low-bitrate speech codec (around 1 kbps) trained with a structured multi-stage, multi-task strategy that aligns discrete speech representations with text while preserving fine-grained acoustic details for reconstruction. This design explicitly mitigates the semantic–acoustic conflict observed in prior low-bitrate codecs. Experiments show that XY-Tokenizer achieves stronger semantic alignment than representative semantic-distillation codecs such as SpeechTokenizer and Mimi, while maintaining high-quality speech reconstruction across both clean and out-of-distribution conditions. Furthermore, XY-Tokenizer consistently outperforms existing low-bitrate codecs in LLM-based speech understanding and generation tasks, demonstrating its effectiveness as a general-purpose speech representation for speech–language modeling.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义

- **研究背景**：语音编解码器是连接连续语音信号与大语言模型的关键接口，需要同时保留声学信息与语义信息。
- **核心矛盾**：在低码率条件下，声学重建质量与语义对齐性之间存在固有冲突，现有方法难以在约1 kbps码率下同时取得高性能。
- **整体目标**：设计一种低码率语音编解码器，显式缓解语义-声学冲突，为语音大模型提供语义丰富且保真度高的通用语音表示。

### 2. 方法论

- **核心思想**：通过**双塔架构 + 多阶段多任务训练**，最小化语义建模与声学建模之间的参数共享，从而缓解冲突。
- **架构设计**：
  - **编码器**：分为语义通道（冻结的Whisper-small编码器）和声学通道（可训练编码器），两者输出拼接后送入残差矢量量化模块。
  - **量化器**：8层RVQ，码本大小1024，4倍下采样至12.5 Hz，比特率约1 kbps。
  - **解码器**：语义分支采用基于Qwen2.5-0.5B的LLM进行文本转录，声学分支通过对称解码器和Vocos声码器重建波形。
- **两阶段训练**：
  1. **预训练阶段**（X形结构）：同时优化重建损失（多尺度梅尔谱L1）、ASR交叉熵损失和量化提交损失，对齐文本并捕获粗粒度声学特征。
  2. **后训练阶段**（Y形结构）：冻结编码器和量化器，仅用GAN（MPD/MSD/MS-STFTD）和重建损失优化解码器，提升细粒度音频质量。
- **关键公式**：
  - 预训练总损失：  
    \[
    \mathcal{L}_{\text{pre}} = \lambda_{\text{asr}} \mathcal{L}_{\text{asr}} + \lambda_{\text{rec}} \mathcal{L}_{\text{rec}} + \lambda_{\text{cmt}} \mathcal{L}_{\text{cmt}}
    \]
  - 后训练生成器损失：  
    \[
    \mathcal{L}_G = \lambda_{\text{rec}} \mathcal{L}_{\text{rec}} + \lambda_{\text{feat}} \mathcal{L}_{\text{feat}} + \lambda_{\text{adv}} \mathcal{L}_{\text{adv}}
    \]
    其中 \(\mathcal{L}_{\text{feat}}\) 为特征匹配损失，\(\mathcal{L}_{\text{adv}}\) 采用LSGAN。

### 3. 实验设计

- **数据集**：
  - 训练：Emilia数据集（约10.1万小时，3700万对音频-文本）。
  - 评估：
    - 英文：LibriSpeech test-clean、VoxPopuli-EN dev-clean。
    - 中文：AISHELL-2 dev_ios、Common Voice ZH test。
- **基准对比**：
  - 语义-声学联合编解码器：SpeechTokenizer、Semanticodec、Mimi、XCodec2.0。
  - 纯声学编解码器：EnCodec、DAC、BigCodec、WavTokenizer。
- **评价指标**：
  - 重建质量：SIM、STOI、PESQ（窄带/宽带）、MUSHRA主观评分。
  - 语义对齐：ASR探测任务中的WER/CER。
  - 下游任务：LLM语音理解（WER/CER）与零样本TTS生成（WER、SIM）。

### 4. 资源与算力

- **预训练**：32块 NVIDIA H100 GPU，每卡batch size 4，累计训练80万步，采用DeepSpeed ZeRO-2。
- **后训练**：1块 NVIDIA H100 GPU，batch size 16，生成器与判别器分别以 \(1\times10^{-5}\)、\(1\times10^{-4}\) 学习率训练。
- **基础配置**：模型参数量约246M（基础版）至2185M（大版本），训练数据统一重采样至16 kHz。
- **注**：文中未显式提供总训练时长（如天数或小时），仅给出步数和硬件数量。

### 5. 实验数量与充分性

- **实验组数**：
  - 主评估表格2（4个测试集×9种编解码器，约36组对比）。
  - 语音理解任务（3个英文+3个中文测试集，8种编解码器）。
  - 语音生成任务（中英双语，与Mimi、Llasa、Spark-TTS等对比）。
  - 消融实验：参数共享方式（3种）、LLM可训练性（2种×3步数）、模型规模（3种）、ASR监督组合（3种）、解码器选择（3种）、双阶段必要性等，共计约25组消融。
- **充分性**：实验覆盖主要低码率基线、多语言、多场景，消融针对核心设计，对比公平。
- **客观性**：指标标准化，使用公开数据集，部分采用主观MUSHRA，训练配置统一。

### 6. 主要结论与发现

- XY-Tokenizer在约1 kbps下实现了语义对齐（ASR探测WER 0.13）与声学重建（PESQ 3.10/2.50）的强平衡，优于现有低码率编解码器。
- 双塔分离语义与声学信道、冻结LLM强化编码器语义监督、模型容量扩展能有效缓解冲突。
- 显式ASR监督比教师蒸馏能更有效地将语音表示对齐至文本。
- 在LLM语音理解和自回归生成任务中一致领先，验证了其作为通用语音表示的潜力。

### 7. 优点

- **创新冲突缓解机制**：通过最小化共享参数，明确分离语义与声学建模路径。
- **高效训练策略**：冻结关键组件（语义编码器、LLM）稳定文本对齐，再引入GAN优化音质，避免低码率GAN训练不稳定。
- **翔实的分析**：通过可视化、编码器级分析、消融实验，系统验证了设计动机。
- **全面评估**：涵盖中英双语、干净与真实场景、重建指标、语义探测和下游任务，对比充分。
- **低码率实用性**：1 kbps适合资源受限的语音交互场景。

### 8. 不足与局限

- **训练成本与复杂性**：两阶段训练流程和较大模型参数量增加了计算开销，文中未明确单次训练的端到端耗时。
- **数据集限制**：仅使用Emilia数据训练，未测试在极低资源或重口音场景下的泛化能力。
- **语义评价单一**：语义对齐仅依赖ASR WER，未分析说话人不变性、韵律信息等更细粒度语义保留情况。
- **生成评估局限**：TTS实验仅对比Mimi-8等少数系统，且未报道生成语音的自然度主观评价（如MOS）。
- **模型透明度**：部分细节如后训练阶段的具体训练步数未披露，可能影响可复现性。
- **应用偏见风险**：文中指出代码本身不直接涉及伦理问题，但下游应用可能继承训练数据中的偏差。

（完）
