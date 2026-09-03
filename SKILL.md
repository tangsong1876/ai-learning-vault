---
name: ai-learning-vault
description: 个人 Obsidian 知识库的「搭建 + 半自动对话沉淀 + 每日维护 + git 快照」一体化方法论。当用户要搭建结构化个人知识库、治理 Obsidian vault（status 四态 / SCHEMA·INDEX·MAINTENANCE 三件套 / 20-笔记子目录分类）、配置"对话沉淀→收件箱→转正→笔记"闭环、或复刻 AI学习知识库的维护流程时使用。Trigger: "搭知识库" / "知识库治理" / "obsidian 流程" / "AI学习知识库" / "沉淀笔记" / "整理我的笔记"。
agent_created: true
---

# AI学习知识库流程（结构化 Obsidian 个人知识库方法论）

把任意 Obsidian vault 治理成「可沉淀、可审计、不乱」的个人第二大脑。本 skill 沉淀自 `D:\Obsidian\AI学习` 的实战配置：四态状态机 + 三件套治理 + 半自动对话沉淀闭环 + 每日维护自动化 + git 快照 + 每日黑匣子。

## 何时使用
- 从零搭建一个结构化个人知识库（视频/文章/对话 → 笔记）。
- 治理已有 vault：统一 status、补齐 SCHEMA/INDEX/MAINTENANCE、给笔记分目录。
- 配置"对话沉淀 → 收件箱 → 转正 → 笔记"的自动化闭环。
- 复刻本项目：把维护规则抽成 `scripts/`，由自动化定时跑。

## 核心目录结构
| 目录 | 用途 | 可改？ |
|---|---|---|
| `00-收件箱/` | 新生成/待确认的草稿（`status: draft`/`wip`）；`已转正/` 子目录存已处理副本 | ✅ 仅此新建 |
| `10-MOC/` | 主题地图，文件名 `MOC-` 开头 | ✅ |
| `20-笔记/` | 正式笔记，按类型分 `概念/` `课程/` `技能与工具/` `沉淀/` 子目录 | ✅ |
| `30-素材/` | 原始资料（视频/原网页/原文），**只读 raw 区，不改写不删除** | ❌ |
| `90-模板/` | 笔记模板（视频/概念/通用/MOC），均含 `uncertainty` + `## 演进` | ✅ |
| `outputs/` | `daily-report.md` 每日维护黑匣子 | ✅ |
| `scripts/` | 维护规则 prompt（权威单一来源，自动化引用） | ✅ |
| `INDEX.md`/`SCHEMA.md`/`MAINTENANCE.md`/`Home.md` | 治理与导航文档 | ✅ |

> `30-素材/` 是 raw 区：原件原样留存，笔记用 frontmatter `source` 指回它实现来源可溯。

## 关键规范（详见 references/SCHEMA.md）
- **status 四态**（全库只有这四种）：`draft`(待读) / `wip`(撰写中) / `reviewed`(已消化·终态) / `archived`(弃用/已转正副本)。禁用旧词（完整/已完成/已整理…）。
- **字段必填**：`title`(也是 WikiLink 名) / `type`(note|concept|moc|home) / `source` / `created` / `updated` / `status`。可选：`tags` / `confidence` / `uncertainty`(诚实标注) / `related` / `moc`。
- **`## 演进` 必填区块**：每篇必须含，新建记「初稿」，后续追加新区块不删旧内容（可审计）。
- **链接以 `title` 为准**：`[[链接名]]` 解析 frontmatter 的 `title`；改名即断链，须全库替换。
- **git 版本化**：vault 根 `git init`，`.gitignore` 忽略 `.obsidian`/`models`/`~*.md`；日常维护自动 `git commit`（维护即快照）。

## 半自动对话沉淀闭环（重要背景）
真·全自动沉淀在本环境**不可行**：`conversation_search` 云端未索引历史对话（实测返回 0），本地 `logs/` 仅为 SDK 遥测、无可读对话正文。因此采用半自动闭环——**内容由用户当场手喂**，绕过"读对话"缺口：
1. 用户在 WorkBuddy 会话发来想沉淀的要点。
2. AI 按"只留长期复用价值"写入 `00-收件箱/`（`status: draft` + 标签 `AI对话沉淀`/`待整理`）。
3. 用户阅读后把 `status` 改为 `reviewed`（即转正开关）。
4. 每晚 23:00「收件箱转正」自动化扫描 `reviewed`+`AI对话沉淀` → 入 `20-笔记/` 子目录 + 补 MOC + 原文件移 `已转正/`。
> 当 `conversation_search` 能返回当天对话时，可把「每日对话沉淀」自动化改回 ACTIVE，实现全自动。

