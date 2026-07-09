---
title: End-to-End Contrastive Language-Speech Pretraining Model for Long-Form Spoken Question Answering
title_zh: 面向长语音问答的端到端对比语言-语音预训练模型
authors: "Jiliang Hu, Zuchao Li, Baoyuan Qi, Guoming Liu, Ping Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40364/44325"
tags: ["query:speech-model"]
score: 8.0
evidence: 提出对比语言-语音检索器处理长音频，增强对话AI中的语音问答
tldr: 现有语音问答模型难以处理长音频，本文提出的CLSR采用端到端对比语言-语音预训练，通过中间步骤将声学特征转换为文本表示，有效检索长录音中的相关片段，从而辅助下游问答系统，实验证明在长语音问答任务上性能优于现有检索器。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音问答模型面对长音频时性能受限，检索器表现不佳。
method: 提出端到端对比语言-语音检索器，通过中间文本转换提取相关片段。
result: 在长语音问答任务上优于现有方法。
conclusion: 对比预训练检索器有效支撑了基于检索增强的语音问答。
---

## Abstract
Significant progress has been made in spoken question answering (SQA) in recent years. However, many existing methods, including large audio language models, struggle with processing long audio. Follow the success of retrieval augmented generation, a speech-related retriever shows promising in help preprocessing long-form speech. But the performance of existing speech-related retrievers is lacking. To address this challenge, we propose CLSR, an end-to-end contrastive language-speech retriever that efficiently extracts question-relevant segments from long audio recordings for downstream SQA task. Unlike conventional speech-text contrastive models, CLSR incorporates an intermediate step that converts acoustic features into text-like representations prior to alignment, thereby more effectively bridging the gap between modalities. Experimental results across four cross-modal retrieval datasets demonstrate that CLSR surpasses both end-to-end speech related retrievers and pipeline approaches combining speech recognition with text retrieval, providing a robust foundation for advancing practical long-form SQA applications.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：口语问答（SQA）中，输入上下文常为长录音（如会议、讲座，超过10分钟），现有方法（包括大音频语言模型）处理长语音时效果不佳。
- **核心思路**：受检索增强生成（RAG）在文本长上下文问答中的成功启发，提出使用语音检索器从长音频中提取与问题相关的短片段，再将精简后的上下文送入下游SQA模型，以提升准确率和推理速度。
- **现有痛点**：已有的语音相关检索器（如端到端对比模型或ASR+文本检索的级联模型）性能不足，难以有效弥合语音与文本的模态差异。

