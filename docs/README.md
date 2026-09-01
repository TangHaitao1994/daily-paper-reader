<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-09-01
- 运行时间：2026-09-01 22:42:47 UTC
- 运行状态：成功
- 本次总论文数：17
- 精读区：8
- 速读区：9

### 今日简报（AI）
今日精读8篇、速读9篇语音AI论文，聚焦音频大模型在“听清、说准、评得好”上的闭环能力。
最值得看：TTS的AudioLLM诊断-修正闭环，以及用语义锚定提升低资源语言语音识别；速读还提示语音增强可能“听感更好但语义更差”。
建议普通读者优先跟踪低资源多模态适配与音频LLM评估基准，这是语音智能落地的关键下一步。
- 详情：[/202609/01/README](/202609/01/README)

### 精读区论文标签
1. [Diagnose, Then Refine: A Closed-Loop TTS System with AudioLLM-Guided Correction](/202609/01/2608.28970v1-diagnose-then-refine-a-closed-loop-tts-system-with-audiollm-guided-correction)  
   标签：评分：9.0/10、query:speech-model
   evidence：AudioLLM判断与精修器闭环提升TTS韵律质量
2. [Anchoring Speech with Semantics: A Multimodal Adapter Mechanism for Automatic Speech Recognition in Low-Resource Languages](/202609/01/2608.29239v1-anchoring-speech-with-semantics-a-multimodal-adapter-mechanism-for-automatic-speech-recognition-in-low-resource-languages)  
   标签：评分：9.0/10、query:speech-model
   evidence：提出SAMA-ASR，通过语义锚定适配器结合翻译语义和语音嵌入提升低资源ASR
3. [TEMPO: Temporally-grounded Multi-task Post-training for Large Audio-Language Models](/202609/01/2608.29999v1-tempo-temporally-grounded-multi-task-post-training-for-large-audio-language-models)  
   标签：评分：9.0/10、query:speech-model
   evidence：时间锚定多任务后训练，使用时间戳令牌和时间感知投影器用于大型音频语言模型
4. [Sequential Trajectories and Simultaneous Blending: Multi-Emotion Modeling for Instruction-Following TTS](/202609/01/2608.30325v1-sequential-trajectories-and-simultaneous-blending-multi-emotion-modeling-for-instruction-following-tts)  
   标签：评分：9.0/10、query:speech-model
   evidence：用于多情感TTS的HybridEmo后训练框架
5. [Parallel Time-Band Mixing with Learned Observation-Adding for Robust ASR Front-Ends](/202609/01/2608.30326v1-parallel-time-band-mixing-with-learned-observation-adding-for-robust-asr-front-ends)  
   标签：评分：9.0/10、query:speech-model
   evidence：用于鲁棒ASR的并行分带增强前端
6. [Likelihood-Constrained Acoustic Reranking for Training-Free Hallucination Mitigation in LLM-Based ASR](/202609/01/2608.30776v1-likelihood-constrained-acoustic-reranking-for-training-free-hallucination-mitigation-in-llm-based-asr)  
   标签：评分：9.0/10、query:speech-model
   evidence：无训练声学重排序缓解LLM ASR幻觉，提升识别准确率
7. [When Does Predictor-Based RL Align with Human Perception? A Study of Subjective Rewards in Codec-Based Speech Language Models](/202609/01/2608.31035v1-when-does-predictor-based-rl-align-with-human-perception-a-study-of-subjective-rewards-in-codec-based-speech-language-models)  
   标签：评分：9.0/10、query:speech-model
   evidence：研究在编解码器语音语言模型中使用学习到的感知奖励进行强化学习，以提升TTS风格和自然度
8. [Context-Aware Interleaved Batching for WhisperX](/202609/01/2608.31170v1-context-aware-interleaved-batching-for-whisperx)  
   标签：评分：9.0/10、query:speech-model
   evidence：上下文感知交错批处理降低WhisperX词错率并改善专有名词转录同时保持吞吐量

### 速读区论文标签
1. [Auditing Generative Audio Calls for Known-Task Audio-LLM Evaluation](/202609/01/2608.27817v1-auditing-generative-audio-calls-for-known-task-audio-llm-evaluation)  
   标签：评分：7.0/10、query:speech-model
   evidence：评估在音频LLM任务中何时调用生成式音频模型而非仅用ASR转录
2. [VocalAffectBench: Evaluating Vocal Emotion Recognition in AI Audio Models](/202609/01/2608.28932v1-vocalaffectbench-evaluating-vocal-emotion-recognition-in-ai-audio-models)  
   标签：评分：7.0/10、query:speech-model
   evidence：评估音频模型声音情感识别的基准，对对话式AI至关重要
3. [Perceptually Better, Semantically Worse: Measuring Speech Enhancement Impact on LLM-Based Voice Systems](/202609/01/2608.30348v1-perceptually-better-semantically-worse-measuring-speech-enhancement-impact-on-llm-based-voice-systems)  
   标签：评分：7.0/10、query:speech-model
   evidence：测量语音增强失真对下游LLM语音任务的影响
4. [Linguistic Distance Segregates Latent Representations in Automatic Speech Recognition Systems](/202609/01/2608.30853v1-linguistic-distance-segregates-latent-representations-in-automatic-speech-recognition-systems)  
   标签：评分：7.0/10、query:speech-model
   evidence：研究说话人母语距离如何影响ASR错误率，揭示准确性差异
5. [Stride-k Subsampling: Train-Free Audio Token Reduction for Whisper](/202609/01/2608.30927v1-stride-k-subsampling-train-free-audio-token-reduction-for-whisper)  
   标签：评分：7.0/10、query:speech-model
   evidence：无训练stride-k子采样减少Whisper音频token与计算量，加速推理
6. [VoiceCodeBench: Evaluating Exact Structured-Token Recovery in Automatic Speech Recognition](/202609/01/2608.28916v1-voicecodebench-evaluating-exact-structured-token-recovery-in-automatic-speech-recognition)  
   标签：评分：6.0/10、query:speech-model
   evidence：ASR中精确结构化token恢复基准，评估实体识别准确性
7. [HEAR Who Said What: Unlocking Speaker-Attributed Reasoning via Counterfactual Voice Grounding](/202609/01/2608.29120v1-hear-who-said-what-unlocking-speaker-attributed-reasoning-via-counterfactual-voice-grounding)  
   标签：评分：6.0/10、query:speech-model
   evidence：语音语言模型说话人归因推理的基准与训练方法
8. [Learning to Reason and Use Tools through Unsupervised Fine-Tuning in Task-Oriented Dialog Systems](/202609/01/2608.30426v1-learning-to-reason-and-use-tools-through-unsupervised-fine-tuning-in-task-oriented-dialog-systems)  
   标签：评分：6.0/10、query:speech-model
   evidence：通过无监督微调提升任务导向对话系统的推理和工具使用能力，减少幻觉
9. [Towards Balanced Spectral Reconstruction: Spectrally Adaptive Loss for Streaming Speech Enhancement](/202609/01/2608.30739v1-towards-balanced-spectral-reconstruction-spectrally-adaptive-loss-for-streaming-speech-enhancement)  
   标签：评分：6.0/10、query:speech-model
   evidence：频谱自适应损失用于流式语音增强，改善ASR输入质量


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
