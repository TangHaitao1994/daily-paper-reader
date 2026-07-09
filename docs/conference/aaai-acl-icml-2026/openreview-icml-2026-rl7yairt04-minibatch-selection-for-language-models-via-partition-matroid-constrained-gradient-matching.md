---
title: Minibatch selection for Language Models via Partition Matroid Constrained Gradient Matching
title_zh: 基于划分拟阵约束梯度匹配的语言模型小批量选择
authors: "Prayas Agrawal, Prateek Chanda, Ishita Khatri, Ganesh Ramakrishnan, Bamdev Mishra, Pratik Jawanpuria"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e376c6adaf200813e7c96d774622b7afd29b1541.pdf"
tags: ["query:speech-model"]
score: 5.0
evidence: LLM小批量选择方法可应用于语音模型训练
tldr: 该论文旨在解决大模型训练中小批次数据选择的问题，提出PartitionSel方法。该方法利用划分拟阵约束，在每个领域预算下最大化验证集引导的梯度匹配效用，从而协调跨领域样本选择，减少信息冗余。算法通过弱次模性质保证近似解质量，使用正交匹配追踪高效实现。在语言模型上的实验表明，该方法能提升模型收敛速度和最终性能，且对数据异质性鲁棒。这一框架为语音识别或合成等大模型的训练数据策展提供了技术借鉴。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 训练大模型时，小批量选择需平衡收敛速度与领域覆盖。
method: 利用划分拟阵约束的梯度匹配最大化验证效用，减少冗余。
result: 在语言模型训练中展示了更好的跨域覆盖和收敛效率。
conclusion: 该方法可推广到其他模态模型训练，辅助语音大模型训练。
---

## Abstract
Training large language models (LLMs) on heterogeneous data requires selecting minibatches that balance convergence speed with coverage across domains. Existing methods either select samples independently within each domain or rely on computationally expensive proxy models to learn continuous domain weights. We propose PartitionSel, a cross-domain minibatch selection approach that  maximizes a validation-guided gradient-matching utility under per-domain budgets encoded as a partition-matroid constraint. By coupling the per-domain budgets through a single utility, PartitionSel is designed to reduce redundancy in selections across domains. The proposed objective is weakly submodular and admits an orthogonal matching pursuit algorithm with provable approximation guarantees. Empirically, we evaluate PartitionSel for minibatch selection during the fine-tuning of Qwen2.5 and Llama-3 on MetaMathQA and Mol-Instructions. PartitionSel achieves robust gains over per-domain and domain-agnostic baselines on both benchmarks. It also reduces the number of conflicting gradient pairs within each batch, indicating that the cross-domain coupling translates into more compatible training updates.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **核心问题**：在异构数据上训练大语言模型时，如何高效地选择小批量数据，以在加快收敛和确保不同数据领域充分覆盖之间取得平衡。
- **研究动机**：传统方法通常在每个领域内独立挑选样本，忽略了跨领域的互补性，容易造成信息冗余；而基于代理模型学习连续领域权重的方案则计算开销过大。因此，需要一种能够协调跨领域样本选择的轻量方法，使每次更新梯度更“兼容”，从而提升训练效率与模型表现。
- **整体含义**：本文提出一种跨领域的小批量选择框架，将选择过程建模为划分拟阵约束下的梯度匹配最大化问题，旨在减少跨领域挑选的冗余，用更少的训练步数达到更好的泛化能力。

### 2. 方法论
- **核心思想**：利用验证集引导的梯度匹配效用函数，并在每个领域分别设定的样本预算（划分拟阵约束）下最大化该效用，从而一次性完成跨领域的小批量筛选。
- **关键技术细节**（用文字说明）：
  - **梯度匹配效用**：设候选样本池来自不同领域，对每一条候选样本计算其梯度（或梯度近似）与验证集梯度的匹配程度，作为选择该样本的“收益”。
  - **划分拟阵约束**：将候选样本按领域划分为互不相交的子集，每个子集拥有一个允许选取的最大数量（预算）。通过这种约束，强制算法在各领域间分配选择名额，避免某一领域样本被过度选取。
  - **目标函数**：最大化所选样本的整体梯度匹配收益，同时满足各领域的预算上限。由于子模性质较弱，该问题可通过**正交匹配追踪（OMP）** 算法高效求解，且具有可证明的近似保证。
  - **跨领域耦合**：因为所有领域共享同一个拟合目标（验证集梯度），算法在分配预算时会自然避免选择梯度高度相似的冗余样本，使最终小批量的梯度方向更为一致，减少训练更新冲突。

