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

### 🌐 website-cloner

自动化网站复刻工具 — 使用 Playwright 浏览网站、截图、测试交互，生成完整复刻文档。

**核心能力：**
- 自动导航到指定 URL，分析网站结构
- 截图所有页面和区块（全页、首屏、导航、主内容等）
- 测试所有交互：悬停、点击、拖拽、表单填写、动画等待
- 记录所有状态变化，截图保存
- 生成分层复刻文档（概览 + 详细技术附录）
- 输出有序的目录结构，截图命名规范

**使用方法：**
```
/website-cloner https://example.com
```

[查看详情](./website-cloner/SKILL.md)

---

## 🚀 一键安装（复制发给 Claude Code / 任何 AI Agent）

> 把下面这段话复制发给你的 AI 助手，它会自动完成安装和配置：

```
请帮我安装 GitHub 上 wpz1212ccl/pgstar_skills 仓库中的 skills：
1. 从 https://github.com/wpz1212ccl/pgstar_skills 下载需要的 skill 文件夹
2. 放到全局 skills 目录：~/.config/opencode/skills/（Windows: C:\Users\<用户名>\.config\opencode\skills\）
3. 可用的 skills：
   - creating-obsidian-vaults：自动整理 Obsidian 笔记
   - website-cloner：自动化网站复刻工具
4. 配置完成后，重启 AI 工具即可使用
```

---

## 手动安装

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
