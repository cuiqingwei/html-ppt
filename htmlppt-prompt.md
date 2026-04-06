# 培训讲义生成模板 · html-ppt Prompt

> 使用方式：将本文件内容发给 Claude Code，再附上你的【内容大纲】，即可生成风格完全一致的 HTML 讲义。

---

## 使用指令

```
请根据以下【内容大纲】，生成一份 HTML 格式的培训讲义，严格遵循【设计规范】、【主题切换规范】和【技术规范】中的所有要求。
```

---

## 设计规范

### 双主题 Token 系统

HTML 根元素使用 `data-theme="dark"` 或 `data-theme="light"` 属性驱动全局主题。
所有颜色必须通过 CSS 变量引用，禁止硬编码颜色值（tmux 图示内部除外）。

**通用 Token（两个主题共享）**

| 变量 | 值 | 用途 |
|------|----|------|
| `--accent`  | `#00d4aa` | 主强调色（青绿） |
| `--accent2` | `#ff6b35` | 警告色（橙） |
| `--accent3` | `#7c5cfc` | 次强调色（紫） |
| `--green`   | `#4ade80` | 成功 |
| `--red`     | `#f87171` | 错误 |
| `--blue`    | `#60a5fa` | 信息 |
| `--pink`    | `#f472b6` | 语法高亮 flag |

**深色主题 `[data-theme="dark"]`**

| 变量 | 值 |
|------|----|
| `--bg`           | `#0d0f14` |
| `--bg2`          | `#13161d` |
| `--bg3`          | `#1a1e28` |
| `--panel`        | `#1e2330` |
| `--border`       | `#2a3045` |
| `--text`         | `#e2e8f4` |
| `--text-dim`     | `#8892a4` |
| `--text-bright`  | `#ffffff` |
| `--term-bg`      | `#0a0c10` |
| `--term-comment` | `#3d4f66` |
| `--term-out`     | `#4a6278` |
| `--grid-color`   | `rgba(0,212,170,.022)` |
| `--nav-bg`       | `rgba(13,15,20,.88)` |
| `--kbd-bg`       | `#1a1e28` |
| `--yellow-display` | `#f0c040` |

**浅色主题 `[data-theme="light"]`**

| 变量 | 值 |
|------|----|
| `--bg`           | `#f4f6fb` |
| `--bg2`          | `#eaecf4` |
| `--bg3`          | `#dde1ee` |
| `--panel`        | `#ffffff` |
| `--border`       | `#c8cfe0` |
| `--text`         | `#2a3050` |
| `--text-dim`     | `#6b7599` |
| `--text-bright`  | `#0d1020` |
| `--term-bg`      | `#1a1d28`（终端保持深色背景） |
| `--term-comment` | `#5a7a96` |
| `--term-out`     | `#7a98b0` |
| `--grid-color`   | `rgba(0,140,110,.04)` |
| `--nav-bg`       | `rgba(244,246,251,.92)` |
| `--kbd-bg`       | `#eaecf4` |
| `--yellow-display` | `#b07800`（深色以保证浅色背景下的对比度）|

> **注意**：终端块（`.terminal`）在浅色主题下仍使用深色背景 `var(--term-bg)`，保持代码可读性，这是刻意设计。

### 视觉风格

- 字体：`Fira Code`（英文/符号/代码）+ `LXGW WenKai` 霞鹜文楷（中文正文），均从国内 CDN 引入
- 全局背景网格：使用 `var(--grid-color)` 的水平+垂直线条，间距 44px，`position: fixed`
- 进度条：顶部固定，渐变 `var(--accent) → var(--accent3)`，带发光阴影

### 字体系统

**原则：所有中文使用 `LXGW WenKai`（霞鹜文楷），所有英文及 ASCII 符号使用 `Fira Code`，通过 `@font-face unicode-range` 精确切割，无需依赖 fallback 猜测。**

