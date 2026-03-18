# Overlay 层组件

## 目录
1. [Avatar 头像](#avatar)
2. [Progress Bar/Ring 进度条](#progress)
3. [Search Bar 搜索框](#search)
4. [Command Palette ⌘K 面板](#command-palette)
5. [Drawer 侧滑抽屉](#drawer)
6. [Chip/Tag 可删除标签](#chip)
7. [Popover 浮层卡片](#popover)
8. [Carousel 轮播](#carousel)
9. [Context Menu 右键菜单](#context-menu)
10. [FAB 悬浮按钮](#fab)

---

## 26. Avatar 头像 & 头像组 {#avatar}

```html
<!-- 单个头像：图片 -->
<div class="avatar avatar--md" aria-label="Dario Amodei">
  <img src="avatar.jpg" alt="Dario Amodei" class="avatar__img">
</div>

<!-- 单个头像：文字缩写（无图时降级） -->
<div class="avatar avatar--md avatar--orange" aria-label="Dario Amodei" aria-hidden="false">
  <span class="avatar__initials" aria-hidden="true">DA</span>
</div>

<!-- 头像组：堆叠 -->
<div class="avatar-group" aria-label="5 位成员">
  <div class="avatar avatar--sm avatar--orange" title="Dario A."><span class="avatar__initials">DA</span></div>
  <div class="avatar avatar--sm avatar--blue"   title="Tom B."><span class="avatar__initials">TB</span></div>
  <div class="avatar avatar--sm avatar--green"  title="Sam R."><span class="avatar__initials">SR</span></div>
  <div class="avatar avatar--sm avatar--sand"   title="Lisa K."><span class="avatar__initials">LK</span></div>
  <div class="avatar avatar--sm avatar--overflow" aria-label="另外 3 人">+3</div>
</div>
```

```css
/* ── 尺寸 ── */
.avatar {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-full);
  overflow: hidden;
  flex-shrink: 0;
  border: 2px solid var(--color-bg-base);
  font-family: var(--font-heading);
  font-weight: var(--weight-medium);
  user-select: none;
}
.avatar--xs  { width: 24px; height: 24px; font-size: 10px; }
.avatar--sm  { width: 32px; height: 32px; font-size: 11px; }
.avatar--md  { width: 40px; height: 40px; font-size: var(--text-sm); }
.avatar--lg  { width: 56px; height: 56px; font-size: var(--text-base); }
.avatar--xl  { width: 80px; height: 80px; font-size: var(--text-xl); }

.avatar__img { width: 100%; height: 100%; object-fit: cover; }

/* ── 颜色变体 ── */
.avatar--orange { background: rgba(217,119,87,0.15); color: var(--color-accent-orange); }
.avatar--blue   { background: rgba(106,155,204,0.15); color: var(--color-accent-blue); }
.avatar--green  { background: rgba(120,140,93,0.15);  color: var(--color-accent-green); }
.avatar--sand   { background: rgba(196,185,154,0.25); color: #7A6E58; }

/* ── 头像组 ── */
.avatar-group {
  display: flex;
  flex-direction: row-reverse; /* 右边的先渲染，叠在下方 */
}
.avatar-group .avatar {
  margin-left: -8px;
  transition: transform 0.2s var(--ease-default), z-index 0s;
  position: relative;
  z-index: 1;
}
.avatar-group .avatar:hover {
  transform: translateY(-3px);
  z-index: 10;
}
.avatar-group .avatar:last-child { margin-left: 0; }

.avatar--overflow {
  background: var(--color-bg-overlay);
  border-color: var(--color-border-default);
  color: var(--color-text-secondary);
  font-size: 11px;
  font-weight: var(--weight-medium);
  letter-spacing: -0.02em;
}
```

---

## 27. Progress Bar / Ring 进度条 {#progress}

```html
<!-- 线形进度条 -->
<div class="progress" role="progressbar" aria-valuenow="68" aria-valuemin="0" aria-valuemax="100" aria-label="上传进度">
  <div class="progress__header">
    <span class="progress__label">上传中…</span>
    <span class="progress__value">68%</span>
  </div>
  <div class="progress__track">
    <div class="progress__fill" style="width: 68%"></div>
  </div>
</div>

<!-- 细条（用于页面顶部加载指示） -->
<div class="progress-bar-top" aria-hidden="true">
  <div class="progress-bar-top__fill" style="width: 42%"></div>
</div>

<!-- 环形进度（SVG） -->
<svg class="progress-ring" width="80" height="80" viewBox="0 0 80 80"
     role="progressbar" aria-valuenow="72" aria-valuemin="0" aria-valuemax="100" aria-label="存储用量 72%">
  <!-- 轨道 -->
  <circle class="progress-ring__track" cx="40" cy="40" r="32"
          fill="none" stroke-width="6"/>
  <!-- 进度 -->
  <circle class="progress-ring__fill" cx="40" cy="40" r="32"
          fill="none" stroke-width="6"
          stroke-dasharray="201.06"
          stroke-dashoffset="56.3"   <!-- (1 - 0.72) × 201.06 -->
          transform="rotate(-90 40 40)"/>
  <!-- 中心文字 -->
  <text x="40" y="44" text-anchor="middle" class="progress-ring__text">72%</text>
</svg>
```

```css
/* ── 线形 ── */
.progress { display: flex; flex-direction: column; gap: var(--space-2); }
.progress__header {
  display: flex;
  justify-content: space-between;
  font-family: var(--font-heading);
  font-size: var(--text-xs);
}
.progress__label { color: var(--color-text-secondary); }
.progress__value { color: var(--color-text-primary); font-weight: var(--weight-medium); font-variant-numeric: tabular-nums; }

.progress__track {
  height: 6px;
  background: var(--color-border-subtle);
  border-radius: var(--radius-full);
  overflow: hidden;
}
.progress__fill {
  height: 100%;
  background: var(--color-accent-orange);
  border-radius: var(--radius-full);
  transition: width 0.4s var(--ease-default);
}

/* 不确定态（加载中） */
.progress__fill--indeterminate {
  width: 40% !important;
  animation: indeterminate 1.5s ease-in-out infinite;
}
@keyframes indeterminate {
  0%   { transform: translateX(-100%); }
  100% { transform: translateX(300%); }
}

/* ── 页顶细条 ── */
.progress-bar-top {
  position: fixed;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: var(--color-border-subtle);
  z-index: 9999;
}
.progress-bar-top__fill {
  height: 100%;
  background: var(--color-accent-orange);
  transition: width 0.3s var(--ease-default);
}

/* ── 环形 ── */
.progress-ring__track { stroke: var(--color-border-subtle); }
.progress-ring__fill {
  stroke: var(--color-accent-orange);
  stroke-linecap: round;
  transition: stroke-dashoffset 0.5s var(--ease-default);
}
.progress-ring__text {
  font-family: var(--font-heading);
  font-size: 14px;
  font-weight: var(--weight-medium);
  fill: var(--color-text-primary);
}

/* JS 辅助：动态设置环形进度
   圆周 = 2 × π × r = 2 × 3.14159 × 32 ≈ 201.06
   offset = (1 - percent/100) × 201.06
*/
```

---

## 28. Search Bar 搜索框带联想 {#search}

```html
<div class="search" role="combobox" aria-expanded="false" aria-haspopup="listbox" aria-owns="search-results">
  <div class="search__input-wrap">
    <svg class="search__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
      <circle cx="7" cy="7" r="5" stroke="currentColor" stroke-width="1.5"/>
      <path d="M11 11l3 3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
    <input
      class="search__input"
      type="search"
      placeholder="搜索模型、文档、日志…"
      aria-autocomplete="list"
      aria-controls="search-results"
      autocomplete="off"
      spellcheck="false"
    >
    <kbd class="search__shortcut" aria-label="快捷键 Escape 清除">Esc</kbd>
  </div>

  <!-- 联想下拉 -->
  <ul class="search__results" id="search-results" role="listbox" aria-label="搜索结果" hidden>
    <li class="search__section-label" role="presentation">最近搜索</li>
    <li class="search__result" role="option" aria-selected="false">
      <svg class="search__result-icon" width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
        <circle cx="7" cy="7" r="5.5" stroke="currentColor" stroke-width="1.2"/>
        <path d="M4.5 7h5M7 4.5v5" stroke="currentColor" stroke-width="1.2" stroke-linecap="round"/>
      </svg>
      <span class="search__result-text">Claude 3.7 <mark class="search__highlight">Sonnet</mark></span>
      <span class="search__result-type">模型</span>
    </li>
    <li class="search__result" role="option" aria-selected="false">
      <svg class="search__result-icon" width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
        <path d="M2 2h10v10H2z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/>
        <path d="M4 5h6M4 7.5h4" stroke="currentColor" stroke-width="1.2" stroke-linecap="round"/>
      </svg>
      <span class="search__result-text">API <mark class="search__highlight">速率</mark>限制文档</span>
      <span class="search__result-type">文档</span>
    </li>
    <li class="search__divider" role="separator"></li>
    <li class="search__result search__result--action" role="option" aria-selected="false">
      <span>搜索全站「<strong class="search__query-echo"></strong>」</span>
      <kbd class="search__shortcut">↵</kbd>
    </li>
  </ul>
</div>
```

```css
.search { position: relative; width: 100%; max-width: 480px; }

.search__input-wrap {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-overlay);
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.search__input-wrap:focus-within {
  border-color: var(--color-accent-orange);
  box-shadow: 0 0 0 3px rgba(217,119,87,0.15);
}
.search__icon { color: var(--color-text-muted); flex-shrink: 0; }
.search__input {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  background: none;
  border: none;
  outline: none;
  min-width: 0;
}
.search__input::placeholder { color: var(--color-text-muted); }
.search__input::-webkit-search-cancel-button { display: none; }

.search__shortcut {
  flex-shrink: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 2px var(--space-2);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-sm);
  font-family: var(--font-heading);
  font-size: 11px;
  color: var(--color-text-muted);
}

/* ── 联想面板 ── */
.search__results {
  position: absolute;
  top: calc(100% + var(--space-2));
  left: 0; right: 0;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  box-shadow: 0 8px 40px rgba(20,20,19,0.12);
  padding: var(--space-2) 0;
  z-index: 250;
  list-style: none;
  margin: 0;
}
.search__results[hidden] { display: none; }

.search__section-label {
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}
.search__result {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-4);
  cursor: pointer;
  transition: background 0.12s ease;
}
.search__result:hover,
.search__result[aria-selected="true"] {
  background: var(--color-bg-raised);
}
.search__result-icon { color: var(--color-text-muted); flex-shrink: 0; }
.search__result-text {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
}
.search__highlight {
  background: rgba(217,119,87,0.2);
  color: var(--color-accent-warm);
  border-radius: 2px;
  padding: 0 2px;
  font-style: normal;
}
.search__result-type {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  background: var(--color-bg-base);
  padding: 1px var(--space-2);
  border-radius: var(--radius-sm);
}
.search__divider {
  height: 1px;
  background: var(--color-border-subtle);
  margin: var(--space-2) 0;
}
.search__result--action {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  justify-content: space-between;
}
```

---

## 29. Command Palette ⌘K 面板 {#command-palette}

```html
<!-- 触发层：全局监听 Cmd/Ctrl + K -->
<div class="cmd-overlay" id="cmd-overlay" role="dialog" aria-modal="true" aria-label="命令面板" hidden>
  <div class="cmd-palette">
    <div class="cmd-palette__search">
      <svg class="cmd-palette__icon" width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
        <circle cx="8" cy="8" r="6" stroke="currentColor" stroke-width="1.5"/>
        <path d="M13 13l3.5 3.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <input class="cmd-palette__input" type="text" placeholder="输入命令或搜索…" aria-autocomplete="list" autocomplete="off" spellcheck="false">
      <kbd class="cmd-palette__esc-hint">Esc</kbd>
    </div>

    <div class="cmd-palette__list" role="listbox">
      <div class="cmd-palette__group">
        <div class="cmd-palette__group-label">最近使用</div>
        <button class="cmd-palette__item cmd-palette__item--active" role="option" aria-selected="true">
          <span class="cmd-palette__item-icon">📄</span>
          <span class="cmd-palette__item-label">新建对话</span>
          <span class="cmd-palette__item-hint">模型 · Claude Sonnet</span>
          <kbd class="cmd-palette__item-kbd">↵</kbd>
        </button>
        <button class="cmd-palette__item" role="option" aria-selected="false">
          <span class="cmd-palette__item-icon">🔑</span>
          <span class="cmd-palette__item-label">生成 API 密钥</span>
          <span class="cmd-palette__item-hint">设置 · 密钥管理</span>
        </button>
        <button class="cmd-palette__item" role="option" aria-selected="false">
          <span class="cmd-palette__item-icon">🌙</span>
          <span class="cmd-palette__item-label">切换深色模式</span>
          <span class="cmd-palette__item-hint">外观</span>
          <kbd class="cmd-palette__item-kbd">⌘T</kbd>
        </button>
      </div>

      <div class="cmd-palette__group">
        <div class="cmd-palette__group-label">导航</div>
        <button class="cmd-palette__item" role="option" aria-selected="false">
          <span class="cmd-palette__item-icon">📊</span>
          <span class="cmd-palette__item-label">前往 · 控制台</span>
        </button>
        <button class="cmd-palette__item" role="option" aria-selected="false">
          <span class="cmd-palette__item-icon">📖</span>
          <span class="cmd-palette__item-label">前往 · API 文档</span>
        </button>
      </div>
    </div>

    <div class="cmd-palette__footer">
      <span><kbd>↑↓</kbd> 导航</span>
      <span><kbd>↵</kbd> 确认</span>
      <span><kbd>Esc</kbd> 关闭</span>
    </div>
  </div>
</div>
```

```css
.cmd-overlay {
  position: fixed;
  inset: 0;
  z-index: 500;
  background: rgba(20,20,19,0.55);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding-top: clamp(60px, 12vh, 140px);
  animation: fadeIn 0.15s ease;
}
.cmd-overlay[hidden] { display: none; }

.cmd-palette {
  width: 100%;
  max-width: 580px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-xl);
  box-shadow: 0 32px 100px rgba(20,20,19,0.3);
  overflow: hidden;
  animation: modalUp 0.2s var(--ease-default);
}

/* 搜索栏 */
.cmd-palette__search {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
  border-bottom: 1px solid var(--color-border-subtle);
}
.cmd-palette__icon { color: var(--color-text-muted); flex-shrink: 0; }
.cmd-palette__input {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-base);
  color: var(--color-text-primary);
  background: none;
  border: none;
  outline: none;
}
.cmd-palette__input::placeholder { color: var(--color-text-muted); }
.cmd-palette__esc-hint {
  font-family: var(--font-heading);
  font-size: 11px;
  color: var(--color-text-muted);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-sm);
  padding: 2px var(--space-2);
}

/* 列表 */
.cmd-palette__list {
  max-height: 360px;
  overflow-y: auto;
  padding: var(--space-2) 0;
  scroll-behavior: smooth;
}
.cmd-palette__group { padding: var(--space-2) 0; }
.cmd-palette__group + .cmd-palette__group { border-top: 1px solid var(--color-border-subtle); }
.cmd-palette__group-label {
  padding: var(--space-2) var(--space-5);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.07em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.cmd-palette__item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-5);
  text-align: left;
  cursor: pointer;
  transition: background 0.1s ease;
  border-radius: 0;
}
.cmd-palette__item:hover,
.cmd-palette__item--active {
  background: rgba(217,119,87,0.08);
}
.cmd-palette__item-icon { font-size: 14px; line-height: 1; flex-shrink: 0; }
.cmd-palette__item-label {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.cmd-palette__item-hint {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}
.cmd-palette__item-kbd {
  font-family: var(--font-heading);
  font-size: 11px;
  color: var(--color-text-muted);
  background: var(--color-bg-base);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-sm);
  padding: 1px var(--space-2);
}

/* 底部提示 */
.cmd-palette__footer {
  display: flex;
  gap: var(--space-5);
  padding: var(--space-3) var(--space-5);
  border-top: 1px solid var(--color-border-subtle);
  background: var(--color-bg-base);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}
.cmd-palette__footer kbd {
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-default);
  border-radius: 3px;
  padding: 1px 4px;
  font-size: 10px;
  margin-right: 3px;
}
```

```js
// ⌘K / Ctrl+K 打开
document.addEventListener('keydown', e => {
  if ((e.metaKey || e.ctrlKey) && e.key === 'k') {
    e.preventDefault();
    const overlay = document.getElementById('cmd-overlay');
    overlay.removeAttribute('hidden');
    overlay.querySelector('.cmd-palette__input')?.focus();
  }
  if (e.key === 'Escape') {
    document.getElementById('cmd-overlay')?.setAttribute('hidden', '');
  }
});
// 点击遮罩关闭
document.getElementById('cmd-overlay')?.addEventListener('click', e => {
  if (e.target.id === 'cmd-overlay') e.target.setAttribute('hidden', '');
});
```

---

## 30. Drawer 侧滑抽屉 {#drawer}

```html
<!-- 触发 -->
<button class="btn btn-secondary" onclick="openDrawer('settings-drawer')">打开设置</button>

<!-- 抽屉 -->
<div class="drawer-overlay" id="settings-drawer" hidden>
  <div class="drawer" role="dialog" aria-modal="true" aria-labelledby="drawer-title">
    <div class="drawer__header">
      <h3 class="drawer__title" id="drawer-title">项目设置</h3>
      <button class="drawer__close" aria-label="关闭抽屉" onclick="closeDrawer('settings-drawer')">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <path d="M3 3l10 10M13 3L3 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
    </div>
    <div class="drawer__body">
      <!-- 内容区，可滚动 -->
      <p>抽屉内容区域，支持长内容独立滚动。</p>
    </div>
    <div class="drawer__footer">
      <button class="btn btn-secondary" onclick="closeDrawer('settings-drawer')">取消</button>
      <button class="btn btn-primary">保存更改</button>
    </div>
  </div>
</div>
```

```css
.drawer-overlay {
  position: fixed;
  inset: 0;
  z-index: 450;
  background: rgba(20,20,19,0.45);
  backdrop-filter: blur(3px);
  display: flex;
  justify-content: flex-end;
}
.drawer-overlay[hidden] { display: none; }

.drawer {
  width: min(480px, 100vw);
  height: 100%;
  display: flex;
  flex-direction: column;
  background: var(--color-bg-overlay);
  border-left: 1px solid var(--color-border-default);
  box-shadow: -16px 0 60px rgba(20,20,19,0.12);
  animation: slideInRight 0.3s var(--ease-default);
}
@keyframes slideInRight {
  from { transform: translateX(100%); }
  to   { transform: translateX(0); }
}
.drawer-overlay[hidden] .drawer {
  animation: slideOutRight 0.25s var(--ease-gentle) forwards;
}
@keyframes slideOutRight {
  to { transform: translateX(100%); }
}

.drawer__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-5) var(--space-6);
  border-bottom: 1px solid var(--color-border-subtle);
  flex-shrink: 0;
}
.drawer__title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 400;
  color: var(--color-text-primary);
}
.drawer__close {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-md);
  color: var(--color-text-muted);
  cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
}
.drawer__close:hover { background: var(--color-bg-raised); color: var(--color-text-primary); }
.drawer__body {
  flex: 1;
  overflow-y: auto;
  padding: var(--space-6);
  overscroll-behavior: contain;
}
.drawer__footer {
  display: flex;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-5) var(--space-6);
  border-top: 1px solid var(--color-border-subtle);
  flex-shrink: 0;
  background: var(--color-bg-base);
}
```

```js
function openDrawer(id) {
  const overlay = document.getElementById(id);
  overlay.removeAttribute('hidden');
  document.body.style.overflow = 'hidden';
  overlay.querySelector('.drawer__close')?.focus();
}
function closeDrawer(id) {
  document.getElementById(id).setAttribute('hidden', '');
  document.body.style.overflow = '';
}
document.addEventListener('keydown', e => {
  if (e.key === 'Escape')
    document.querySelectorAll('.drawer-overlay:not([hidden])').forEach(d => {
      d.setAttribute('hidden', ''); document.body.style.overflow = '';
    });
});
```

---

## 31. Chip / Tag 可删除标签 {#chip}

```html
<!-- 静态标签组 -->
<div class="chip-group" role="group" aria-label="已选标签">
  <span class="chip chip--orange">
    安全 AI
    <button class="chip__remove" aria-label="移除「安全 AI」">
      <svg width="10" height="10" viewBox="0 0 10 10" fill="none" aria-hidden="true">
        <path d="M2 2l6 6M8 2l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
  </span>
  <span class="chip chip--blue">
    宪法 AI
    <button class="chip__remove" aria-label="移除「宪法 AI」">
      <svg width="10" height="10" viewBox="0 0 10 10" fill="none" aria-hidden="true">
        <path d="M2 2l6 6M8 2l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
  </span>
  <span class="chip chip--green">研究 <button class="chip__remove" aria-label="移除「研究」"><svg width="10" height="10" viewBox="0 0 10 10" fill="none"><path d="M2 2l6 6M8 2l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg></button></span>
  <!-- 添加标签按钮 -->
  <button class="chip chip--add">
    <svg width="10" height="10" viewBox="0 0 10 10" fill="none" aria-hidden="true">
      <path d="M5 2v6M2 5h6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
    添加标签
  </button>
</div>
```

```css
.chip-group { display: flex; flex-wrap: wrap; gap: var(--space-2); align-items: center; }

.chip {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-1) var(--space-3);
  border-radius: var(--radius-full);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  border: 1px solid transparent;
  transition: all 0.15s ease;
  white-space: nowrap;
}
.chip--orange { background: rgba(217,119,87,0.12); color: var(--color-accent-warm);  border-color: rgba(217,119,87,0.25); }
.chip--blue   { background: rgba(106,155,204,0.12); color: var(--color-accent-blue); border-color: rgba(106,155,204,0.25); }
.chip--green  { background: rgba(120,140,93,0.12);  color: var(--color-accent-green);border-color: rgba(120,140,93,0.25); }
.chip--default{ background: var(--color-bg-raised); color: var(--color-text-secondary); border-color: var(--color-border-default); }

.chip__remove {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 16px;
  height: 16px;
  border-radius: var(--radius-full);
  opacity: 0.6;
  cursor: pointer;
  transition: opacity 0.15s ease, background 0.15s ease;
}
.chip__remove:hover { opacity: 1; background: rgba(0,0,0,0.08); }

.chip--add {
  background: transparent;
  color: var(--color-text-muted);
  border: 1px dashed var(--color-border-default);
  cursor: pointer;
}
.chip--add:hover { color: var(--color-text-primary); border-color: var(--color-border-strong); border-style: solid; }
```

---

## 32. Popover 浮层卡片 {#popover}

```html
<!-- Popover 由触发元素 + 浮层卡片组成 -->
<div class="popover-wrap" data-popover>
  <button class="btn btn-secondary popover-trigger">
    查看详情
  </button>
  <div class="popover" role="tooltip">
    <div class="popover__header">
      <h4 class="popover__title">Claude 3.7 Sonnet</h4>
      <span class="badge badge-green">运行中</span>
    </div>
    <div class="popover__body">
      <dl class="popover__meta">
        <div class="popover__meta-row">
          <dt>上下文</dt><dd>200K tokens</dd>
        </div>
        <div class="popover__meta-row">
          <dt>输入价格</dt><dd>$3 / 1M tokens</dd>
        </div>
        <div class="popover__meta-row">
          <dt>输出价格</dt><dd>$15 / 1M tokens</dd>
        </div>
      </dl>
    </div>
    <div class="popover__footer">
      <a href="#" class="btn btn-ghost" style="font-size:var(--text-xs)">查看完整文档 →</a>
    </div>
    <div class="popover__arrow" aria-hidden="true"></div>
  </div>
</div>
```

```css
.popover-wrap { position: relative; display: inline-block; }

.popover {
  position: absolute;
  bottom: calc(100% + 12px);
  left: 50%;
  transform: translateX(-50%) translateY(6px);
  width: 260px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  box-shadow: 0 12px 48px rgba(20,20,19,0.14);
  z-index: 300;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.2s var(--ease-default), transform 0.2s var(--ease-default), visibility 0.2s;
  pointer-events: none;
}
.popover-wrap:hover .popover,
.popover-wrap.is-open .popover {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(0);
  pointer-events: auto;
}

.popover__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4) var(--space-4) var(--space-3);
  border-bottom: 1px solid var(--color-border-subtle);
}
.popover__title {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.popover__body { padding: var(--space-3) var(--space-4); }
.popover__meta { display: flex; flex-direction: column; gap: var(--space-2); }
.popover__meta-row {
  display: flex;
  justify-content: space-between;
  font-family: var(--font-heading);
  font-size: var(--text-xs);
}
.popover__meta-row dt { color: var(--color-text-muted); }
.popover__meta-row dd { color: var(--color-text-primary); font-weight: var(--weight-medium); font-variant-numeric: tabular-nums; }
.popover__footer {
  padding: var(--space-2) var(--space-4) var(--space-3);
  border-top: 1px solid var(--color-border-subtle);
}

/* 箭头 */
.popover__arrow {
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%) rotate(45deg);
  width: 10px;
  height: 10px;
  background: var(--color-bg-overlay);
  border-right: 1px solid var(--color-border-default);
  border-bottom: 1px solid var(--color-border-default);
}
```

---

## 33. Carousel 轮播 {#carousel}

```html
<div class="carousel" data-carousel aria-label="产品特性展示" aria-roledescription="carousel">
  <div class="carousel__track-wrap">
    <ul class="carousel__track" role="list">
      <li class="carousel__slide" role="group" aria-roledescription="幻灯片" aria-label="第 1 张，共 3 张">
        <div class="carousel__card">
          <div class="carousel__card-icon" aria-hidden="true">🛡️</div>
          <h3 class="carousel__card-title">安全优先</h3>
          <p class="carousel__card-body">通过宪法 AI 训练，使模型天然倾向于安全和诚实。</p>
        </div>
      </li>
      <li class="carousel__slide" role="group" aria-roledescription="幻灯片" aria-label="第 2 张，共 3 张">
        <div class="carousel__card">
          <div class="carousel__card-icon" aria-hidden="true">🔬</div>
          <h3 class="carousel__card-title">前沿研究</h3>
          <p class="carousel__card-body">持续推进对齐研究，探索 AI 可解释性的边界。</p>
        </div>
      </li>
      <li class="carousel__slide" role="group" aria-roledescription="幻灯片" aria-label="第 3 张，共 3 张">
        <div class="carousel__card">
          <div class="carousel__card-icon" aria-hidden="true">🌍</div>
          <h3 class="carousel__card-title">普惠 AI</h3>
          <p class="carousel__card-body">让强大且安全的 AI 工具惠及每一个人。</p>
        </div>
      </li>
    </ul>
  </div>

  <!-- 控制条 -->
  <div class="carousel__controls" aria-label="轮播控制">
    <button class="carousel__btn carousel__btn--prev" aria-label="上一张">
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M10 4l-4 4 4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
    <div class="carousel__dots" role="tablist" aria-label="幻灯片导航">
      <button class="carousel__dot carousel__dot--active" role="tab" aria-selected="true"  aria-label="第 1 张"></button>
      <button class="carousel__dot" role="tab" aria-selected="false" aria-label="第 2 张"></button>
      <button class="carousel__dot" role="tab" aria-selected="false" aria-label="第 3 张"></button>
    </div>
    <button class="carousel__btn carousel__btn--next" aria-label="下一张">
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M6 4l4 4-4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </button>
  </div>
</div>
```

```css
.carousel { overflow: hidden; }
.carousel__track-wrap { overflow: hidden; border-radius: var(--radius-xl); }
.carousel__track {
  display: flex;
  list-style: none;
  padding: 0; margin: 0;
  transition: transform 0.4s var(--ease-default);
}
.carousel__slide { flex: 0 0 100%; padding: var(--space-2); }
.carousel__card {
  padding: var(--space-10) var(--space-8);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-xl);
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-4);
}
.carousel__card-icon { font-size: 2.5rem; }
.carousel__card-title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 400;
  color: var(--color-text-primary);
}
.carousel__card-body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
  max-width: 40ch;
}

.carousel__controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-4);
  margin-top: var(--space-5);
}
.carousel__btn {
  width: 36px; height: 36px;
  display: flex; align-items: center; justify-content: center;
  border-radius: var(--radius-full);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-default);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition: all 0.15s ease;
}
.carousel__btn:hover { background: var(--color-bg-overlay); color: var(--color-text-primary); border-color: var(--color-border-strong); }
.carousel__dots { display: flex; gap: var(--space-2); }
.carousel__dot {
  width: 8px; height: 8px;
  border-radius: var(--radius-full);
  background: var(--color-border-default);
  border: none; padding: 0; cursor: pointer;
  transition: all 0.2s ease;
}
.carousel__dot--active {
  background: var(--color-accent-orange);
  width: 20px;
}
```

```js
(function() {
  const carousel = document.querySelector('[data-carousel]');
  if (!carousel) return;
  const track = carousel.querySelector('.carousel__track');
  const slides = carousel.querySelectorAll('.carousel__slide');
  const dots   = carousel.querySelectorAll('.carousel__dot');
  let current = 0;

  function goTo(n) {
    current = (n + slides.length) % slides.length;
    track.style.transform = `translateX(-${current * 100}%)`;
    dots.forEach((d, i) => {
      d.classList.toggle('carousel__dot--active', i === current);
      d.setAttribute('aria-selected', i === current);
    });
  }

  carousel.querySelector('.carousel__btn--prev').addEventListener('click', () => goTo(current - 1));
  carousel.querySelector('.carousel__btn--next').addEventListener('click', () => goTo(current + 1));
  dots.forEach((d, i) => d.addEventListener('click', () => goTo(i)));
})();
```

---

## 34. Context Menu 右键菜单 {#context-menu}

```html
<!-- 在任意容器上绑定 contextmenu 事件 -->
<div class="context-target" data-context-menu="file-menu">
  右键点击这里
</div>

<!-- 菜单（绝对定位，JS 控制位置） -->
<ul class="context-menu" id="file-menu" role="menu" hidden>
  <li role="menuitem"><button class="context-menu__item">
    <svg class="context-menu__icon" width="14" height="14" viewBox="0 0 14 14" fill="none"><path d="M7 2v10M2 7h10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>
    新建文件
  </button></li>
  <li role="menuitem"><button class="context-menu__item">
    <svg class="context-menu__icon" width="14" height="14" viewBox="0 0 14 14" fill="none"><rect x="2" y="2" width="6" height="8" rx="1" stroke="currentColor" stroke-width="1.5"/><path d="M8 4h2a1 1 0 011 1v5a1 1 0 01-1 1H6a1 1 0 01-1-1v-2" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>
    复制
    <kbd class="context-menu__kbd">⌘C</kbd>
  </button></li>
  <li role="separator"><hr class="context-menu__divider"></li>
  <li role="menuitem"><button class="context-menu__item context-menu__item--danger">
    <svg class="context-menu__icon" width="14" height="14" viewBox="0 0 14 14" fill="none"><path d="M2 4h10M5 4V2h4v2M4 4l1 8h4l1-8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
    删除
    <kbd class="context-menu__kbd">⌫</kbd>
  </button></li>
</ul>
```

```css
.context-menu {
  position: fixed;
  z-index: 600;
  min-width: 180px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  box-shadow: 0 8px 32px rgba(20,20,19,0.15);
  padding: var(--space-2) 0;
  list-style: none;
  margin: 0;
  animation: scaleIn 0.12s var(--ease-default);
}
.context-menu[hidden] { display: none; }
@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.96); }
  to   { opacity: 1; transform: scale(1); }
}
.context-menu__item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  text-align: left;
  cursor: pointer;
  transition: background 0.1s ease, color 0.1s ease;
}
.context-menu__item:hover { background: var(--color-bg-raised); color: var(--color-text-primary); }
.context-menu__item--danger { color: var(--color-error); }
.context-menu__item--danger:hover { background: rgba(192,69,58,0.08); }
.context-menu__icon { flex-shrink: 0; opacity: 0.7; }
.context-menu__kbd {
  margin-left: auto;
  font-size: 11px;
  color: var(--color-text-muted);
  background: var(--color-bg-base);
  border: 1px solid var(--color-border-default);
  border-radius: 3px;
  padding: 1px 4px;
}
.context-menu__divider { border: none; border-top: 1px solid var(--color-border-subtle); margin: var(--space-1) 0; }
```

```js
const contextMenu = document.getElementById('file-menu');
document.querySelectorAll('[data-context-menu]').forEach(el => {
  el.addEventListener('contextmenu', e => {
    e.preventDefault();
    const vw = window.innerWidth, vh = window.innerHeight;
    let x = e.clientX, y = e.clientY;
    contextMenu.removeAttribute('hidden');
    const w = contextMenu.offsetWidth, h = contextMenu.offsetHeight;
    if (x + w > vw) x = vw - w - 8;
    if (y + h > vh) y = vh - h - 8;
    contextMenu.style.left = x + 'px';
    contextMenu.style.top  = y + 'px';
  });
});
document.addEventListener('click',      () => contextMenu.setAttribute('hidden', ''));
document.addEventListener('keydown', e => { if (e.key === 'Escape') contextMenu.setAttribute('hidden', ''); });
```

---

## 35. Floating Action Button 悬浮按钮 {#fab}

```html
<!-- 单个 FAB -->
<button class="fab" aria-label="新建对话">
  <svg width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
    <path d="M10 4v12M4 10h12" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
  </svg>
</button>

<!-- 可展开 FAB 组 -->
<div class="fab-group" data-fab-group>
  <!-- 子按钮（展开时显示） -->
  <div class="fab-group__items">
    <div class="fab-group__item">
      <span class="fab-group__item-label">上传文件</span>
      <button class="fab fab--sm fab--secondary" aria-label="上传文件">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M8 12V4M4 7l4-4 4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
    <div class="fab-group__item">
      <span class="fab-group__item-label">新建对话</span>
      <button class="fab fab--sm fab--secondary" aria-label="新建对话">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none"><path d="M8 4v8M4 8h8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>
      </button>
    </div>
  </div>
  <!-- 主按钮 -->
  <button class="fab fab-group__trigger" aria-label="展开操作" aria-expanded="false">
    <svg class="fab-group__icon" width="20" height="20" viewBox="0 0 20 20" fill="none" aria-hidden="true">
      <path d="M10 4v12M4 10h12" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
  </button>
</div>
```

```css
/* ── 基础 FAB ── */
.fab {
  width: 52px;
  height: 52px;
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--color-accent-orange);
  color: white;
  box-shadow: 0 4px 20px rgba(217,119,87,0.45);
  cursor: pointer;
  border: none;
  transition: transform 0.2s var(--ease-bounce), box-shadow 0.2s ease, background 0.2s ease;
}
.fab:hover {
  background: var(--color-accent-warm);
  transform: scale(1.08);
  box-shadow: 0 8px 28px rgba(217,119,87,0.5);
}
.fab:active { transform: scale(0.95); }
.fab--sm { width: 40px; height: 40px; }
.fab--secondary {
  background: var(--color-bg-overlay);
  color: var(--color-text-primary);
  border: 1px solid var(--color-border-default);
  box-shadow: 0 4px 16px rgba(20,20,19,0.1);
}
.fab--secondary:hover { background: var(--color-bg-raised); box-shadow: 0 6px 20px rgba(20,20,19,0.12); }

/* ── FAB 组 ── */
.fab-group {
  position: fixed;
  bottom: var(--space-8);
  right: var(--space-8);
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--space-3);
  z-index: 400;
}
.fab-group__items {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--space-3);
  opacity: 0;
  visibility: hidden;
  transform: translateY(12px);
  transition: opacity 0.25s var(--ease-default), transform 0.25s var(--ease-default), visibility 0.25s;
}
.fab-group.is-open .fab-group__items {
  opacity: 1;
  visibility: visible;
  transform: none;
}
.fab-group__item { display: flex; align-items: center; gap: var(--space-3); }
.fab-group__item-label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  color: var(--color-text-inverted);
  background: var(--color-bg-inverted);
  padding: var(--space-1) var(--space-3);
  border-radius: var(--radius-full);
  white-space: nowrap;
  box-shadow: 0 2px 8px rgba(20,20,19,0.2);
}
.fab-group__icon {
  transition: transform 0.3s var(--ease-default);
}
.fab-group.is-open .fab-group__icon { transform: rotate(45deg); }
```

```js
document.querySelectorAll('[data-fab-group]').forEach(group => {
  group.querySelector('.fab-group__trigger').addEventListener('click', () => {
    const open = group.classList.toggle('is-open');
    group.querySelector('.fab-group__trigger').setAttribute('aria-expanded', open);
  });
  document.addEventListener('click', e => {
    if (!group.contains(e.target)) group.classList.remove('is-open');
  });
});
```

---

## ══════════════════════════════
## 补充缺失组件（v2 升级）
## ══════════════════════════════

