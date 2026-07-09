---
title: "TellWhisper: Tell Whisper Who Speaks When"
title_zh: TellWhisper：告诉Whisper谁在何时说话
authors: "Yifan Hu, Peiji Yang, Zhisheng Wang, Yicheng Zhong, Rui Liu"
date: 2026-07-01
pdf: "https://aclanthology.org/2026.acl-long.861.pdf"
tags: ["query:speech-model"]
score: 9.0
evidence: 统一框架联合建模说话人身份和时间信息，提升多说话人语音识别准确率
tldr: 针对多说话人语音识别中时间与说话人建模分离导致的性能衰减问题，本文提出TellWhisper框架。该方法在Whisper编码器内统一建模说话人身份和时间线索，避免了现有方法中信息损失或内容混淆。实验在重叠语音和快速话轮转换场景下取得显著增益，为多说话人对话理解提供了更鲁棒的识别技术。
source: ACL-2026-Long
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.861/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 802, \"height\": 623, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.861/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1650, \"height\": 986, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.861/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 805, \"height\": 686, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.861/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 784, \"height\": 622, \"label\": \"Figure\"}, {\"url\": \"assets/figures/acl-2026-long/anthology-2026.acl-long.861/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1652, \"height\": 1557, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 797, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1487, \"height\": 535, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 800, \"height\": 506, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1487, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 799, \"height\": 571, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 802, \"height\": 588, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 796, \"height\": 838, \"label\": \"Table\"}, {\"url\": \"assets/tables/acl-2026-long/anthology-2026.acl-long.861/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 796, \"height\": 1243, \"label\": \"Table\"}]"
motivation: 现有多说话人语音识别解耦时间建模和说话人建模，在快速话轮转换和重叠语音下性能脆弱。
method: 提出统一框架TellWhisper，在语音编码器中联合建模说话人身份和时间信息，避免信息损失和内容混淆。
result: 实验表明该方法显著提升了多说话人场景下的识别准确率。
conclusion: 为多说话人语音识别提供了更鲁棒的联合建模方案。
---

## Abstract
Multi-speaker automatic speech recognition (MASR) aims to predict ”who spoke when and what” from multi-speaker speech, a key technology for multi-party dialogue understanding. However, most existing approaches decouple temporal modeling and speaker modeling when addressing ”when” and ”who”: some inject speaker cues before encoding (e.g., speaker masking), which can cause irreversible information loss; others fuse identity by mixing speaker posteriors after encoding, which may entangle acoustic content with speaker identity. This separation is brittle under rapid turn-taking and overlapping speech, often leading to degraded performance. To address these limitations, we propose TellWhisper , a unified framework that jointly models speaker identity and temporal within the speech encoder. Specifically, we design TS - RoPE , a time-speaker rotary positional encoding: time coordinates are derived from frame indices, while speaker coordinates are derived from speaker activity and pause cues. By applying region-specific rotation angles, the model explicitly captures per-speaker continuity, speaker-turn transitions, and state dynamics, enabling the attention mechanism to simultaneously attend to ”when” and ”who”. Moreover, to estimate frame-level speaker activity, we develop Hyper - SD , which casts speaker classification in hyperbolic space to enhance inter-class separation and refine speaker-activity estimates. Extensive experiments demonstrate the effectiveness of the proposed approach. The project webpage is available at https://walker-hyf.github.io/TellWhisper.

---

## 论文详细总结（自动生成）

# TellWhisper：统一时间-说话人建模的多说话人语音识别

## 1. 核心问题与整体含义
- **研究动机**：多说话人自动语音识别（MASR）要求同时预测“谁在何时说了什么”，但现有方法通常将时间建模和说话人建模解耦，例如在编码前注入说话人掩码（导致不可逆信息丢失）或在编码后混合后人概率（纠缠声学内容与身份）。这种分离在快速话轮转换和重叠语音下性能脆弱。
- **整体含义**：提出 **TellWhisper** 框架，在语音编码器内联合建模说话人身份和时间线索，避免信息损失与内容混淆，更自然地处理“何时”与“谁”的对齐。

## 2. 方法论
### 2.1 整体架构
- 基于 Whisper 大模型，由三部分组成：
  1. **说话人活动估计器 Hyper‑SD**（双曲空间内分类）
  2. **说话人–时间感知编码器**（注入 TS‑RoPE）
  3. **结构化内容预测器**（自回归解码输出 ⟨spk⟩、⟨time⟩、⟨text⟩）

### 2.2 Hyper‑SD（双曲空间说话人分类）
- **特征提取**：WavLM 多层特征加权融合 + Conformer 上下文编码。
- **双曲映射**：线性变换后正则裁剪，再通过指数映射投影到庞加莱球。
- **原型分类**：为每种说话人组合（≤4 人，共 16 类，含静音、单人、多人的所有组合）设定可学习双曲原型，计算帧特征与各原型距离，激活后边缘化得到每帧的说话人活动概率 πt,s。
- **优化**：双曲原型用 RiemannianAdam，其余用 AdamW。

