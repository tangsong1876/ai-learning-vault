# 每日对话沉淀规则（scripts/daily-sediment.md）

> 半自动闭环规则。供「AI学习库·每日对话沉淀」自动化（当前 PAUSED）与 `00-收件箱/README.md` 引用。
> 适用范围：`D:\Obsidian\AI学习`
> 配套：scripts/inbox-promote.md（转正规则）· MAINTENANCE.md §2

## 半自动闭环
1. **投喂**：用户在 WorkBuddy 会话发来想沉淀的对话 / 要点。
2. **写入**：AI 按"只留长期复用价值"规则写入 `00-收件箱/`（`status: draft` + 标签 `AI对话沉淀` / `待整理`）。
3. **确认**：用户阅读后把 `status` 改为 `reviewed`（这一改动即转正开关）。
4. **转正**：每晚 23:00「收件箱转正」自动化扫描 `reviewed` + `AI对话沉淀` → 入 `20-笔记/` + 补 MOC + 原文件移 `已转正/`（详见 scripts/inbox-promote.md）。

## 筛选原则（只留长期复用价值）
- **可沉淀**：可复用的结论 / 方法 / 踩坑 / 概念 / 决策；跨会话仍有用的经验。
- **不沉淀**：一次性问答、闲聊、临时查询、与 AI 学习无关内容、含敏感信息。
- **来源可溯**：笔记 `source` 指向原对话主题或链接；不确定项记 `uncertainty`，绝不编造。

## 全自动恢复条件
当 `conversation_search` 能返回当天对话时，把「AI学习库·每日对话沉淀」自动化改回 ACTIVE，按天自动扫描，无需手动投喂。
