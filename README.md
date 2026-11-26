# 论文阅读笔记 · Paper Reading Notes
**中文** | [English](README.en.md)
> AI 学习视角的结构化论文精读归档：每两天一篇，由豆包定时任务自动检索、精读并推送。
> 每期产出：结构化阅读笔记（Markdown）+ 论文结构示意图（HTML）。
![更新频率](https://img.shields.io/badge/Update-every%202%20days-orange)
![论文数量](https://img.shields.io/badge/%E8%AE%BA%E6%96%87-25%E7%AF%87-blue)
![选题策略](https://img.shields.io/badge/Strategy-survey%20first%2C%20then%20classics-brightgreen)
![开源协议](https://img.shields.io/badge/License-MIT-green)
## 仓库内容
| 期数 | 论文 | 主题 | 难度 | 状态 |
| --- | --- | --- | --- | --- |
| 第 1 期 | Deep learning（LeCun, Bengio, Hinton, *Nature* 2015） | 深度学习综述 | 基础 | 已完成 |
| 第 2 期 | ImageNet Classification with Deep CNNs（Krizhevsky et al., NIPS 2012） | 经典·视觉 | 经典 | 已完成 |
| 第 3 期 | Very Deep Convolutional Networks（Simonyan & Zisserman, ICLR 2015） | 经典·视觉 | 经典 | 已完成 |
| 第 4 期 | Deep Residual Learning for Image Recognition（He et al., CVPR 2016） | 经典·视觉 | 经典 | 已完成 |
| 第 5 期 | Long Short-Term Memory（Hochreiter & Schmidhuber, Neural Computation 1997） | 经典·序列 | 经典 | 已完成 |
| 第 6 期 | Sequence to Sequence Learning with Neural Networks（Sutskever et al., NIPS 2014） | 经典·序列 | 经典 | 已完成 |
| 第 7 期 | Neural Machine Translation by Jointly Learning to Align and Translate（Bahdanau et al., ICLR 2015） | 范式·Attention | 经典 | 已完成 |
| 第 8 期 | Attention Is All You Need（Vaswani et al., NeurIPS 2017） | 范式·Transformer | 经典 | 已完成 |
| 第 9 期 | A Survey on Coverage Path Planning for Robotics（Galceran & Carreras, RAS 2013） | 前沿·覆盖路径规划 | 前沿 | 已完成 |
| 第 10 期 | Media use and brain development during adolescence（Crone & Konijn, Nat Commun 2018） | 前沿·社交媒体与福祉 | 前沿 | 已完成 |
| 第 11 期 | Gender Shades: Intersectional Accuracy Disparities in Commercial Gender Classification（Buolamwini & Gebru, FAT* 2018） | 前沿·AI 伦理 | 前沿 | 已完成 |
| 第 12 期 | Demystifying the Draft EU Artificial Intelligence Act（Veale & Borgesius, COMLAW 2021） | 前沿·AI 监管 | 前沿 | 已完成 |
| 第 13 期 | Learning Coverage Paths in Unknown Environments with Reinforcement Learning（Jonnarth et al., arXiv 2023） | 前沿·CPP × 深度强化学习 | 前沿 | 已完成 |
| 第 14 期 | An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale（Dosovitskiy et al., ICLR 2021） | 前沿·视觉 Transformer | 前沿 | 已完成 |
| 第 15 期 | BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding（Devlin et al., NAACL 2019） | 范式·预训练语言模型 | 经典 | 已完成 |
| 第 16 期 | Learning Transferable Visual Models From Natural Language Supervision（Radford et al., ICML 2021） | 范式·多模态对比学习 | 经典 | 已完成 |
| 第 17 期 | Denoising Diffusion Probabilistic Models（Ho, Jain & Abbeel, NeurIPS 2020） | 范式·生成模型（扩散） | 中级 | 已完成 |
| 第 18 期 | Denoising Diffusion Implicit Models（Song, Meng & Ermon, ICLR 2021） | 范式·生成模型（加速采样） | 中级 | 已完成 |
| 第 19 期 | Diffusion Models Beat GANs on Image Synthesis（Dhariwal & Nichol, NeurIPS 2021） | 范式·生成模型（架构与引导） | 中级 | 已完成 |
| 第 20 期 | Classifier-Free Diffusion Guidance（Ho & Salimans, NeurIPS 2021 Workshop / arXiv 2022） | 范式·生成模型（无分类器引导） | 中级 | 已完成 |
| 第 21 期 | High-Resolution Image Synthesis with Latent Diffusion Models（Rombach et al., CVPR 2022 Oral） | 范式·生成模型（潜空间扩散与通用条件接口） | 中级 | 已完成 |
| 第 22 期 | Diffusion Policy: Visuomotor Policy Learning via Action Diffusion（Chi et al., RSS 2023 / IJRR 2025） | 前沿·机器人学习（扩散策略） | 中级 | 已完成 |
| 第 23 期 | Scalable Diffusion Models with Transformers（Peebles & Xie, ICCV 2023） | 范式·生成模型（Transformer 骨干与缩放） | 中级 | 已完成 |
| 第 24 期 | Learning to Recharge: UAV Coverage Path Planning through Deep Reinforcement Learning（Theile et al., arXiv 2023） | 前沿·CPP × 深度强化学习（能量约束与充电） | 中级 | 已完成 |
| 第 25 期 | Large-Scale Multi-Robot Coverage Path Planning via Local Search（Tang & Ma, AAAI 2024） | 前沿·多机器人覆盖路径规划（局部搜索与强基线） | 中级 | 已完成 |
> 后续每期生成后会自动追加本表。
## 每期交付物
进入 `notes/<期数>-<论文简称>/` 目录查看：
- **论文阅读笔记-第N期-<简称>.md** — 结构化精读笔记，固定四段骨架：
  问题 Problem → 方法 Method → 效果 Results → 局限 Limitations，并附创新点、个人启发、下一步问题；
- **论文结构示意图-第N期-<简称>.html** — 论文结构/方法/对比示意图，浏览器直接打开。
## 阅读路线（Roadmap）
按「先综述 → 再经典 → 工具发现」由浅入深：
- [x] 综述：Deep learning（Nature 2015）
- [x] 经典：AlexNet（2012）
- [x] 经典：VGG（2015）
- [x] 经典：ResNet（2016）
- [x] 经典：LSTM（1997）
- [x] 经典：Seq2Seq（2014）
- [x] 范式：Attention（Bahdanau 2015）
- [x] 范式：Transformer（Attention Is All You Need, 2017）
- [x] 前沿：覆盖路径规划（Coverage Path Planning Survey, 2013）
- [x] 前沿：社交媒体与青少年福祉（Media & Brain Development, 2018）
- [x] 前沿：AI 伦理 / 监管（Gender Shades, FAT* 2018）
- [x] 前沿：AI 监管法规（EU AI Act, Veale & Borgesius 2021）
- [x] 前沿：覆盖路径规划 × 深度强化学习（CPP with RL, Jonnarth et al. 2023）
- [x] 前沿：视觉 Transformer（ViT, ICLR 2021）
- [x] 范式：预训练语言模型（BERT, NAACL 2019）
- [x] 范式：多模态对比预训练（CLIP, ICML 2021）
- [x] 范式：生成模型 · 扩散（DDPM, NeurIPS 2020）
- [x] 范式：生成模型 · 加速采样（DDIM, ICLR 2021）
- [x] 范式：生成模型 · 架构与分类器引导（ADM, NeurIPS 2021）
- [x] 范式：生成模型 · 无分类器引导（CFG, NeurIPS 2021 Workshop）
- [x] 范式：生成模型 · 潜空间扩散（LDM, CVPR 2022 Oral）
- [x] 前沿：机器人策略 · 扩散策略（Diffusion Policy, RSS 2023）
- [x] 范式：生成模型 · Transformer 骨干（DiT, ICCV 2023）
- [x] 前沿：覆盖路径规划 · 能量约束与充电（Learning to Recharge, arXiv 2023）
- [x] 前沿：多机器人覆盖 · 局部搜索与强基线（LS-MCPP, AAAI 2024）
## 选题策略
- 难度：从基础入门论文开始，随期数递增；
- 方向：深度学习基础、移动机器人覆盖路径规划、AI 伦理与监管、社交媒体与青少年福祉；
- 来源：arXiv、Semantic Scholar、ACM、IEEE 等公开文献，优先官方开放全文。
## 配套 Skill（已开源）
本仓库同时开源了驱动这套流程的 **efficient-paper-reading** 技能，位于 `skills/efficient-paper-reading/`：
- `SKILL.md` — 高效阅读工作流：摘要/结论先行、分层精读、固定笔记模板；
- `references/method.md` — 选论文策略（先综述 → 再经典 → 工具发现）与阅读方法论；
- `references/note-template.md` 与 `assets/note-template.md` — 可复用的笔记模板。
> 该 Skill 由豆包定时任务调用，也可独立复用于任何 AI 助手的论文阅读与精读场景（按各自平台的 Skill 规范放置即可）。
## 更新机制
- 定时任务「每两天读一篇论文」：每 2 天 20:30（Europe/London）触发；
- 流程：检索候选 → 筛 1 篇 → 读全文 → 生成笔记 + 示意图 → 推送到本仓库（含 README 维护）→ 写入 Obsidian vault（或交付 Obsidian 版文件）；
- 若某期检索失败，会更换公开来源重试，不会跳过。
## 使用建议
- 网页端直接浏览本仓库即可阅读 Markdown 笔记；
- HTML 示意图可下载后用浏览器打开，或 clone 到本地查看；
- 阅读顺序：先摘要 + 结论建立全局 → 再按方法分层精读 → 最后回看示意图。
## 目录结构
```
.
├── README.md              # 中文说明
├── README.en.md           # English version
├── LICENSE                # MIT 开源协议
├── notes/                 # 每期论文阅读笔记
│   ├── 01-DeepLearning-2015/
│   ├── 02-AlexNet-2012/
│   ├── 03-VGG-2015/
│   ├── 04-ResNet-2016/
│   ├── 05-LSTM-1997/
│   ├── 06-Seq2Seq-2014/
│   ├── 07-BahdanauAttention-2015/
│   ├── 08-Transformer-2017/
│   ├── 09-CPPSurvey-2013/
│   ├── 10-MediaBrain-2018/
│   ├── 11-GenderShades-2018/
│   ├── 12-EUAIAct-2021/
│   ├── 13-CPPDRL-2023/
│   ├── 14-ViT-2021/
│   ├── 15-BERT-2019/
│   ├── 16-CLIP-2021/
│   ├── 17-DDPM-2020/
│   ├── 18-DDIM-2021/
│   ├── 19-ADM-2021/
│   ├── 20-CFG-2022/
│   ├── 21-LDM-2022/
│   ├── 22-DiffusionPolicy-2023/
│   ├── 23-DiT-2023/
│   ├── 24-CPPRecharge-2023/
│   └── 25-LSMCPP-2024/
└── skills/                 # 配套开源 Skill
    └── efficient-paper-reading/
        ├── SKILL.md
        ├── references/
        └── assets/
```
## 开源协议
本仓库采用 [MIT License](LICENSE)。笔记内容与 Skill 均可自由使用、修改与分发，但须保留版权声明。
## 免责声明
笔记为个人学习整理的辅助材料，可能存在理解偏差或时效局限，请以原论文为准；本仓库为公开仓库，请勿提交任何账号、密钥、个人隐私等敏感信息。