# 🧠 Obsidian Knowledge Base — 三种模式，一个大脑

> **快速记录 × 深度编译 × 网络拉取。越用越聪明。**

[![GitHub stars](https://img.shields.io/badge/Claude%20Code-Ready-blue)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-Ready-purple)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](#)

## 📖 这是什么？

一个融合了三种知识管理模式的 Claude Code + Obsidian 个人知识库方案：

| 模式 | 一句话概括 | 灵感来源 |
|------|-----------|---------|
| **A：快记** 🟢 | 解决问题 → 告诉 Claude "记住" → 自动存档 | 你的"少即是多"理念 |
| **B：编译** 🔵 | 投入资料 → LLM 自动编织知识网络 → Wiki | [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) |
| **C：拉取** 🟣 | 从网站/Wiki/论坛拉取内容 → 自动入库 | 新增能力 |

**核心飞轮：**
```
你遇到问题 / 看到文章 / 想到点子
  │
  ├─ A：一句话保存经验到 笔记/
  ├─ B：投入 raw/ → 自动编译到 wiki/（10-15页）
  └─ C：Web → raw/ → wiki/
  │
  ▼
  知识沉淀 → Obsidian 图谱 → 下次回答更准
```

---

## ✨ 功能特性

- 🚀 **三合一**：快速记录 + Karpathy LLM Wiki + 网络拉取，一套 Vault 全搞定
- 🧠 **跨会话记忆**：Claude Code Memory 自动持久化关键事实
- 📝 **Markdown 原生**：纯 Markdown，Obsidian 原生读取，零 Vendor Lock-in
- 🔗 **知识图谱**：`[[wikilinks]]` 自动构建 Obsidian Graph View
- 📂 **分区清晰**：笔记/（快记）、raw/（资料）、wiki/（编译知识）
- 🌐 **网络拉取**：从技术网站/Wiki/论坛抓取内容，自动消化入库

---

## 🚀 快速开始

### 1. 克隆本项目

```bash
git clone https://github.com/你的用户名/obsidian-knowledge-base.git
cd obsidian-knowledge-base
```

### 2. 在 Obsidian 中打开

- 打开 Obsidian → **打开本地文件夹** → 选择本项目目录
- 推荐插件（可选但好用）：
  - **Dataview** — 查询笔记索引
  - **Graph View**（内置）— 知识图谱
  - **Templates**（内置）— 笔记模板

### 3. 在 Claude Code 中启动

```bash
# 在本项目目录下启动
cd obsidian-knowledge-base
claude
```

Claude Code 会自动读取 `CLAUDE.md`，知道你在这个 Vault 里该怎么工作。

### 4. 开始积累知识

在 Claude Code 中：

```
你：帮我写一个 Python 脚本处理 CSV 文件
Claude: [完成任务]
你：把这个经验保存到知识库
→
Claude 会在 笔记/技术/ 下创建 Python处理CSV文件经验.md
并自动写入 Memory，下次会话直接可用
```

---

## 📁 目录结构

```
obsidian-knowledge-base/       ← Obsidian Vault = Claude Code 工作目录
├── CLAUDE.md                  ← 项目指令（三种模式规范）
├── 📝 笔记/                   ← [A] 快速记录
│   ├── 技术/ 生活/ 项目/
├── 📁 项目/                   ← [A] 项目资料
├── 📚 引用/                   ← [A] 外部书摘
├── 📅 日记/                   ← [A] Daily Notes
├── 📥 raw/                    ← [B+C] 原始资料（只读）
│   └── 01-articles/ 02-web/ 03-forum/ 09-archive/
├── 🧠 wiki/                   ← [B] 编译知识层
│   ├── index.md + log.md
│   └── concepts/ entities/ sources/ syntheses/
└── .claude/skills/            ← 5 个 Skill
    ├── obsidian-knowledge-base/  ingest/  query/  lint/  web-pull/
```

---

## ⚡ CC Switch 安装

本项目包含 **5 个 Skill**，可通过 [CC Switch](https://github.com/farion1231/cc-switch) 管理：

| Skill | 作用 |
|-------|------|
| `obsidian-knowledge-base` | 主 Skill — 三种模式统一入口 |
| `ingest` | raw/ → wiki/ 深度编译管线 |
| `query` | wiki/ 索引导航深度检索 |
| `lint` | 知识库健康检查 |
| `web-pull` | 从互联网拉取内容入库 |

### 方式一：仓库添加（推荐）

在 CC Switch 的「仓库管理」中添加：

| 字段 | 值 |
|------|----|
| Owner | `dragoncaosl` |
| Name | `obsidian-knowledge-base` |
| Branch | `main` |
| Subdirectory | `skill` |

添加后 CC Switch 会自动发现全部 5 个技能，一键安装。

### 方式二：从 Actions 下载 ZIP

1. 打开仓库 [Actions 页面](https://github.com/dragoncaosl/obsidian-knowledge-base/actions) → **Package Skills for CC Switch**
2. 点击最新运行记录 → 下载 **Artifacts** 中的 `all-skills`
3. 解压后得到 5 个 `.skill.zip` 文件（每个对应一个 skill）
4. 在 CC Switch Skills 面板点击「从 ZIP 安装」，选择需要的 ZIP

> Actions 工作流在 skill 目录更新时自动打包，ZIP 直接可用无需解压。

---

## 📖 使用指南

### 🟢 保存经验（Retain）

**一句话就够了：**

| 场景 | 说 |
|------|----|
| 保存当前经验 | "把这个经验存到知识库" |
| 指定分类 | "存到 笔记/技术/ 下" |
| 关联其他笔记 | "关联到 [[相关笔记名]]" |
| 保存关键事实 | "记住：Python 3.11 的性能比 3.10 提升了 30%" |

Claude Code 会自动：
1. 创建/更新 Markdown 文件（含 `[[wikilinks]]`）
2. 将关键事实写入 Memory 系统
3. 下次会话自动加载

### 🔵 检索知识（Retrieve）

| 场景 | 效果 |
|------|------|
| 直接问 | Claude Code 自动搜索本地知识库 |
| 指定搜索 | "翻一下我的知识库，关于 CSV 处理的" |
| 综合判断 | 本地知识 + 大模型知识 一起参考 |

### 🟣 构建知识图谱

**关键就是 `[[wikilinks]]`**：

当 Claude Code 写入：
```markdown
# Python 异步编程
核心是 [[事件循环]] 和 [[协程]]
对比 [[多线程]] 的差异
```

Obsidian 的 **Graph View** 自动展示：
```
Python 异步编程 → 事件循环 → asyncio
               → 协程
               → 对比: 多线程
```

> **存得越多，图谱越密，你的个人"第二大脑"越强大。**

---

## 🔄 与同类方案对比

| 维度 | 本方案 | LLM Wiki 类方案（llm-obsidian-wiki 等） |
|------|--------|----------------------------------------|
| 上手时间 | **1 分钟** | 10-30 分钟 |
| 脚本依赖 | **零** | 需要 Python/Node |
| 管线复杂度 | **无** | Ingest→Compile→Lint→Query |
| 控制方式 | **完全手动** | 自动化但受管线约束 |
| 适合人群 | 所有人、初学者 | 重度知识管理用户 |

---

## 🧩 进阶技巧

### 批量整理
```
帮我整理 笔记/下所有文件：
1. 给每个文件加合适的 tags
2. 建立 [[链接]] 关联
3. 合并重复内容
```

### 项目知识库
```
在这个 项目/my-app 目录下，也按同样的方式管理经验
每次解决 bug 后自动记录到 项目/my-app/bug-fix-log/
```

### 周回顾
```
帮我总结这周我在 日记/ 中记录的内容
整理成周报存到 笔记/项目/周报/
```

---

## 🛠 技术栈

| 技术 | 作用 |
|------|------|
| [Claude Code](https://claude.ai/code) | AI 助手，读写知识库 |
| [Obsidian](https://obsidian.md) | Markdown 知识编辑与可视化 |
| Claude Code Memory | 跨会话持久化关键事实 |
| Markdown + [[wikilinks]] | 知识存储与关联 |
| Glob / Grep | 本地知识检索 |

---

## 📦 如何贡献

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/xxx`)
3. 提交修改 (`git commit -am 'Add xxx'`)
4. 推送到分支 (`git push origin feature/xxx`)
5. 创建 Pull Request

---

## 📄 开源协议

MIT License — 随便用，随便改，随便分享。

---

## 🙏 致谢

- [Andrej Karpathy](https://github.com/karpathy) — LLM Wiki 理念的启发
- Obsidian 社区 — 优秀的 Markdown 知识管理工具
- Claude Code — 让 AI 成为知识管理的伙伴

---

<p align="center"> ⭐ 如果这个项目对你有帮助，欢迎 Star！<br>
<sub>少即是多。不写一行脚本，知识自动沉淀。</sub></p>
