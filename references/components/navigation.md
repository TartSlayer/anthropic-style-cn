## 11. Sidebar 侧边栏 {#sidebar}

```html
<div class="app-layout">
  <aside class="sidebar" id="sidebar" aria-label="主导航">
    <div class="sidebar__brand">
      <span class="sidebar__logo">⬡</span>
      <span class="sidebar__brand-name">Anthropic</span>
    </div>

    <nav class="sidebar__nav">
      <div class="sidebar__section-label">主菜单</div>
      <ul class="sidebar__list">
        <li>
          <a href="#" class="sidebar__item sidebar__item--active" aria-current="page">
            <svg class="sidebar__icon" width="16" height="16" viewBox="0 0 16 16" fill="none">
              <rect x="2" y="2" width="5" height="5" rx="1" fill="currentColor"/>
              <rect x="9" y="2" width="5" height="5" rx="1" fill="currentColor" opacity=".5"/>
              <rect x="2" y="9" width="5" height="5" rx="1" fill="currentColor" opacity=".5"/>
              <rect x="9" y="9" width="5" height="5" rx="1" fill="currentColor" opacity=".5"/>
            </svg>
            概览
          </a>
        </li>
        <li>
          <a href="#" class="sidebar__item">
            <svg class="sidebar__icon" width="16" height="16" viewBox="0 0 16 16" fill="none">
              <circle cx="8" cy="6" r="3" stroke="currentColor" stroke-width="1.5"/>
              <path d="M2 14c0-3.314 2.686-5 6-5s6 1.686 6 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
            用户管理
          </a>
        </li>
        <li>
          <a href="#" class="sidebar__item">
            <svg class="sidebar__icon" width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M8 2L10 6H14L11 9L12 13L8 11L4 13L5 9L2 6H6L8 2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            </svg>
            模型
            <span class="sidebar__badge">3</span>
          </a>
        </li>
        <li>
          <a href="#" class="sidebar__item">
            <svg class="sidebar__icon" width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M2 4h12M2 8h8M2 12h10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
            日志
          </a>
        </li>
      </ul>

      <div class="sidebar__section-label">设置</div>
      <ul class="sidebar__list">
        <li>
          <a href="#" class="sidebar__item">
            <svg class="sidebar__icon" width="16" height="16" viewBox="0 0 16 16" fill="none">
              <circle cx="8" cy="8" r="2.5" stroke="currentColor" stroke-width="1.5"/>
              <path d="M8 1v2M8 13v2M1 8h2M13 8h2M3.05 3.05l1.41 1.41M11.54 11.54l1.41 1.41M3.05 12.95l1.41-1.41M11.54 4.46l1.41-1.41" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
            偏好设置
          </a>
        </li>
      </ul>
    </nav>

    <div class="sidebar__footer">
      <div class="sidebar__user">
        <div class="sidebar__avatar">DA</div>
        <div class="sidebar__user-info">
          <div class="sidebar__user-name">Dario A.</div>
          <div class="sidebar__user-role">管理员</div>
        </div>
      </div>
    </div>
  </aside>

  <main class="sidebar__main">
    <!-- 页面主内容 -->
  </main>
</div>
```

```css
.app-layout {
  display: flex;
  min-height: 100vh;
}

/* ── 侧边栏容器 ── */
.sidebar {
  width: 240px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  background: var(--color-bg-raised);
  border-right: 1px solid var(--color-border-subtle);
  padding: var(--space-4);
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

/* ── 品牌区 ── */
.sidebar__brand {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-2);
  margin-bottom: var(--space-6);
}
.sidebar__logo {
  font-size: 1.4rem;
  color: var(--color-accent-orange);
  line-height: 1;
}
.sidebar__brand-name {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-bold);
  color: var(--color-text-primary);
  letter-spacing: -0.01em;
}

/* ── 导航 ── */
.sidebar__nav { flex: 1; display: flex; flex-direction: column; gap: var(--space-1); }
.sidebar__section-label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-text-muted);
  padding: var(--space-4) var(--space-2) var(--space-2);
}
.sidebar__list { display: flex; flex-direction: column; gap: 2px; }

.sidebar__item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-md);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-regular);
  color: var(--color-text-secondary);
  text-decoration: none;
  transition: background 0.15s ease, color 0.15s ease;
  position: relative;
}
.sidebar__item:hover {
  background: var(--color-bg-overlay);
  color: var(--color-text-primary);
}
.sidebar__item--active {
  background: rgba(217,119,87,0.1);
  color: var(--color-accent-orange);
  font-weight: var(--weight-medium);
}
.sidebar__icon {
  flex-shrink: 0;
  opacity: 0.7;
}
.sidebar__item--active .sidebar__icon { opacity: 1; }

.sidebar__badge {
  margin-left: auto;
  background: var(--color-accent-orange);
  color: white;
  font-size: 10px;
  font-weight: var(--weight-bold);
  line-height: 1;
  padding: 2px 6px;
  border-radius: var(--radius-full);
}

/* ── 用户区 ── */
.sidebar__footer {
  padding-top: var(--space-4);
  border-top: 1px solid var(--color-border-subtle);
  margin-top: var(--space-4);
}
.sidebar__user {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-2);
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: background 0.15s ease;
}
.sidebar__user:hover { background: var(--color-bg-overlay); }
.sidebar__avatar {
  width: 32px;
  height: 32px;
  border-radius: var(--radius-full);
  background: rgba(217,119,87,0.15);
  color: var(--color-accent-orange);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-bold);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.sidebar__user-name {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.sidebar__user-role {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}

.sidebar__main {
  flex: 1;
  padding: var(--space-8);
  overflow-y: auto;
}
```

