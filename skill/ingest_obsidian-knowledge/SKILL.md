---
name: ingest_obsidian-knowledge
description: "Use when needing to compile raw source materials into structured wiki pages, when told to 'ingest', 'compile', 'process', or 'extract' knowledge from files in raw/ into the wiki/ knowledge base. Triggered by /ingest commands or user requests to process source materials into the knowledge base"
---

# Ingest — 知识编译管线

## Overview

将 `raw/` 目录中的原始资料"编译"成结构化的 `wiki/` 知识网络。这是 Karpathy LLM Wiki 模式的核心操作。

```
raw/01-articles/xxx.md     raw/02-web/xxx.md
         │                        │
         └────────┬───────────────┘
                  ▼
         读取 → 提取实体+概念
                  │
           ┌──────┼──────┐
           ▼      ▼      ▼
    sources/  entities/  concepts/
    (摘要)    (人物/工具)  (方法论/框架)
                  │
          更新 index.md + log.md
                  │
          移动源文件到 archive/
```

## 触发条件

- 用户输入 `/ingest`（扫描 raw/ 下所有未归档文件）
- 用户输入 `/ingest <路径>`（处理指定文件）
- 用户说："编译这篇文章"、"把这个资料加进知识库"、"摄入这个文件"
- 模式 C（web-pull）完成后自动触发

## 目录规则

| 目录 | 角色 | LLM 权限 |
|------|------|---------|
| `raw/01-articles/` | 手动收集的文章 | 读取 + 处理后移动到 09-archive |
| `raw/02-web/` | 网络拉取的内容 | 读取 + 处理后移动到 09-archive |
| `raw/03-forum/` | 论坛/讨论帖 | 读取 + 处理后移动到 09-archive |
| `raw/09-archive/` | 已归档 | **禁止读取** |
| `wiki/sources/` | 来源摘要 | 创建/更新 |
| `wiki/entities/` | 实体页 | 创建/更新 |
| `wiki/concepts/` | 概念页 | 创建/更新 |
| `wiki/syntheses/` | 综合分析 | 创建 |

## 编译流水线

对每个待处理源文件，按以下步骤执行：

### 步骤 1：读取源文件
完整读取源文件内容。如果是 PDF，仅提取文本元信息。

### 步骤 2：提炼核心信息
提取：核心主旨（1-2 句）、实体（人物/公司/工具）、概念（方法论/框架）。
非中文内容翻译为简体中文。

### 步骤 3：创建/更新 sources 摘要
在 `wiki/sources/` 创建摘要文件：
```markdown
---
title: "摘要-文件slug"
type: source
tags: [来源, 原始文件]
sources: [raw/原路径]
last_updated: YYYY-MM-DD
---

## 核心摘要
[3-5 句话的核心总结]

## 关联链接
- [[EntityName]] — 关联实体
- [[ConceptName]] — 关联概念
```

### 步骤 4：知识网络化
对每个实体和概念：

**实体 → `wiki/entities/`**（用 TitleCase 命名，如 `RustLanguage`）
**概念 → `wiki/concepts/`**（用 kebab-case 命名，如 `ownership-system`）

- 页面不存在 → 创建新页
- 页面已存在 → 增量合并新信息
- **发现冲突 → 暂停，询问用户**

页面模板：
```markdown
---
title: "名称"
type: entity | concept
tags: []
sources: [摘要文件路径]
last_updated: YYYY-MM-DD
---

## 定义

## 关键信息

## 关联链接
- [[摘要-source-slug]] — 来源
- [[RelatedEntity]] — 相关实体
```

### 步骤 5：更新注册表
**`wiki/index.md`** — 按分类加入新页面及其一句话描述。
**`wiki/log.md`** — 追加操作日志（Append-only）：
```markdown
## [YYYY-MM-DD] ingest | 操作简述
- **变更**: 新增 [[Page]]; 更新 [[index.md]]
- **冲突**: 无 (或: 冲突 [[Page]], 已暂停等待决策)
```

### 步骤 6：归档
确认上述全部完成后，将源文件移动到 `raw/09-archive/`。

## 冲突处理

发现新旧知识冲突时：
1. **暂停**流程
2. **报告**冲突内容给用户
3. **询问**：A) 保留新旧两者 B) 覆盖旧知识 C) 放弃本次 ingest
4. 根据用户选择继续

## 强制约束

- **不读取 `raw/09-archive/`** 下的任何文件
- **不修改源文件内容**（除非归档移动）
- **所有 wiki 页面必须含 `## 关联链接`**，不能有孤岛页面
- **使用简体中文**编写所有内容
