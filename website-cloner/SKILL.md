---
name: website-cloner
description: >
  自动化网站复刻工具（v4增强版）。使用Playwright MCP浏览指定网站，通过子agent并行处理各区域，
  全量遍历所有可交互元素，对每个元素测试默认/悬停/点击三种状态并截图，录制动态效果（MP4/GIF），
  爬取所有素材资源（用多模态能力描述命名），生成详细的分层复刻文档（含HTML骨架+CSS布局+状态矩阵）。
  当用户想要：(1) 复刻某个网站设计 (2) 分析网站的交互和动画 (3) 记录网站的完整UI/UX (4) 生成网站设计文档时触发。
  关键词：复刻网站、clone website、网站截图、网站分析、website capture、site analyzer。
---

# Website Cloner v4

自动化网站复刻工具 — 子agent并行、全量遍历、动态录制、多模态命名、详细文档。

## ⚠️ 核心原则：杜绝乱截图

**每一张截图都必须对应明确的区域和元素，禁止无意义截图！**

```
截图前必须确认：
1. 这是哪个区域？（hero/cooking/work/about...）
2. 这是哪个元素？（title/button/card...）
3. 这是什么状态？（default/hover/click）
4. 是否需要录制？（静态截图 or 动态录制）
```

## 架构设计

```
主Agent（协调+监督+整合）
├── 子Agent-1：Hero区域 → 只处理Hero内的元素
├── 子Agent-2：Cooking区域 → 只处理Cooking内的元素
├── 子Agent-3：Work区域 → 只处理Work内的元素
└── ...
```

## 主Agent职责

1. **协调**：分析页面结构，划分区域，分配任务给子agent
2. **监督**：监控子agent进度，确保截图对应正确区域和元素
3. **整合**：收集子agent结果，生成最终文档

## 子Agent职责

每个子agent负责一个区域，独立完成：
- 区域内所有元素的状态截图（必须对应元素！）
- 动态效果录制（动画、悬停动效）
- 素材资源下载（多模态描述命名）
- 直接写入对应文件夹

## 工作流程

### 阶段1：主Agent - 结构分析

```
1. 设置视口：1920x1080
2. 导航到URL，等待加载
3. 分析页面结构，识别所有区域
4. 为每个区域创建子agent任务，包含：
   - 区域名称和位置
   - 该区域内的所有元素清单
   - 每个元素的类型和是否可交互
5. 并行启动所有子agent
```

### 阶段2：子Agent - 区域处理

每个子agent接收以下输入：

```
输入信息：
- URL：目标网站地址
- 区域信息：名称、位置、描述
- 元素清单（必须明确列出）：
  1. title - 标题文本 - 静态
  2. subtitle - 副标题文本 - 静态
  3. cta-button - 按钮 - 可交互
  4. card-1 - 项目卡片 - 可交互
  ...
- 输出路径：该区域的文件夹路径
- 素材路径：该区域的素材文件夹路径
```

子agent执行（严格对应区域和元素）：

```
对每个元素：
1. 滚动到元素位置
2. 确认：这是{区域名}的{元素名}
3. 截图默认状态 → 写入 {元素名}/default.png
4. 悬停到该元素 → 截图 → 写入 {元素名}/hover.png
5. 点击该元素 → 截图 → 写入 {元素名}/click.png
6. 如果有动画 → 录制 → 写入 {元素名}/animation.mp4 或 hover.gif
```

### 阶段3：主Agent - 文档生成

```
1. 等待所有子agent完成
2. 检查每个区域的截图是否完整
3. 生成HTML骨架文档（structure.html）
4. 生成完整README.md
5. 输出统计报告
```

## 目录结构（严格规范）

