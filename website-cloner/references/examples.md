# Website Cloner - 参考示例

## 目录结构示例

```
website-clone-jackiehu.design/
├── README.md
├── structure.html
├── screenshots/
│   ├── 01-hero/
│   │   ├── title/
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   └── click.png
│   │   ├── subtitle/
│   │   │   ├── default.png
│   │   │   └── hover.png
│   │   ├── cta-button/
│   │   │   ├── default.png
│   │   │   ├── hover.png
│   │   │   ├── click.png
│   │   │   └── hover.gif
│   │   ├── music-player/
│   │   │   ├── default.png
│   │   │   └── hover.png
│   │   └── airdrop-popup/
│   │       ├── default.png
│   │       └── hover.png
│   ├── 02-cooking/
│   │   ├── title/
│   │   ├── project-name/
│   │   └── description/
│   ├── 03-made/
│   │   ├── title/
│   │   ├── zenly-card/
│   │   ├── just-card/
│   │   ├── drigmo-card/
│   │   └── discover-card/
│   ├── 04-work/
│   │   ├── title/
│   │   ├── alora-card/
│   │   ├── nba-card/
│   │   ├── 100-days-card/
│   │   └── oddio-card/
│   ├── 05-about/
│   │   ├── title/
│   │   ├── bio/
│   │   └── social-links/
│   ├── full-page.png
│   └── scroll-*.png
├── assets/
│   ├── 01-hero/
│   │   ├── 01-jackie-hu-title.png
│   │   ├── 02-product-design-subtitle.png
│   │   ├── 03-music-player-widget.png
│   │   └── 04-airdrop-popup-decoration.png
│   ├── 02-cooking/
│   │   ├── 01-little-things-app-icon.png
│   │   └── 02-app-screenshot.png
│   ├── 03-made/
│   │   ├── 01-zenly-project-cover.png
│   │   └── 02-just-project-cover.png
│   └── 04-work/
│       └── ...
└── videos/
    ├── 01-hero-load-animation.mp4
    ├── 02-card-hover-effect.gif
    ├── 03-scroll-reveal.mp4
    └── 04-button-click.gif
```

## README示例

```markdown
# 网站复刻文档 — Jackie Hu Design

## 基本信息
- 网站名称：Jackie Hu — Designer
- 网站URL：https://jackiehu.design/
- 复刻日期：2026-07-18
- 视口尺寸：1920x1080
- 技术栈：Framer

## HTML骨架
→ 查看 [structure.html](./structure.html)

## 设计总结

### 配色方案
| 用途 | 颜色值 | 说明 |
|------|--------|------|
| 背景色 | #FFFAF5 | 温暖奶油色 |
| 主文字 | rgb(62, 62, 66) | 深灰近黑 |
| 链接色 | rgb(0, 122, 255) | iOS蓝色 |

### 字体使用
| 元素 | 字体 | 大小 | 字重 |
|------|------|------|------|
| 标题 | Historia Sky Script | 85px | 400 |
| 正文 | IBM Plex Mono | 16px | 400 |

### 布局特点
- 单页滚动布局
- 居中对齐为主
- 大量留白
- 卡片式作品展示

## 区域划分
1. Hero区域 → [截图](screenshots/01-hero/)
2. Cooking区域 → [截图](screenshots/02-cooking/)
3. Made区域 → [截图](screenshots/03-made/)
4. Work区域 → [截图](screenshots/04-work/)
5. About区域 → [截图](screenshots/05-about/)

## 截图统计
- 总截图数：64
- 动态录制数：8
- 素材资源数：12

---

## 区域分析

### 1. Hero 区域

#### HTML结构
```html
<section class="hero">
  <h1 class="title">Jackie Hu</h1>
  <p class="subtitle">Product Design Verb & Noun</p>
  <p class="description">a thoughtful process of crafting experiences...</p>
  <div class="music-player">...</div>
  <div class="airdrop-popup">...</div>
