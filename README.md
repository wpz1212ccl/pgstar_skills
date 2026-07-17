# PgStar Skills

> 我的 OpenCode 自定义 Skills 合集 — 让 AI 更懂你的工作流。

这是一个持续维护的个人 skill 仓库，包含我为 [OpenCode](https://opencode.ai) 编写的自定义 skill。

---

## Skills 列表

### 📚 creating-obsidian-vaults

将网页、文章、想法等内容自动整理成结构化的 Obsidian 笔记。

**核心能力：**
- 爬取网页内容，自动分类归入知识库
- 下载图片到本地 `assets/`，在笔记中本地引用
- 自动生成 Frontmatter（标题、日期、来源、标签、分类、摘要）
- 建立笔记间 [[双向链接]]，维护关系图谱
- 自动创建/更新 MOC 索引（分类 README.md + 根目录总索引）
- 一键整理知识库：清理垃圾、补充元数据、修复链接

[查看详情](./creating-obsidian-vaults/SKILL.md)

---

## 安装

将对应 skill 的文件夹复制到 OpenCode 的 skills 目录：

```bash
# 全局目录（推荐）
# Linux/macOS
~/.config/opencode/skills/
# Windows
C:\Users\<你的用户名>\.config\opencode\skills\

# 或项目目录
.opencode/skills/
```

然后重启 OpenCode，skill 即可自动加载。

---

## 关于

- 作者：[wpz1212ccl](https://github.com/wpz1212ccl)
- 许可证：MIT