```
website-clone-{域名}/
├── README.md                          # 复刻文档
├── structure.html                     # HTML骨架+CSS布局
├── screenshots/                       # 截图文件夹（按区域分）
│   ├── 01-hero/                       # Hero区域
│   │   ├── title/                     # 标题元素
│   │   │   ├── default.png            # 默认状态
│   │   │   ├── hover.png              # 悬停状态
│   │   │   └── click.png              # 点击状态
│   │   ├── subtitle/                  # 副标题元素
│   │   │   ├── default.png
│   │   │   └── hover.png
│   │   ├── cta-button/                # CTA按钮
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   ├── click.png
│   │   │   └── hover.gif              # 悬停动效
│   │   └── music-player/              # 音乐播放器装饰
│   │       ├── default.png
│   │       └── hover.png
│   ├── 02-cooking/                    # Cooking区域
│   │   ├── title/
│   │   ├── project-name/
│   │   └── description/
│   ├── 03-made/                       # Made区域
│   │   ├── title/
│   │   ├── zenly-card/
│   │   ├── just-card/
│   │   └── ...
│   ├── 04-work/                       # Work区域
│   │   └── ...
│   ├── 05-about/                      # About区域
│   │   └── ...
│   ├── full-page.png                  # 全页截图
│   └── scroll-*.png                   # 滚动位置截图
├── assets/                            # 素材资源（按区域分文件夹）
│   ├── 01-hero/                       # Hero区域素材
│   │   ├── 01-jackie-hu-title.png     # 多模态描述命名
│   │   ├── 02-product-design-logo.png
│   │   └── 03-music-player-icon.png
│   ├── 02-cooking/                    # Cooking区域素材
│   │   ├── 01-little-things-app.png
│   │   └── 02-app-screenshot.png
│   ├── 03-made/                       # Made区域素材
│   │   └── ...
│   └── 04-work/                       # Work区域素材
│       └── ...
└── videos/                            # 录制的动态效果（必须有内容！）
    ├── 01-hero-load.mp4               # 页面加载动画
    ├── 02-card-hover.gif              # 卡片悬停动效
    ├── 03-scroll-reveal.mp4           # 滚动显示动画
    └── 04-button-click.gif            # 按钮点击动效
```

## 截图命名规则（严格规范）

```
每个截图必须对应明确的区域和元素：

格式：screenshots/{区域序号}-{区域名}/{元素名}/{状态}.png

示例：
screenshots/01-hero/title/default.png      # Hero区域标题默认状态
screenshots/01-hero/title/hover.png        # Hero区域标题悬停状态
screenshots/01-hero/cta-button/hover.png   # Hero区域CTA按钮悬停状态
screenshots/03-made/zenly-card/click.png   # Made区域Zenly卡片点击状态

禁止的命名：
❌ screenshots/untitled-1.png
❌ screenshots/random-screenshot.png
❌ screenshots/img-001.png
```

## 素材命名规则（多模态描述）

```
命名方式：多模态描述 + HTML属性辅助

1. 截图素材 → 调用多模态模型生成描述
2. 读取HTML的alt/title属性作为参考
3. 结合位置信息生成最终命名

格式：{序号}-{多模态描述}.{扩展名}

示例：
01-jackie-hu-title.png              # 多模态：优雅手写体标题"Jackie Hu"
02-product-design-subtitle.png      # 多模态：产品设计副标题
03-music-player-widget.png          #多模态：音乐播放器小部件
04-airdrop-popup-decoration.png     # 多模态：AirDrop弹窗装饰
05-little-things-app-icon.png       # 多模态：Little Things应用图标
```

## 动态效果录制（必须执行！）

```
以下场景必须录制MP4或GIF：

1. 页面加载动画 → MP4（1-3秒）
2. 滚动触发显示 → MP4（2-3秒）
3. 卡片悬停效果 → GIF（24fps）
4. 按钮悬停效果 → GIF（24fps）
5. 展开/收起动画 → MP4（1-2秒）
6. 任何CSS动画 → 根据复杂度选择MP4或GIF
```

## 链接处理规则