</section>
```

#### CSS布局
- 布局方式：Flexbox 居中
- 最小高度：100vh
- 背景色：#FFFAF5

#### 元素状态矩阵

| 元素 | 默认 | 悬停 | 点击 | 动态 |
|------|------|------|------|------|
| 标题 | [截图](screenshots/01-hero/title/default.png) | 无 | 无 | 无 |
| 副标题 | [截图](screenshots/01-hero/subtitle/default.png) | 无 | 无 | 无 |
| CTA按钮 | [截图](screenshots/01-hero/cta-button/default.png) | [截图](screenshots/01-hero/cta-button/hover.png) | [截图](screenshots/01-hero/cta-button/click.png) | GIF |
| 音乐播放器 | [截图](screenshots/01-hero/music-player/default.png) | [截图](screenshots/01-hero/music-player/hover.png) | - | - |

#### 元素详细规格

**标题（title）**
- 文本："Jackie Hu"
- 字体：Historia Sky Script 85px
- 颜色：rgb(62, 62, 66)
- 默认状态：[截图](screenshots/01-hero/title/default.png)
- 悬停状态：无变化
- 动画：无

**CTA按钮**
- 文本："Get Started"
- 字体：SF Pro Variable 13px
- 背景色：rgb(0, 122, 255)
- 圆角：8px
- 默认状态：[截图](screenshots/01-hero/cta-button/default.png)
- 悬停状态：[截图](screenshots/01-hero/cta-button/hover.png) - 背景色变深
- 点击状态：[截图](screenshots/01-hero/cta-button/click.png)
- 动画：[GIF](videos/02-card-hover.gif) - 颜色过渡 0.2s ease
```css
.cta-button { transition: background-color 0.2s ease; }
.cta-button:hover { background-color: #0056CC; }
```

**音乐播放器（music-player）**
- 类型：装饰元素
- 显示：歌曲名、艺术家、播放进度
- 默认状态：[截图](screenshots/01-hero/music-player/default.png)
- 悬停状态：[截图](screenshots/01-hero/music-player/hover.png)

---

## 附录A：素材资源清单

| 区域 | 序号 | 文件名 | 类型 | 描述 |
|------|------|--------|------|------|
| Hero | 01 | 01-jackie-hu-title.png | 图片 | 优雅手写体标题 |
| Hero | 02 | 02-product-design-subtitle.png | 图片 | 产品设计副标题 |
| Cooking | 01 | 01-little-things-app-icon.png | 图片 | 应用图标 |

## 附录B：动画清单

| 区域 | 动画名称 | 类型 | 触发 | 时长 | 缓动 |
|------|----------|------|------|------|------|
| Hero | 按钮悬停 | 过渡 | hover | 0.2s | ease |
| Made | 卡片展开 | 动画 | click | 0.5s | cubic-bezier |

## 附录C：链接清单

| 区域 | 链接文本 | URL | 位置 |
|------|----------|-----|------|
| Made | Zenly | https://zenly.com/ | Zenly卡片 |
| Made | JUST | https://www.getjust.eu/ | JUST卡片 |
| About | LinkedIn | https://linkedin.com/in/jackiehu-design/ | 社交链接 |
```

## structure.html 示例

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>Jackie Hu Design - HTML骨架</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    
    :root {
      --color-bg: #FFFAF5;
      --color-text: rgb(62, 62, 66);
      --color-text-light: rgb(105, 100, 94);
      --color-link: rgb(0, 122, 255);
      --font-title: "Historia Sky Script", sans-serif;
      --font-body: "IBM Plex Mono", monospace;
    }
    
    body {
      font-family: var(--font-body);
      color: var(--color-text);
      background: var(--color-bg);
    }
    
    .hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 40px;
    }
    
    .title {
      font-family: var(--font-title);
      font-size: 85px;
      font-weight: 400;
      margin-bottom: 16px;
    }
    
    .subtitle {
      font-size: 16px;
      color: var(--color-text-light);
      margin-bottom: 24px;
    }
    
    .description {
      font-size: 16px;
      max-width: 600px;
      text-align: center;
      line-height: 1.6;
    }
    
    .music-player {
      position: fixed;
      bottom: 20px;
      right: 20px;
      padding: 12px;
      background: rgba(255, 255, 255, 0.9);
      border-radius: 8px;
      font-size: 9px;
    }
    
    .airdrop-popup {
      position: fixed;
      bottom: 100px;
      right: 20px;
      padding: 16px;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    }
    
    .section {
      padding: 80px 40px;
    }
    
    .section-title {
      font-size: 16px;
      color: var(--color-text-light);
      margin-bottom: 32px;
    }
    
    .card {
      padding: 20px;
      margin-bottom: 16px;
      border-bottom: 1px solid #eee;
      transition: background-color 0.2s ease;
    }
    
    .card:hover {
      background-color: rgba(0, 0, 0, 0.02);
    }
    
    a {
      color: var(--color-link);
      text-decoration: underline;
    }
  </style>
</head>
<body>
  <section class="hero">
    <h1 class="title">Jackie Hu</h1>
    <p class="subtitle">Product Design Verb & Noun</p>
    <p class="description">a thoughtful process of crafting experiences that engage people, shape clarity, and spark delights.</p>
  </section>
  
  <section class="section">
    <h2 class="section-title">Currently cooking ☺︎</h2>
    <div class="project">
      <h3>The Little Things</h3>
      <p>A fun lil app that helps people better self-introspect with guided questions & personal AI insights.</p>
    </div>
  </section>
  
  <section class="section">
    <h2 class="section-title">Recently Made ▶</h2>
    <div class="card">
      <a href="https://zenly.com/" target="_blank">Zenly (Snap. Inc)</a>
      <p>Live map of close friends and family</p>
    </div>
    <div class="card">
      <a href="https://www.getjust.eu/" target="_blank">JUST</a>
      <p>Simple, smart, social shopping</p>
    </div>
  </section>
  
  <section class="section">
    <h2 class="section-title">Other Work ⁕</h2>
    <div class="card">
      <h3>Alora</h3>
      <p>Chrome extension | 2020</p>
      <p>Personal data management and data tracking transparency.</p>
    </div>
  </section>
  
  <section class="section">
    <h2 class="section-title">About ⌘</h2>
    <p>I'm a product designer(she/her) who loves crafting meaningful interactions and bringing fun ideas to life. Currently based in Paris, France.</p>
    <p>
      <a href="https://linkedin.com/in/jackiehu-design/">LinkedIn</a> | 
      <a href="https://x.com/itsjackiehu">Twitter (x)</a>
    </p>
  </section>
  
  <div class="music-player">
    <div>1:10 / 3:32</div>
    <div>Jazz et thé vert</div>
    <div>Souleance</div>
  </div>
  
  <div class="airdrop-popup">
    <p>Final_Final_Final</p>
    <p>AirDrop</p>
    <p>Jackie would like to share a photo</p>
    <button>Decline</button>
    <button>Accept</button>
  </div>
</body>
</html>
```
