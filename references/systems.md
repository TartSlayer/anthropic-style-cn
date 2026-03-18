# 系统性规范

前端整体搭建中最容易积累技术债的 10 个系统层问题。
**每次构建新项目时，agent 必须从头到尾对照本文件检查一遍。**

---

## 目录
1. [Z-index 分层管理体系](#z-index)
2. [响应式断点 & 移动端规范](#breakpoints)
3. [暗色模式切换完整方案](#dark-mode)
4. [CSS 动画性能规范](#animation-perf)
5. [焦点陷阱 Focus Trap](#focus-trap)
6. [SVG 图标系统](#svg-icons)
7. [字体加载策略（防闪）](#font-loading)
8. [滚动行为规范](#scroll)
9. [表单验证完整模式](#form-validation)
10. [图片优化规范](#image-optimize)

---

## 1. Z-index 分层管理体系 {#z-index}

**问题根源**：多人协作或 AI 多次生成时，各处随意写 `z-index: 999` / `9999` / `99999`，最终层级关系一片混乱，弹窗被导航遮住、Tooltip 被 Modal 压住。

### 标准层级表（写入 base.css）

```css
:root {
  /* ── 页面内层级（< 100）── */
  --z-below:      -1;   /* 背景装饰元素 */
  --z-base:        0;   /* 普通页面流 */
  --z-raised:     10;   /* 卡片悬浮态 */
  --z-sticky:     20;   /* sticky 表格列头 */
  --z-dropdown:   50;   /* Dropdown / Popover（锚定某元素） */

  /* ── 页面级浮层（100–500）── */
  --z-navbar:    100;   /* 导航栏 / Sidebar */
  --z-fab:       200;   /* Floating Action Button */
  --z-tooltip:   250;   /* Tooltip（无背景遮罩） */
  --z-drawer:    300;   /* Drawer 抽屉 */
  --z-modal:     400;   /* Modal 弹窗 */
  --z-toast:     450;   /* Toast 通知（覆盖 Modal） */

  /* ── 全局顶层（500+）── */
  --z-command:   500;   /* Command Palette */
  --z-context:   600;   /* Context Menu */
  --z-progress:  999;   /* 页顶进度条（永远最顶） */
}
```

### 使用规则

```css
/* ✅ 正确：使用 Token */
.navbar      { z-index: var(--z-navbar); }
.modal-overlay { z-index: var(--z-modal); }
.toast       { z-index: var(--z-toast); }

/* ❌ 禁止：硬编码任意数字 */
.my-thing    { z-index: 9999; }  /* 绝对禁止 */
```

### 层叠上下文陷阱

某些 CSS 属性会创建新的层叠上下文，导致子元素的 `z-index` 无法突破父级。**以下属性触发新上下文：**
- `transform: translate*()`（最常踩坑）
- `opacity < 1`
- `filter`
- `will-change: transform`
- `isolation: isolate`

```css
/* 解决方案：Modal 等需要突破父级的元素，使用 Portal 挂在 <body> 直接子级 */
/* 或者对父元素明确设置 isolation */
.card { isolation: isolate; } /* 建立独立层叠上下文，内部 z-index 不影响外部 */
```

---

## 2. 响应式断点 & 移动端规范 {#breakpoints}

### 断点系统（写入 base.css）

```css
:root {
  /* 以内容最优宽度为依据，非设备宽度 */
  --bp-xs:  480px;   /* 小屏手机（折叠屏展开前）*/
  --bp-sm:  640px;   /* 大屏手机 / 竖屏小平板 */
  --bp-md:  768px;   /* 平板 / 小笔记本 */
  --bp-lg:  1024px;  /* 标准笔记本 */
  --bp-xl:  1280px;  /* 宽屏笔记本 */
  --bp-2xl: 1440px;  /* 桌面显示器 */
}
```

```css
/* 使用方式：Mobile First（从小到大覆写） */
.section-title {
  font-size: var(--text-xl);   /* 默认：手机 */
}
@media (min-width: 768px) {
  .section-title { font-size: var(--text-2xl); }
}
@media (min-width: 1024px) {
  .section-title { font-size: var(--text-3xl); }
}
```

### 移动端必检清单

```css
/* 1. 禁止横向溢出 */
html, body { overflow-x: hidden; }

/* 2. 触摸目标最小 44×44px（WCAG 2.5.5） */
.btn, .nav-link, .chip__remove, .toggle {
  min-height: 44px;
  min-width: 44px;
}
/* 内容较小时用 padding 撑大点击区域 */
.icon-btn {
  padding: var(--space-3);  /* 视觉小但触摸区域大 */
}

/* 3. 禁止移动端双击缩放 */
html { touch-action: manipulation; }

/* 4. 去除 iOS 输入框默认样式 */
input, textarea, select {
  -webkit-appearance: none;
  border-radius: var(--radius-md); /* iOS 会忽略 0 以外的值，需明确设置 */
}

/* 5. 防止移动端长按选中文字（仅用于 UI 元素） */
.btn, .chip, .badge, .nav-link {
  -webkit-user-select: none;
  user-select: none;
}

/* 6. 平滑滚动惯性 */
.sidebar__main, .drawer__body, .cmd-palette__list {
  -webkit-overflow-scrolling: touch;
  overscroll-behavior: contain;
}
```

### 组件移动端行为规范

| 组件 | 桌面 | 移动端适配 |
|------|------|-----------|
| Navbar | 横向链接 | 变为 Hamburger → Drawer |
| Sidebar | 固定展开 | 默认收起，手势或按钮触发 |
| Modal | 居中弹窗 | 宽度 100vw，从底部滑入（sheet 样式）|
| Dropdown | 绝对定位 | 宽度至少 200px，防止溢出屏幕 |
| Table | 完整列 | 横向滚动或关键列固定 |

```css
/* Modal 移动端 Sheet 变体 */
@media (max-width: 640px) {
  .modal-overlay {
    align-items: flex-end;
    padding: 0;
  }
  .modal {
    max-width: 100%;
    border-radius: var(--radius-xl) var(--radius-xl) 0 0;
    animation: sheetUp 0.3s var(--ease-default);
  }
  @keyframes sheetUp {
    from { transform: translateY(100%); }
    to   { transform: translateY(0); }
  }
}
```

---

## 3. 暗色模式切换完整方案 {#dark-mode}

### 三层实现策略

**第 1 层：CSS 变量覆盖（已在 base.css 定义）**

```css
/* 自动跟随系统 */
@media (prefers-color-scheme: dark) {
  :root { /* ... 覆盖色彩 Token ... */ }
}
```

**第 2 层：手动切换（用 data-theme 属性）**

```css
/* 支持手动覆盖系统偏好 */
[data-theme="dark"] {
  --color-bg-base:        #1A1916;
  --color-bg-raised:      #222119;
  --color-bg-overlay:     #2C2B26;
  --color-bg-inverted:    #F5F3EC;
  --color-text-primary:   #EAE7DC;
  --color-text-secondary: #9D9A91;
  --color-text-muted:     #5C5A54;
  --color-text-inverted:  #141413;
  --color-border-default: #3A3830;
  --color-border-subtle:  #2E2D27;
  /* 强调色保持不变 */
}
[data-theme="light"] {
  /* 强制亮色，覆盖系统 dark 偏好 */
  --color-bg-base:        #ECE9E0;
  --color-text-primary:   #141413;
  /* ...完整亮色 Token... */
}
```

**第 3 层：JS 切换逻辑**

```js
const THEME_KEY = 'anthropic-theme';

function getTheme() {
  const stored = localStorage.getItem(THEME_KEY);
  if (stored) return stored;
  return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
}

function applyTheme(theme) {
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem(THEME_KEY, theme);
  // 更新按钮状态
  document.querySelectorAll('[data-theme-toggle]').forEach(btn => {
    btn.setAttribute('aria-pressed', theme === 'dark');
    btn.setAttribute('aria-label', theme === 'dark' ? '切换到亮色模式' : '切换到暗色模式');
  });
}

function toggleTheme() {
  applyTheme(getTheme() === 'dark' ? 'light' : 'dark');
}

// 初始化：在 <head> 内尽早执行，防止闪白（FOUC）
applyTheme(getTheme());

// 监听系统偏好变化
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
  if (!localStorage.getItem(THEME_KEY)) applyTheme(e.matches ? 'dark' : 'light');
});
```

### 防闪白（FOUC）

```html
<!-- 在 <head> 最前面，CSS 之前注入此脚本 -->
<script>
  (function() {
    const t = localStorage.getItem('anthropic-theme')
      || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
    document.documentElement.setAttribute('data-theme', t);
  })();
</script>
```

### 暗色模式下的特殊处理

```css
/* 图片在暗色模式下降低亮度，避免刺眼 */
[data-theme="dark"] img:not([data-no-dim]) {
  filter: brightness(0.9) saturate(0.95);
}

/* 深色模式下的阴影用半透明叠加而非扩散 */
[data-theme="dark"] .card:hover {
  box-shadow: 0 0 0 1px rgba(250,249,245,0.08), 0 8px 32px rgba(0,0,0,0.4);
}

/* 过渡动画（切换时平滑） */
*, *::before, *::after {
  transition: background-color 0.2s ease, border-color 0.2s ease, color 0.15s ease;
}
/* 但动画元素不要被影响到 */
.carousel__track, .progress__fill, .toggle__thumb {
  transition: none; /* 覆盖上面的全局 transition，自己管理 */
}
```

---

## 4. CSS 动画性能规范 {#animation-perf}

### 只动这四个属性（GPU 加速）

```css
/* ✅ 高性能：只触发合成层，不引发重排/重绘 */
transform: translate(), rotate(), scale()
opacity: 0 → 1
filter: blur()
clip-path  /* 部分浏览器 */

/* ❌ 低性能：会引发重排（Reflow），避免 animate */
width, height, top, left, right, bottom
margin, padding
font-size
```

### will-change 使用规范

```css
/* ✅ 正确：只在即将动画前加，动画后移除 */
.card { transition: transform 0.25s ease; }
.card:hover { will-change: transform; } /* hover 时提示浏览器提升层级 */

/* ✅ 对确定会动画的元素提前声明 */
.progress__fill    { will-change: transform; }
.modal             { will-change: transform, opacity; }
.sidebar           { will-change: transform; } /* 移动端有时需要 */

/* ❌ 禁止滥用 will-change */
* { will-change: transform; }          /* 严禁：内存爆炸 */
.static-card { will-change: transform; } /* 没有动画的元素不需要 */
```

### 强制 GPU 合成层的标准写法

```css
/* 需要硬件加速的动画元素 */
.animated-element {
  transform: translateZ(0);  /* 或 translate3d(0,0,0) */
  backface-visibility: hidden;
  perspective: 1000px;
}
```

### 动画帧率控制

```js
// 复杂动画用 requestAnimationFrame，不用 setInterval
function animate(timestamp) {
  // 更新状态
  element.style.transform = `translateX(${x}px)`;
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);

// 低帧率限速（省电模式 / 低性能设备）
let lastTime = 0;
function throttledAnimate(timestamp) {
  if (timestamp - lastTime < 16.67) { // 限 60fps
    requestAnimationFrame(throttledAnimate);
    return;
  }
  lastTime = timestamp;
  // 更新...
  requestAnimationFrame(throttledAnimate);
}
```

---

## 5. 焦点陷阱 Focus Trap {#focus-trap}

**问题**：Modal、Drawer、Command Palette 打开时，Tab 键可以跑到背景内容，违反无障碍规范（WCAG 2.1 A）。

### 原生实现（无依赖）

```js
function createFocusTrap(container) {
  // 获取所有可聚焦元素
  const FOCUSABLE = [
    'a[href]', 'button:not(:disabled)', 'input:not(:disabled)',
    'select:not(:disabled)', 'textarea:not(:disabled)',
    '[tabindex]:not([tabindex="-1"])'
  ].join(', ');

  function getFocusable() {
    return [...container.querySelectorAll(FOCUSABLE)]
      .filter(el => !el.closest('[hidden]') && getComputedStyle(el).display !== 'none');
  }

  function handleKeydown(e) {
    if (e.key !== 'Tab') return;
    const focusable = getFocusable();
    if (!focusable.length) { e.preventDefault(); return; }

    const first = focusable[0];
    const last  = focusable[focusable.length - 1];

    if (e.shiftKey) {
      if (document.activeElement === first) { e.preventDefault(); last.focus(); }
    } else {
      if (document.activeElement === last)  { e.preventDefault(); first.focus(); }
    }
  }

  return {
    activate() {
      this._previousFocus = document.activeElement;
      container.addEventListener('keydown', handleKeydown);
      // 聚焦容器内第一个可交互元素
      getFocusable()[0]?.focus();
    },
    deactivate() {
      container.removeEventListener('keydown', handleKeydown);
      this._previousFocus?.focus(); // 还原焦点
    }
  };
}

// 使用示例
const modalTrap = createFocusTrap(document.querySelector('.modal'));

function openModal(id) {
  const overlay = document.getElementById(id);
  overlay.removeAttribute('hidden');
  document.body.style.overflow = 'hidden';
  modalTrap.activate();
}
function closeModal(id) {
  document.getElementById(id).setAttribute('hidden', '');
  document.body.style.overflow = '';
  modalTrap.deactivate();
}
```

### 需要 Focus Trap 的组件清单

| 组件 | 必须 | 原因 |
|------|------|------|
| Modal 弹窗 | ✅ 必须 | WCAG 2.1 Level A 要求 |
| Drawer 抽屉 | ✅ 必须 | 同上 |
| Command Palette | ✅ 必须 | 全屏覆盖 |
| Dropdown | ⚠️ 建议 | 防止 Tab 跑到菜单外 |
| Tooltip | ❌ 不需要 | 无交互元素 |

---

## 6. SVG 图标系统 {#svg-icons}

### 方案选择

| 方案 | 适用场景 | 优点 | 缺点 |
|------|---------|------|------|
| Inline SVG | 需要 CSS 控制颜色/动效 | 可用 currentColor | HTML 体积大 |
| SVG Sprite | 图标量大、重复使用多 | 缓存好，HTML 干净 | 需构建工具 |
| `<img src=".svg">` | 装饰性大图 | 简单 | 无法 CSS 控制色 |
| CSS Mask | 单色图标 | 可换色 | 稍复杂 |

### Inline SVG 规范（推荐首选）

```html
<!-- ✅ 标准写法 -->
<svg
  class="icon"
  width="16"
  height="16"
  viewBox="0 0 16 16"
  fill="none"
  aria-hidden="true"   <!-- 装饰性图标：隐藏于读屏 -->
  focusable="false"    <!-- IE/Edge 防止 Tab 聚焦 -->
>
  <path d="..." stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
</svg>

<!-- 独立有语义的图标（无文字说明时）：加 role + title -->
<svg role="img" aria-labelledby="icon-title-1" ...>
  <title id="icon-title-1">删除</title>
  <path .../>
</svg>
```

```css
/* 图标基础样式 */
.icon {
  display: inline-block;
  vertical-align: middle;
  flex-shrink: 0;
  /* 颜色继承自父元素，通过 currentColor */
  color: inherit;
}
/* 尺寸语义化 */
.icon--xs  { width: 12px; height: 12px; }
.icon--sm  { width: 14px; height: 14px; }
.icon--md  { width: 16px; height: 16px; }  /* 默认 */
.icon--lg  { width: 20px; height: 20px; }
.icon--xl  { width: 24px; height: 24px; }
.icon--2xl { width: 32px; height: 32px; }
```

### SVG Sprite 方案（图标量 > 20 个时切换）

```html
<!-- 在 <body> 开头注入 Sprite（隐藏） -->
<svg xmlns="http://www.w3.org/2000/svg" style="display:none">
  <symbol id="icon-search" viewBox="0 0 16 16">
    <circle cx="7" cy="7" r="5" stroke="currentColor" stroke-width="1.5"/>
    <path d="M11 11l3 3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </symbol>
  <symbol id="icon-close" viewBox="0 0 16 16">
    <path d="M3 3l10 10M13 3L3 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </symbol>
  <!-- ... 更多 ... -->
</svg>

<!-- 使用 -->
<svg class="icon icon--md" aria-hidden="true" focusable="false">
  <use href="#icon-search"/>
</svg>
```

### 图标绘制规范（让 AI 生成 SVG 时遵守）

```
- 画布：统一 16×16 或 24×24
- 线条：stroke-width="1.5"（16px 画布）/ "1.5–2"（24px 画布）
- 端点：stroke-linecap="round"
- 转角：stroke-linejoin="round"
- 颜色：stroke="currentColor" 或 fill="currentColor"，禁止硬编码颜色值
- 填充：fill="none"（线性图标）
- 对齐：路径在画布内留 1–2px 内边距，避免被截断
```

---

## 7. 字体加载策略（防闪） {#font-loading}

### 两种闪烁问题

- **FOUT**（Flash of Unstyled Text）：先显示备用字体，加载完后切换，产生"跳字"感
- **FOIT**（Flash of Invisible Text）：字体未加载时文字不可见，造成布局空白

### 本 skill 的解决方案（字体已内置）

```html
<!-- 1. 字体文件已在 assets/fonts/ 中，无需联网 -->
<!-- 2. fonts.css 中已声明 font-display: swap -->
<!-- 3. 在 <head> 中按顺序引入 -->
<head>
  <!-- Step 1：尽早声明字体，让浏览器预加载 -->
  <link rel="preload" href="assets/fonts/dm-serif-display-latin-400-normal.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="assets/fonts/dm-sans-latin-400-normal.woff2" as="font" type="font/woff2" crossorigin>

  <!-- Step 2：字体 @font-face 声明 -->
  <link rel="stylesheet" href="assets/fonts/fonts.css">

  <!-- Step 3：样式（引用字体变量） -->
  <link rel="stylesheet" href="assets/base.css">
</head>
```

### font-display 策略对比

```css
/* 本 skill 使用 swap（已写入 fonts.css） */
@font-face {
  font-display: swap;
  /* swap = 立即用备用字体，加载完换回
     优点：文字始终可读
     缺点：有轻微布局跳动（用 size-adjust 缓解） */
}

/* 补充：用 size-adjust 减少 FOUT 跳动感 */
@font-face {
  font-family: 'DM Serif Display Fallback';
  src: local('Georgia');
  size-adjust: 105%;           /* 调整备用字体大小接近目标字体 */
  ascent-override: 90%;
  descent-override: 10%;
}
/* 然后在 --font-display 的 fallback 列表中引用 */
```

### 字体子集化（减小文件体积）

```
本 skill 中的 woff2 文件已针对 Latin 字符子集优化（unicode-range 已在 fonts.css 声明）。
如需支持中文字符，需额外引入中文字体或使用系统字体：
```

```css
:root {
  /* 中文界面：系统字体兜底 */
  --font-heading-cn: 'DM Sans', 'PingFang SC', 'Hiragino Sans GB',
                     'Microsoft YaHei', sans-serif;
  --font-body-cn:    'DM Serif Text', 'Songti SC', 'SimSun', serif;
}
```

---

## 8. 滚动行为规范 {#scroll}

### 三类滚动场景及实现

**场景 A：页面级平滑滚动**

```css
html { scroll-behavior: smooth; }

/* 锚点定位时留出 navbar 高度 */
:root { --navbar-height: 64px; }
[id] { scroll-margin-top: calc(var(--navbar-height) + var(--space-4)); }
```

**场景 B：弹窗/抽屉打开时锁定背景滚动**

```js
// 简单方案（有轻微跳动，因为 scrollbar 消失）
document.body.style.overflow = 'hidden';

// 无跳动方案（保留 scrollbar 宽度）
function lockScroll() {
  const scrollbarW = window.innerWidth - document.documentElement.clientWidth;
  document.body.style.paddingRight = scrollbarW + 'px';
  document.body.style.overflow = 'hidden';
}
function unlockScroll() {
  document.body.style.paddingRight = '';
  document.body.style.overflow = '';
}
```

**场景 C：容器内独立滚动（Drawer、Modal 内容区）**

```css
.drawer__body,
.modal__body,
.cmd-palette__list {
  overflow-y: auto;
  overscroll-behavior: contain;  /* 防止滚动穿透到背景 */
  -webkit-overflow-scrolling: touch; /* iOS 惯性滚动 */
  /* 自定义滚动条（与品牌一致） */
  scrollbar-width: thin;
  scrollbar-color: var(--color-border-default) transparent;
}
.drawer__body::-webkit-scrollbar { width: 6px; }
.drawer__body::-webkit-scrollbar-track { background: transparent; }
.drawer__body::-webkit-scrollbar-thumb {
  background: var(--color-border-default);
  border-radius: var(--radius-full);
}
.drawer__body::-webkit-scrollbar-thumb:hover {
  background: var(--color-border-strong);
}
```

**场景 D：虚拟滚动（列表超过 500 条时必用）**

```js
// 轻量级虚拟滚动实现（不依赖库）
class VirtualList {
  constructor(container, items, itemHeight, renderItem) {
    this.container = container;
    this.items = items;
    this.itemHeight = itemHeight;
    this.renderItem = renderItem;
    this.container.style.position = 'relative';
    this.container.style.overflow = 'auto';
    this.render();
    this.container.addEventListener('scroll', () => this.render());
  }

  render() {
    const { scrollTop, clientHeight } = this.container;
    const totalHeight = this.items.length * this.itemHeight;
    const startIdx = Math.floor(scrollTop / this.itemHeight);
    const endIdx   = Math.min(
      this.items.length,
      Math.ceil((scrollTop + clientHeight) / this.itemHeight) + 3 // buffer
    );

    this.container.innerHTML = `
      <div style="height:${totalHeight}px;position:relative">
        <div style="position:absolute;top:${startIdx * this.itemHeight}px;width:100%">
          ${this.items.slice(startIdx, endIdx).map(this.renderItem).join('')}
        </div>
      </div>`;
  }
}
// 使用：new VirtualList(container, dataArray, 48, item => `<div class="list-item">${item.name}</div>`)
```

---

## 9. 表单验证完整模式 {#form-validation}

### 双阶段验证策略

- **实时验证**：`blur`（失焦）时触发，`input` 时清除错误——不在用户打字时打断
- **提交验证**：`submit` 时对所有字段全量检查，聚焦第一个错误字段

```js
// 验证规则定义
const VALIDATORS = {
  required: (v) => v.trim() ? null : '此字段为必填项',
  email:    (v) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v) ? null : '请输入有效的邮箱地址',
  minLen:   (n) => (v) => v.length >= n ? null : `至少需要 ${n} 个字符`,
  maxLen:   (n) => (v) => v.length <= n ? null : `不能超过 ${n} 个字符`,
  pattern:  (re, msg) => (v) => re.test(v) ? null : msg,
};

// 通用字段验证函数
function validateField(input, rules) {
  const value = input.value;
  for (const rule of rules) {
    const error = rule(value);
    if (error) return error;
  }
  return null;
}

// 显示/清除错误
function showError(input, message) {
  const field = input.closest('.form__field');
  input.classList.add('input--error');
  let errorEl = field.querySelector('.form__field-error');
  if (!errorEl) {
    errorEl = document.createElement('p');
    errorEl.className = 'form__field-error';
    errorEl.setAttribute('role', 'alert');
    field.appendChild(errorEl);
  }
  errorEl.textContent = message;
  input.setAttribute('aria-invalid', 'true');
  input.setAttribute('aria-describedby', errorEl.id || (errorEl.id = 'err-' + input.id));
}

function clearError(input) {
  const field = input.closest('.form__field');
  input.classList.remove('input--error');
  field.querySelector('.form__field-error')?.remove();
  input.removeAttribute('aria-invalid');
  input.removeAttribute('aria-describedby');
}

// 绑定表单
function initForm(form, fieldRules) {
  // 实时验证：blur 触发，input 清除
  Object.entries(fieldRules).forEach(([name, rules]) => {
    const input = form.elements[name];
    if (!input) return;
    input.addEventListener('blur',  () => {
      const err = validateField(input, rules);
      err ? showError(input, err) : clearError(input);
    });
    input.addEventListener('input', () => clearError(input));
  });

  // 提交验证
  form.addEventListener('submit', (e) => {
    e.preventDefault();
    let firstError = null;
    let valid = true;

    Object.entries(fieldRules).forEach(([name, rules]) => {
      const input = form.elements[name];
      if (!input) return;
      const err = validateField(input, rules);
      if (err) {
        showError(input, err);
        if (!firstError) firstError = input;
        valid = false;
      }
    });

    if (!valid) {
      firstError.focus(); // 聚焦第一个错误，满足无障碍要求
      form.querySelector('.form__error-banner')?.removeAttribute('hidden');
      return;
    }

    // 验证通过，提交
    submitForm(new FormData(form));
  });
}

// 使用示例
initForm(document.querySelector('.form'), {
  keyName:    [VALIDATORS.required, VALIDATORS.minLen(3), VALIDATORS.maxLen(50)],
  email:      [VALIDATORS.required, VALIDATORS.email],
  permission: [VALIDATORS.required],
});
```

---

## 10. 图片优化规范 {#image-optimize}

### 格式选择

| 场景 | 推荐格式 | 备注 |
|------|---------|------|
| 照片/复杂渐变 | WebP（首选）/ JPEG | AVIF 更优但兼容性稍差 |
| 图标/Logo/UI | SVG | 矢量，任意缩放 |
| 截图/有文字的图 | PNG / WebP | 保持锐利边缘 |
| 动图 | WebP 动图 / 视频 `<video>` | 避免 GIF（体积大） |

### 响应式图片（srcset）

```html
<!-- 标准响应式图片 -->
<img
  src="hero-800.jpg"
  srcset="hero-480.jpg 480w, hero-800.jpg 800w, hero-1200.jpg 1200w, hero-1600.jpg 1600w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 80vw, 1200px"
  alt="Anthropic 研究团队合影"
  width="1200"
  height="675"
  loading="lazy"
  decoding="async"
>

<!-- 格式降级（优先 WebP，回退 JPEG） -->
<picture>
  <source srcset="hero.webp 1x, hero@2x.webp 2x" type="image/webp">
  <source srcset="hero.jpg 1x, hero@2x.jpg 2x"   type="image/jpeg">
  <img src="hero.jpg" alt="…" width="1200" height="675" loading="lazy">
</picture>
```

### 懒加载规范

```html
<!-- 原生懒加载（现代浏览器，覆盖率 > 95%） -->
<img loading="lazy" decoding="async" src="..." alt="...">

<!-- LCP（最大内容绘制）关键图片：不要 lazy！ -->
<img loading="eager" fetchpriority="high" src="hero.jpg" alt="...">
```

```js
// IntersectionObserver 懒加载（需要更精细控制时）
const lazyImages = document.querySelectorAll('img[data-src]');
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      if (img.dataset.srcset) img.srcset = img.dataset.srcset;
      img.removeAttribute('data-src');
      imageObserver.unobserve(img);
    }
  });
}, { rootMargin: '200px' }); // 提前 200px 开始加载
lazyImages.forEach(img => imageObserver.observe(img));
```

### 图片 CSS 规范

```css
/* 防止图片撑破容器 */
img { max-width: 100%; height: auto; display: block; }

/* 保持宽高比（现代方案，无需 padding-hack） */
.img-wrap {
  aspect-ratio: 16 / 9;
  overflow: hidden;
  border-radius: var(--radius-lg);
}
.img-wrap img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  transition: transform 0.4s var(--ease-default);
}
.img-wrap:hover img { transform: scale(1.04); } /* 轻微放大悬停 */

/* 加载占位（Skeleton 样式） */
.img-placeholder {
  background: var(--color-border-subtle);
  animation: shimmer 1.5s infinite linear;
  background-image: linear-gradient(
    90deg,
    var(--color-border-subtle) 25%,
    var(--color-bg-raised) 50%,
    var(--color-border-subtle) 75%
  );
  background-size: 600px 100%;
}
```

### alt 文本规范

```html
<!-- ✅ 有意义的描述 -->
<img src="claude-interface.jpg" alt="Claude 对话界面截图，展示多轮对话功能">

<!-- ✅ 装饰性图片：空 alt -->
<img src="background-texture.png" alt="">

<!-- ❌ 无意义 alt -->
<img src="photo.jpg" alt="图片">
<img src="photo.jpg" alt="photo.jpg">
```

---

## 11. 上下文感知 Token 用法 {#contextual-tokens}

**核心问题**：同一个 Token（如 `--color-border-subtle`）在 landing page 是「柔和分割」，在监控系统是「弱到看不见」。Token 是静态的，但视觉权重是场景相关的。

### 11.1 Token 密度调整表

根据 SKILL.md 第 0.6 节的四种模式，对 Token 做有限调整：

| Token 类别 | 默认模式 | 数据密集模式 | 工具优先模式 |
|-----------|---------|------------|------------|
| 边框 | `--color-border-subtle` | `--color-border-default` | `--color-border-default` |
| 间距（内容区） | `--space-8`（32px） | `--space-5`（20px） | `--space-4`（16px） |
| 间距（区块间） | `--space-16`（64px） | `--space-10`（40px） | `--space-8`（32px） |
| 字号（辅助文字） | `--text-sm`（14px） | `--text-xs`（12px） | `--text-xs`（12px） |
| 卡片阴影 | 仅 hover 显示 | 常态显示（区分层级） | 无阴影（用边框代替） |
| 背景层次 | 2 层（base + raised） | 3 层（base + raised + overlay） | 2 层（减少视觉噪音） |

### 11.2 对比度按场景调整

```css
/* 默认模式：柔和，大量留白 */
.content-default {
  color: var(--color-text-secondary);  /* 中灰棕，舒适阅读 */
  border-color: var(--color-border-subtle);
}

/* 数据密集模式：提高对比度，便于快速扫描 */
.content-data-dense {
  color: var(--color-text-primary);    /* 近黑，快速识别 */
  border-color: var(--color-border-default);
}

/* 工具优先模式：最高对比度，操作效率优先 */
.content-utility {
  color: var(--color-text-primary);
  border-color: var(--color-border-strong);
  /* 移除过渡动画：响应速度比动效更重要 */
  transition: none;
}
```

### 11.3 复杂流程的布局连续性

多步骤流程（表单向导、配置流程、权限管理）的特有规则：

```
跨步骤保持一致：
  每一步的主要内容区位置、宽度、字体大小必须完全相同
  只有内容变化，容器不变 → 降低认知切换成本

步骤指示器始终可见：
  Step Indicator 固定在顶部或侧边，不随步骤内容滚动
  当前步骤高亮，已完成步骤可点击返回

不可回退节点的视觉标记：
  在即将进入「提交/部署/发布」等不可逆步骤前，
  用 Banner--warning 提示 + 步骤条特殊标记（橙色边框）

步骤间过渡动画：
  向前（下一步）→ 内容从右侧淡入（translateX(20px) → 0）
  向后（上一步）→ 内容从左侧淡入（translateX(-20px) → 0）
  duration: var(--duration-normal) = 250ms
```

```css
/* 多步骤流程容器：内容切换动画 */
.wizard-step {
  animation: stepEnter var(--duration-normal) var(--ease-default);
}
.wizard-step--back {
  animation: stepEnterBack var(--duration-normal) var(--ease-default);
}
@keyframes stepEnter {
  from { opacity: 0; transform: translateX(20px); }
  to   { opacity: 1; transform: translateX(0); }
}
@keyframes stepEnterBack {
  from { opacity: 0; transform: translateX(-20px); }
  to   { opacity: 1; transform: translateX(0); }
}

/* 不可回退节点：步骤条特殊标记 */
.steps__step--irreversible .steps__node {
  border-color: var(--color-warning);
}
.steps__step--irreversible .steps__label::after {
  content: ' ⚠';
  color: var(--color-warning);
  font-size: 0.8em;
}
```

### 11.4 紧急/报警场景的设计规则

用户模式分三类，不同场景需要不同的信息优先策略：

```
浏览模式（默认）：用户在探索内容
  → 完整执行本 skill，留白、克制、叙事感

操作模式：用户在完成特定任务
  → 突出操作路径，减少干扰信息，CTA 明显

紧急处理模式：用户在处理故障或异常
  → 0.5 秒内必须能读懂关键信息
  → 放弃所有美学追求，只保留：
     ① 问题是什么（大号标题，高对比）
     ② 严重程度（颜色语义：红/橙/黄）
     ③ 下一步操作（一个明确的行动按钮）
```

```css
/* 报警/紧急状态全屏覆盖 */
.alert-critical {
  position: fixed;
  inset: 0;
  z-index: var(--z-modal);
  background: rgba(20, 20, 19, 0.92);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-8);
}
.alert-critical__panel {
  background: var(--color-bg-overlay);
  border: 2px solid var(--color-error);
  border-radius: var(--radius-xl);
  padding: var(--space-10);
  max-width: 480px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: var(--space-5);
}
.alert-critical__severity {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-bold);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--color-error);
}
.alert-critical__title {
  font-family: var(--font-display);
  font-size: var(--text-2xl);
  font-weight: 400;
  color: var(--color-text-primary);
  line-height: var(--leading-tight);
}
.alert-critical__body {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
}
.alert-critical__actions {
  display: flex;
  gap: var(--space-3);
  padding-top: var(--space-3);
  border-top: 1px solid var(--color-border-subtle);
}
```
