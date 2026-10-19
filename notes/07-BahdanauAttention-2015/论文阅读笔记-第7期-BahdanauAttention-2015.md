# 论文阅读笔记 · 第 7 期（范式篇：Attention 起源）

> 定时任务「每两天读一篇论文」· 2026-10-19 触发 · 承接第 6 期：Seq2Seq 的「固定向量瓶颈」由本文的**注意力机制**直接对症解决。

## 基本信息

- **标题**：Neural Machine Translation by Jointly Learning to Align and Translate（RNNsearch / Bahdanau Attention）
- **作者/机构**：Dzmitry Bahdanau（Jacobs University Bremen）、KyungHyun Cho、Yoshua Bengio（Université de Montréal）
- **发表**：ICLR 2015（并获 ICLR 时间检验奖亚军）
- **链接**：arXiv:1409.0473（官方 PDF：https://arxiv.org/pdf/1409.0473）

## 一句话总结

> 基本 encoder-decoder 必须把整句压缩进一个**固定长度向量**，长句性能因此急剧下降。本文让解码器生成每个目标词时**软搜索（soft-search）源句中相关的词**：用可微的对齐模型为每个源词算权重 α，取源注解的加权和作为**逐词上下文向量 c_i**，并在双向 RNN 编码器上做联合训练——这就是注意力机制（attention）的起源。在 WMT'14 英法翻译上，单模型（无 UNK 句）BLEU 36.15，超过短语 SMT（Moses 35.63），长句不再退化。

## 为什么选这篇（本期理由）

- Roadmap「范式：Attention / Transformer」第一站：本文是 attention 的源头（比 Transformer 早三年），直接承接第 6 期的固定向量瓶颈；读懂「加法注意力」再读 Transformer 的 scaled dot-product attention 才顺。
- 经典双料：ICLR 2015 论文 + ICLR 时间检验奖亚军；Bengio 参与，是 NMT 三驾马车之一（Kalchbrenner & Blunsom、Sutskever et al.、Cho et al. 的集大成者）。
- 提供了 attention 的可视化证据（对齐矩阵），对「attention 为什么 work」的理解非常直观。

## 核心笔记

### 问题 Problem

- 基本 encoder-decoder（Sutskever 2014 / Cho 2014a）把整句压成一个固定向量 c（如 Seq2Seq 的 $h_T$），解码全程复用。
- **固定向量是信息瓶颈**：Cho et al. (2014b) 实证基本 encoder-decoder 的性能随句子长度急剧恶化，尤其是长于训练语料的句子——长句信息被「压扁」丢失。
- 传统 SMT 依赖显式词对齐（硬对齐/潜在变量），需处理短语长度不匹配（[NULL] 映射）等反直觉问题；神经模型应当端到端联合学习对齐。

### 方法 Method（RNNsearch）

1. **逐词上下文向量（核心）**：生成第 i 个目标词时，不再用固定 c，而是
   $$c_i=\sum_{j=1}^{T_x}\alpha_{ij}h_j$$
   其中 $\{h_j\}$ 是编码器对每个源词的**注解（annotation）**序列；$c_i$ 是「期望注解」（对可能对齐的期望）。
2. **软对齐模型（attention 本身）**：
   $$e_{ij}=a(s_{i-1},h_j),\quad \alpha_{ij}=\frac{\exp(e_{ij})}{\sum_k \exp(e_{ik})}$$
   $a$ 是单层前馈网络 $v_a^\top\tanh(W_a s_{i-1}+U_a h_j)$（加法注意力，additive attention），与整个模型**联合训练**——不是潜在变量、不是硬对齐，梯度直接回传。
3. **编码器：双向 RNN（BiRNN）**：正向 $\overrightarrow{h}_j$（读 $x_1..x_T$）+ 反向 $\overleftarrow{h}_j$（读 $x_T..x_1$），注解 $h_j=[\overrightarrow{h}_j;\overleftarrow{h}_j]$ 拼接——每个词同时看到前后文，注解聚焦该词周围。
4. **解码器**：GRU 门控单元（Cho 2014a，与 LSTM 同属门控、计算路径导数积≈1 缓解梯度消失），状态 $s_i=f(s_{i-1},y_{i-1},c_i)$；输出层用 maxout 深层输出（500 单元）。
5. **退化为基本模型**：若固定 $c_i=\overrightarrow{h}_{T_x}$，本文模型即 RNNencdec——以此验证「attention 带来的提升源于逐词检索而非其他改动」。
6. **实验设置**：WMT'14 英法（数据选择后 348M 词），词表各 30k + `[UNK]`（无 lowercasing/stemming）；hidden n=1000、词嵌入 m=620、maxout l=500、对齐模型 n'=1000；SGD + Adadelta、梯度 L2 裁剪到 1、minibatch 80 句、训练约 5 天（RNNsearch-50\* 252 小时）；beam search 解码。模型分别训练到源句长 ≤30 / ≤50 词。

