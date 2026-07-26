---
name: obsidian-knowledge-base
description: "Use when setting up or managing a personal knowledge base with Claude Code and Obsidian, when saving experiences from problem-solving, when compiling research into structured wiki pages, or when pulling content from the web into your knowledge base"
user-invocable: true
---

# 🧠 Obsidian Knowledge Base — 三种模式，一个大脑

## Overview

本 skill 提供三种知识管理模式的统一入口，覆盖从快速记录到深度编译再到网络拉取的完整场景。

```
你解决问题 / 看到文章 / 想到点子
  │
  ├─ 模式A：快速记录 ← 一句话保存经验到 笔记/
  ├─ 模式B：深度编译 ← 投入资料到 raw/ → 自动编织 wiki/
  └─ 模式C：网络拉取 ← web → raw/ → wiki/
  │
  ▼
  知识沉淀 → Obsidian 图谱 → 下次回答更准
```

## 🚀 一键初始化：/init-vault

**安装本 Skill 后，只需跑一次初始化命令，整个知识库自动搭建完成。**

### 如何触发

有两种方式：
- **输入 `/` 选择 `obsidian-knowledge-base`**，然后说"初始化知识库"
- **直接说**："帮我初始化知识库" 或 "帮我搭建一个知识库"

### Init 流程

当用户触发并说"初始化知识库"时，按以下步骤执行：

#### Step 1：确认路径
```
你：❓ 请指定 Vault 存放路径（直接回车则使用当前目录）：
用户：[输入路径或回车]
```

#### Step 2：创建目录结构
```创建
Vault/
├── 笔记/{技术,生活,项目}/
├── 项目/
├── 引用/
├── 日记/
├── raw/{01-articles,02-web,03-forum,09-archive}/
├── wiki/{concepts,entities,sources,syntheses}/
└── .claude/skills/
```

#### Step 3：拉取项目文件
使用 WebFetch 从 GitHub 获取以下文件并写入对应位置：
- `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/CLAUDE.md` → `Vault/CLAUDE.md`
- `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/wiki/index.md` → `Vault/wiki/index.md`
- `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/wiki/log.md` → `Vault/wiki/log.md`

#### Step 4：拉取并安装子 Skill
使用 WebFetch 从 GitHub 获取以下 SKILL.md 文件，保存到 `Vault/.claude/skills/<skill名>/SKILL.md`：
- `ingest_obsidian-knowledge` — `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/.claude/skills/ingest_obsidian-knowledge/SKILL.md`
- `query_obsidian-knowledge` — `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/.claude/skills/query_obsidian-knowledge/SKILL.md`
- `lint_obsidian-knowledge` — `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/.claude/skills/lint_obsidian-knowledge/SKILL.md`
- `web-pull_obsidian-knowledge` — `https://raw.githubusercontent.com/dragoncaosl/obsidian-knowledge-base/main/.claude/skills/web-pull_obsidian-knowledge/SKILL.md`

#### Step 5：完成提示
```
✅ 知识库初始化完成！

下一步：
  1. 打开 Obsidian → 打开本地文件夹 → 选择 Vault 路径
  2. 在 Claude Code 中开始使用：
     - "把这个经验存到知识库" → 模式A
     - "/ingest raw/..." → 模式B
     - "从 MDN 拉取 fetch API" → 模式C
```

### 注意
- 如果 WebFetch 拉取 GitHub 失败（网络问题），提示用户手动从 `https://github.com/dragoncaosl/obsidian-knowledge-base` 下载并复制文件
- 如果路径下已有文件，询问是否覆盖

---

## 三种模式速览

| 模式 | 你说的话 | Claude 做的事 | 适合场景 |
|------|---------|-------------|---------|
| **A：快速记录** | "把这个 Python 经验存到知识库" | 写 Markdown 到 `笔记/` + 记 Memory | 解决问题后随手记录 |
| **B：深度编译** | "把这篇文章编译进知识库" | 放 `raw/` → Ingest → 更新 `wiki/` 链接网络 | 研究课题、长期跟踪 |
| **C：网络拉取** | "从 MDN 拉取 fetch API 文档" | WebFetch → 存 `raw/02-web/` → 自动 Ingest | 从网站/Wiki/论坛采集 |

## 目录结构

