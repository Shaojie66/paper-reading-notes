# 论文阅读笔记 · 第 8 期（范式篇：Transformer）

> 定时任务「每两天读一篇论文」· 2026-10-21 触发 · 承接第 7 期：Bahdanau Attention 的「加法注意力」由本文升级为「缩放点积注意力 + 多头」，并彻底去掉 RNN——Attention 范式收官。

## 基本信息

- **标题**：Attention Is All You Need（Transformer）
- **作者/机构**：Ashish Vaswani、Noam Shazeer、Niki Parmar、Jakob Uszkoreit、Llion Jones、Aidan N. Gomez、Łukasz Kaiser、Illia Polosukhin（Google Brain / Google Research / University of Toronto）
- **发表**：NeurIPS 2017（Advances in Neural Information Processing Systems 30）
- **链接**：arXiv:1706.03762（官方 PDF：https://arxiv.org/pdf/1706.03762）

## 一句话总结

> 主流的序列转导模型都建立在循环或卷积网络之上，attention 只是 RNN 的辅助。本文提出 **Transformer**——完全摒弃循环与卷积、**只靠注意力机制**（缩放点积注意力 + 多头）建模输入输出全局依赖的架构。训练更快（并行化）、效果更好：WMT'14 英德 28.4 BLEU（超此前所有含集成的结果 2+ BLEU）、英法单模型 41.8 BLEU（训练成本不到此前 SOTA 的 1/4），并成功泛化到句法分析。

## 为什么选这篇（本期理由）

- Roadmap「范式：Attention / Transformer」收官站：第 7 期读完 Bahdanau 加法注意力后，本文是 attention 从「编码器-解码器之间」走向「自注意力 + 并行化 + 去 RNN」的决定性一步；第 9 期起即可进入你的研究方向选题（覆盖路径规划 / AI 伦理 / 社交媒体与福祉）。
- 现代大模型（BERT/GPT/LLaMA 等）的架构源头，AI 硕士必读的「范式论文」，引用量最高论文之一。
- 与第 7 期形成完美的演进对照：加法注意力 → 缩放点积注意力 → 多头自注意力。

## 核心笔记

### 问题 Problem

- **RNN 的串行瓶颈**：循环模型按序列位置逐步计算（$h_t$ 依赖 $h_{t-1}$），天然无法在样本内并行；序列越长，批内并行受内存限制越严重。
- **远距离依赖难学**：卷积模型（ByteNet/ConvS2S）虽可并行，但连接任意两个位置的操作数随距离增长（线性/对数），路径越长越难学；RNN 的最大路径长是 O(n)。
- **attention 只是配角**：此前 attention（如第 7 期）几乎总是与循环网络搭配使用，没人证明「只用 attention」就够了。

### 方法 Method（Transformer）

1. **总体架构**：encoder-decoder，各 N=6 层（图 1）。所有子层与 embedding 输出维度 $d_{model}=512$，便于残差连接。
   - **Encoder 层**：① 多头自注意力；② position-wise 前馈网络（FFN）。每个子层后接残差连接 + LayerNorm：$\text{LayerNorm}(x+\text{Sublayer}(x))$。
   - **Decoder 层**：在 encoder 两层之上插入第三子层——**encoder-decoder 注意力**（query 来自 decoder，key/value 来自 encoder）；decoder 自注意力用**掩码**禁止关注后续位置（置 $-\infty$），配合输出右移一位保证自回归性质。
2. **缩放点积注意力**（图 2 左）：
   $$\text{Attention}(Q,K,V)=\text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$$
   - 为何缩放：$d_k$ 大时点积幅度增大，把 softmax 推到梯度极小的区域；除以 $\sqrt{d_k}$ 抵消。
   - 相比加法注意力：理论复杂度相同，但点积可用高度优化的矩阵乘法实现，**更快更省空间**。
3. **多头注意力**（图 2 右）：把 Q/K/V 线性投影 h=8 次（$d_k=d_v=d_{model}/h=64$），并行做注意力再拼接投影：
   $$\text{MultiHead}(Q,K,V)=\text{Concat}(head_1,...,head_h)W^O$$
   允许模型在不同表示子空间联合关注不同位置；单头会因平均而削弱。
4. **前馈网络**：$\text{FFN}(x)=\max(0,xW_1+b_1)W_2+b_2$，逐位置相同、层间参数不同；$d_{ff}=2048$（等价于两个 kernel size 1 的卷积）。
5. **位置编码**：正弦/余弦不同频率 $PE_{(pos,2i)}=\sin(pos/10000^{2i/d_{model}})$；固定偏移 k 时 $PE_{pos+k}$ 可表示为 $PE_{pos}$ 的线性函数 → 便于按相对位置学习，且可外推到更长序列；学到的位置嵌入效果几乎相同（Table 3 行 E）。
6. **权重共享**：两个 embedding 层与 pre-softmax 线性层共享权重，embedding 乘以 $\sqrt{d_{model}}$。
7. **训练设置**：WMT'14 英德 4.5M 句对（BPE 37k 词表）/ 英法 36M 句对（32k word-piece）；每批约 25k 源 + 25k 目标 token；8×P100；base 100k 步（12h）、big 300k 步（3.5 天）；Adam（β1=0.9, β2=0.98, ε=1e-9），学习率 warmup 4000 步线性升、之后按步数平方根倒数衰减；正则化三件套：子层 dropout（P_drop=0.1，big EN-FR 用 0.3）+ embedding/位置编码和 dropout + label smoothing（ε_ls=0.1）；推理：beam size 4、长度惩罚 α=0.6、平均最后 5/20 个 checkpoint。