### 效果 Results

- **BLEU（WMT'14 英→法 news-test-2014）**（Table 1）：

| 模型 | 全部句子 | 无 UNK 句 |
| --- | --- | --- |
| RNNencdec-30 | 13.93 | 24.19 |
| RNNsearch-30 | 21.50 | 31.44 |
| RNNencdec-50 | 17.82 | 26.71 |
| RNNsearch-50 | 26.75 | 34.16 |
| **RNNsearch-50\*** | **28.45** | **36.15** |
| Moses（短语 SMT，额外用 418M 词单语语料） | 33.30 | 35.63 |

  - 任何长度下 RNNsearch 都明显优于 RNNencdec；**无 UNK 句上 RNNsearch-50\*（36.15）超过 Moses（35.63）**，且 Moses 还额外使用了 4.18 亿词的单语语料。
  - RNNsearch-30 甚至超过 RNNencdec-50——attention 的增益超过「多训练 20 个词的长句」。
- **长句鲁棒性（Fig.2）**：RNNencdec 随句长急剧下滑；RNNsearch-50 在 50 词以上也不退化。
- **软对齐可视化（Fig.3）**：英法对齐大体单调（对角线强权重），但存在关键非单调对齐：`European Economic Area → zone économique européenne`（先跳两词对齐 `Area→zone`，再回看）；`the man → l'homme`（软对齐同时看 `the` 与 `man`，正确选出 `l'`，硬对齐做不到）；无需 [NULL] 映射。
- **定性长句（§5.2.2）**：RNNencdec-50 约 30 词后跑偏（把 "based on his status as a health care worker" 错译成 "en fonction de son état de santé"）；RNNsearch-50 完整保留原意。

### 局限 Limitations

- **未知词/稀有词**：词表 30k，`[UNK]` 处理粗糙；论文明确列为未来首要挑战（后由 subword/BPE 解决）。
- **计算开销**：每个目标词需对所有 $T_x$ 源词计算权重，$O(T_x\times T_y)$；对 15–40 词句可行，但可能限制更长序列/其他任务扩展（后由 Transformer 的并行化与线性/近似注意力改进）。
- **加法注意力**：$a(s_{i-1},h_j)$ 的 `tanh` 形式在长序列上不如后续 scaled dot-product 高效/稳定（Transformer 2017 的改进点）。
- 与 LSTM 的关系：本文用 GRU 单元，论文说明可用 LSTM 替代（Sutskever 2014 即如此），二者非本质区别。

## 扩展（可选）

- **创新点**：首次把「软对齐」做成**可微模块并与翻译模型联合训练**（attention 机制起源）；双向编码器提供词级前后文注解；摆脱固定向量 → 长句不再退化；对齐矩阵可解释。
- **相关工作**：Graves (2013) 手写合成用高斯核做单调对齐（单向移动，不适合翻译重排）；Kalchbrenner & Blunsom (2013) 与 Cho et al. (2014a) 的 encoder-decoder 框架；Sutskever et al. (2014) 用 LSTM 的 seq2seq（第 6 期）；Bengio et al. (2003) NNLM。
- **个人启发**：① attention 本质是「让模型自己决定看哪里」的可微路由——把「压缩」换成「检索」，信息瓶颈迎刃而解；② 本文的消融设计很干净（固定 c_i 即退化回 RNNencdec），验证了机制贡献而非工程堆料；③ 加法注意力 → 点积注意力 → 自注意力（Transformer）的演进每一步都对应一个明确的痛点，适合做理解「范式转移」的案例。
- **下一步问题**：attention 从「编码器-解码器之间」走向「自注意力 + 并行化」→ 下期读 Transformer（Attention Is All You Need, 2017）；另外可对比「加法注意力 vs 点积注意力」的动机。

## 阅读记录

- **阅读日期**：2026-10-19
- **阅读遍数**：精读（全文 3 段分页读完，含 Table 1/2/3、Fig.1 架构图、Fig.2 长句曲线、Fig.3 对齐热图、附录 A/B 模型细节）
- **备注**：第 7 期（范式篇）。建议重点看：§3（学习对齐与翻译）、Fig.1（模型结构）、Fig.2（长句曲线）、Fig.3（对齐可视化）、Table 1（BLEU 对比）、§A.1.2（对齐模型公式）。