```
Vault/
├── 📝 笔记/              ← [A] 快速经验记录
│   ├── 技术/
│   ├── 生活/
│   └── 项目/
├── 📁 项目/              ← [A] 项目资料
├── 📚 引用/              ← [A] 外部书摘
├── 📅 日记/              ← [A] Daily Notes
│
├── 📥 raw/               ← [B+C] 原始资料（仅 LLM 读，不修改）
│   ├── 01-articles/      ←   手动收集的文章
│   ├── 02-web/           ←   网络拉取的内容
│   ├── 03-forum/         ←   论坛/讨论帖
│   └── 09-archive/       ←   已处理（仅 LLM 可移动至此）
│
├── 🧠 wiki/              ← [B] LLM 编译的知识网络
│   ├── index.md           ←   全局索引（每次变更后更新）
│   ├── log.md             ←   操作日志（Append-only）
│   ├── concepts/          ←   概念、框架、方法论
│   ├── entities/          ←   人物、公司、工具、产品
│   ├── sources/           ←   原始资料摘要
│   └── syntheses/         ←   综合分析报告
│
└── .claude/skills/
    ├── obsidian-knowledge-base/SKILL.md        ← 本文件
    ├── ingest_obsidian-knowledge/SKILL.md      ← 编译管线
    ├── query_obsidian-knowledge/SKILL.md       ← 深度检索
    ├── lint_obsidian-knowledge/SKILL.md        ← 健康检查
    └── web-pull_obsidian-knowledge/SKILL.md    ← 网络拉取
```

## 模式详解

### 🟢 模式A：快速记录（Retain）

**一句话保存经验。** 解决问题后告诉 Claude "记住这个"。

```
你：把这个经验存到知识库 — pandas 读 CSV 用 encoding='gb2312'
→
Claude：
  1. 在 笔记/技术/ 下创建 Markdown
  2. 含场景 + 解决方案 + [[相关笔记]]
  3. 关键事实写入 Memory
```

**典型触发词**："存到知识库" / "记住这个" / "保存经验"

---

### 🔵 模式B：深度编译（Ingest/Query）

**Karpathy LLM Wiki 模式。** 投入原始资料 → LLM 自动编译成结构化知识网络。

**ingest 流程**：
```
你：编译 raw/01-articles/xxx.md
→
Claude：
  1. 读取源文件，提取实体（人/公司/工具）和概念（方法论/框架）
  2. 在 wiki/sources/ 创建摘要页
  3. 在 wiki/entities/ 和 wiki/concepts/ 创建/更新页面
  4. 发现知识冲突 → 暂停并询问
  5. 更新 index.md（全局目录）+ log.md（操作日志）
  6. 移动源文件到 raw/09-archive/
```

**query 流程**：
```
你：从知识库里查一下 Rust 所有权系统
→
Claude：
  1. 读取 wiki/index.md 定位相关页面
  2. 深度阅读 entities/concepts/sources 页面
  3. 综合回答 + [[双链引用]]
  4. 高质量回答 → 询问是否存为 synthesis
```

**典型触发词**："编译这篇" / "摄入" / "/ingest" / "查一下 XX"

---

### 🟣 模式C：网络拉取（Web Pull）

**从互联网自动获取信息。** 搜索 → 抓取 → 存 raw/ → 自动融入 wiki。

```
你：从 Rust 官网拉取所有权系统的教程
→
Claude：
  1. WebSearch 搜索目标 URL
  2. WebFetch 抓取内容
  3. 转 Markdown 存入 raw/02-web/
  4. 自动触发 Ingest（模式B流程）
```

**典型触发词**："从 XXX 拉取" / "抓取 XX 的内容" / "把 XX 网站的资料收入"

---

## 模式选择指南

```
遇到新信息 →
  是否只是解决了一个具体问题？
  ├─ 是 → 模式A：快速记录到 笔记/
  └─ 否 →
      是否来自网页/网络？
      ├─ 是 → 模式C：网络拉取 → 自动进入模式B
      └─ 否 → 模式B：手动放 raw/ → /ingest
```

## Quick Reference

| 你想做什么 | 对 Claude 说 |
|-----------|-------------|
| 快速保存经验 | "把这个存到知识库" / "记住这个" |
| 编译资料 | "编译这篇资料" / "/ingest raw/01-articles/xxx.md" |
| 网络拉取 | "从 MDN 拉取 fetch API 的内容" |
| 深度提问 | "从知识库里查一下 XXX" / "/query XXX" |
| 健康检查 | "检查知识库健康" / "/lint" |

## 注意事项

- **raw/ 目录 LLM 只读** — 除非归档，绝不修改源文件
- **wiki/ 页面必须含 `## 关联链接`** — 不能有孤岛页面
- **知识冲突时暂停询问** — 不要静默覆盖
- **模式A 和模式B 不冲突** — 快速笔记可升格为 wiki 页面
- **token 消耗** — 模式B/C 比模式A 消耗更多 token，注意使用频率