## 复刻步骤
1. 按上表建目录 + `git init`。
2. 复制 `references/SCHEMA.md` 为 vault 的 `SCHEMA.md`；新建 `INDEX.md`/`MAINTENANCE.md`/`Home.md`（结构见 SCHEMA §1）。
3. 把 `references/` 下三个流程脚本放入 vault 的 `scripts/`（注意把硬编码 vault 路径改成你的实际路径）。
4. 创建三个 WorkBuddy 自动化（prompt 模板见下），与现有 vault 错峰。
5. 在 Obsidian 里套 `90-模板/` 新建笔记，保证 `## 演进` 区块与必填字段齐全。

## 自动化 Prompt 模板（可直接用于 WorkBuddy 自动化）

### ① 收件箱转正（每日 23:00，ACTIVE）
```
你是「收件箱转正助手」，服务于 <VAULT路径>。完整规则见 <VAULT路径>/scripts/inbox-promote.md（以它为准）。
【扫描】用 Glob 列出 <VAULT>/00-收件箱/*.md，找同时满足 status: reviewed 且 tags 含 AI对话沉淀 的笔记。
【目标子目录】type:concept→概念/；标题以「AI开窍计划」开头→课程/；标题含「沉淀」→沉淀/；其余→技能与工具/。
【转正】改写 frontmatter（去「待整理」、status:reviewed、补 uncertainty/confidence）→ 写入目标子目录 → 在对应 10-MOC/<MOC>.md 追加 - [[标题]] → 原文件移 00-收件箱/已转正/（status:archived, promoted_to:路径）。
【收尾】向 outputs/daily-report.md 按 ## YYYY-MM-DD 追加记录；git add -A && git commit -m "chore: 每日收件箱转正 YYYY-MM-DD"（nothing to commit 跳过）。
【红线】只处理 reviewed+AI对话沉淀；不删除；冲突先确认。
```

### ② 20-笔记整理（每日 21:00，ACTIVE）
```
你是「20-笔记整理助手」，服务于 <VAULT路径>。完整规则见 <VAULT>/scripts/notes-organize.prompt.md。
【步骤】Glob <VAULT>/20-笔记/*.md（仅根目录）找未归入子目录的文件 → 按 type/标题判定目标子目录（概念/课程/技能与工具/沉淀）→ 移动到子目录（双链按标题解析，移动不断链）→ 同步 INDEX（[[标题]] 链接）→ 列待确认项（不擅改）→ 向 outputs/daily-report.md 追加 → git add -A && git commit。
【边界】只在库内；不删不改正文；删除/覆盖/敏感/冲突只列待确认。
```

### ③ 每日对话沉淀（PAUSED，半自动）
```
半自动：用户投喂要点 → AI 写入 00-收件箱（status:draft, tags:AI对话沉淀/待整理）→ 用户改 reviewed 触发①。
筛选原则：只留可复用结论/方法/踩坑/概念/决策；不沉淀一次性问答/闲聊/敏感。来源可溯，不确定项记 uncertainty 绝不编造。
恢复条件：conversation_search 能返回当天对话 → 改回 ACTIVE 按天扫描。
```

## 校验脚本（可选）
全库一致性校验可扫：非法 status/type、空 source、缺 `## 演进` 区块、MOC 缺字段。修复原则：非法 type 归一 `note`；MOC 补 `status:reviewed`+`source`+演进；概念 `source` 诚实填「由 20-笔记 相关主题聚合提炼」；存量按 `created` 反推补录「初稿」。

## 红线
- 30-素材 raw 区不改写/不删除/不重命名；不确定项只记 `uncertainty`，绝不写回原件。
- 只处理用户明确标注（reviewed + 标签）的笔记，不改动的草稿永留收件箱。
- 删除/覆盖/敏感/事实冲突一律先确认。
- 来源可溯、不确定显式标注、不编造。
