---
name: website-cloner
description: >
  自动化网站复刻工具（v3增强版）。使用Playwright MCP浏览指定网站，通过子agent并行处理各区域，
  全量遍历所有可交互元素，对每个元素测试默认/悬停/点击三种状态并截图，录制动态效果（MP4/GIF），
  爬取所有素材资源（用多模态能力描述命名），生成详细的分层复刻文档（含HTML骨架+CSS布局+状态矩阵）。
  当用户想要：(1) 复刻某个网站设计 (2) 分析网站的交互和动画 (3) 记录网站的完整UI/UX (4) 生成网站设计文档时触发。
  关键词：复刻网站、clone website、网站截图、网站分析、website capture、site analyzer。
---

# Website Cloner v3

自动化网站复刻工具 — 子agent并行、全量遍历、动态录制、多模态命名、详细文档。

## 架构设计

```
主Agent（协调+监督+整合）
├── 子Agent-1：Hero区域
├── 子Agent-2：Work区域
├── 子Agent-3：Footer区域
└── ...
```

## 主Agent职责

1. **协调**：分析页面结构，划分区域，分配任务给子agent
2. **监督**：监控子agent进度，处理异常
3. **整合**：收集子agent结果，生成最终文档

## 子Agent职责

每个子agent负责一个区域，独立完成：
- 区域内所有元素的状态截图
- 动态效果录制
- 素材资源下载（多模态描述命名）
- 直接写入对应文件夹

## 工作流程

### 阶段1：主Agent - 结构分析

```
1. 设置视口：1920x1080
2. 导航到URL，等待加载
3. 分析页面结构，识别所有区域
4. 为每个区域创建子agent任务
5. 并行启动所有子agent
```

### 阶段2：子Agent - 区域处理

每个子agent接收以下输入：

```
输入信息：
- URL：目标网站地址
- 区域信息：名称、位置、描述
- 元素列表：该区域所有可交互元素
- 命名规则：截图和素材的命名规范
- 输出路径：该区域的文件夹路径
```

子agent执行：

```
1. 打开新页面，导航到URL
2. 滚动到目标区域
3. 遍历区域内每个元素：
   a. 截图默认状态
   b. 悬停 → 截图悬停状态
   c. 点击 → 截图点击状态
   d. 如是动态元素 → 录制MP4/GIF
   e. 如是链接 → 记录URL，不跳转
4. 下载区域内所有素材：
   a. 截图素材 → 多模态描述命名
   b. HTML alt/title辅助
   c. 写入assets/{区域名}/
5. 直接写入文件到输出路径
```

### 阶段3：主Agent - 文档生成

```
1. 等待所有子agent完成
2. 收集各区域结果
3. 生成HTML骨架文档
4. 生成完整README.md
```

## 目录结构

```
website-clone-{域名}/
├── README.md                          # 复刻文档
├── structure.html                     # HTML骨架+CSS布局
├── screenshots/                       # 截图文件夹
│   ├── 01-hero/                       # 区域1
│   │   ├── title/
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   └── click.png
│   │   ├── cta-button/
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   ├── click.png
│   │   │   └── hover.gif
│   │   └── ...
│   ├── 02-work/                       # 区域2
│   │   └── ...
│   └── full-page.png
├── assets/                            # 素材资源（按区域分文件夹）
│   ├── 01-hero/                       # Hero区域素材
│   │   ├── 01-warm-cream-background.jpg
│   │   ├── 02-elegant-script-logo.png
│   │   └── ...
│   ├── 02-work/                       # Work区域素材
│   │   ├── 01-project-showcase-1.jpg
│   │   ├── 02-project-video-preview.mp4
│   │   └── ...
│   └── 03-footer/                     # Footer区域素材
│       └── ...
└── videos/                            # 录制的动态效果
    ├── 01-hero-load.mp4
    ├── 02-card-hover.gif
    └── ...
```

## 素材命名规则

```
命名方式：多模态描述 + HTML属性辅助

1. 截图素材 → 调用多模态模型生成描述
2. 读取HTML的alt/title属性作为参考
3. 结合位置信息生成最终命名

格式：{序号}-{位置}-{描述}.{扩展名}

示例：
01-warm-cream-background.jpg      # 多模态描述：温暖奶油色背景
02-elegant-script-logo.png       # 多模态描述：优雅手写体Logo
03-project-showcase-1.jpg        # 多模态描述：项目展示图1
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

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>{网站名称} - HTML骨架</title>
  <style>
    /* 页面整体布局 */
    body { margin: 0; padding: 0; font-family: sans-serif; }
    
    /* 区域划分 */
    .hero { min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center; }
    .work { padding: 80px 0; }
    .footer { padding: 40px 0; text-align: center; }
    
    /* 元素样式 */
    .title { font-size: 48px; font-weight: bold; }
    .subtitle { font-size: 16px; color: #666; }
    .cta-button { padding: 12px 24px; background: #007AFF; color: white; border: none; border-radius: 8px; }
    .card { padding: 20px; margin: 10px; border: 1px solid #eee; }
  </style>
</head>
<body>
  <!-- Hero 区域 -->
  <section class="hero">
    <h1 class="title">Jackie Hu</h1>
    <p class="subtitle">Product Design Verb & Noun</p>
    <button class="cta-button">Get Started</button>
  </section>
  
  <!-- Work 区域 -->
  <section class="work">
    <div class="card">
      <h2>Project 1</h2>
      <a href="https://example.com" target="_blank">查看项目</a>
    </div>
  </section>
  
  <!-- Footer 区域 -->
  <footer class="footer">
    <a href="https://linkedin.com">LinkedIn</a>
    <a href="https://twitter.com">Twitter</a>
  </footer>
</body>
</html>
```

