# ai-learning-vault · 个人 Obsidian 知识库流程 skill

把任意 Obsidian vault 治理成「可沉淀、可审计、不乱」的个人第二大脑。本 skill 沉淀自 `D:\Obsidian\AI学习` 的实战配置。

## 它解决什么
- 视频 / 文章 / 对话 → 结构化笔记的闭环
- status 四态状态机 + SCHEMA·INDEX·MAINTENANCE 治理三件套
- 半自动「对话沉淀 → 收件箱 → 转正 → 20-笔记」自动化闭环
- 每日维护自动化（整理归档）+ git 版本快照 + 每日黑匣子

## 目录结构
```
ai-learning-vault/
├── SKILL.md                      # 方法论主文件：结构/规范/复刻步骤/自动化 prompt 模板
├── README.md                     # 本文件
└── references/
    ├── SCHEMA.md                 # 字段规范 + status 四态 + ## 演进 必填
    ├── inbox-promote.md          # 「收件箱转正」自动化权威规则
    ├── daily-sediment.md         # 「半自动对话沉淀」规则
    └── notes-organize.md         # 「20-笔记整理」自动化权威规则
```

## 关键设计
| 模块 | 做法 |
|---|---|
| 状态机 | `draft`/`wip`/`reviewed`/`archived` 四态，禁用旧词 |
| 来源可溯 | `source` 字段指回 `30-素材/` raw 区；不确定项写 `uncertainty`，绝不编造 |
| 知识生长 | `## 演进` 必填区块，旧内容不删，只追加 |
| 子目录归类 | `20-笔记/` 按 `概念/` `课程/` `技能与工具/` `沉淀/` 四分类 |
| 维护即快照 | 每次自动化收尾 `git add -A && git commit` |
| 黑匣子 | `outputs/daily-report.md` 按日期记录每天库的变化 |

## 关于「全自动对话沉淀」
真·全自动在本环境不可行：`conversation_search` 云端未索引历史对话，本地 `logs/` 仅为 SDK 遥测、无可读对话正文。故采用**半自动闭环**——内容由用户当场手喂，绕过"读对话"缺口。当平台开放对话历史可检索/可导出时，把「每日对话沉淀」自动化改回 ACTIVE 即可全自动。

## 复刻到自己的 vault
1. 按 SKILL.md「核心目录结构」建目录并 `git init`。
2. 把 `references/SCHEMA.md` 作为 vault 的 `SCHEMA.md`；新建 `INDEX.md`/`MAINTENANCE.md`/`Home.md`。
3. 把 `references/` 下 3 个流程脚本放入 vault 的 `scripts/`，**把硬编码的 vault 路径改成你自己的**。
4. 在 WorkBuddy 创建 3 个自动化（prompt 模板见 SKILL.md）。
5. 套 `90-模板/` 新建笔记，保证 `## 演进` 与必填字段齐全。

> ⚠️ 复刻者注意：`references/` 内的脚本路径硬编码为 `D:\Obsidian\AI学习`，使用前请全局替换为你的 vault 实际路径。
