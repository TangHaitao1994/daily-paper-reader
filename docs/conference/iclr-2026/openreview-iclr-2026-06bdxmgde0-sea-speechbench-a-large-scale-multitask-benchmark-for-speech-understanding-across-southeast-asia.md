---
title: "SEA-SpeechBench: A Large-Scale Multitask Benchmark for Speech Understanding Across Southeast Asia"
title_zh: SEA-SpeechBench：面向东南亚语音理解的大规模多任务基准
authors: "Jingyi Liao, Wenyu Zhang, Zhuohan Liu, Yingxu He, Geyu Lin, Xunlong Zou, Shuo Sun, Syed Ali Redha Alsagoff, Dacheng Tao, AiTi Aw"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=06bDxmgdE0"
tags: ["query:speech-model"]
score: 6.0
evidence: 东南亚多语言语音理解多任务基准，涵盖ASR、翻译、副语言学
tldr: 为解决语音评估英语中心化问题，SEA-SpeechBench构建了覆盖11种东南亚语言、9项任务的大规模多任务基准，包括语音识别、翻译、副语言分析等。该基准为多语言语音模型提供了标准化测试平台，有助于推动语音识别与理解技术在资源稀缺语言上的发展，对多语言语音大模型研究具有促进作用。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有语音评估基准以英语为主，东南亚语言严重缺乏。
method: "收集97,000+样本、597小时音频，涵盖9个任务，构建多语言基准。"
result: 提供了首个东南亚语言语音理解评测框架。
conclusion: 基准对推进多语言语音模型公平评测至关重要。
---

## Abstract
The rapid advancement of audio and multimodal large language models has unlocked transformative speech understanding capabilities, yet evaluation frameworks remain predominantly English-centric, leaving Southeast Asian (SEA) languages critically underrepresented. We introduce SEA-SpeechBench, the first large-scale multitask benchmark that evaluates speech understanding in 11 SEA languages through more than 97,000 samples and 597 hours of curated audio data. Our benchmark comprises 9 diverse tasks across 3 categories: speech processing (automatic speech recognition, speech translation, spoken question answering), paralinguistic analysis (emotion, gender, age, speaker recognition), and temporal understanding, a novel dimension featuring timestamped content queries and temporal localization within extended audio sequences up to 3 minutes. We implement multilingual prompting in both native SEA languages and English to reflect user interactions with audio-language models. 
Evaluation of leading open-source and proprietary systems reveals marked performance gaps. Across all models, performance remains underwhelming on temporal reasoning, emotion recognition, and speech translation, with most scores falling below 20. Prompting in low-resource languages such as Burmese, Lao, Tamil, and Khmer lag behind English by over 5%.
Our findings expose critical model limitations and underscore the need for inclusive model development.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义
- **核心问题**：现有多模态/语音大模型的评测体系严重偏向英语，东南亚（SEA）语言在语音理解评测中几乎完全缺失，导致难以衡量模型在该地区的真实能力。
- **整体含义**：本工作旨在打破“英语中心主义”的评测格局，构建首个大规模、多任务、覆盖11种东南亚语言的语音理解基准，为多语言语音模型提供公平、全面的测试平台，并揭示当前模型在低资源语言和复杂语音任务上的显著短板，从而推动更具包容性的模型研发。

## 2. 论文提出的方法论
- **核心思想**：构建一个多语言、多任务、多维度的语音理解评测基准 `SEA-SpeechBench`，从语音处理、副语言分析、时间理解三个层面全面考察模型对东南亚语言的语音理解能力。
- **关键技术细节**（基于摘要及元信息）：
  - **规模与语种**：超过97,000个样本，约597小时精心整理的音频数据，覆盖11种东南亚语言。
  - **任务设计**：包含9项任务，划分为3个类别：
    - **语音处理**：自动语音识别（ASR）、语音翻译、口语问答。
    - **副语言分析**：情感识别、性别识别、年龄识别、说话人识别。
    - **时间理解**（新颖维度）：要求在最长3分钟的音频序列中进行带时间戳的内容查询和时序定位。
  - **提示设计**：同时使用东南亚本地语言和英语进行多语言提示，以模拟用户与音频‑语言模型的真实交互场景。
- **算法/公式**：本工作属于基准构建，不涉及新模型或算法公式，主要贡献在评测框架与数据集。

## 3. 实验设计
- **数据集**：自建的 `SEA-SpeechBench`，11种语言，597小时音频，97k+样本。
- **评测场景/任务**：ASR、语音翻译、口语问答、情感/性别/年龄/说话人识别、时序定位与内容查询等9项任务。
- **对比方法**：评估了当前领先的开源和商业/专有语音/多模态大模型（摘要未列出具体模型名称）。基准本身提供了标准化评测协议，以便其他研究者复现和对比。

## 4. 资源与算力
- **文中未明确说明**构建基准或进行模型评测所使用的算力细节（如GPU型号、数量、训练/推理时长等）。摘要及提供的元数据中均未提及相关资源消耗。

## 5. 实验数量与充分性
- **实验数量**：由于仅有摘要，无法精确获取实验组数。但从描述推断，至少评估了若干开源与专有系统，并在各任务、各语言、不同提示语言（本地语 vs 英语）上进行了交叉对比，应包含数十项评测结果。
- **充分性/客观性**：
  - 任务覆盖语音到语义、副语言、时序三个维度，种类较为完备，且首次引入时间理解，实验设计具有层次性。
  - 评测采用统一基准和指标，能够公平对比不同模型。但由于未列出具体模型信息和敏感的数据分布细节，客观性一定程度上依赖后续公开的完整论文和数据集说明。
  - 摘要提及分数普遍低于20、低资源语言差距超过5%等定量结果，证明实验能够有效暴露模型弱点，具有一定的诊断力。

## 6. 论文的主要结论与发现
- **模型整体表现不足**：所有参评模型在时间推理、情感识别、语音翻译等任务上表现严重欠佳，多数任务得分低于20。
- **低资源语言显著落后**：使用缅甸语、老挝语、泰米尔语、高棉语等低资源语言进行提示时，性能比英语条件下低5%以上，反映出当前模型在多语公平性上的缺陷。
- **紧迫性与方向**：基准清晰地揭示了当前语音模型在东南亚语言上的关键限制，强调发展更包容、更均衡的多语言语音理解能力的必要性。

## 7. 优点
- **填补空白**：首个面向东南亚语言的大规模、多任务语音理解基准，打破了评测的英语垄断。
- **任务维度新颖**：引入最长3分钟的时序定位与时间戳内容查询，推动语音模型向细粒度、长时间上下文理解发展。
- **场景真实**：采用本地语言与英语双语提示，贴近实际应用，能更好地检验模型的跨语言泛化能力。
- **系统性强**：一次覆盖11种语言、9项任务，为后续研究提供了丰富的对比基准。

## 8. 不足与局限
- **细节缺失**：由于仅依据摘要和元信息，无法评估基准的构建质量（如数据来源、标注一致性、说话人多样性等），可能隐藏偏差风险。
- **模型覆盖不明**：未列出所评测的具体模型与版本，难以判断实验的全面性和结论的普适性。
- **算力与效率未报告**：未提供基准构建及评测所需资源，对资源受限的研究团队复现或扩展可能形成障碍。
- **跨域/复杂场景有限**：当前任务虽已多样，但仍局限于较封闭的预设任务，未涉及复杂对话、多轮交互或真实环境噪声等挑战，应用场景的代表性可能受限。
- **语言覆盖仍存缺口**：东南亚语言众多且方言复杂，11种语言虽已显著扩展，但尚未完全覆盖该地区所有主要语系。

（完）
