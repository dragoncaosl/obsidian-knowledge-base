# 🧠 Obsidian Knowledge Base

> **v1.1.0** — 快速记录 × 深度编译 × 网络拉取。越用越聪明。
> **v1.1.0** — Quick Save × Deep Compile × Web Pull. Get Smarter Every Day.

[![Version](https://img.shields.io/badge/version-v1.1.0-blue)](#)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Ready-blue)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-Ready-purple)](#)
[![CC Switch](https://img.shields.io/badge/CC%20Switch-Ready-green)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](#)

---

## 📖 这是什么？/ What is this?

**中文：** 一个融合三种知识管理模式的 Claude Code + Obsidian 个人知识库方案。只需安装一个 Skill，跑一条命令，自动搭好整套环境。

**English:** A personal knowledge base solution combining three knowledge management modes with Claude Code + Obsidian. Install one Skill, run one command, auto-setup everything.

| Mode | 中文 | English | Inspired by |
|------|------|---------|-------------|
| 🟢 **A** | 快记 — 一句话保存经验 | Quick Save | 少即是多理念 |
| 🔵 **B** | 编译 — raw/ → wiki/ 知识网络 | Deep Compile | [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) |
| 🟣 **C** | 拉取 — 从网络抓取自动入库 | Web Pull | — |

```
🧠 飞轮 / Flywheel:
你遇到问题 / 看到文章 / 想到点子  →  存入知识库  →  下次回答更准
```

---

## ✨ 功能特性 / Features

| 中文 | English |
|------|---------|
| 🚀 三合一：快记 + LLM Wiki + 网络拉取 | All-in-one: Quick Save + LLM Wiki + Web Pull |
| 🧠 跨会话记忆 (Memory) | Cross-session memory via Claude Code Memory |
| 📝 纯 Markdown，零 Vendor Lock-in | Pure Markdown, zero vendor lock-in |
| 🔗 `[[wikilinks]]` 自动构建知识图谱 | Auto-built knowledge graph via `[[wikilinks]]` |
| 🌐 从技术网站/Wiki/论坛拉取内容 | Pull content from tech sites, wikis, forums |
| 📦 一键初始化，自动安装 Obsidian | One-command init, auto-install Obsidian |

---

## 🚀 快速开始 / Quick Start

> **仅需 3 步 / Just 3 steps**

### 1️⃣ 下载总包 ZIP / Download Bundle ZIP

| 方式 / Method | 操作 / Action |
|--------------|--------------|
| ⭐ **Releases** (推荐) | 打开 [Releases 页面](https://github.com/dragoncaosl/obsidian-knowledge-base/releases) → 下载 `obsidian-knowledge-base-bundle.zip` |

### 2️⃣ CC Switch 安装 / Install via CC Switch

1. 打开 **CC Switch** → **Skills** 面板 / Open CC Switch → Skills tab
2. 点击 **「从 ZIP 安装」** / Click "Install from ZIP"
3. 选择下载的 ZIP / Select the downloaded ZIP
4. ✅ **5 个 Skill 一次装好 / All 5 skills installed at once**

### 3️⃣ 一键初始化 / One-Click Init

```bash
# 在新目录中启动 Claude Code / Start Claude Code in a new directory
cd D:\你的知识库目录       # 中文用户 / Chinese users
cd ~/your-vault-path       # English users
claude
```

在 Claude Code 中 / In Claude Code:

```
/obsidian-knowledge-base
```

选择 **"1. 初始化知识库"** → 自动：

| 步骤 | 中文 | English |
|------|------|---------|
| 📦 | 安装 Obsidian（如未安装） | Install Obsidian (if missing) |
| 📁 | 创建完整目录结构 | Create full directory structure |
| 📄 | 拉取 CLAUDE.md + wiki 文件 | Pull CLAUDE.md + wiki files |
| 🎯 | 安装全部 5 个子 Skill | Install all 5 sub-skills |
| ✅ | 完成！告诉你下一步 | Done! Show next steps |

---

## 📁 目录结构 / Directory Structure

```
your-vault/
├── 📝 笔记/                 ← [A] 快速记录 / Quick notes
│   ├── 技术/ 生活/ 项目/    ←    Tech / Life / Projects
├── 📁 项目/                 ← [A] 项目资料 / Project files
├── 📚 引用/                 ← [A] 书摘 / References
├── 📅 日记/                 ← [A] Daily Notes
├── 📥 raw/                  ← [B+C] 原始资料 / Raw sources
│   └── 01-articles/ 02-web/ 03-forum/ 09-archive/
├── 🧠 wiki/                 ← [B] 编译知识 / Compiled knowledge
│   ├── index.md + log.md
│   └── concepts/ entities/ sources/ syntheses/
└── .claude/skills/          ← 5 Skills
    ├── obsidian-knowledge-base/  ingest_obsidian-knowledge/
    ├── query_obsidian-knowledge/ lint_obsidian-knowledge/
    └── web-pull_obsidian-knowledge/
```

---

## 🎯 5 个 Skill / 5 Skills

| Skill | 中文 | English |
|-------|------|---------|
| `obsidian-knowledge-base` | 🧠 主入口 — 三种模式 + 初始化 | Main entry — 3 modes + init |
| `ingest_obsidian-knowledge` | 📥 编译 raw/ → wiki/ 管线 | Compile raw/ → wiki/ pipeline |
| `query_obsidian-knowledge` | 🔍 wiki 深度检索 | Deep wiki search |
| `lint_obsidian-knowledge` | 🩺 健康检查（死链/孤岛/冲突） | Health check (dead links/orphans) |
| `web-pull_obsidian-knowledge` | 🌐 从网络拉取内容 | Pull content from web |

---

## 📖 使用指南 / Usage Guide

### 🟢 模式 A：快速记录 / Quick Save

| 场景 / Scenario | 说 / Say |
|----------------|---------|
| 保存经验 | "把这个经验存到知识库" |
| 指定分类 | "存到 笔记/技术/ 下" |
| 建立链接 | "关联到 [[相关笔记]]" |
| 保存事实 | "记住：Python 3.11 性能提升了 30%" |

### 🔵 模式 B：深度编译 / Deep Compile

| 场景 / Scenario | 说 / Say |
|----------------|---------|
| 编译资料 | "编译这篇资料" / "/ingest_obsidian-knowledge" |
| 深度提问 | "从知识库查一下 Rust 所有权" / "/query_obsidian-knowledge" |
| 健康检查 | "检查知识库健康" / "/lint_obsidian-knowledge" |

### 🟣 模式 C：网络拉取 / Web Pull

| 场景 / Scenario | 说 / Say |
|----------------|---------|
| 拉取文档 | "从 MDN 拉取 fetch API 的文档" |
| 拉取文章 | "把这篇 Wikipedia 文章抓进知识库" |
| 技术论坛 | "从 Stack Overflow 拉取这个问题的讨论" |

---

## 🔄 对比 / Comparison

| 维度 / Dimension | 本方案 / This Project | LLM Wiki 类方案 |
|-----------------|----------------------|----------------|
| 🚀 上手 / Setup | **1 分钟** / 1 min | 10-30 分钟 |
| 📜 脚本依赖 / Scripts | **零** / Zero | 需要 Python/Node |
| 🔧 管线复杂度 / Pipeline | **无** / None | Ingest→Compile→Lint→Query |
| 🎯 适合人群 / Audience | 所有人 / Everyone | 重度用户 / Power users |

---

## ⚡ CC Switch 仓库添加（备选）/ Alternative: Add Warehouse

在 CC Switch 「仓库管理」中添加 / Add in CC Switch Repository:

| Field | Value |
|-------|-------|
| Owner | `dragoncaosl` |
| Name | `obsidian-knowledge-base` |
| Branch | `main` |
| Subdirectory | `skill` |

---

## 🛠 技术栈 / Tech Stack

| Technology | 作用 / Purpose |
|-----------|---------------|
| [Claude Code](https://claude.ai/code) | AI 助手，读写知识库 / AI assistant & knowledge curator |
| [Obsidian](https://obsidian.md) | Markdown 知识编辑与可视化 / Knowledge visualization |
| Claude Code Memory | 跨会话持久化 / Cross-session persistence |
| Markdown + `[[wikilinks]]` | 知识存储与关联 / Knowledge storage & linking |
| Glob / Grep | 本地知识检索 / Local knowledge retrieval |

---

## 📦 如何贡献 / Contributing

1. Fork 本仓库 / Fork this repo
2. `git checkout -b feature/xxx`
3. `git commit -am 'Add xxx'`
4. `git push origin feature/xxx`
5. Create Pull Request

---

## 📄 开源协议 / License

MIT License — 随便用，随便改，随便分享。
Free to use, modify, and share.

---

## 🙏 致谢 / Credits

- [Andrej Karpathy](https://github.com/karpathy) — LLM Wiki 理念
- [Obsidian](https://obsidian.md) — 优秀的 Markdown 知识管理工具
- [Claude Code](https://claude.ai/code) — AI 知识管理伙伴

---

<p align="center">
  ⭐ 如果这个项目对你有帮助，欢迎 Star！<br>
  <sub>少即是多。不写一行脚本，知识自动沉淀。</sub><br>
  <sub>Less is more. Zero scripts, knowledge compounds.</sub>
</p>
