# 示例：OpenCode-Skin 首页壁纸遮挡问题

## 求助文档示例

### 一、项目背景

#### 1.1 这是什么
**OpenCode-Skin** 是一个为 OpenCode Desktop（Electron 应用）设计的**皮肤注入工具**。它通过 CDP（Chrome DevTools Protocol）连接 OpenCode 的渲染进程，注入自定义 CSS/JS，实现**全屏背景壁纸 + 半透明毛玻璃 UI** 的主题效果。

#### 1.2 技术架构
```
┌─────────────────────────────────────────────────┐
│  OpenCode Desktop (Electron)                    │
│  ┌───────────────────────────────────────────┐  │
│  │  渲染进程 (Renderer)                       │  │
│  │  ┌─────────────────────────────────────┐  │  │
│  │  │  <html class="opencode-dream-skin"> │  │  │
│  │  │    background-image: url(壁纸)      │  │  │  ← CSS 注入
│  │  │    ┌─ #root ──────────────────────┐  │  │  │
│  │  │    │  div (主容器, 半透明)          │  │  │  │  ← JS 注入
│  │  │    │    ├─ header (标题栏)          │  │  │  │
│  │  │    │    └─ main (内容区)            │  │  │  │
│  │  │    └──────────────────────────────┘  │  │  │
│  │  └─────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────┘  │
│                                                   │
│  CDP 端口: 9335 (--remote-debugging-port=9335)   │
└─────────────────────────────────────────────────┘
          ↑ 注入
┌─────────────────────────────────────────────────┐
│  injector.mjs (Node.js)                         │
│  1. 连接 CDP                                     │
│  2. 读取 dream-skin.css → 注入到 <style>          │
│  3. 读取 renderer-inject.js → Runtime.evaluate    │
│  4. 持续监控新窗口并自动注入                        │
└─────────────────────────────────────────────────┘
```

### 二、当前实现方式

#### 2.1 壁纸注入方式
**核心思路**：在 `<html>` 元素上设置壁纸背景图片，然后让 OpenCode 的 UI 元素变为半透明，使壁纸"透过来"。

```css
/* 壁纸设在 html 上 */
html.opencode-dream-skin {
  background-image: var(--dream-art) !important;  /* 壁纸 */
  background-size: cover !important;
  background-attachment: fixed !important;
}

/* 主容器半透明 */
html.opencode-dream-skin #root > div:first-child {
  background-color: rgba(249, 247, 241, 0.65) !important;  /* 65% 不透明 */
  backdrop-filter: blur(3px) !important;  /* 毛玻璃 */
}
```

### 三、遇到的问题

#### 3.1 问题描述
**"新建会话"页面的壁纸不显示**，被不透明的 UI 元素遮挡。但对话页面和主页（侧边栏）的壁纸正常显示。

#### 3.2 错误信息
通过 CDP 分析 "新建会话" 页面的 DOM 结构：
```
html.opencode-dream-skin          ← 壁纸设在这里 ✅
  └─ #root                        ← bg: transparent ✅
      └─ #root > div:first-child  ← bg: transparent ✅ (dream-home 类已正确添加)
          ├─ header               ← bg: rgba(253,251,245, 0.6) 半透明 ✅
          └─ main                 ← bg: rgba(253,251,245, 0.6) 半透明 ✅
      └─ div (sibling)            ← bg: rgba(255,255,255, 0.85) ← ⚠️ 这个元素 85% 不透明！
```

**关键发现**：`#root` 下有一个 `div` 兄弟元素（dialog/toast），它的背景是 `rgba(255, 255, 255, 0.85)`（85% 不透明白色），直接覆盖在壁纸上面。

