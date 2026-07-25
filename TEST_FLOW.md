# 🧪 测试流程：从 GitHub ZIP 安装 Skill 到 CC Switch

> 目标：验证 `obsidian-knowledge-base` skill 能否通过 CC Switch 的「从 ZIP 安装」功能正常安装并在 Claude Code 中生效。

---

## 总体流程

```
GitHub 下载 ZIP → 提取 skill 文件夹 → 重新打包 → 导入 CC Switch → 在 Claude Code 中使用
```

---

## Step 1：从 GitHub 下载 ZIP

### 方法 A：直接在 GitHub 页面下载

1. 打开仓库：https://github.com/dragoncaosl/obsidian-knowledge-base
2. 点击绿色 `Code` 按钮
3. 选择 **Download ZIP**
4. 保存到本地，如 `D:\Downloads\obsidian-knowledge-base-main.zip`

### 方法 B：用命令行下载

```bash
# Windows PowerShell / CMD
curl -L -o D:\Downloads\obsidian-knowledge-base.zip ^
  https://github.com/dragoncaosl/obsidian-knowledge-base/archive/refs/heads/main.zip
```

---

## Step 2：解压并提取 skill 文件夹

### 解压

解压后目录结构是这样的：

```
obsidian-knowledge-base-main/
├── skill/                          ← 我们要的就是这个！
│   └── obsidian-knowledge-base/    ← skill 文件夹
│       └── SKILL.md
├── CLAUDE.md
├── README.md
├── 笔记/
├── 项目/
├── 引用/
├── 日记/
└── ...
```

### 提取 skill 文件夹

**关键要求**：CC Switch 从 ZIP 安装时，ZIP 解压后**第一级必须是 skill 文件夹本身**，不能多一层嵌套。

所以需要这样做：

1. 进入解压后的 `obsidian-knowledge-base-main/skill/` 目录
2. 你会看到 `obsidian-knowledge-base/` 文件夹
3. 把这个文件夹单独复制出来

---

## Step 3：重新打包 ZIP

> ⚠️ **重要**：不要直接使用下载的整个仓库 ZIP！必须只打包 skill 文件夹。

```bash
# 方法一：用命令行（确保你在 skill/ 目录的上级）
cd D:\Downloads\obsidian-knowledge-base-main\skill\
# 把 obsidian-knowledge-base 文件夹打包
# Windows 用：
tar -acf D:\Downloads\obsidian-knowledge-base.skill.zip obsidian-knowledge-base\
```

或者直接在文件资源管理器操作：
1. 打开 `D:\Downloads\obsidian-knowledge-base-main\skill\`
2. 右键 `obsidian-knowledge-base` 文件夹 → **发送到 → 压缩(zipped)文件夹**
3. 重命名为 `obsidian-knowledge-base.skill.zip`

**打包后的 ZIP 内部结构**（必须是这样）：
```
obsidian-knowledge-base.skill.zip
└── obsidian-knowledge-base/       ← 第一级就是 skill 文件夹
    └── SKILL.md
```

---

## Step 4：在 CC Switch 中安装

1. 打开 **CC Switch** 桌面应用
2. 切换到 **Skills** 面板
3. 点击 **「从 ZIP 安装」** 按钮
4. 在文件选择对话框中选择刚才打包的 `obsidian-knowledge-base.skill.zip`
5. 等待安装完成

**预期结果**：
- CC Switch 的 Skills 列表中出现 `obsidian-knowledge-base`
- 状态显示为已安装
- skill 被部署到 `~/.claude/skills/obsidian-knowledge-base/` 目录

---

## Step 5：在 Claude Code 中验证

### 验证 skill 是否被 Claude Code 识别

1. 打开终端，进入任意 Claude Code 工作目录
2. 启动 Claude Code：
   ```bash
   claude
   ```
3. 输入 `/` 查看可用技能列表
4. 列表中应该能看到 `obsidian-knowledge-base`

### 测试 skill 功能

**测试 1：Skill 触发**

```
帮我设置一个个人知识库
```

如果 skill 生效，Claude 的回答应该自动包含本 SKILL.md 中的核心理念和流程。

**测试 2：知识保存**

```
我现在要记录一个 Python 经验：
用 pandas 读取 CSV 时，如果遇到编码问题，可以用 encoding='gb2312' 参数
把这个存到知识库，分类到 笔记/技术/
```

预期结果：
- Claude Code 在 `笔记/技术/` 下创建 Markdown 文件
- 内容包含 frontmatter、场景、解决方案
- 包含 `[[wikilinks]]`

**测试 3：知识检索**

```
从我的知识库里查一下 CSV 相关的经验
```

预期结果：
- Claude Code 搜索本地文件
- 读取相关内容
- 综合回答时引用你的笔记

---

## Step 6：验证 Obsidian 图谱（可选）

1. 在 Obsidian 中打开安装了 skill 的 vault
2. 打开 **Graph View**（左侧工具栏或 `Ctrl/Cmd + G`）
3. 检查是否有节点显示
4. 如果笔记中有 `[[链接]]`，图谱中应该显示连线

---

## ❗ 常见问题与排查

| 问题 | 原因 | 解决 |
|------|------|------|
| ZIP 安装后 CC Switch 不显示 | ZIP 内目录多了一层 | 确保 ZIP 解压后第一级就是 `obsidian-knowledge-base/` 文件夹 |
| Claude Code 中找不到 skill | 未重启 Claude Code | 关闭 Claude Code 重新打开 |
| Skill 不生效 | CLAUDE.md 冲突 | 检查项目内 CLAUDE.md 是否覆盖了 skill 指令 |
| Windows 符号链接失败 | 未开启开发者模式 | 设置 → 开发者选项 → 开启开发者模式 |

---

## ✅ 测试通过标准

- [ ] CC Switch 成功安装 skill
- [ ] `~/.claude/skills/obsidian-knowledge-base/SKILL.md` 文件存在
- [ ] Claude Code 中 `/` 命令能看到 `obsidian-knowledge-base`
- [ ] 输入"保存知识"相关指令，Claude 按 skill 流程响应
- [ ] 知识能保存到本地 Markdown 文件
- [ ] 知识检索能搜到已保存的内容