### 2. 提出的方法论
- **整体框架（CLSR）**：端到端对比语言-语音检索器。核心创新在于先将声学特征转化为“文本表示”，再与真实文本表示对齐，而非直接进行跨模态对齐。
- **关键模块**：
  - **语音编码器**：基于SAN‑M（自注意力+DFSMN）提取声学特征 \(H_s\)。
  - **连续积分发放（CIF）**：将帧级声学特征映射到令牌级，生成与令牌数对齐的声学表示 \(E_a\)。
  - **语音解码器+线性层**：预测令牌概率分布 \(D\)。
  - **VQ适配器**：通过两步将概率分布 \(D\) 转换为文本嵌入 \(E_{Y'}\)：
    1. 量化：对每个令牌分布取最大概率索引 \(q_i\)，用温度softmax和直通梯度估计保持梯度流。
    2. 映射：将量化后的one‑hot向量乘以文本编码器的嵌入矩阵（权重冻结），得到“文本表示”。
  - **文本编码器**：冻结的BGE‑base，输入文本问题嵌入和文本表示，取`[CLS]`输出作为句子级表示，用余弦相似度计算匹配分数。
  - **训练采样器**：在第二轮训练中，用真实转写文本的嵌入替换声学特征中错误令牌的嵌入（按采样比例 \(\lambda\)），增强ASR和对比学习效果。
- **损失函数**：总损失为 \(L_{total} = (1-\alpha-\beta)L_{ASR} + \alpha L_{MAE} + \beta L_{NLL}\)，其中 \(L_{ASR}\) 包含交叉熵和最小词错误率损失，\(L_{MAE}\) 用于CIF的收敛，\(L_{NLL}\) 是对称的对比学习负对数似然损失（问题-上下文与上下文-问题双向）。

### 3. 实验设计
- **数据集**：
  - Spoken‑SQuAD*：英文，文本问题‑语音上下文，从SQuAD多对一过滤为一对一（29,227训练/3,884验证）。
  - LibriSQA：英文，文本问题‑语音上下文，使用ChatGPT生成问题（104k训练/2,620验证）。
  - SLUE‑SQA‑5：英文，语音问题‑语音上下文，来自真实录音（46k训练/1,939验证/2,382测试）。
  - DRCD*：中文，语音问题‑语音上下文，原始文本数据集过滤后用TTS合成（25k训练/1,425验证）。
- **对比方法**：
  - 端到端（E2E）语音‑文本对比模型：CLAP、SpeechDPR。
  - 级联模型：Whisper（ASR）+BGE（文本检索）。
  - 纯文本上限：BGE在干净文本上的检索。
  - 消融对比：ParaBGE（Paraformer语音编码器+BGE文本编码器，无文本中间表示）。
- **评价指标**：ASR性能用词错误率（WER），检索性能用Top‑1/5/10召回率（问题→上下文、上下文→问题）。
- **长语音问答评估**：在修改的SLUE‑SQA‑5测试集（上下文替换为30分钟长度的Spoken Wikipedia文档）上，用Qwen‑Audio配合CLSR进行问答，比较准确率（精确匹配EM、宏观F1）和推理耗时。

### 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量或训练时长。仅提到使用FunASR工具包和ModelScope平台，模型参数规模为Paraformer 220M、BGE‑base 109M、Whisper 244M，优化器为Adam，学习率5e-5。未提供训练批次大小或持续时间，因此无法评估算力消耗。

### 5. 实验数量与充分性
- 共设计 **4组主体实验**（四个数据集上的检索对比，表2），**3组消融实验**（量化器、采样器、预训练策略，表3），**1组传统E2E对比实验**（ParaBGE vs CLSR，表4），**1组长语音SQA应用实验**（表5），以及WER‑检索性能关系分析（图6）。
- 实验覆盖英文和中文、文本‑语音和语音‑语音对齐场景，对比了多个强基线，消融实验完整展示了各模块贡献，并在真实应用场景下验证端到端效益。整体设计**充分、客观且公平**，能够支撑论文主要结论。

### 6. 主要结论与发现
- CLSR在所有数据集上的检索召回率显著优于CLAP和SpeechDPR等端到端语音检索器，也与级联模型Whisper+BGE相当甚至更优（特别是在WER更高时仍保持竞争力）。
- 通过文本表示桥接模态，使得无需大规模预训练，即可借助成熟文本对比模型获得强对齐能力。
- 在长语音SQA中，使用CLSR预处理可将下游模型的EM/F1分别提升约9.6/11.5个百分点，并使推理速度加快10倍。

### 7. 优点
- **新颖的模态桥接方式**：声学特征→文本表示→文本对比学习，有效缓解模态差异，且无需海量语音‑文本对预训练。
- **高效模块化设计**：CIF + VQ适配器与冻结的文本编码器结合，训练参数少，可复用已有强大的文本检索模型。
- **采样器机制**：利用真实文本嵌入纠正ASR错误，同时提升转录和检索性能。
- **多维度验证**：覆盖四个跨模态数据集、长语音真实应用、消融和上限对比，实验设计全面。
- 提供代码开源，便于复现。

### 8. 不足与局限
- **长语音评估数据合成性**：长语音SQA实验使用拼接的Spoken Wikipedia模拟，与真实会议、对话分布可能有差异，不能完全代表实际长录音场景。
- **依赖特定文本编码器**：实验使用冻结的BGE‑base，未探索此类模块的可微调性及跨语言适应能力，且级联模型在部分指标上差距微小，优势不绝对。
- **分割策略简单**：检索时将长音频切分为固定时长（40秒）片段，未讨论自适应分割对召回的影响。
- **中文数据真实性**：DRCD使用的是TTS合成语音，与自然口语分布存在偏差，可能被高估了跨模态检索能力。
- **资源信息缺失**：无训练时间、算力等报告，不利于成本评估和复现预期。
- **未测试极长音频**：最长测试仅30分钟，现实场景中可能存在小时级录音，分割策略和检索效率需进一步考察。

（完）
