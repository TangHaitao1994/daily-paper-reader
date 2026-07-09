---
title: "LCMA-SRT: Language-Conditional Mixture-of-Experts Adapters for Joint Multilingual Speech Recognition and Translation"
title_zh: LCMA-SRT：面向联合多语言语音识别与翻译的语言条件混合专家适配器
authors: "Nanjie Li, Xiaoyong Guo, Hao Huang, Xu Haihua, Wei Shi"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.1634.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 提出基于适配器的方法改进多语言语音识别与翻译，减少负迁移
tldr: 分层换能器在多语言多对多语音识别与翻译中面临负迁移和不稳定生成问题。本文提出LCMA-SRT，引入语言条件混合专家适配器，使模型能够动态调整参数处理不同语言方向，在维持参数效率的同时有效缓解负迁移，在多个语种对上显著提升了ASR和ST的性能。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1634/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1125, \"height\": 1103, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.1634/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1652, \"height\": 713, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1667, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1668, \"height\": 470, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1669, \"height\": 473, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1089, \"height\": 275, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1662, \"height\": 1451, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1660, \"height\": 1451, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1665, \"height\": 1452, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1000, \"height\": 324, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 851, \"height\": 275, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.1634/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1665, \"height\": 1449, \"label\": \"Table\"}]"
motivation: 多语言分层换能器存在负迁移和训练不稳定。
method: 引入语言条件MoE适配器增强分层换能器。
result: 在多语言ASR和ST任务上性能显著提升，负迁移得到缓解。
conclusion: 适配器方法为多语言语音处理提供了参数高效的解。
---

## Abstract
Neural transducers offer an alignment-free framework for speech-to-text modeling, and hierarchical transducer architectures further improve multilingual joint automatic speech recognition (ASR) and speech translation (ST) by stacking a translation-focused encoder on top of an ASR encoder. However, extending hierarchical transducers to multilingual many-to-many settings remains challenging: fully shared models often suffer from negative transfer and unstable target-language generation, while training separate models for each direction is computationally prohibitive. We propose LCMA-SRT (Language-Conditional Mixture-of-Experts Adapters for Speech Recognition and Translation), which augments a hierarchical transducer with language-conditional Mixture-of-Experts (MoE) adapters. A source-conditioned MoE adapter (SRC-MoE) uses source-language embeddings to reduce cross-language interference and improve multilingual ASR. A target-conditioned MoE adapter (TGT-MoE) uses the desired target language to reduce cross-target interference and stabilize target-language generation in many-to-many ST. Experiments on Europarl-ST (9 languages, 72 directions) show that LCMA-SRT improves both ASR and ST within a single joint model, reducing average WER and improving BLEU and COMET over strong hierarchical transducer baselines. We release our code and models at https://github.com/linanjie0820/LCMA-SRT .

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：语音翻译系统中，联合处理多语言自动语音识别（ASR）与语音翻译（ST）是一个关键挑战。分层神经换能器（Hierarchical Transducer）通过将识别导向的编码器与翻译导向的编码器堆叠，缓解了识别与翻译间严格单调对齐的差异。
- **核心问题**：当把分层换能器扩展到**多语言、多对多（many-to-many）**场景时，模型面临两大困难：
  - **负迁移**：完全共享参数的模型在多语言源语识别时会相互干扰。
  - **目标语言生成不稳定**：在翻译侧，共享解码器难以稳定地生成正确的目标语言，容易发生语言漂移（language drift）。
  - **计算成本**：为每个翻译方向单独训练模型（如 72 个）会导致极高的计算开销。
- **整体含义**：本文提出 LCMA‑SRT，通过**语言条件化的混合专家适配器**（Language‑Conditional Mixture‑of‑Experts Adapters）增强分层换能器，目标是**用一个统一模型实现高效的多语言多对多联合 ASR 和 ST**，同时减轻负迁移和语言干扰。

### 2. 方法论
- **整体架构**：基于分层换能器（ASR 编码器→ST 编码器），分别在 ASR 编码器输出和 ST 编码器输出插入两个轻量的 MoE 适配器。
- **源语言条件适配器 SRC‑MoE**：
  - 位置：ASR 编码器后。
  - 输入：ASR 编码器产生的语音对齐表示 `F(s)` 和**源语言身份的嵌入** `e_s`。
  - 路由机制：通过路由器 `g([h_t; e_s])` 生成混合权重 `w_t`，由 `E` 个专家 `f_i` 进行变换，输出带残差的适配表示。
  - 作用：将专家容量动态分配给不同源语言的声学‑音素变化，降低多语言 ASR 中的跨语言干扰。
- **目标语言条件适配器 TGT‑MoE**：
  - 位置：ST 编码器后。
  - 输入：ST 编码器输出 `F(t)` 和**目标语言嵌入** `e_t`。
  - 机制与 SRC‑MoE 类似，但路由以目标语言为条件。
  - 作用：抑制多方向翻译时的跨目标干扰，稳定目标语言生成。