---

## 12. Tabs 标签页 {#tabs}

```html
<div class="tabs" role="tablist" aria-label="内容分区">
  <button class="tabs__tab tabs__tab--active" role="tab" aria-selected="true" aria-controls="panel-overview" data-tab="overview">
    概览
  </button>
  <button class="tabs__tab" role="tab" aria-selected="false" aria-controls="panel-models" data-tab="models">
    模型
    <span class="tabs__count">4</span>
  </button>
  <button class="tabs__tab" role="tab" aria-selected="false" aria-controls="panel-logs" data-tab="logs">
    日志
  </button>
  <button class="tabs__tab" role="tab" aria-selected="false" aria-controls="panel-settings" data-tab="settings">
    设置
  </button>
</div>

<div class="tabs__panel" id="panel-overview" role="tabpanel">
  <!-- 概览内容 -->
</div>
<div class="tabs__panel tabs__panel--hidden" id="panel-models" role="tabpanel">
  <!-- 模型内容 -->
</div>
```

```css
.tabs {
  display: flex;
  gap: 0;
  border-bottom: 1px solid var(--color-border-subtle);
  padding-bottom: 0;
  position: relative;
}
.tabs__tab {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-regular);
  color: var(--color-text-muted);
  background: none;
  border: none;
  border-bottom: 2px solid transparent;
  margin-bottom: -1px;
  cursor: pointer;
  transition: color 0.15s ease, border-color 0.15s ease;
  white-space: nowrap;
}
.tabs__tab:hover { color: var(--color-text-primary); }
.tabs__tab--active {
  color: var(--color-text-primary);
  font-weight: var(--weight-medium);
  border-bottom-color: var(--color-accent-orange);
}
.tabs__count {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 18px;
  height: 18px;
  padding: 0 var(--space-1);
  background: var(--color-border-default);
  color: var(--color-text-secondary);
  font-size: 11px;
  font-weight: var(--weight-medium);
  border-radius: var(--radius-full);
}
.tabs__tab--active .tabs__count {
  background: rgba(217,119,87,0.15);
  color: var(--color-accent-orange);
}
.tabs__panel { padding-top: var(--space-6); }
.tabs__panel--hidden { display: none; }
```

```js
// Tabs 切换逻辑
document.querySelectorAll('.tabs__tab').forEach(tab => {
  tab.addEventListener('click', () => {
    const target = tab.dataset.tab;
    document.querySelectorAll('.tabs__tab').forEach(t => {
      t.classList.remove('tabs__tab--active');
      t.setAttribute('aria-selected', 'false');
    });
    document.querySelectorAll('.tabs__panel').forEach(p => p.classList.add('tabs__panel--hidden'));
    tab.classList.add('tabs__tab--active');
    tab.setAttribute('aria-selected', 'true');
    document.getElementById(`panel-${target}`)?.classList.remove('tabs__panel--hidden');
  });
});
```

---

## 13. Breadcrumb 面包屑 {#breadcrumb}

```html
<nav aria-label="面包屑导航">
  <ol class="breadcrumb">
    <li class="breadcrumb__item">
      <a href="/" class="breadcrumb__link">首页</a>
    </li>
    <li class="breadcrumb__item" aria-hidden="true">
      <span class="breadcrumb__sep">›</span>
    </li>
    <li class="breadcrumb__item">
      <a href="/models" class="breadcrumb__link">模型管理</a>
    </li>
    <li class="breadcrumb__item" aria-hidden="true">
      <span class="breadcrumb__sep">›</span>
    </li>
    <li class="breadcrumb__item">
      <span class="breadcrumb__current" aria-current="page">Claude 3.7 Sonnet</span>
    </li>
  </ol>
</nav>
```

```css
.breadcrumb {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-1);
  list-style: none;
  padding: 0;
  margin: 0;
}
.breadcrumb__item {
  display: flex;
  align-items: center;
}
.breadcrumb__link {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  text-decoration: none;
  transition: color 0.15s ease;
}
.breadcrumb__link:hover { color: var(--color-text-primary); }
.breadcrumb__sep {
  font-size: var(--text-sm);
  color: var(--color-border-strong);
  margin: 0 var(--space-1);
  user-select: none;
}
.breadcrumb__current {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
```

---

## 14. Pagination 分页 {#pagination}