### 效果 Results

- **机器翻译（Table 2）**：

| 模型 | EN-DE BLEU | EN-FR BLEU | 训练成本 (FLOPs) |
| --- | --- | --- | --- |
| ByteNet | 23.75 | — | — |
| ConvS2S Ensemble | 26.36 | 41.29 | 7.7e19 / 1.2e21 |
| GNMT + RL Ensemble | 26.30 | 41.16 | 1.8e20 / 1.1e21 |
| MoE | 26.03 | 40.56 | 2.0e19 / 1.2e20 |
| **Transformer (base)** | **27.3** | **38.1** | 3.3e18 |
| **Transformer (big)** | **28.4** | **41.8** | 2.3e19 |

  - **英德**：big 28.4 BLEU，超过此前所有模型**含集成** 2+ BLEU（SOTA）；base 27.3 也已超所有已发表模型。
  - **英法**：big 41.8 BLEU，单模型 SOTA，训练成本不到此前 SOTA 的 1/4。（注：正文 §6.1 曾写 41.0，与摘要/Table 2 的 41.8 不一致——以摘要与表格为准。）
- **计算复杂度对比（Table 1）**：自注意力每层 O(n²·d)、O(1) 顺序操作、O(1) 最大路径长；RNN O(n·d²) / O(n) / O(n)；CNN O(k·n·d²) / O(1) / O(log_k n)。
- **消融（Table 3，EN-DE dev newstest2013；base PPL 4.92 / BLEU 25.8）**：
  - (A) 头数：单头 24.9（-0.9），头过多也下降 → 多头必要但非越多越好；
  - (B) 减小 $d_k$ 伤质量 → 兼容性函数（compatibility）本身难学；
  - (C)(D) 大模型更好、dropout 显著防过拟合；
  - (E) 学习位置嵌入 vs 正弦：几乎相同（25.7 vs 25.8）。
- **泛化：英文成分句法分析（Table 4，WSJ §23 F1）**：WSJ-only 4 层 Transformer 91.3（超过 BerkeleyParser 90.4，且只用 40K 训练句）；半监督 92.7。（论文称「除 RNN 文法外超过所有已报告模型」，但表中 Luong multi-task 93.0 略高——正文与表格有小出入，精读时注意。）
- **注意力可解释（附录 Fig.3-5）**：encoder 第 5/6 层多个头学会跟踪长距离依赖（"making … more difficult"）；两个头参与指代消解（"its" → Law/application）；不同头学到不同任务（句法/语义结构相关）。

### 局限 Limitations

- **O(n²) 自注意力**：长序列成本高（论文提出 restricted attention 为未来方向；后续由 Longformer/BigBird/FlashAttention 等改进）。
- **缺乏归纳偏置**：位置信息全靠注入，绝对位置编码表达能力有限（后续 RoPE、ALiBi 等改进相对位置建模）；无递归/卷积的结构先验，小数据场景不如专门架构。
- **生成仍是顺序的**：decoder 自回归逐 token 输出（论文把「让生成更少顺序化」列为未来目标）。
- **训练成本与数据需求**：big 模型 3.5 天 × 8 GPU、300k 步，需要大语料。
- 论文内部两处不一致：英法 BLEU 41.0 vs 41.8；句法「除 RNN 文法外最优」vs 表中 Luong multi-task 93.0。

## 扩展（可选）

- **创新点**：第一个完全基于 attention 的序列转导模型（彻底去 RNN/CNN）；缩放点积注意力 + 多头机制；正弦位置编码；全并行化训练。
- **相关工作**：第 7 期 Bahdanau 加法注意力（本文化为缩放点积）；Sutskever 2014 Seq2Seq（第 6 期）；Hochreiter & Schmidhuber LSTM（第 5 期）；ResNet 残差连接（第 4 期，直接借用到子层）；ByteNet/ConvS2S（并行卷积对手）；GNMT（当年 SOTA 系统）。
- **个人启发**：① 范式的力量：「去掉什么」比「加上什么」更难——RNN 是整个序列建模的默认前提，本文证明可去掉并更好；② 每个设计点都对应明确痛点（缩放防梯度消失、多头防平均稀释、掩码保自回归、位置编码补顺序信息），是「机制-动机」对照学习的好案例；③ O(n²) 注意力是当前长上下文研究的核心约束，与你未来可能做的机器人序列决策也有连接。
- **下一步问题**：进入研究方向选题：覆盖路径规划（Coverage Path Planning）、AI 伦理与监管（如 EU AI Act / Gender Shades）、社交媒体与青少年福祉（如 Ofcom 2023）；或先补一篇 BERT（预训练 + Transformer 双向编码）衔接 NLP 主线。

## 阅读记录

- **阅读日期**：2026-10-21
- **阅读遍数**：精读（全文 3 段分页读完，含 Table 1/2/3/4、Fig.1 架构图、Fig.2 注意力细节、附录注意力可视化）
- **备注**：第 8 期（范式篇收官）。建议重点看：§3.2（注意力与多头）、§3.5（位置编码）、§4（为什么自注意力 + Table 1）、§6.1（翻译结果 + Table 2）、§6.2（消融 + Table 3）。