```
遇到链接时：
1. 不跳转到链接目标
2. 记录以下信息：
   - 链接文本
   - 链接URL
   - 链接位置（区域、元素）
   - 链接样式（颜色、字体、装饰）
3. 截图链接在当前页面的样式（默认+悬停）
4. 在文档中标注：[链接] → URL
```

## HTML骨架文档（structure.html）

必须生成完整的HTML骨架，包含：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>{网站名称} - HTML骨架</title>
  <style>
    /* 页面整体布局 */
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: var(--font-body); color: var(--color-text); background: var(--color-bg); }
    
    /* CSS变量 */
    :root {
      --color-primary: #xxx;
      --color-bg: #xxx;
      --font-title: "xxx", sans-serif;
      --font-body: "xxx", sans-serif;
    }
    
    /* 区域布局 */
    .hero { min-height: 100vh; display: flex; flex-direction: column; align-items: center; }
    .cooking { padding: 80px 0; }
    .made { padding: 80px 0; }
    .work { padding: 80px 0; }
    .about { padding: 80px 0; }
    
    /* 元素样式 */
    .title { font-family: var(--font-title); font-size: 85px; }
    .subtitle { font-family: var(--font-body); font-size: 16px; }
    .card { padding: 20px; border: 1px solid #eee; }
    .card:hover { background: #f5f5f5; }
    
    /* 动画 */
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    .fade-in { animation: fadeIn 0.5s ease; }
  </style>
</head>
<body>
  <!-- Hero 区域 -->
  <section class="hero">
    <h1 class="title">Jackie Hu</h1>
    <p class="subtitle">Product Design Verb & Noun</p>
    <div class="music-player"><!-- 音乐播放器 --></div>
    <div class="airdrop-popup"><!-- AirDrop弹窗 --></div>
  </section>
  
  <!-- Cooking 区域 -->
  <section class="cooking">
    <h2>Currently cooking ☺︎</h2>
    <div class="project">...</div>
  </section>
  
  <!-- 其他区域... -->
</body>
</html>
```

## README文档结构（详细版）

### 概览层

```markdown
# 网站复刻文档 — {网站名称}

## 基本信息
- 网站名称：
- 网站URL：
- 复刻日期：
- 视口尺寸：1920x1080
- 技术栈：{框架/构建工具}

## HTML骨架
→ 查看 [structure.html](./structure.html)

## 设计总结
- 整体风格：
- 配色方案（表格）：
- 字体使用（表格）：
- 布局特点：

## 区域划分
1. Hero区域 → [截图](screenshots/01-hero/)
2. Cooking区域 → [截图](screenshots/02-cooking/)
3. Made区域 → [截图](screenshots/03-made/)
4. Work区域 → [截图](screenshots/04-work/)
5. About区域 → [截图](screenshots/05-about/)

## 截图统计
- 总截图数：{数量}
- 动态录制数：{数量}
- 素材资源数：{数量}
```

### 区域详细分析（每个区域都要详细！）

```markdown
## 区域分析

### 1. Hero 区域

#### HTML结构
```html
<section class="hero">
  <h1 class="title">Jackie Hu</h1>
  <p class="subtitle">Product Design Verb & Noun</p>
  <p class="description">a thoughtful process...</p>
</section>
```

#### CSS布局
- 布局方式：Flexbox 居中
- 最小高度：100vh
- 背景色：#FFFAF5
- 对齐方式：居中对齐

#### 元素状态矩阵

| 元素 | 默认 | 悬停 | 点击 | 动态 |
|------|------|------|------|------|
| 标题 | [截图] | 无 | 无 | 无 |
| 副标题 | [截图] | 无 | 无 | 无 |
| CTA按钮 | [截图] | [截图] | [截图] | GIF |
| 音乐播放器 | [截图] | [截图] | - | - |

#### 元素详细规格

**标题（title）**
- 文本："Jackie Hu"
- 字体：Historia Sky Script 85px
- 颜色：rgb(62, 62, 66)
- 位置：居中
- 默认状态：[截图链接](screenshots/01-hero/title/default.png)
- 悬停状态：无变化
- 点击状态：无变化
- 动画：无

**音乐播放器（music-player）**
- 类型：装饰元素
- 显示内容：歌曲名、艺术家、播放进度
- 默认状态：[截图链接](screenshots/01-hero/music-player/default.png)
- 悬停状态：[截图链接](screenshots/01-hero/music-player/hover.png)
- 动画：播放进度条动画

**AirDrop弹窗（airdrop-popup）**
- 类型：装饰元素
- 显示内容："Jackie would like to share a photo"
- 按钮：Decline / Accept
- 默认状态：[截图链接](screenshots/01-hero/airdrop/default.png)
- 悬停状态：[截图链接](screenshots/01-hero/airdrop/hover.png)

#### 装饰元素清单
| 元素 | 类型 | 位置 | 描述 |
|------|------|------|------|
| 音乐播放器 | 装饰 | Hero右侧 | 显示歌曲信息 |
| AirDrop弹窗 | 装饰 | Hero下方 | 模拟macOS弹窗 |
```

## 子Agent任务模板（详细版）

```
## 任务：处理 {区域名} 区域

### 输入信息
- URL：{网站URL}
- 区域名：{区域名}
- 区域位置：{页面位置}
- 区域描述：{区域功能描述}

### 元素清单（必须逐个处理！）
1. {元素1名} - {元素类型} - {是否可交互} - {是否动态}
2. {元素2名} - {元素类型} - {是否可交互} - {是否动态}
...

### 输出路径
- 截图：website-clone-{域名}/screenshots/{区域序号}-{区域名}/
- 素材：website-clone-{域名}/assets/{区域序号}-{区域名}/

### 命名规则
- 截图：{元素名}/default.png, hover.png, click.png
- 动态：{元素名}/hover.gif 或 animation.mp4
- 素材：{序号}-{多模态描述}.{扩展名}

### 任务清单（严格执行）
1. 打开新页面，导航到URL
2. 滚动到目标区域
3. 对每个元素执行：
   a. 确认：这是{区域名}的{元素名}
   b. 截图默认状态 → {元素名}/default.png
   c. 悬停到该元素 → 截图 → {元素名}/hover.png
   d. 点击该元素 → 截图 → {元素名}/click.png
   e. 如果有动画 → 录制 → {元素名}/animation.mp4
4. 下载素材，多模态描述命名
5. 直接写入文件

### 输出要求
- 截图文件写入：{输出路径}
- 素材文件写入：{素材路径}
- 完成后返回：区域名 + 元素数量 + 截图数量 + 素材数量
```

## 调用方式

用户输入：
```
/website-cloner https://example.com
```

## 执行策略

### 阶段1：结构分析（主Agent）
1. 导航到URL
2. 分析页面结构，识别所有区域
3. 为每个区域列出所有元素
4. 创建子agent任务

### 阶段2：并行处理（子Agent）
- 无限制并发
- 每个区域一个子agent
- 严格对应区域和元素
- 直接写入文件

### 阶段3：文档生成（主Agent）
1. 等待所有子agent完成
2. 检查截图完整性
3. 生成HTML骨架
4. 生成README文档
5. 输出统计报告

## 错误处理

- URL无法访问 → 停止并报告
- 页面加载超时（30秒）→ 停止并报告
- Playwright MCP不可用 → 停止并报告
- 子agent失败 → 主agent重试或跳过

## 注意事项

- 视口：1920x1080
- 不处理登录内容
- 不执行破坏性操作
- 截图：PNG
- 动态：MP4+GIF
- 素材：多模态描述命名
- 链接：只记录不跳转
- 文档：含HTML骨架+CSS布局
- **截图必须对应区域和元素，禁止乱截图！**
