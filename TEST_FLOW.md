# 🧪 测试流程：通过 GitHub Actions 下载 Skill ZIP → 导入 CC Switch

> 目标：验证 `obsidian-knowledge-base` skill 能否通过 CC Switch 安装并在 Claude Code 中生效。

---

## 总体流程

```
GitHub Actions 下载 Skill ZIP → 导入 CC Switch → 在 Claude Code 中使用
```

**只需 2 步，无需手动解压和重新打包。**

---

## Step 1：下载 Skill ZIP

从 GitHub Actions 的 Artifacts 下载已经打包好的 skill ZIP：

### 方式一：浏览器操作

1. 打开仓库：https://github.com/dragoncaosl/obsidian-knowledge-base
2. 点击 **Actions** 标签页
3. 在左侧选择 **Package Skill for CC Switch**
4. 找到最近的运行记录（会有绿色 ✓ 标记）
5. 点击运行记录进入详情
6. 在 **Artifacts** 区域下载 `obsidian-knowledge-base.skill`
7. 保存到本地，如 `D:\Downloads\obsidian-knowledge-base.skill.zip`

### 方式二：手动触发打包

如果 Actions 还没运行过：

1. 打开 https://github.com/dragoncaosl/obsidian-knowledge-base/actions
2. 左侧选择 **Package Skill for CC Switch**
3. 点击 **Run workflow** → **Run workflow**
4. 等待几秒刷新页面
5. 点击运行记录 → 下载 Artifacts

---

## Step 2：导入 CC Switch

1. 打开 **CC Switch** 桌面应用
2. 切换到 **Skills** 面板
3. 点击 **「从 ZIP 安装」** 按钮
4. 选择刚才下载的 `obsidian-knowledge-base.skill.zip`
5. 等待安装完成

**预期结果**：
- CC Switch 的 Skills 列表中出现 `obsidian-knowledge-base`
- 状态显示为已安装
- skill 被部署到 `~/.claude/skills/obsidian-knowledge-base/` 目录

---

## Step 3：在 Claude Code 中验证

### 验证 skill 是否被加载

```bash
claude
# 然后输入：
/
```

查看可用技能列表，应该能看到 `obsidian-knowledge-base`。

### 测试保存知识

```
帮我保存一个经验到知识库：
用 pandas 读取 CSV 时，遇到编码问题用 encoding='gb2312' 参数
这个属于 笔记/技术/ 分类
```

**预期**：Claude Code 在 `笔记/技术/` 下创建 Markdown 文件，含场景、解决方案、`[[wikilinks]]`。

### 测试检索知识

```
翻一下知识库里 CSV 相关的内容
```

**预期**：Claude Code 搜索本地文件，读取相关内容，综合回答时引用你的笔记。

---

## ✅ 测试通过标准

- [ ] CC Switch 成功安装 skill
- [ ] `~/.claude/skills/obsidian-knowledge-base/SKILL.md` 文件存在
- [ ] Claude Code 中 `/` 能看到 `obsidian-knowledge-base`
- [ ] 知识能保存到本地 Markdown 文件
- [ ] 知识检索能搜到已保存的内容

---

## ❓ 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| Actions 没运行过 | 首次需要手动触发 | 去 Actions 点 **Run workflow** |
| 下载的 ZIP 无法安装 | 包结构不对 | 确保是从 Actions Artifacts 下载的，不是 GitHub 的 Code→Download ZIP |
| Claude Code 找不到 skill | 未重启 | 关闭 Claude Code 重新打开 |
| Windows 符号链接失败 | 开发者模式未开启 | 设置 → 开发者选项 → 开启开发者模式 |

---

## 📌 原理说明

GitHub Actions 中的 `package-skill.yml` 工作流会自动执行：

```
skill/obsidian-knowledge-base/   ← 仓库中的 skill 源码
         ↓
obsidian-knowledge-base.skill.zip  ← 打包成 CC Switch 识别的格式
         ↓
上传为 Artifact                ← 可在 Actions 页面下载
```

每次推送修改到 `skill/` 目录下的文件时，Actions 会自动重新打包，始终提供最新版本。
