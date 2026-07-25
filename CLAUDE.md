# 🧠 个人知识库 — 项目指令

本仓库是 Claude Code + Obsidian 个人知识库的模板 Vault，支持三种知识管理模式。

## 目录结构

```
obsidian-knowledge-base/        ← Vault 根目录
│
├── 📝 笔记/                    ← [A] 日常快速笔记
│   ├── 技术/                   ←   编程、工具、架构
│   ├── 生活/                   ←   生活记录、感悟
│   └── 项目/                   ←   项目相关
├── 📁 项目/                    ← [A] 每个项目独立子目录
├── 📚 引用/                    ← [A] 外部资料、书摘
├── 📅 日记/                    ← [A] Daily Notes
│
├── 📥 raw/                     ← [B+C] 原始资料（只读层）
│   ├── 01-articles/            ←   手动收集的文章
│   ├── 02-web/                 ←   网络拉取的内容
│   ├── 03-forum/               ←   论坛/讨论帖
│   └── 09-archive/             ←   已处理归档（跳过）
│
├── 🧠 wiki/                    ← [B] LLM 编译的知识网络
│   ├── index.md                ←   全局索引
│   ├── log.md                  ←   操作日志（Append-only）
│   ├── concepts/               ←   概念、框架、方法论
│   ├── entities/               ←   人物、公司、工具、产品
│   ├── sources/                ←   原始资料摘要
│   └── syntheses/              ←   综合分析报告
│
└── .claude/
    └── skills/
        ├── obsidian-knowledge-base/SKILL.md  ← 主 Skill
        ├── ingest/SKILL.md                   ← 编译管线
        ├── query/SKILL.md                    ← 深度检索
        ├── lint/SKILL.md                     ← 健康检查
        └── web-pull/SKILL.md                 ← 网络拉取
```

## 三种工作模式

### 🟢 模式A：快速记录

**触发词**："记住这个" / "存到知识库" / "保存经验"

流程：
1. 判断分类：技术问题 → `笔记/技术/`，生活 → `笔记/生活/`，项目 → `笔记/项目/`
2. 创建 Markdown 文件（含 frontmatter + `[[wikilinks]]`）
3. 关键事实写入 Memory

```markdown
---
created: {{date}}
tags: []
---

# 标题

## 场景

## 解决方案

## 要点

## 相关笔记
[[笔记1]] [[笔记2]]
```

### 🔵 模式B：深度编译（Karpathy LLM Wiki）

**触发词**："编译这篇" / "/ingest" / "/query"

**ingest 规则**（raw/ → wiki/）：
- `raw/` 只读，LLM 绝不修改源文件内容
- 处理完成后将源文件移动到 `raw/09-archive/`
- 创建/更新 `wiki/sources/`、`wiki/entities/`、`wiki/concepts/` 页面
- 每次变更后更新 `wiki/index.md` 和 `wiki/log.md`
- 发现知识冲突时暂停询问

**query 规则**（wiki/ 检索）：
- 先读 `wiki/index.md` 定位 → 再深度阅读相关页面
- 回答必须带 `[[双链引用]]`
- 高质量回答可存为 `wiki/syntheses/`

**wiki 页面规范**：
```markdown
---
title: "页面名称"
type: entity | concept | source | synthesis
tags: [标签]
sources: [关联文件路径]
last_updated: YYYY-MM-DD
---

## 定义/摘要

## 关键信息

## 关联链接
- [[相关页面]]
```
- 实体用 TitleCase 命名（如 `RustLanguage`）
- 概念/来源用 kebab-case（如 `ownership-system`）
- **所有 wiki 页面必须含 `## 关联链接`**，不能有孤岛页面

### 🟣 模式C：网络拉取

**触发词**："从 XXX 拉取" / "抓取 XX 内容"

流程：
1. WebSearch 搜索目标 → WebFetch 抓取内容
2. 转 Markdown 存入 `raw/02-web/`（含 `source_url`）
3. 自动触发 Ingest 编译到 wiki

## 通用规范

- **语言**：使用简体中文编写所有内容
- **格式**：统一 Markdown，UTF-8 编码
- **双向链接**：优先使用 `[[wikilink]]` 语法
- **冲突处理**：发现新旧知识冲突时暂停询问，不静默覆盖
- **token 注意**：模式B/C 消耗更多 token，合理安排使用频率
