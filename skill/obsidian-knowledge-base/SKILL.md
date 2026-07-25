---
name: obsidian-knowledge-base
description: "Use when setting up a personal knowledge base with Claude Code and Obsidian, when you want Claude Code to persist learnings across sessions without complex pipelines, or when you want a 'save experience, retrieve smarter' workflow using only Markdown files + Memory"
---

# Obsidian Knowledge Base — 少即是多的个人知识库

## Overview

把 Claude Code 当成你的**大脑皮层**，把 Obsidian 当成你的**外接硬盘**。

- **存**：解决问题后说"把经验存到知识库" → Claude Code 写入 Markdown 文件 + Memory
- **取**：问问题时 → Claude Code 先搜本地文件 + 读 Memory → 再结合自身知识综合回答
- **核心理念**：不引入管线、脚本、向量数据库、RAG。纯文件 + Memory，越用越聪明

```
解决问题 → 告诉 Claude 保存经验
       ↙                    ↘
下次回答更准 ← 检索本地知识 + 大模型综合判断
```

## 适用场景

- ✅ 你已经在用 Claude Code，希望知识能跨会话积累
- ✅ 你同时在用 Obsidian，想要一个统一的 Markdown 知识仓库
- ✅ 你厌倦了"装插件、配管线、写脚本"的复杂方案
- ✅ 你想让 AI 越用越懂你，而不只是每次从零开始

- ❌ 不需要：你已经用着成熟的 LLM Wiki 方案且满意
- ❌ 不适合：你需要大规模自动爬取和编译知识（有其他项目更适合）

## 目录结构

```
你的Vault/                    ← Obsidian Vault = 工作目录
├── CLAUDE.md                 ← 项目指令（Claude Code 自动加载）
├── 笔记/
│   ├── 技术/
│   ├── 生活/
│   └── 项目/
├── 项目/                     ← 每个项目独立子目录
├── 引用/                     ← 外部资料、书摘
├── 日记/                     ← Daily Notes
└── .claude/
    ├── skills/               ← 可选：自定义 Skills
    └── 以上由 Memory 系统管理
```

## 核心工作流

### 🟢 存知识（Retain）

**场景**：你用 Claude Code 解决了某个问题

```
你：帮我写一个 Python 脚本处理 CSV 文件
Claude: [写完了，运行成功]
你：把这个经验保存到知识库，关键点是 pandas 的 read_csv 参数、编码处理
→
Claude 会：
1. 在 笔记/技术/ 下创建 "Python处理CSV文件经验.md"
2. 写入 Markdown 内容 + [[相关笔记]] 链接
3. 自动记住关键事实到 Memory
4. 下次会话自动知道你有这个经验
```

**输出示例**：
```markdown
---
created: 2026-07-25
tags: [python, csv, pandas]
---

# Python 处理 CSV 文件经验

## 场景
需要处理 GB2312 编码的 CSV 文件，包含中文字段名。

## 解决方案
```python
import pandas as pd
df = pd.read_csv('file.csv', encoding='gb2312')
```

## 关键要点
- `read_csv` 的 `encoding` 参数处理非 UTF-8 文件
- `dtype` 参数避免数据类型自动推断问题

## 相关笔记
[[pandas常用操作]] [[数据处理最佳实践]]
```

### 🔵 取知识（Retrieve）

**场景**：你遇到类似问题，问 Claude Code

```
你：我有个 CSV 文件读出来乱码怎么办？
→
Claude Code 会：
1. 用 Glob/Grep 搜索 笔记/ 下的相关文件
2. 读取匹配的 Markdown 内容
3. 读取相关 Memory
4. 结合大模型知识综合回答
5. 回答中引用知识库来源
```

**回答效果**：
```
根据你的知识库记录（"Python处理CSV文件经验.md"），
你之前遇到过 GB2312 编码的问题，解决方案是：
  pd.read_csv('file.csv', encoding='gb2312')

这次的文件可能编码不同，可以尝试：
1. 先用 chardet 检测编码
2. 再用检测到的编码读取
```

### 🟣 图谱化（Visualize）

当 Claude Code 写笔记时使用了 `[[wikilink]]` 语法：
```markdown
# Python 异步编程
核心概念是 [[事件循环]] 和 [[协程]]
```

Obsidian 的 **Graph View** 会自动构建知识图谱：
```
Python 异步编程 → 事件循环
               → 协程
               → 对比: [[多线程]]
```

**存得越多，图谱越密，知识网络越强大。**

## Quick Reference

| 操作 | 对 Claude Code 说的话 |
|------|----------------------|
| **保存经验** | "把这个经验存到知识库" / "记住这个方案" |
| **指定分类** | "存到 笔记/技术/ 下" |
| **建立链接** | "关联到 [[XXX]] 笔记" |
| **整理笔记** | "帮我整理 笔记/ 下所有文件，加 [[链接]]" |
| **批量操作** | "给所有 技术 类笔记加上标签" |

## 实现细节

### 技术原理

| 组件 | 作用 |
|------|------|
| **Claude Code Memory** | 跨会话记住关键事实（谁、什么、为什么） |
| **Markdown 文件** | 完整知识记录，Obsidian 直接读取 |
| **[[wikilinks]]** | 建立知识关联，触发 Obsidian 图谱 |
| **Glob/Grep 工具** | 检索本地知识库内容 |
| **CLAUDE.md** | 项目指令，让 Claude 知道 vault 结构 |

### 配置步骤

1. **创建 Vault**：`mkdir 你的Vault && cd 你的Vault`
2. **创建目录**：`笔记/`、`项目/`、`引用/`、`日记/`
3. **写 CLAUDE.md**：告诉 Claude Code vault 结构和规则
4. **在 Obsidian 中**：打开该目录作为 Vault
5. **开始使用**：解决问题 → "记住这个"

> **Memory 系统会自动工作**，你不需要额外配置。CLAUDE.md 确保 Claude Code 始终知道 vault 结构。

## 与同类方案对比

| 维度 | 本方案 | LLM Wiki 类方案 |
|------|--------|----------------|
| 上手时间 | 1 分钟（建目录就行） | 10-30 分钟（学习管线） |
| 脚本依赖 | 零 | 需要 Python/Node 运行 |
| 管线复杂度 | 无 | Ingest→Compile→Lint→Query |
| 灵活性 | 完全手动控制 | 自动化但受管线约束 |
| 适合人群 | 普通用户、初学者 | 重度知识管理用户 |

## 常见误区

- ❌ **"是不是需要装插件？"** → 不需要，纯 Markdown + Memory
- ❌ **"和 RAG 有什么区别？"** → 这不是 RAG，是直接用文件做知识检索，不需要向量库
- ❌ **"每次都要手动存太麻烦？"** → 一句话的事："记住这个"
- ❌ **"用 [[链接]] 好麻烦"** → 告诉 Claude Code "加上 [[链接]]"，它自动处理

## Real-World Impact

- 你遇到的问题**只解决一次**，经验永久沉淀
- Claude Code **越用越懂你的上下文**，回答越来越精准
- Obsidian 图谱 **可视化你的知识增长**，给你正反馈
- **没有 vendor lock-in**——所有知识都是纯 Markdown，离开 Claude/ Obsidian 也能读

---

> **Less is more. 不写一行脚本，知识自动沉淀。**
