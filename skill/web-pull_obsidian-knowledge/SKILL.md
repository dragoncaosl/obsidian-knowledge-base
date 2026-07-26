---
name: web-pull_obsidian-knowledge
description: "Use when the user wants to pull content from websites, technical documentation, wikis, forums, or blogs into their knowledge base. Triggered by phrases like 'pull from', 'fetch from', 'grab content from', '拉取', '抓取' followed by a URL or website name. Also used when the user wants to research and save online resources"
---

# Web Pull — 网络内容拉取

## Overview

从互联网自动获取内容并纳入知识库。搜索目标网站 → 抓取页面内容 → 保存为 Markdown 到 `raw/` → 自动触发 Ingest 编译到 `wiki/`。

```
用户请求拉取某个网站的内容
  │
  ▼
WebSearch 搜索目标
  │
  ▼
WebFetch 抓取页面内容
  │
  ▼
转换为 Markdown → 存入 raw/02-web/
  │
  ▼
自动触发 /ingest → 编译到 wiki/
  │
  ▼
更新 wiki/index.md + wiki/log.md
```

## 触发条件

- 用户说："从 MDN 拉取 fetch API 的文档"、"把 Rust 官网的教程抓进来"、"从 Wikipedia 拉取 XXX 的词条"
- 用户说："帮我从网上找关于 XXX 的资料并存到知识库"
- 用户说："搜索 YYY 的最新信息并入库"
- 用户提供 URL："/web-pull https://example.com"

## 工作流程

### 步骤 1：确认目标
如果用户只给了描述（如"拉取 Rust 所有权教程"）：
1. 用 WebSearch 搜索目标
2. 确认最相关的 URL
3. 向用户确认"是否拉取 [URL] 的内容？"

如果用户直接给了 URL，跳过此步骤。

### 步骤 2：抓取内容
用 WebFetch 获取页面内容，提取：
- 标题
- 正文（去除导航、广告等无关元素）
- 关键代码示例
- 原文 URL（在笔记中保留来源）

### 步骤 3：转换为 Markdown
保存到 `raw/02-web/`，文件格式：

```markdown
---
source_url: "https://原始URL"
source_site: "网站名称"
fetched_at: YYYY-MM-DD
tags: [web, 来源标签]
---

# 标题

> 来源：[网站名称](https://原始URL)

## 正文

[抓取的内容，转换为干净的 Markdown]
```

### 步骤 4：触发 Ingest
告知用户内容已拉取并询问：
> 内容已存入 raw/02-web/xxx.md，是否需要编译到 wiki？(默认自动编译)

除非用户拒绝，否则自动执行 `/ingest` 流程。

### 特殊场景：技术文档/Wiki

拉取技术文档（如 MDN、Wikipedia、官方文档）时：
1. 优先拉取**概述页面** + 关键子页面（最多 3-5 个）
2. 在 sources 摘要中标注"系列文档"类型
3. 实体页面记录文档中提到的 API/概念

### 特殊场景：论坛/讨论帖

拉取论坛帖子（如 Stack Overflow、Reddit、V2EX）时：
1. 保存到 `raw/03-forum/`
2. 提取：问题描述 + **最佳答案** + 高赞评论
3. 在摘要中标注"论坛讨论"类型
4. 实体页面记录问题中的工具/库名

## 强制约束

- **尊重 robots.txt** — 不强制爬取禁止抓取的网站
- **保留来源 URL** — 每个拉取的文件必须含 source_url
- **征求确认** — 搜索目标不确定时先问用户
- **不过度拉取** — 一次请求最多拉取 5 个页面
- **注意版权** — 标注来源，不用于商业用途

## 输出示例

```
你：从 MDN 拉取 fetch API 的文档
Claude：
  已搜索到 MDN fetch API 文档 (URL: https://developer.mozilla.org/zh-CN/docs/Web/API/fetch)
  正在抓取...
  ✅ 内容已保存到 raw/02-web/mdn-fetch-api.md
  
  自动编译到 wiki...
  ✅ 已创建/更新：
     - wiki/sources/摘要-mdn-fetch-api.md
     - wiki/entities/FetchAPI.md
     - wiki/concepts/promise-based-http.md
     - wiki/index.md
     - wiki/log.md
```
