# 🧠 Obsidian Knowledge Base — Claude Code 零管线个人知识库

> **不写一行脚本，知识自动沉淀。越用越聪明。**

[![GitHub stars](https://img.shields.io/badge/Claude%20Code-Ready-blue)](#)
[![Obsidian](https://img.shields.io/badge/Obsidian-Ready-purple)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](#)

## 📖 这是什么？

一个**极简的 Claude Code + Obsidian 个人知识库方案**。

与市面上流行的"LLM Wiki"方案不同——那些方案需要你搭管线、写脚本、跑编译——本方案秉持 **少即是多** 的理念：

| 其他方案 | 本方案 |
|---------|--------|
| 需要 Python/Node 脚本 | **零脚本** |
| 管线：Ingest→Compile→Lint→Query | **零管线** |
| 需要学习整套工作流 | **一句话就能用** |
| 知识需要"编译"才能用 | **存进去=活的，下次直接搜** |

**核心飞轮：**
```
解决问题 → 告诉 Claude "记住这个"
       ↙                    ↘
下次回答更准 ← 检索本地知识 + 大模型综合判断
```

---

## ✨ 功能特性

- 🚀 **零配置启动**：`git clone` → 在 Obsidian 打开 → 开始用
- 🧠 **跨会话记忆**：Claude Code Memory 自动持久化关键事实
- 📝 **Markdown 原生**：所有知识是纯 Markdown，Obsidian 原生读取
- 🔗 **知识图谱**：Claude Code 用 `[[wikilinks]]` 写笔记 → Obsidian 自动生成图谱
- 📂 **分类清晰**：笔记/、项目/、引用/、日记/ 四个分区
- 🔍 **自动检索**：问问题时，Claude Code 自动搜索本地知识库
- 💾 **零 Vendor Lock-in**：离开 Claude 或 Obsidian，你的知识依然是可读的 Markdown

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
├── CLAUDE.md                  ← 项目指令（Claude Code 自动加载）
├── 笔记/                      ← 日常笔记
│   ├── 技术/                  ← 技术相关
│   ├── 生活/                  ← 生活记录
│   └── 项目/                  ← 项目相关
├── 项目/                      ← 每个项目独立子目录
├── 引用/                      ← 外部资料、书摘
├── 日记/                      ← Daily Notes
├── .claude/
│   └── skills/
│       └── obsidian-knowledge-base/
│           └── SKILL.md       ← Claude Code Skill 定义
└── .gitignore
```

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
