# Paper Reading Notes

**English** | [中文](README.md)

> A structured archive of paper reading notes from an AI MSc perspective — one paper every two days, automatically searched, read and pushed by a scheduled Doubao task.
> Each issue ships: a structured reading note (Markdown) + a paper structure diagram (HTML).

![Update](https://img.shields.io/badge/Update-every%202%20days-orange)
![Papers](https://img.shields.io/badge/Papers-1-blue)
![Strategy](https://img.shields.io/badge/Strategy-survey%20first%2C%20then%20classics-brightgreen)
![License](https://img.shields.io/badge/License-MIT-green)

## Contents

| Issue | Paper | Topic | Level | Status |
| --- | --- | --- | --- | --- |
| 1 | Deep learning (LeCun, Bengio, Hinton, *Nature* 2015) | Deep learning survey | Beginner | Done |

> New issues are appended automatically.

## Per-Issue Deliverables

Inside `notes/<issue>-<paper-short-name>/`:

- **论文阅读笔记-第N期-<short>.md** — structured reading note with a fixed skeleton:
  Problem → Method → Results → Limitations, plus innovation, personal takeaways and next questions;
- **论文结构示意图-第N期-<short>.html** — a structure/method/comparison diagram, open directly in a browser.

## Reading Roadmap

Progression: survey first, then classics, then discovery tools.

- [x] Survey: Deep learning (Nature 2015)
- [ ] Classic: AlexNet (2012)
- [ ] Classic: VGG / ResNet
- [ ] Classic: LSTM / Seq2Seq
- [ ] Paradigm: Attention / Transformer
- [ ] Frontier: selected dynamically by research direction (coverage path planning / AI ethics / social media & wellbeing)

## Topic Strategy

- Difficulty: starts from beginner-friendly papers and increases issue by issue;
- Directions: deep learning fundamentals, coverage path planning for mobile robots, AI ethics & regulation, social media & youth wellbeing;
- Sources: public literature (arXiv, Semantic Scholar, ACM, IEEE), preferring official open-access full texts.

## Bundled Skill (open source)

The **efficient-paper-reading** skill that powers this workflow is also open-sourced in `skills/efficient-paper-reading/`:

- `SKILL.md` — the reading workflow: abstract/conclusion first, layered close reading, fixed note template;
- `references/method.md` — paper-selection strategy (survey → classics → discovery) and reading methodology;
- `references/note-template.md` and `assets/note-template.md` — reusable note templates.

> The skill is invoked by the Doubao scheduled task and can also be reused independently in any AI assistant's paper-reading workflow (place it according to your platform's skill conventions).

## Update Mechanism

- Scheduled task "One paper every two days": triggered every 2 days at 20:30 (Europe/London);
- Pipeline: search candidates → pick one → read full text → generate note + diagram → push to this repo (including README maintenance);
- If retrieval fails, it retries with alternative public sources — no issue is skipped.

## Usage

- Read Markdown notes directly on GitHub;
- Download HTML diagrams and open them in a browser, or clone the repo locally;
- Reading order: abstract + conclusion first, then layered close reading, then review the diagram.

## Repository Layout

```
.
├── README.md              # Chinese
├── README.en.md           # English
├── LICENSE                # MIT
├── notes/                 # per-issue reading notes
│   └── 01-DeepLearning-2015/
└── skills/                # bundled open-source skill
    └── efficient-paper-reading/
        ├── SKILL.md
        ├── references/
        └── assets/
```

## License

This repository is released under the [MIT License](LICENSE). The notes and the skill may be freely used, modified and distributed provided the copyright notice is retained.

## Disclaimer

The notes are personal study aids and may contain misunderstandings or become outdated; the original papers remain authoritative. This is a public repository — do not commit any accounts, credentials, private data or internal materials.