**字体CDN引入（`<head>` 内，使用国内 CDN）**

```html
<!-- LXGW WenKai 霞鹜文楷（原始版，非屏幕阅读版） -->
<link rel="stylesheet" href="https://cdn.staticfile.org/lxgw-wenkai-webfont/1.6.0/style.css">
<!-- Fira Code -->
<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/firacode/6.2.0/fira_code.min.css">
```

> **注意**：必须使用原始版 `lxgw-wenkai-webfont`，而非 `lxgw-wenkai-screen-webfont`（屏幕阅读版）。

**双字体虚拟字体族（CSS `@font-face`）**

> **简化方案**：仅声明 `SiteFontMono` 用于需要精确切割的场景（如终端块、代码块）。其他场景直接使用 CDN 字体 + 后备字体。

在 `<style>` 内声明 `SiteFontMono` 字体族，通过两条 `@font-face` 规则切割 unicode-range：

```css
/* SiteFontMono：代码/标签/终端/导航等等宽场景 */
/* Fira Code 负责 ASCII + 拉丁扩展 */
@font-face {
  font-family: 'SiteFontMono';
  src: local('Fira Code');
  font-weight: 100 900;
  unicode-range:
    U+0020-007E, U+00A0-02FF, U+0300-036F, U+0370-03FF, U+0400-04FF,
    U+2000-206F, U+2070-209F, U+20A0-20CF, U+2100-214F, U+2150-218F,
    U+2190-21FF, U+2200-22FF, U+2300-23FF, U+2400-243F,
    U+25A0-25FF, U+2600-26FF, U+FB00-FB06, U+FE70-FEFF;
}
/* LXGW WenKai 负责全部 CJK 字符 */
@font-face {
  font-family: 'SiteFontMono';
  src: local('LXGW WenKai');
  font-weight: 100 900;
  unicode-range:
    U+2E80-2EFF, U+2F00-2FDF, U+3000-303F, U+3040-309F, U+30A0-30FF,
    U+3100-312F, U+3130-318F, U+3190-31EF, U+31F0-31FF, U+3200-33FF,
    U+3400-4DBF, U+4E00-9FFF, U+A000-A4CF, U+A960-A97F, U+AC00-D7FF,
    U+F900-FAFF, U+FE10-FE6F, U+FF00-FFEF, U+20000-2A6DF, U+2A700-2CEAF;
}

/* 全局应用：仅对页面根元素和幻灯片容器应用字体 */
html, body, .slide-inner, .slide {
  font-family: 'LXGW WenKai', serif;
}
```

**各场景 font-family 使用规则**

| 场景 | font-family 值 |
|------|---------------|
| `body` 正文、标题、卡片 | `'LXGW WenKai', serif` |
| 终端块、代码、标签、导航、kbd | `'SiteFontMono', 'Fira Code', 'LXGW WenKai', monospace` |
| 内联 `code` 元素 | `'SiteFontMono', 'Fira Code', 'LXGW WenKai', monospace` |

> **注意**：所有 `font-family` 均保留 `'LXGW WenKai'` 和 `'Fira Code'` 作为显式后备，防止 `local()` 字体未安装时退化为系统字体。

### 幻灯片结构

- **封面页**（centered）：大标题 + 副标题 + 元信息（难度 / 时长 / 形式）
- **大纲页**：卡片宫格展示章节概览
- **章节过渡页**（centered）：超大空心章节编号 + 章节标题 + 简短描述
- **内容页**：左对齐，chapter-num 标签 + 大标题 + 描述 + 内容区
- **结束页**（centered）：emoji 图标 + 总结语 + 外链按钮组

### 内容组件样式

