# PgStar Skills

> AI Agent 自定义 Skills 合集 — 让 AI 更懂你的工作流。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Skills 列表

| Skill | 描述 | 触发词 |
|-------|------|--------|
| [大哥救救我](#大哥救救我) | 半自动求助智谱GLM5.2 | `大哥救救我` |
| [creating-obsidian-vaults](#creating-obsidian-vaults) | 自动整理 Obsidian 笔记 | `记一下`、`存到知识库`、`帮我存` |
| [website-cloner](#website-cloner) | 自动化网站复刻工具 | `复刻网站`、`/website-cloner` |

---

## 大哥救救我

半自动求助 skill — AI 生成文档，你去智谱提问，AI 读取结果并修改代码。

**核心能力：**
- 自动分析对话历史，提取错误信息和代码上下文
- 生成结构化求助文档（问题描述 + 已尝试方案 + 需要帮助）
- 读取智谱 GLM5.2 的回复，分析解决方案
- 显示修改方案让你确认，确认后自动修改代码

**工作流程：**
```
1. 你说"大哥救救我"
2. AI 分析问题，生成 求助文档.md
3. 你复制文档到智谱，得到回复
4. 你把回复保存到 求助结果.md
5. 回复"下一步"
6. AI 读取结果，显示方案，确认后修改代码
```

**使用示例：**
```
大哥救救我
```

[查看完整文档 →](./大哥救救我/SKILL.md)

---

## creating-obsidian-vaults

将网页、文章、想法等内容自动整理成结构化的 Obsidian 笔记。

**核心能力：**
- 爬取网页内容，自动分类归入知识库
- 下载图片到本地 `assets/`，在笔记中本地引用
- 自动生成 Frontmatter（标题、日期、来源、标签、分类、摘要）
- 建立笔记间 `[[双向链接]]`，维护关系图谱
- 自动创建/更新 MOC 索引（分类 README.md + 根目录总索引）
- 一键整理知识库：清理垃圾、补充元数据、修复链接

**使用示例：**
```
记一下 https://example.com/article
帮我把这个存到知识库：https://example.com/post
整理一下知识库
```

[查看完整文档 →](./creating-obsidian-vaults/SKILL.md)

---

## website-cloner

自动化网站复刻工具 — 使用 Playwright 浏览网站、截图、测试交互，生成完整复刻文档。

**核心能力：**
- 自动导航到指定 URL，分析网站结构
- 截图所有页面和区块（全页、首屏、导航、主内容等）
- 测试所有交互：悬停、点击、拖拽、表单填写、动画等待
- 记录所有状态变化，截图保存
- 生成分层复刻文档（概览 + 详细技术附录）
- 输出有序的目录结构，截图命名规范

**使用示例：**
```
/website-cloner https://example.com
帮我复刻这个网站：https://example.com
```

**前置依赖：** 需要安装 Playwright MCP

[查看完整文档 →](./website-cloner/SKILL.md)

---

## 安装方法

### 方式一：一键安装（推荐）

复制以下内容发给你的 AI 助手：

```
请帮我从 GitHub 安装 wpz1212ccl/pgstar_skills 仓库中的 skills：
1. 克隆 https://github.com/wpz1212ccl/pgstar_skills
2. 将需要的 skill 文件夹复制到我的全局 skills 目录
3. 重启 AI 工具生效
```

### 方式二：手动安装

将 skill 文件夹复制到对应工具的全局目录：

| 工具 | Windows 路径 | Linux/macOS 路径 |
|------|-------------|-----------------|
| Claude Code | `C:\Users\<用户名>\.claude\skills\` | `~/.claude/skills/` |
| OpenCode | `C:\Users\<用户名>\.config\opencode\skills\` | `~/.config/opencode/skills/` |
| Mimo Code | `C:\Users\<用户名>\.local\share\mimocode\custom_skills\` | `~/.local/share/mimocode/custom_skills/` |

安装后重启 AI 工具即可自动加载。

---

## 目录结构

```
pgstar_skills/
├── README.md                      # 本文件
├── 大哥救救我/
│   ├── SKILL.md                   # 半自动求助 skill
│   ├── config.json                # 配置文件
│   ├── agents/
│   │   └── openai.yaml            # UI 元数据
│   ├── references/
│   │   └── examples.md            # 示例文档
│   └── templates/
│       ├── help-doc.md            # 求助文档模板
│       └── prompt.txt             # 提示词模板
├── creating-obsidian-vaults/
│   └── SKILL.md                   # Obsidian 笔记整理 skill
└── website-cloner/
    └── SKILL.md                   # 网站复刻工具 skill
```

---

## 贡献

欢迎提交新的 skill！请确保：
1. 创建独立的文件夹，包含 `SKILL.md` 文件
2. SKILL.md 包含正确的 YAML frontmatter（name 和 description）
3. 更新本 README 添加新 skill 说明

---

## 许可证

[MIT License](https://opensource.org/licenses/MIT)

---

## 作者

[wpz1212ccl](https://github.com/wpz1212ccl)
