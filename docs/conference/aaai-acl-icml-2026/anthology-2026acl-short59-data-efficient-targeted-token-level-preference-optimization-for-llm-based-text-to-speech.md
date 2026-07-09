---
title: Data-efficient Targeted Token-level Preference Optimization for LLM-based Text-to-Speech
title_zh: 基于LLM的文本转语音中数据高效的定向token级偏好优化
authors: "Rikuto Kotoge, Yuichi Sasaki"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-short.59.pdf"
tags: ["query:speech-model"]
score: 10.0
evidence: token级偏好优化提升LLM文本转语音质量
tldr: 现有基于LLM的TTS模型偏好优化依赖句级成对数据，且无法实现细粒度对齐。该论文提出TKTO，一个无需成对数据的token级偏好优化方法，自动提供细粒度发音对齐信号。在数据有限条件下，该方法显著提升了合成语音的鲁棒性和自然度，为高效TTS对齐训练开辟了新路径。
source: ACL-2026-Short
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 461, \"height\": 245, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1596, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 793, \"height\": 427, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 775, \"height\": 316, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 803, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-short/anthology-2026.acl-short.59/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 774, \"height\": 211, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-short/anthology-2026.acl-short.59/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1413, \"height\": 643, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-short/anthology-2026.acl-short.59/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 529, \"height\": 131, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-short/anthology-2026.acl-short.59/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 805, \"height\": 373, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-short/anthology-2026.acl-short.59/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 794, \"height\": 236, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-short/anthology-2026.acl-short.59/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 757, \"height\": 274, \"label\": \"Table\"}]"
motivation: LLM-TTS偏好优化受限于配对数据稀缺和缺乏token级对齐。
method: 设计无需配对样本的token级偏好优化，自动生成细粒度对齐信号。
result: 在数据高效设置下，提升了TTS输出的鲁棒性和自然度。
conclusion: TKTO实现了更精细的语音合成对齐，降低了数据标注成本。
---

## Abstract
Aligning text-to-speech (TTS) system outputs with human feedback through preference optimization has been shown to effectively improve the robustness and naturalness of LLM-based TTS models. Current approaches primarily require paired desirable and undesirable samples at the utterance level. However, such pairs are often limited in TTS output data, and utterance-level formulation prevents fine-grained token-level optimization needed for accurate pronunciation alignment. In this study, we propose TKTO that eliminates the need for paired data, enabling a more data-efficient training paradigm, and directly targets token-level units, automatically providing fine-grained alignment signals without token-level annotations. TKTO improves the challenging Japanese TTS accuracy by 39% and reduces CER by 54%, leveraging 6× more training data and assigning 12.8× stronger reward to targeted tokens.

---

## 论文详细总结（自动生成）

# 论文详细总结：Data-efficient Targeted Token-level Preference Optimization for LLM-based Text-to-Speech

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **问题背景**：大型语言模型（LLM）已成功应用于文本转语音（TTS），可以直接从原始文本生成语音，而不依赖传统音素转换器（G2P）。然而，在日语等歧义性强的语言中，同一个字符（如“辛い”）的读音随上下文变化，模型容易出错。
- **现有缺陷**：
  - **必须使用配对数据**：当前主流的偏好优化方法（如DPO）需要在同一输入下同时拥有“好的”与“坏的”合成样本，这类样本在TTS中稀少，因为模型往往对某个输入只产生单一倾向的输出，导致大量反馈数据无法使用，数据效率低下。
  - **只能在样本（句子）级别优化**：发音正确性本质上是字符/令牌级别的任务，而现有的偏好对齐只在整句级别给予信号，无法细粒度地针对出错的令牌进行反馈，优化粗糙。
- **研究目标**：提出一种无需配对数据、且能在令牌级别进行偏好优化的方法，以更高效地利用人类反馈，提升TTS系统的鲁棒性和发音准确率。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：设计一种两阶段框架TKTO（Token-level Kahneman-Tversky Optimization），第一步从非配对数据中估算每个令牌对偏好贡献的重要度权重，第二步将这些权重用于令牌级的KTO优化。
- **步骤一：基于对比LLM的令牌重要性估计**
  - 利用KTO（Kahneman-Tversky Optimization）训练两个对比性的LLM：$\pi^+$（用原始好/坏标签训练）和$\pi^-$（交换标签训练）。
  - 计算每个令牌$y_t$的奖励值：$\log \frac{\pi^+(y_t|x,y_{<t})}{\pi^-(y_t|x,y_{<t})}$，并通过缩放因子$\mu$（对好样本$\mu>0$，对坏样本$\mu<0$）和对数比值的钳位（clamp到$[L,U]$）得到令牌权重$w_t$。该权重自动突出了对偏好判别关键的令牌（如歧义字的令牌）。
- **步骤二：令牌级KTO（TKTO）优化**
  - 定义令牌级奖励：$r_{\theta, t} = \log \frac{\pi_\theta(y_t|x,y_{<t})}{\pi_{\text{ref}}(y_t|x,y_{<t})}$，以及参考基准$z_{0,t} = \text{KL}(\pi_\theta \parallel \pi_{\text{ref}})$。
  - 为每个令牌构建价值函数$v_t$：对于好样本，$v_t = \lambda_D \sigma(\beta (r_{\theta, t} - z_{0,t}))$；对于坏样本，$v_t = \lambda_U \sigma(\beta (z_{0,t} - r_{\theta, t}))$。
  - 最终损失函数：$\mathcal{L}_{\text{TKTO}} = \mathbb{E} \left[ -\sum_{t} w_t \cdot v_t \right]$，通过权重$w_t$让优化聚焦于关键令牌。