- **卡片**（`.card`）：圆角 8px，悬停时顶部出现 2px `var(--accent)` 线条，轻微上移
- **终端块**（`.terminal`）：`var(--term-bg)` 底色（始终深色），顶部 macOS 三色圆点 + 标签，代码多色高亮（prompt / cmd / comment / out / kw / str / flag / success / highlight）
- **tmux 图示**（`.tmux-diagram`）：`#050709` 深黑底，顶部状态栏模拟窗口切换，内部分割窗格
- **快捷键表**（`.key-table`）：表头下有 2px `var(--accent)` 线，行悬停微亮
- **kbd 标签**（`.kbd`）：`var(--kbd-bg)` 背景 + `var(--yellow-display)` 文字 + 底部加粗边框（模拟键帽）
- **标签徽章**（`.tag`）：绿/蓝/紫，半透明背景
- **工作流步骤**（`.workflow-step`）：圆形编号 + 竖线连接 + 标题 + 描述
- **提示框**（`.callout`）：左侧 3px 竖线 + 淡色背景；普通=青绿，warn=橙，tip=紫
- **会话结构图**（`.session-map`）：`var(--bg3)` 背景，等宽字体树状结构，三色分层（session=accent / window=blue / pane=yellow-display）
- **两列布局**（`.two-col`）：`grid-template-columns: 1fr 1fr`
- **内联代码**（`code`）：`var(--bg3)` 背景 + `var(--accent)` 文字 + 细边框

---

## 主题切换规范

### HTML 结构

```html
<html lang="zh-CN" data-theme="dark">
```

右上角控件区域（`id="top-controls"`）包含三个元素，从左到右：
1. 主题文字标签（`id="theme-label"`）：显示 `DARK` 或 `LIGHT`，10px 等宽字体
2. 切换轨道（`id="theme-track"`）：胶囊形拨动开关，点击触发 `toggleTheme()`
3. 目录按钮（`id="menu-btn"`）：`☰ 目录`

### 切换轨道样式

```css
/* 默认深色：轨道灰色，圆球居左 */
#theme-track {
  width: 40px; height: 22px; border-radius: 11px;
  background: var(--bg3); border: 1px solid var(--border);
  position: relative; cursor: pointer; transition: background .3s;
}
#theme-track::after {
  content: ''; position: absolute; top: 3px; left: 3px;
  width: 14px; height: 14px; border-radius: 50%;
  background: var(--text-dim); transition: transform .3s, background .3s;
}

/* 激活浅色：轨道变为 accent 色，圆球滑到右侧变白 */
[data-theme="light"] #theme-track        { background: var(--accent); }
[data-theme="light"] #theme-track::after { transform: translateX(18px); background: #fff; }
```

### JS 逻辑

```javascript
let theme = localStorage.getItem('slide-theme') || 'dark';

function applyTheme(t, animate) {
  if (!animate) document.documentElement.classList.add('no-transition');
  document.documentElement.setAttribute('data-theme', t);
  document.getElementById('theme-label').textContent = t === 'dark' ? 'DARK' : 'LIGHT';
  localStorage.setItem('slide-theme', t);
  if (!animate) {
    document.documentElement.getBoundingClientRect(); // 强制 reflow
    document.documentElement.classList.remove('no-transition');
  }
}

function toggleTheme() {
  theme = theme === 'dark' ? 'light' : 'dark';
  applyTheme(theme, true);
  showToast(theme === 'light' ? '☀️ 亮色模式' : '🌙 暗色模式');
}

// 页面加载时无动画恢复上次偏好
applyTheme(theme, false);
```

**防闪烁**：需定义 `.no-transition * { transition: none !important; }`，在无动画切换时短暂挂载。

**主题偏好持久化**：使用 `localStorage` key `'slide-theme'`。

---

## 技术规范

### 整页翻页引擎