### 3. 实验设计
- **数据集 / 场景**：
  - **MetaMathQA**：数学问答数据，用于微调大模型以增强数学推理能力。
  - **Mol-Instructions**：分子指令数据，用于微调模型以提升在分子与材料科学领域的遵循指令能力。
- **基础模型**：Qwen2.5 和 Llama-3，均为业内具有代表性的开源大规模语言模型。
- **对比方法**：
  - **Per-domain 基线**：在每个领域内部独立进行样本挑选（如按梯度匹配、随机选择等）。
  - **Domain-agnostic 基线**：不考虑领域属性、统一从全集中选择样本的方法。
- **评估指标**：主要考察下游任务性能（如数学推理、分子指令遵循的准确率）以及训练过程中的收敛速度。此外，还统计了每批内“冲突梯度对”的数量，以衡量训练更新的内部一致性。

### 4. 资源与算力
- 文中（基于提供的摘要与元数据）**未明确说明**所使用的GPU型号、数量、训练时长等算力细节。仅提到在 Qwen2.5 和 Llama-3 上进行微调实验，但未给出具体硬件配置或计算开销的量化数据。

### 5. 实验数量与充分性
- **实验组数概况**：
  - 2种基础模型 × 2个数据集 = 4个主要实验场景。
  - 每个场景下对比了至少两类基线（per-domain 与 domain-agnostic），还会涉及不同预算分配或选择策略的变体。
  - 额外设计了冲突梯度分析实验，从梯度兼容角度验证方法有效性。
- **充分性与客观性**：
  - 覆盖了数学推理和科学指令两种性质差异较大的数据域，具有一定的领域多样性。
  - 采用了公开的知名模型与基准数据集，对比基线清晰，评估方式客观。
  - 虽未在文中详细说明消融实验或超参数敏感性分析，但以提供的材料看，已有的比较和对内部机制的分析已能支撑主要论断，整体实验设计较为扎实且公平。

### 6. 主要结论与发现
- PartitionSel 在两个基准数据集和两种大模型微调任务上，均**一致优于**基于单领域独立选择和领域无关的基线方法。
- 该方法能够**有效降低小批次内冲突梯度对的数量**，说明跨领域耦合的选择方式使得训练更新方向更协调，减少了梯度相互抵消的现象。
- 这一现象印证了通过单一梯度匹配效用耦合各领域预算，确实有助于筛选出对模型更新更具协同性的样本组合，从而加速收敛并提升最终性能。

### 7. 优点
- **理论支撑强**：将问题形式化为带有划分拟阵约束的弱次模优化，并借助正交匹配追踪算法保证了近似解质量，兼具效果与效率。
- **跨领域协同**：打破领域壁垒，通过统一目标同时进行多领域样本选择，能够显式地抑制冗余，提升数据利用效率。
- **实现简洁**：无需训练额外的代理模型来估计领域权重，避免了繁重的元学习或超参数搜索，易集成到现有训练流程中。
- **实验验证多角度**：不仅对比下游性能，还创新性地引入冲突梯度对指标，从优化动力学层面解释方法为何有效。

### 8. 不足与局限
- **算力信息缺失**：没有给出具体 GPU 型号、数量或训练时间，读者难以评估该方法在大规模训练中的实际计算开销。
- **实验范围受限**：仅在两个相对中等规模的数据集上进行微调实验，未展示在更大规模预训练或更多样化领域（如代码、多语言、语音等）上的适用性。
- **局限性讨论不足**：从现有材料看，未系统分析该方法对划分预算敏感度、对验证集质量的依赖，或当领域数剧增时算法的扩展性。
- **潜在偏差风险**：梯度匹配依赖于验证集梯度，若验证集与真实目标分布存在偏差，可能引导选择偏向特定数据，过度优化验证指标。
- **适用边界**：方法有效性假设领域中样本的“可替代性”较强，若各领域内部数据极度稀缺或高度专业化，预算约束可能需要更精细的调整。

（完）
