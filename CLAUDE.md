# 🧠 个人知识库 — 项目指令 / Project Instructions

本仓库是 Claude Code + Obsidian 个人知识库的模板 Vault，支持三种知识管理模式。
This is a template Vault for Claude Code + Obsidian personal knowledge base, supporting 3 knowledge management modes.

---

## 目录结构 / Directory Structure

```
your-vault/
├── 📝 笔记/                 ← [A] 快速记录 / Quick notes
│   ├── 技术/ 生活/ 项目/
├── 📁 项目/                 ← [A] 项目资料 / Projects
├── 📚 引用/                 ← [A] 书摘 / References
├── 📅 日记/                 ← [A] Daily Notes
├── 📥 raw/                  ← [B+C] 原始资料 / Raw sources (read-only)
│   └── 01-articles/ 02-web/ 03-forum/ 09-archive/
├── 🧠 wiki/                 ← [B] 编译知识 / Compiled knowledge
│   ├── index.md + log.md
│   └── concepts/ entities/ sources/ syntheses/
└── .claude/skills/
    ├── obsidian-knowledge-base/   ingest_obsidian-knowledge/
    ├── query_obsidian-knowledge/  lint_obsidian-knowledge/
    └── web-pull_obsidian-knowledge/
```

---

## 三种工作模式 / Three Modes

### 🟢 模式 A：快速记录 / Quick Save

**触发词 / Triggers:** "记住这个" / "存到知识库" / "保存经验"

流程 / Flow:
1. 判断分类 → `笔记/技术|生活|项目/`
2. 创建 Markdown（frontmatter + `[[wikilinks]]`）
3. 写入 Memory

```markdown
---
created: {{date}}
tags: []
---

# 标题 / Title

## 场景 / Context
<!-- 什么情况下遇到这个问题？/ What was the situation? -->

## 解决方案 / Solution
<!-- 怎么解决的？/ How was it solved? -->

## 要点 / Key Points
<!-- 核心结论 / Conclusions -->

## 相关笔记 / Related Notes
[[笔记1]] [[笔记2]]
```

### 🔵 模式 B：深度编译 / Deep Compile (Karpathy LLM Wiki)

**触发词 / Triggers:** "编译这篇" / "/ingest_obsidian-knowledge" / "/query_obsidian-knowledge"

**Ingest 规则 / Rules (raw/ → wiki/):**
- `raw/` 只读，不修改源文件 / Read-only, never modify source files
- 处理后归档到 `raw/09-archive/` / Archive after processing
- 创建/更新 `wiki/sources/`, `wiki/entities/`, `wiki/concepts/`
- 更新 `wiki/index.md` + `wiki/log.md`
- **知识冲突时暂停询问** / **Pause and ask on conflicts**

**Query 规则 / Rules (wiki/ search):**
- 先读 `index.md` 定位 → 深度阅读 / Read index first → deep read
- 回答用 `[[双链引用]]` / Answer with `[[wikilinks]]`
- 高质量回答可存 `wiki/syntheses/` / Good answers saved to syntheses

**Wiki 页面规范 / Page Spec:**
```markdown
---
title: "页面名称 / Page Name"
type: entity | concept | source | synthesis
tags: [标签 / tags]
sources: [关联文件路径 / source file path]
last_updated: YYYY-MM-DD
---

## 定义/摘要 / Definition/Summary

## 关键信息 / Key Information

## 关联链接 / Related Links
- [[相关页面 / Related Page]]
```
- 实体用 TitleCase（如 `RustLanguage`），概念用 kebab-case（如 `ownership-system`）
- **所有页面必须含 `## 关联链接`** / **Every page MUST have `## Related Links`**

### 🟣 模式 C：网络拉取 / Web Pull

**触发词 / Triggers:** "从 XXX 拉取" / "抓取 XX 内容"

流程 / Flow:
1. WebSearch → WebFetch
2. 转 Markdown 存 `raw/02-web/`（含 `source_url`）
3. 自动触发 Ingest → wiki

---

## 通用规范 / General Rules

| 中文 | English |
|------|---------|
| 使用简体中文编写 | Write in Simplified Chinese |
| 统一 Markdown，UTF-8 | Use Markdown, UTF-8 |
| `[[wikilinks]]` 建立关联 | Use `[[wikilinks]]` for relations |
| 知识冲突时暂停，不静默覆盖 | Pause on conflicts, never silently overwrite |
| 模式B/C token 消耗较大 | Mode B/C consumes more tokens |