- 所有 `.slide` 使用 `position: absolute; inset: 0` 绝对定位铺满视口
- 默认状态：`opacity: 0; pointer-events: none; transform: translateY(36px)`
- 激活状态（`.active`）：`opacity: 1; pointer-events: all; transform: translateY(0)`；过渡 `.42s cubic-bezier(.4,0,.2,1)`
- 退出动画（`.exit-up`）：`opacity: 0; transform: translateY(-36px)`；过渡 `.3s ease`
- 进入准备（`.enter-from-below`）：`opacity: 0; transform: translateY(56px); transition: none`
- `goTo(n)` 函数：busy 锁防抖，先加 exit-up，再强制 reflow，再激活新页，420ms 后解锁
- 每次翻页重置目标页 `.slide-inner` 的 `scrollTop = 0`
- centered 页：`.slide-inner` 用 flex column + center 对齐，`overflow: visible`
- 内容页：`.slide-inner` 限高 `calc(100vh - 136px)`，允许 `overflow-y: auto`，细滚动条

### 键盘快捷键（必须全部实现）

| 按键 | 行为 |
|------|------|
| `→` / `空格` | 下一页 |
| `←` | 上一页 |
| `↓` | 可滚动且未到底：自然滚动；已到底或不可滚动：下一页 |
| `↑` | 可滚动且未到顶：自然滚动；已到顶或不可滚动：上一页 |
| `PageDown` | 下一页 |
| `PageUp` | 上一页 |
| `Home` | 跳到第一页，显示 Toast |
| `End` | 跳到最后一页，显示 Toast |
| `G` / `g` | **自定义模态框** 输入页码跳转（禁止使用原生 prompt()，全屏模式下会退出全屏） |
| `M` / `m` | 切换目录侧边栏 |
| **`T` / `t`** | **切换亮色 / 暗色主题，显示 Toast** |
| `F` / `f` | 切换全屏 |
| `Esc` | 关闭目录侧边栏；若跳转模态框打开则关闭模态框 |

- 输入框聚焦时（INPUT/TEXTAREA）不拦截按键

**跳转模态框实现规范：**

```html
<!-- 跳转模态框 -->
<div id="goto-modal" class="modal-overlay" onclick="if(event.target===this)closeGotoModal()">
  <div class="modal-content">
    <div class="modal-title">跳转到页面</div>
    <input type="number" id="goto-input" class="modal-input" min="1" placeholder="1" 
           onkeydown="if(event.key==='Enter')submitGoto();if(event.key==='Escape')closeGotoModal();">
    <div class="modal-hint">共 <span id="goto-total"></span> 页，按 Enter 确认</div>
  </div>
</div>
```

```css
/* ── 跳转模态框 ── */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,.6);
  display: flex; align-items: center; justify-content: center;
  z-index: 500; opacity: 0; pointer-events: none;
  transition: opacity .2s;
}
.modal-overlay.show { opacity: 1; pointer-events: all; }
.modal-content {
  background: var(--panel); border: 1px solid var(--border);
  border-radius: 12px; padding: 24px 28px;
  text-align: center;
}
.modal-title {
  font-family: 'SiteFontMono', 'Fira Code', 'LXGW WenKai', monospace;
  font-size: 13px; color: var(--accent); letter-spacing: .1em; margin-bottom: 14px;
}
.modal-input {
  width: 80px; padding: 8px 12px; font-size: 18px;
  background: var(--bg3); border: 1px solid var(--border);
  border-radius: 6px; color: var(--text); text-align: center;
  outline: none; font-family: 'SiteFontMono', 'Fira Code', monospace;
}
.modal-input:focus { border-color: var(--accent); }
.modal-hint {
  font-family: 'SiteFontMono', 'Fira Code', 'LXGW WenKai', monospace;
  font-size: 11px; color: var(--text-dim); margin-top: 10px;
}
```