#### 3.3 已尝试方案
| 次数 | 问题 | 根因 | 修复方式 |
|------|------|------|----------|
| 第 1 次 | 所有页面壁纸不显示 | `--dream-art` CSS 变量未设置 | JS 中设置 `root.style.setProperty("--dream-art", ...)` |
| 第 2 次 | 页面切换后首页壁纸消失 | MutationObserver 未监听 `childList` | 添加 `childList: true` 监听 |
| 第 3 次 | 新建会话页壁纸不显示 | CSS Specificity 不足，`.dream-home` 被覆盖 | 提高选择器 specificity |
| 第 4 次 | 新建会话页壁纸不显示 | 同上，OpenCode DOM 变化导致再次失效 | 再次调整 CSS |
| **第 5 次（当前）** | **新建会话页壁纸不显示** | **OpenCode 的 dialog/toast 元素遮挡壁纸** | **？？？** |

### 四、需要帮助的内容

#### 4.1 核心问题
**如何在不和 OpenCode 样式打架的前提下，让壁纸在所有页面（包括新建会话页面）都能正常显示？**

#### 4.2 期望结果
- 壁纸在所有页面（包括新建会话页面）都能正常显示
- 不需要为每个新元素添加CSS规则
- 方案具有健壮性，不会因OpenCode更新而失效

## 提示词示例

```
你好，我遇到了一个代码问题需要帮助。

## 问题背景
我在开发一个OpenCode Desktop的皮肤注入工具，通过CDP注入CSS/JS实现壁纸效果。现在遇到一个反复出现的问题：新建会话页面的壁纸总是被遮挡。

## 具体问题
"新建会话"页面的壁纸不显示，被不透明的UI元素遮挡。但对话页面和主页的壁纸正常显示。通过DOM分析发现，#root下有一个div兄弟元素（dialog/toast），背景是85%不透明白色，直接覆盖在壁纸上面。

## 已尝试方案
1. 给具体元素添加CSS规则覆盖背景色 - 无效，新元素出现时又失效
2. 提高CSS specificity - 无效，导致其他功能失效
3. 使用MutationObserver动态检测 - 部分有效，但无法覆盖所有情况

## 需要帮助
1. 如何让壁纸在所有页面都能正常显示？
2. 有没有更健壮的CSS注入方案？
3. 如何避免和OpenCode的样式冲突？

## 相关代码
```css
/* 当前CSS规则 */
html.opencode-dream-skin #root > div:first-child {
  background-color: rgba(249, 247, 241, 0.65) !important;
}
```

## 错误信息
通过CDP分析发现#root下有一个div兄弟元素，背景是rgba(255, 255, 255, 0.85)，覆盖在壁纸上面。

如果需要我给你提供什么其他的文件让你更好理解我的项目的话，请告诉我。

请直接给出解决方案，不要输出废话。
```

## 智谱回复示例

### 解决方案
为了彻底解决 OpenCode 首页壁纸被遮挡的问题，我们需要采取"全局标记 + 通用覆盖"的策略。

#### 1. 修改 JS (`renderer-inject.js`)
**目标**：当检测到首页时，给 `html` 根节点添加一个全局标记 `dream-active-home`，这样 CSS 就能无视 DOM 结构变化，强制清除所有遮挡层。

```javascript
var ACTIVE_HOME_CLASS = "dream-active-home";

// 在检测到首页时添加
root.classList.add(ACTIVE_HOME_CLASS);

// 在离开首页时移除
root.classList.remove(ACTIVE_HOME_CLASS);
```

#### 2. 修改 CSS (`dream-skin.css`)
**目标**：利用 JS 添加的 `dream-active-home` 标记，写一条"霸道"的规则，强制 `#root` 下的所有直接子元素背景透明。

```css
html.dream-active-home #root > div {
    background-color: transparent !important;
    backdrop-filter: none !important;
    box-shadow: none !important;
}
```

#### 方案原理
1. **JS层面**：给html根元素加了dream-active-home，开启"首页透底模式"的总开关
2. **CSS层面**：使用`html.dream-active-home #root > div`选择器，无视DOM结构变化
3. **结果**：无论OpenCode更新后把遮挡层放在哪里，这条CSS规则都会把它变成透明

### 实施效果
- 壁纸在所有页面正常显示
- 不需要为每个新元素添加CSS规则
- 方案具有健壮性，不会因OpenCode更新而失效