## README文档结构

### 概览层

```markdown
# 网站复刻文档 — {网站名称}

## 基本信息
- 网站名称：
- 网站URL：
- 复刻日期：
- 视口尺寸：1920x1080

## HTML骨架
→ 查看 [structure.html](./structure.html)

## 设计总结
- 整体风格：
- 配色方案（表格）：
- 字体使用（表格）：
- 布局特点：

## 区域划分
1. Hero区域 → [截图](screenshots/01-hero/)
2. Work区域 → [截图](screenshots/02-work/)
3. Footer区域 → [截图](screenshots/03-footer/)
```

### 区域详细分析

```markdown
## 区域分析

### 1. Hero 区域

#### HTML结构
```html
<section class="hero">
  <h1 class="title">Jackie Hu</h1>
  <p class="subtitle">Product Design Verb & Noun</p>
  <button class="cta-button">Get Started</button>
</section>
```

#### CSS布局
- 布局方式：Flexbox 居中
- 最小高度：100vh
- 背景色：#FFFAF5

#### 元素状态矩阵

| 元素 | 默认 | 悬停 | 点击 | 动态 |
|------|------|------|------|------|
| 标题 | [截图] | 无 | 无 | 无 |
| CTA按钮 | [截图] | [截图] | [截图] | GIF |

#### 元素详细规格

**标题（title）**
- 文本："Jackie Hu"
- 字体：Historia Sky Script 85px
- 颜色：rgb(62, 62, 66)
- 默认状态：[截图链接]
- 悬停状态：无变化
- 动画：无

**CTA按钮**
- 文本："Get Started"
- 字体：SF Pro Variable 13px
- 背景色：rgb(0, 122, 255)
- 圆角：8px
- 默认状态：[截图链接]
- 悬停状态：[截图链接] - 背景色变深
- 点击状态：[截图链接] - 背景色更深
- 动画：[GIF链接]
  - 类型：颜色过渡
  - 持续时间：0.2s
  - 缓动：ease
  ```css
  .cta-button { transition: background-color 0.2s ease; }
  .cta-button:hover { background-color: #0056CC; }
  ```

#### 链接记录
| 链接文本 | URL | 位置 |
|----------|-----|------|
| Get Started | #contact | CTA按钮 |
```

### 附录

```markdown
## 附录A：素材资源清单

| 区域 | 序号 | 文件名 | 类型 | 描述 |
|------|------|--------|------|------|
| Hero | 01 | 01-warm-cream-background.jpg | 图片 | 温暖奶油色背景 |
| Hero | 02 | 02-elegant-script-logo.png | 图片 | 优雅手写体Logo |
| Work | 01 | 01-project-showcase-1.jpg | 图片 | 项目展示图 |

## 附录B：动画清单

| 区域 | 动画名称 | 类型 | 触发 | 时长 | 缓动 |
|------|----------|------|------|------|------|
| Hero | 按钮悬停 | 过渡 | hover | 0.2s | ease |

## 附录C：链接清单

| 区域 | 链接文本 | URL | 位置 |
|------|----------|-----|------|
| Hero | Get Started | #contact | CTA按钮 |
| Footer | LinkedIn | https://linkedin.com | 页脚链接 |
```

## 子Agent任务模板

```
## 任务：处理 {区域名} 区域

### 输入信息
- URL：{网站URL}
- 区域名：{区域名}
- 区域位置：{页面位置}
- 区域描述：{区域功能描述}
- 元素列表：
  1. {元素1名} - {元素1类型} - {是否可交互}
  2. {元素2名} - {元素2类型} - {是否可交互}
  ...
- 输出路径：website-clone-{域名}/screenshots/{区域序号}-{区域名}/
- 素材路径：website-clone-{域名}/assets/{区域序号}-{区域名}/

### 命名规则
- 截图：{元素名}/default.png, hover.png, click.png
- 动态：{元素名}/hover.gif 或 animation.mp4
- 素材：{序号}-{位置}-{多模态描述}.{扩展名}

### 任务清单
1. 打开新页面，导航到URL
2. 滚动到目标区域
3. 遍历每个元素：
   - 截图默认状态
   - 悬停 → 截图
   - 点击 → 截图
   - 动态元素 → 录制
   - 链接 → 记录URL，不跳转
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
2. 分析页面结构
3. 识别所有区域和元素
4. 创建子agent任务

### 阶段2：并行处理（子Agent）
- 无限制并发
- 每个区域一个子agent
- 直接写入文件

### 阶段3：文档生成（主Agent）
1. 等待所有子agent完成
2. 生成HTML骨架
3. 生成README文档
4. 输出统计信息

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
