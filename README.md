# 论文阅读笔记 · Paper Reading Notes

**中文** | [English](README.en.md)

> AI MSc 视角的结构化论文精读归档：每两天一篇，由豆包定时任务自动检索、精读并推送。
> 每期产出：结构化阅读笔记（Markdown）+ 论文结构示意图（HTML）。

![更新频率](https://img.shields.io/badge/%E6%9B%B4%E6%96%B0-%E6%AF%8F%E4%B8%A4%E5%A4%A9%E4%B8%80%E6%9C%9F-orange)
![论文数量](https://img.shields.io/badge/%E8%AE%BA%E6%96%87-1%E7%AF%87-blue)
![选题策略](https://img.shields.io/badge/%E9%80%89%E9%A2%98-%E5%85%88%E7%BB%BC%E8%BF%B0%E5%90%8E%E7%BB%8F%E5%85%B8-brightgreen)
![开源协议](https://img.shields.io/badge/License-MIT-green)

## 仓库内容

| 期数 | 论文 | 主题 | 难度 | 状态 |
| --- | --- | --- | --- | --- |
| 第 1 期 | Deep learning（LeCun, Bengio, Hinton, *Nature* 2015） | 深度学习综述 | 基础 | 已完成 |

> 后续每期生成后会自动追加本表。

## 每期交付物

进入 `notes/<期数>-<论文简称>/` 目录查看：

- **论文阅读笔记-第N期-<简称>.md** — 结构化精读笔记，固定四段骨架：
  问题 Problem → 方法 Method → 效果 Results → 局限 Limitations，并附创新点、个人启发、下一步问题；
- **论文结构示意图-第N期-<简称>.html** — 论文结构/方法/对比示意图，浏览器直接打开。

## 阅读路线（Roadmap）

按「先综述 → 再经典 → 工具发现」由浅入深：

- [x] 综述：Deep learning（Nature 2015）
- [ ] 经典：AlexNet（2012）
- [ ] 经典：VGG / ResNet
- [ ] 经典：LSTM / Seq2Seq
- [ ] 范式：Attention / Transformer
- [ ] 前沿：按研究方向动态选题（覆盖路径规划 / AI 伦理 / 社交媒体与福祉）

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
- 流程：检索候选 → 筛 1 篇 → 读全文 → 生成笔记 + 示意图 → 推送到本仓库（含 README 维护）；
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
│   └── 01-DeepLearning-2015/
└── skills/                # 配套开源 Skill
    └── efficient-paper-reading/
        ├── SKILL.md
        ├── references/
        └── assets/
```

## 开源协议

本仓库采用 [MIT License](LICENSE)。笔记内容与 Skill 均可自由使用、修改与分发，但须保留版权声明。

## 免责声明

笔记为个人学习整理的辅助材料，可能存在理解偏差或时效局限，请以原论文为准；本仓库为公开仓库，请勿提交任何账号、密钥、个人隐私等敏感信息。