## 3. 实验设计
- **数据集**：
  - 文本数据：使用GPT-5生成5000句包含“辛い”的日语歧义句（一半读作“karai”，一半读作“tsurai”）。
  - 语音数据：男女各一名日语说话人，每人约23小时数据，用TTS模型为每句生成5个样本；选取发音正确且字错率（CER）最低为“好样本”，发音错误且CER最高为“坏样本”。非配对数据来自单侧输出（仅好或仅坏）。
  - 评估用ASR模型：主要是whisper-v3-large，也补充使用了parakeet-tdt_ctc-0.6b-ja以验证鲁棒性。
- **Benchmark与对比方法**：
  - **基线TTS模型**：CosyVoice2 (0.5B) 作为底座，在其上微调进行偏好优化。
  - **偏好优化方法**：SFT（仅用好样本微调）、DPO（配对数据）、KTO（配对与非配对两种配置）、本文的TKTO（配对与非配对）。
  - **外部TTS系统**：gpt-4o-mini-tts、gemini-2.5-flash-preview-tts、gemini-2.5-pro-preview-tts、F5-TTS（流匹配，带或不带G2P）。
- **评价指标**：
  - 客观指标：发音准确率（Acc）、字符错误率（CER）、坏例比率（CER>0.3的比例）。
  - 主观指标：自然度平均意见分（NMOS）、ABX偏好测试。

## 4. 资源与算力
- 训练两个对比LLM额外耗时约10分钟（8张A100 GPU）。
- 主模型偏好优化训练仅需数分钟（8×A100），未提供详细时长，但强调相对于大规模预训练（数周）可忽略。
- 其他超参数：学习率1e-6，训练1个epoch，$\lambda_D=\lambda_U=1$，$\beta=0.10$，$\mu=\pm1$，钳位范围经调优选为$[-2,2]$。

## 5. 实验数量与充分性
- **主要实验**：
  - 日语男女声综合测评（表1）：包含工业模型、基线、多种PO方法、配对/非配对设置。
  - 训练动态分析（图3）：展示TKTO与SFT在好/坏令牌对数似然上的变化。
  - 令牌奖励与权重可视化（图4、图5）：验证定向令牌获得更高奖励和权重。
  - 主观评测：NMOS（表2）与ABX测试（图6）。
  - 中文扩展实验（表3）：验证方法在中文多音字“行”上的泛化能力。
  - 钳位范围敏感度分析（表4）。
  - ASR后端鲁棒性验证（表5）。
- **实验充分性与公平性**：
  - 对比全面，包含了无偏好优化的基线、数据配对的DPO、不配对的KTO，以及工业级TTS。
  - 客观指标结合主观评价，减少单一指标偏差。
  - 钳位范围、ASR后端的消融实验增加了结果可信度。
  - 不足之处：仅在0.5B参数的CosyVoice2上实验，未探究模型规模扩展性；日文和中文各仅测试一个歧义字，领域局限。

## 6. 论文的主要结论与发现
- **性能提升显著**：TKTO将日语TTS准确率从基线的66.8%提升至95.8%，CER降低54%，坏例率大幅下降，超越所有对比模型，包括工业界的gpt-4o-mini-tts。
- **数据效率突破**：通过利用非配对数据，训练数据量达到配对DPO的6倍，解决了配对样本稀缺的瓶颈。
- **细粒度优化有效**：12.8倍更强的奖励被自动分配到目标歧义令牌上，TKTO只提升好令牌的概率，而SFT会同时提高坏令牌的概率。
- **跨语言泛化**：在中文多音字任务上同样取得最佳准确率和最低CER。
- **主观质量领先**：NMOS和ABX测试均表明TKTO合成语音更自然。

## 7. 优点
- **方法创新**：首创无需配对数据、令牌级的偏好优化，解决了TTS领域细粒度发音对齐难题，理论框架基于前景理论，设计合理。
- **自动令牌重要性发现**：利用对比LLM自动估算令牌权重，无需额外人工标注令牌级优劣。
- **实验设计严谨**：多维度评估（客观+主观）、多语种验证（日/中）、多ASR后端鲁棒性测试，消融充分。
- **高效实用**：额外计算开销小（约10分钟A100训练），可灵活嵌入现有流程。

## 8. 不足与局限
- **规模验证不足**：仅在0.5B参数模型上实验，未探究模型规模缩放带来的影响，限制了推广到更大模型的结论可信度。
- **任务覆盖面窄**：仅聚焦于一个日语歧义字和中文一个多音字，其他类型的发音问题（如外来语、数字等）未涉及，方法在其他粒度（如韵律、情感）的适用性未知。
- **线下策略（off-policy）限制**：目前方法基于已有的偏好数据，不能在线交互更新，若反馈分布变化可能需要重新收集数据。
- **对比LLM的质量依赖**：令牌权重来源于两个对比LLM的差异，若这两个模型本身训练不充分或偏差大，权重可能失真。
- **主观评测规模较小**：NMOS和ABX测试由3名日语母语者进行，样本量和评测人数有限，可能存在个体偏差。
- **环境预设单一**：仅使用特定TTS模型底座，未在其他架构上验证，通用性待加强。

（完）
