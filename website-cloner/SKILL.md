---
name: website-cloner
description: >
  自动化网站复刻工具。使用Playwright MCP浏览指定网站，通过子agent并行处理各区域，
  全量遍历所有可交互元素，对每个元素测试默认/悬停/点击三种状态并截图，
  录制动态效果（MP4/GIF），爬取素材资源（多模态描述命名），
  生成详细复刻文档（含HTML骨架+CSS布局+状态矩阵）。
  触发词：复刻网站、clone website、网站截图、website capture。
---

# Website Cloner

自动化网站复刻工具 — 子agent并行、全量遍历、动态录制、多模态命名、详细文档。

## ⚠️ 核心原则

**每张截图必须对应明确的区域和元素，禁止无意义截图！**

```
截图前确认：
1. 哪个区域？（hero/cooking/work/about）
2. 哪个元素？（title/button/card）
3. 什么状态？（default/hover/click）
4. 是否录制？（静态 or 动态）
```

## 架构

```
主Agent（协调+监督+整合）
├── 子Agent-1：Hero区域
├── 子Agent-2：Cooking区域
├── 子Agent-3：Work区域
└── ...
```

## 工作流程

### 阶段1：主Agent - 结构分析
1. 设置视口 1920x1080，导航到URL
2. 分析页面结构，识别所有区域
3. 为每个区域创建子agent任务（包含元素清单）
4. 并行启动所有子agent

### 阶段2：子Agent - 区域处理
每个子agent独立完成：
- 区域内每个元素：截图 default → hover → click
- 动态元素：录制 MP4/GIF
- 素材下载：多模态描述命名
- 直接写入对应文件夹

### 阶段3：主Agent - 文档生成
1. 检查截图完整性
2. 生成 structure.html + README.md
3. 输出统计报告

## 目录结构

```
website-clone-{域名}/
├── README.md                    # 复刻文档
├── structure.html               # HTML骨架+CSS布局
├── screenshots/
│   ├── 01-hero/                 # 按区域分
│   │   ├── title/               # 按元素分
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   └── click.png
│   │   └── cta-button/
│   ├── 02-cooking/
│   └── full-page.png
├── assets/                      # 按区域分文件夹
│   ├── 01-hero/
│   │   └── 01-title-description.png  # 多模态命名
│   └── 02-cooking/
└── videos/                      # 动态录制（必须有内容！）
    ├── 01-hero-load.mp4
    └── 02-card-hover.gif
```

## 命名规则

**截图**：`screenshots/{区域}/{元素}/{状态}.png`
```
✅ screenshots/01-hero/title/default.png
❌ screenshots/untitled-1.png
```

**素材**：`{序号}-{多模态描述}.{扩展名}`
```
✅ 01-jackie-hu-title.png
❌ img-001.png
```

## 动态录制（必须执行）

| 场景 | 格式 | 时长/帧率 |
|------|------|----------|
| 页面加载动画 | MP4 | 1-3秒 |
| 滚动触发显示 | MP4 | 2-3秒 |
| 卡片/按钮悬停 | GIF | 24fps |
| 展开/收起动画 | MP4 | 1-2秒 |

## 链接处理

**不跳转**，只记录：
- 链接文本、URL、位置、样式
- 截图链接在当前页面的样式

## 文档要求

**README.md** 必须包含：
- 基本信息 + HTML骨架链接
- 设计总结（配色、字体表格）
- 每个区域：HTML结构 + CSS布局 + 元素状态矩阵 + 详细规格
- 附录：素材清单 + 动画清单 + 链接清单

**structure.html** 必须包含：
- 完整HTML骨架
- CSS变量定义
- 区域布局样式
- 元素样式
- 动画定义

## 子Agent任务模板

```
## 任务：处理 {区域名}

### 输入
- URL：{网站URL}
- 区域名：{区域名}
- 元素清单：
  1. {元素名} - {类型} - {可交互?} - {动态?}
  2. ...

### 输出路径
- 截图：screenshots/{区域序号}-{区域名}/
- 素材：assets/{区域序号}-{区域名}/

### 执行
1. 导航到URL，滚动到区域
2. 对每个元素：
   a. 确认：这是{区域}的{元素}
   b. 截图 default → {元素}/default.png
   c. 悬停 → 截图 → {元素}/hover.png
   d. 点击 → 截图 → {元素}/click.png
   e. 有动画 → 录制 → {元素}/animation.mp4
3. 下载素材，多模态命名
4. 返回：区域名 + 元素数 + 截图数 + 素材数
```

## 错误处理

- URL无法访问/加载超时/Playwright不可用 → 停止并报告
- 子agent失败 → 重试或跳过

## 注意事项

- 视口：1920x1080
- 不处理登录内容
- 截图PNG，动态MP4+GIF
- 链接只记录不跳转
- **截图必须对应区域和元素，禁止乱截图！**

详细参考：[references/examples.md](references/examples.md)