- **训练策略**：
  - 两阶段训练：先预训练 ASR 分支，再联合训练 ASR+ST。
  - 采用**一致性正则化 CTC（CR‑CTC）** 作为辅助自蒸馏信号，鼓励不同增强视图的 CTC 后验保持一致。
  - 使用**MoE 熵正则化**防止路由坍塌，鼓励专家均衡利用。
- **推理**：通过目标语言前缀标记（`<2xx>`）强制翻译方向，并引入空白惩罚（blank penalty）抑制过多空白输出。源/目标语言身份信号在整个过程中都用于路由。

### 3. 实验设计
- **数据集**：Europarl‑ST，包含 9 种语言（de, en, es, fr, it, nl, pl, pt, ro），覆盖全部 72 个翻译方向。使用官方训练/开发/测试切分。
- **对比基准（Benchmark）**：
  - **HENT‑SRT‑M2O ×9**：为每个目标语言单独训练一个“多对一”分层换能器模型（共 9 个，549M 参数），代表上限。
  - **HENT‑SRT‑M2M**：完全共享的“多对多”分层换能器（61M）。
  - **Whisper Base**：参数量相近（74M）的外部多语言基线（仅评估 X→En 子集）。
  - **消融变体**：去掉 TGT‑MoE、将 TGT‑MoE 替换为无条件 MoE、替换为目标语言偏置（T‑Bias）、去掉 SRC‑MoE 等。
- **评估指标**：ASR 用词错误率（WER），ST 用 BLEU 和 COMET，另引入目标语言匹配率（LMR）衡量语言漂移。

### 4. 资源与算力
- 训练硬件：4 块 NVIDIA A800 GPU。
- 训练配置：两阶段各训练 50 个 epoch；第一阶段最大时长 900 秒/批次，第二阶段 450 秒/批次；使用基于时长的批处理。
- 未提及具体的总训练时长（墙钟时间）。

### 5. 实验数量与充分性
- **实验组数**：包含多语言 ASR 预训练对比（表 1）、多对多联合训练主结果（表 2‑3）、与 Whisper 的对比（表 4）、详细的 72 方向 ASR/ST 结果（附录表 5‑8）、消融实验（替换或移除关键模块）、灵敏度分析（专家数量、熵正则化系数，附录表 9‑10），以及路由权重的可视化分析（图 2）。
- **充分性**：实验覆盖了主要基线、消融、超参数灵敏度和路由行为分析，评估指标多维（WER、BLEU、COMET、LMR），且提供了方向级细粒度结果，整体实验设计全面且公平。
- **公平性**：基线使用相同的主干网络（Zipformer 分层换能器）、相同的两阶段训练策略和评估环境，消融变量控制清晰。

### 6. 主要结论与发现
- LCMA‑SRT 在单一 77M 模型中，相比 9 个方向独立模型（共 549M）平均 BLEU 提升 5.2，COMET 提升 0.076，WER 从 23.28% 降至 15.71%。
- 与全共享基线 HENT‑SRT‑M2M 相比，LCMA‑SRT 大幅降低了语言漂移（LMR 从 84.95% 降至 0.75%），翻译质量从接近失效（BLEU 4.3）提升至可用水平（BLEU 20.5）。
- SRC‑MoE 主要改善多语言 ASR 质量，并为下游 ST 带来小幅收益；TGT‑MoE 是**确保多对多翻译目标语言保真度的关键**，仅靠目标语言前缀无法防止语言泄漏。
- 语言条件路由比单纯增加参数（无条件 MoE）或添加偏置更有效，且受益于适度的熵正则化。
- 在同一参数量级下，LCMA‑SRT 在 Europarl‑ST X→En 任务上显著超越 Whisper Base。

### 7. 优点
- **参数高效**：用一个统一模型支持 72 个翻译方向，参数总量远少于方向独立模型，却取得更优性能。
- **模块化解耦**：源端和目标端的条件适配器分工明确，SRC‑MoE 侧重语音识别，TGT‑MoE 侧重翻译控制，两者互补。
- **缓解负迁移**：通过轻量级适配器而非复制整个骨干网络，实现了语言相关的容量分配，有效减轻了多语言共享模型中的干扰。
- **实验扎实**：提供了详尽的消融、灵敏度分析和可视化，明确验证了每个模块的必要性与贡献。
- **开源**：承诺发布代码与模型，有利于复现。

### 8. 不足与局限
- **场景局限**：仅在离线情境下评估，未测试流式（streaming）性能。
- **领域与语系覆盖**：实验仅限于 Europarl‑ST 的议会演讲领域和印欧语系，对更广领域及其他语系的泛化性未验证。
- **对语言信号的依赖**：方法依赖明确的源语言和目标语言身份标识，当语言标签不确定或缺失时的鲁棒性未探讨。
- **效率‑精度权衡分析不足**：虽进行了专家数和熵系数的灵敏度实验，但未系统地分析不同配置下的推理延迟、内存占用等部署特性。
- **插入位置**：TGT‑MoE 放置在 ST 编码器后，虽在文中解释为一种平衡选择，但未与其他可能的位置（如编码器内部）进行对比，未验证是否最优。

（完）