```html
<nav class="pagination" aria-label="分页">
  <button class="pagination__btn" aria-label="上一页" disabled>
    ← 上一页
  </button>
  <div class="pagination__pages">
    <button class="pagination__page pagination__page--active" aria-current="page">1</button>
    <button class="pagination__page">2</button>
    <button class="pagination__page">3</button>
    <span class="pagination__ellipsis">…</span>
    <button class="pagination__page">12</button>
  </div>
  <button class="pagination__btn" aria-label="下一页">
    下一页 →
  </button>
</nav>
```

```css
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  flex-wrap: wrap;
}
.pagination__btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-secondary);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: all 0.15s ease;
}
.pagination__btn:hover:not(:disabled) {
  color: var(--color-text-primary);
  border-color: var(--color-border-strong);
  background: var(--color-bg-overlay);
}
.pagination__btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
.pagination__pages {
  display: flex;
  align-items: center;
  gap: var(--space-1);
}
.pagination__page {
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  background: transparent;
  border: 1px solid transparent;
  border-radius: var(--radius-md);
  cursor: pointer;
  transition: all 0.15s ease;
  font-variant-numeric: tabular-nums;
}
.pagination__page:hover {
  background: var(--color-bg-raised);
  border-color: var(--color-border-default);
  color: var(--color-text-primary);
}
.pagination__page--active {
  background: var(--color-accent-orange);
  border-color: var(--color-accent-orange);
  color: white;
  font-weight: var(--weight-medium);
}
.pagination__ellipsis {
  width: 36px;
  text-align: center;
  color: var(--color-text-muted);
  font-size: var(--text-sm);
  user-select: none;
}
```

---

## 15. Dropdown 下拉菜单 {#dropdown}

```html
<div class="dropdown" data-dropdown>
  <button class="dropdown__trigger btn btn-secondary" aria-haspopup="true" aria-expanded="false">
    操作
    <svg width="12" height="12" viewBox="0 0 12 12" fill="none" aria-hidden="true">
      <path d="M2 4l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </button>
  <div class="dropdown__menu" role="menu" aria-hidden="true">
    <button class="dropdown__item" role="menuitem">
      <svg class="dropdown__item-icon" width="14" height="14" viewBox="0 0 14 14" fill="none">
        <path d="M7 1v12M1 7h12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      新建模型
    </button>
    <button class="dropdown__item" role="menuitem">
      <svg class="dropdown__item-icon" width="14" height="14" viewBox="0 0 14 14" fill="none">
        <path d="M2 7h10M2 3h10M2 11h6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      查看日志
    </button>
    <div class="dropdown__divider" role="separator"></div>
    <button class="dropdown__item dropdown__item--danger" role="menuitem">
      <svg class="dropdown__item-icon" width="14" height="14" viewBox="0 0 14 14" fill="none">
        <path d="M2 4h10M5 4V2h4v2M6 7v4M8 7v4M3 4l1 8h6l1-8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      删除
    </button>
  </div>
</div>
```

```css
.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown__trigger svg {
  transition: transform 0.2s var(--ease-default);
}
.dropdown--open .dropdown__trigger svg {
  transform: rotate(180deg);
}
.dropdown__menu {
  position: absolute;
  top: calc(100% + var(--space-2));
  left: 0;
  min-width: 180px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  box-shadow: 0 8px 32px rgba(20,20,19,0.1), 0 2px 8px rgba(20,20,19,0.06);
  padding: var(--space-2);
  z-index: 200;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-8px) scale(0.98);
  transform-origin: top left;
  transition: opacity 0.18s var(--ease-default), transform 0.18s var(--ease-default), visibility 0.18s;
}
.dropdown--open .dropdown__menu {
  opacity: 1;
  visibility: visible;
  transform: none;
}
.dropdown__item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-3);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  border-radius: var(--radius-md);
  text-align: left;
  cursor: pointer;
  transition: background 0.12s ease, color 0.12s ease;
}
.dropdown__item:hover {
  background: var(--color-bg-raised);
  color: var(--color-text-primary);
}
.dropdown__item--danger { color: var(--color-error); }
.dropdown__item--danger:hover { background: rgba(192,69,58,0.08); color: var(--color-error); }
.dropdown__item-icon { flex-shrink: 0; opacity: 0.7; }
.dropdown__divider {
  height: 1px;
  background: var(--color-border-subtle);
  margin: var(--space-2) 0;
}
```

```js
// Dropdown 开关逻辑
document.querySelectorAll('[data-dropdown]').forEach(dropdown => {
  const trigger = dropdown.querySelector('.dropdown__trigger');
  trigger.addEventListener('click', e => {
    e.stopPropagation();
    const open = dropdown.classList.toggle('dropdown--open');
    trigger.setAttribute('aria-expanded', open);
    dropdown.querySelector('.dropdown__menu').setAttribute('aria-hidden', !open);
  });
  document.addEventListener('click', () => {
    dropdown.classList.remove('dropdown--open');
    trigger.setAttribute('aria-expanded', 'false');
  });
});
```

---

## ══════════════════════════════
## 表单与交互类
## ══════════════════════════════