### 2.3 TS‑RoPE（时间‑说话人旋转位置编码）
- **位置构建**：每个卷积层输出帧 ft 对应一个时间索引 t 和四个说话人索引 ψspk_s。
- **说话人位置**：ψspk_s(ft) = Ct,s + πt,s，其中 Ct,s 为累积话轮计数（检测上升沿），πt,s 为 Hyper‑SD 输出活动概率。
- **查询端额外偏置**：对 Query 的说话人子空间添加 (1 − πt,s) 相位，鼓励注意力聚焦活跃说话人。
- **通道分配**：每 16 维为一组，8 个旋转对按 [ψtime, ψspk1, ψtime, ψspk2, …] 交错分配，频率 ωi 采用基频 110,000 的旋转嵌入。
- **自注意力**：Query 和 Key 向量在对应子空间根据 ψ 值旋转，点积同时捕获时间和说话人动态，生成融合表示 E。

### 2.4 结构化内容预测器
- 将同一说话人的连续语音编为一个片段，目标序列为 ⟨spk_s⟩, ⟨tstart⟩, ⟨text⟩, ⟨tend⟩，所有片段按时间顺序拼接，采用语言模型式自回归解码，训练时用下一 token 预测。

## 3. 实验设计
### 3.1 数据集
- **MASR**：AMI (远场会议)、NotSoFar (远场会议)、Libri2Mix (模拟重叠)、LibriCSS (模拟会议)，均裁剪为最多 4 说话人。还使用 LibriSpeech 单说话人预微调。
- **SD**（验证 Hyper‑SD）：AISHELL‑4、AliMeeting、AMI、MSDWild、RAMC、VoxConverse。

### 3.2 指标
- **MASR**：CP‑WER（内容+说话人）、TCP‑WER（时间+内容+说话人，0.5s 宽容）、ORC‑WER（仅内容）、TCORC‑WER（时间+内容）。
- **SD**：DER（collar 0s/0.25s）。

### 3.3 对比方法
- **对齐式**：Pyannote3+Whisper、Hyper‑SD+Whisper
- **分离式**：Tiger+Whisper
- **单阶段**：Whisper‑D、SortFormer、Dicow、TellWhisper‑Diarizen（替换 Hyper‑SD 为 Diarizen）
- 所有基线采用同一 Whisper large‑v3‑turbo 主干，公平对比。

## 4. 资源与算力
- 所有实验使用 **8 块 NVIDIA H20 GPU**。
- 未明确提及总训练时长或具体计算量（FLOPs），但给出了优化器、学习率等详细设置。

## 5. 实验数量与充分性
- **SD 验证**：6 个数据集，与 Pyannote3、Diarizen 对比，全面覆盖大小语种、会议和日常对话。
- **MASR 主实验**：4 个数据集，涵盖模拟与真实远场会议，对比 7 个代表性模型，同时报告时间无关和时间约束指标。
- **消融实验**：依次移除查询偏置、累积话轮、后验活动，证明各组件的必要性，共 4 种配置 × 多数据集。
- **可视化分析**：展示双曲原型的类间距离与径向分布，显示良好分离性；并提供不同重叠比例（0%~30%）的识别案例，直观显示鲁棒性。
- 实验覆盖面广，基线公平（相同主干网络、相同训练流程），消融与定性分析充分，结论可信。

## 6. 主要结论与发现
- TellWhisper 在所有会议数据集的 CP‑WER 和 TCP‑WER 上均取得最优，尤其在真实远场场景（AMI、NotSoFar）中优势显著；对完全重叠的 Libri2Mix 保持竞争力。
- Hyper‑SD 在 6 个 SD 数据集上 DER 全面低于 Diarizen 和 Pyannote3，双曲空间分类有效增大类间距离，提供更稳定的说话人活动先验。
- 消融实验证实：查询端偏置、累积话轮、活动后验三者都对最终性能至关重要，去除任一部分均导致明显退化。
- 可视化表明双曲原型无多余层级结层，距离均匀，分类边界清晰。

## 7. 优点
- **联合建模思想**：首次将时间和说话人线索统一编码到 Whisper 的 RoPE 中，避免信息割裂。
- **新颖的 TS‑RoPE**：巧妙利用说话人活动与话轮计数构造相位，既保留每说话人连续性，又能捕捉话轮转换。
- **Hyper‑SD 双曲分类**：显式建模多说话人组合，利用双曲几何增强原型分离，提升说话人活动估计质量。
- **规模化验证**：多套指标、多数据集合、丰富消融与案例，展示方法鲁棒性和通用性。
- **开源复现**：提供项目页面，承诺公开代码。

## 8. 不足与局限
- **说话人数量受限**：当前设计最多支持 4 人，无法直接扩展至大型多人对话（如 >4 人会议）。
- **几何一致性未完善**：Hyper‑SD 仅在分类头使用双曲空间，特征提取仍在欧氏空间，模型内部空间不匹配，可能限制双曲学习的潜力。
- **计算资源记载不完整**：未给出具体训练时长或显存消耗，难以评估实际部署成本。
- **泛化到长时语音**：未在超长会议（如数小时）或完全无限制的任意数量说话人场景下测试。
- **多语言未涉及**：仅评估英文数据集，对跨语言多说话人场景的适用性未知。

（完）