```javascript
function openGotoModal() {
  const modal = document.getElementById('goto-modal');
  document.getElementById('goto-total').textContent = total;
  modal.classList.add('show');
  const input = document.getElementById('goto-input');
  input.value = '';
  input.focus();
}
function closeGotoModal() {
  document.getElementById('goto-input').blur();
  document.getElementById('goto-modal').classList.remove('show');
}
function submitGoto() {
  const input = document.getElementById('goto-input');
  const p = parseInt(input.value, 10);
  if (p >= 1 && p <= total) {
    goTo(p - 1);
    showToast(`跳转到第 ${p} 页`);
  }
  closeGotoModal();
}
```

```javascript
// 键盘事件中需添加：
// 1. 模态框打开时跳过键盘处理
if (document.getElementById('goto-modal').classList.contains('show')) return;

// 2. G 键打开模态框
case 'g': case 'G': openGotoModal(); break;

// 3. Esc 键优先关闭模态框
case 'Escape': 
  if (document.getElementById('goto-modal').classList.contains('show')) {
    closeGotoModal();
  } else {
    toggleSidebar(false);
  }
  break;
```

> **重要**：禁止使用原生 `prompt()` 实现跳转功能，因为浏览器在全屏模式下显示原生对话框会自动退出全屏。必须使用自定义模态框。

### 触控支持

- `touchstart` 记录起点，`touchend` 计算 dx/dy
- `|dx| > |dy|` 且 `|dx| > 50px`：左滑→下一页，右滑→上一页
- 均为 `passive: true`

### UI 固定元素

- **进度条**：`position: fixed; top: 0; left: 0; height: 3px`，宽度 = `current/(total-1)*100%`
- **右上角控件组**（`id="top-controls"`）：`position: fixed; top: 18px; right: 22px`，flex 横排，间距 8px；包含主题标签、拨动开关、目录按钮
- **目录侧边栏**：`position: fixed; right: 0; width: 300px; height: 100%`，`transform: translateX(100%)` 默认隐藏，`.open` 时归位；点击 `top-controls` 之外区域关闭
- **快捷键提示**（右下角）：`position: fixed; bottom: 28px; right: 22px`，9px 等宽字体，灰色，不可交互；需包含 `T 亮暗切换` 一行
- **底部导航栏**：`position: fixed; bottom: 28px; left: 50%`，毛玻璃效果（`backdrop-filter: blur(14px)`），含首页/上页/页码/下页/末页
- **Toast**：`position: fixed; top: 22px; left: 50%`，`translateY(-70px)` 默认隐藏，`.show` 时归位，1800ms 后自动消失

### 其他要求

- 单 HTML 文件，所有 CSS/JS 内联，无外部依赖（除国内 CDN 字体）
- `html, body` 设 `overflow: hidden`，防止页面整体滚动
- 全局背景色/文字颜色需添加 `transition: background .35s ease, color .35s ease, border-color .35s ease` 以保证主题切换平滑
- 目录菜单由 JS 自动从 `data-title` 属性生成，点击跳转并关闭菜单
- 当前页在目录中高亮（`.active` 类）
- 首页/末页时对应导航按钮 `disabled`

---

## 内容大纲格式

在使用时，将下方替换为你的实际内容：

```
【内容大纲】

主题：<培训主题>
副标题：<一句话描述>
元信息：难度 <X>，时长 <X 小时>，形式 <X>

章节列表：
1. <章节名> — <简介>
2. <章节名> — <简介>
...

各章节详细内容：

# 章节 1：<标题>
- 要点 1
- 要点 2
- 代码示例：...
- 需要的组件：卡片 / 终端块 / 工作流 / 表格 / 提示框 ...

# 章节 2：<标题>
...
```

---

## 参考实现

已有的参考实现：`ppts/cc-tmux.html`

该文件共 25 张幻灯片，结构如下：
- 1 张封面
- 1 张大纲（卡片宫格）
- 8 组「章节过渡页 + 1~2 张内容页」
- 1 张结束页

可作为新讲义的参考对照。

---

*模板版本：v1.3 · 2025 · 新增自定义跳转模态框（支持全屏模式下跳转）+ 字体优化*
