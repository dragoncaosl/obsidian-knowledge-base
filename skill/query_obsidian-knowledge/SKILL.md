---
name: query_obsidian-knowledge
description: "Use when the user asks to search or query their knowledge base, when asked about things 'in my notes', 'in my wiki', or 'in the knowledge base', or when the user needs a researched answer that draws from compiled wiki content. Triggered by /query or natural language knowledge-base questions"
---

# Query — 深度检索与知识复利

## Overview

在 `wiki/` 知识库中进行深度检索和综合回答。与直接搜索 `笔记/` 不同，query 走**索引先行**的路线：先读 `wiki/index.md` 定位相关页面，再深度阅读，最后以带双链引用的格式回答。

```
用户提问
  │
  ▼
读取 wiki/index.md 定位相关页面
  │
  ▼
深度阅读 entities / concepts / sources / syntheses
  │
  ▼
综合回答 + [[双链引用]]
  │
  ▼
高质量回答 → 询问是否存为 synthesis
```

## 触发条件

- 用户输入 `/query <问题>`
- 用户说："从知识库里查一下"、"我的笔记里关于 X 是怎么说的"、"翻一下 wiki 里 Y 的内容"
- 问题涉及需要综合多个知识点才能回答的复杂主题

### 降级策略

如果问题属于纯通用知识（如"太阳系有几颗行星"），且 `wiki/index.md` 中无相关内容：
> 本地知识库中未找到相关内容，以下为通用知识回答：[直接回答]

## 检索流水线

### 步骤 1：查阅全局索引
**永远的第一步**：读取 `wiki/index.md`

定位与问题相关的 Entities、Concepts、Sources、Syntheses。

### 步骤 2：深度阅读
选取最相关的页面，读取完整内容。必要时阅读多个页面。

### 步骤 3：综合回答
综合信息后回答，**必须使用 `[[wikilink]]` 标注引用来源**：
- 引用某个 Wiki 页面 → `[[页面名称]]`
- 引用特定原文 → `> 引用内容`

### 步骤 4：高价值内容固化
如果回答满足以下条件，询问用户是否保存为 synthesis：
- 超过 2 个段落
- 具有分析对比性或总结性

询问话术：> 这是一个有价值的总结，需要我将其保存到 wiki/syntheses/ 吗？

用户同意后，创建 synthesis 文件并更新 `index.md` 和 `log.md`。

### 步骤 5：记录日志
在 `wiki/log.md` 末尾追加：

```markdown
## [YYYY-MM-DD] query | <操作简述>
- **输出**: <引用页面列表或"即时回答未保存">
```

## 强制约束

- **禁止凭记忆回答必须先检索知识库**
- **禁止过度引用**同一页面的信息在段落首尾引用一次即可
- **禁止静默回答**知识库无相关内容时必须声明

## 与模式A检索的区别

| 维度 | 模式A（快速检索） | 模式B（/query） |
|------|-----------------|----------------|
| 搜索范围 | Glob/Grep 搜 `笔记/` + Memory | 读 `index.md` 导航 + 深度读 wiki |
| 回答深度 | 直接引用经验笔记 | 综合多页分析 + 双链引用 |
| 知识回填 | 不保存回答 | 高质量回答可存为 synthesis |
| 适用问题 | 具体问题"之前是怎么做的" | 复杂问题"帮我对比分析 XXX" |
