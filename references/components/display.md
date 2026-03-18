## 21. Table 数据表格 {#table}

```html
<div class="table-wrap">
  <table class="table" aria-label="模型列表">
    <thead class="table__head">
      <tr>
        <th class="table__th" scope="col">模型名称</th>
        <th class="table__th table__th--sortable" scope="col" aria-sort="descending">
          上下文长度
          <svg class="table__sort-icon" width="12" height="12" viewBox="0 0 12 12" fill="none">
            <path d="M6 2v8M3 8l3 3 3-3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </th>
        <th class="table__th" scope="col">状态</th>
        <th class="table__th" scope="col">最近更新</th>
        <th class="table__th table__th--action" scope="col">
          <span class="sr-only">操作</span>
        </th>
      </tr>
    </thead>
    <tbody class="table__body">
      <tr class="table__row">
        <td class="table__td">
          <div class="table__cell-primary">Claude 3.7 Sonnet</div>
          <div class="table__cell-secondary">claude-sonnet-4-6</div>
        </td>
        <td class="table__td table__td--mono">200K</td>
        <td class="table__td">
          <span class="badge badge-green">运行中</span>
        </td>
        <td class="table__td table__td--muted">2025 年 3 月</td>
        <td class="table__td table__td--action">
          <button class="btn btn-ghost">管理</button>
        </td>
      </tr>
      <tr class="table__row">
        <td class="table__td">
          <div class="table__cell-primary">Claude 3 Opus</div>
          <div class="table__cell-secondary">claude-opus-4-6</div>
        </td>
        <td class="table__td table__td--mono">200K</td>
        <td class="table__td">
          <span class="badge badge-blue">测试中</span>
        </td>
        <td class="table__td table__td--muted">2025 年 1 月</td>
        <td class="table__td table__td--action">
          <button class="btn btn-ghost">管理</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-wrap {
  width: 100%;
  overflow-x: auto;
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
}
.table {
  width: 100%;
  border-collapse: collapse;
  font-family: var(--font-heading);
}
.table__head { background: var(--color-bg-base); }
.table__th {
  padding: var(--space-3) var(--space-5);
  text-align: left;
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--color-text-muted);
  border-bottom: 1px solid var(--color-border-subtle);
  white-space: nowrap;
}
.table__th--sortable {
  cursor: pointer;
  user-select: none;
  display: table-cell;  /* override if needed */
}
.table__th--sortable:hover { color: var(--color-text-secondary); }
.table__sort-icon {
  display: inline-block;
  vertical-align: middle;
  margin-left: var(--space-1);
  color: var(--color-accent-orange);
}
.table__th--action { text-align: right; }

.table__body .table__row {
  transition: background 0.12s ease;
}
.table__body .table__row:hover { background: var(--color-bg-raised); }
.table__body .table__row + .table__row {
  border-top: 1px solid var(--color-border-subtle);
}

.table__td {
  padding: var(--space-4) var(--space-5);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  vertical-align: middle;
}
.table__td--mono {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
}
.table__td--muted { color: var(--color-text-muted); }
.table__td--action { text-align: right; }
.table__cell-primary {
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.table__cell-secondary {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin-top: 2px;
}
/* 屏幕阅读器专用 */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border: 0;
}
```

---

## 22. Timeline 时间线 {#timeline}

```html
<ol class="timeline" aria-label="版本历史">
  <li class="timeline__item timeline__item--active">
    <div class="timeline__marker" aria-hidden="true"></div>
    <div class="timeline__content">
      <time class="timeline__time" datetime="2025-03">2025 年 3 月</time>
      <h4 class="timeline__title">Claude 3.7 Sonnet 发布</h4>
      <p class="timeline__body">引入扩展思考模式，推理能力大幅提升，支持 200K 上下文。</p>
      <span class="badge badge-orange">最新</span>
    </div>
  </li>
  <li class="timeline__item">
    <div class="timeline__marker" aria-hidden="true"></div>
    <div class="timeline__content">
      <time class="timeline__time" datetime="2024-11">2024 年 11 月</time>
      <h4 class="timeline__title">Claude 3.5 Haiku 上线</h4>
      <p class="timeline__body">速度最快的 Claude 模型，适合高吞吐量场景。</p>
    </div>
  </li>
  <li class="timeline__item">
    <div class="timeline__marker" aria-hidden="true"></div>
    <div class="timeline__content">
      <time class="timeline__time" datetime="2024-06">2024 年 6 月</time>
      <h4 class="timeline__title">Claude 3.5 Sonnet 发布</h4>
      <p class="timeline__body">代码能力和指令遵循大幅优化。</p>
    </div>
  </li>
</ol>
```

```css
.timeline {
  position: relative;
  list-style: none;
  padding: 0;
  margin: 0;
}
/* 竖线 */
.timeline::before {
  content: '';
  position: absolute;
  left: 7px;
  top: 8px;
  bottom: 8px;
  width: 1px;
  background: var(--color-border-default);
}
.timeline__item {
  position: relative;
  display: flex;
  gap: var(--space-5);
  padding-bottom: var(--space-8);
}
.timeline__item:last-child { padding-bottom: 0; }

.timeline__marker {
  flex-shrink: 0;
  width: 15px;
  height: 15px;
  border-radius: var(--radius-full);
  background: var(--color-bg-base);
  border: 2px solid var(--color-border-strong);
  margin-top: 3px;
  transition: border-color 0.2s ease, background 0.2s ease;
  position: relative;
  z-index: 1;
}
.timeline__item--active .timeline__marker {
  background: var(--color-accent-orange);
  border-color: var(--color-accent-orange);
  box-shadow: 0 0 0 4px rgba(217,119,87,0.18);
}

.timeline__content {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  padding-top: 0;
}
.timeline__time {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.05em;
  color: var(--color-text-muted);
  text-transform: uppercase;
}
.timeline__title {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.timeline__body {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
  margin: 0;
}
```

---

## 23. Empty State 空状态 {#empty-state}

```html
<!-- 通用空状态 -->
<div class="empty-state">
  <div class="empty-state__icon" aria-hidden="true">
    <svg width="48" height="48" viewBox="0 0 48 48" fill="none">
      <circle cx="24" cy="24" r="20" stroke="currentColor" stroke-width="1.5" opacity=".3"/>
      <path d="M16 24h16M24 16v16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </div>
  <h3 class="empty-state__title">还没有任何模型</h3>
  <p class="empty-state__body">创建你的第一个模型配置，开始使用 Claude API。</p>
  <button class="btn btn-primary empty-state__action">
    创建模型
  </button>
</div>

<!-- 搜索无结果变体 -->
<div class="empty-state empty-state--search">
  <div class="empty-state__icon" aria-hidden="true">
    <svg width="48" height="48" viewBox="0 0 48 48" fill="none">
      <circle cx="21" cy="21" r="14" stroke="currentColor" stroke-width="1.5" opacity=".3"/>
      <path d="M31 31l8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" opacity=".3"/>
      <path d="M16 21h10M21 16v10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </div>
  <h3 class="empty-state__title">找不到匹配结果</h3>
  <p class="empty-state__body">试试其他关键词，或清除筛选条件。</p>
  <button class="btn btn-secondary empty-state__action">清除筛选</button>
</div>
```

```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: clamp(var(--space-16), 8vw, var(--space-32)) var(--space-8);
  gap: var(--space-4);
}
.empty-state__icon {
  color: var(--color-accent-sand);
  margin-bottom: var(--space-2);
}
.empty-state__title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 400;
  color: var(--color-text-primary);
}
.empty-state__body {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  max-width: 36ch;
  line-height: var(--leading-normal);
  margin: 0;
}
.empty-state__action { margin-top: var(--space-2); }
```

---

## 24. Banner / Alert 提示横幅 {#banner}

```html
<!-- 信息型 -->
<div class="banner banner--info" role="status">
  <svg class="banner__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
    <circle cx="8" cy="8" r="7" stroke="currentColor" stroke-width="1.5"/>
    <path d="M8 7v5M8 5v.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  <div class="banner__content">
    <strong class="banner__title">系统维护通知</strong>
    <span class="banner__body">计划于 3 月 20 日 02:00–04:00 进行例行维护，届时 API 可能短暂不可用。</span>
  </div>
  <button class="banner__close" aria-label="关闭通知">×</button>
</div>

<!-- 成功型 -->
<div class="banner banner--success" role="status">
  <svg class="banner__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
    <circle cx="8" cy="8" r="7" stroke="currentColor" stroke-width="1.5"/>
    <path d="M5 8l2 2 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
  </svg>
  <div class="banner__content">
    <strong class="banner__title">密钥创建成功</strong>
    <span class="banner__body">请立即复制密钥，此后将无法再次查看完整内容。</span>
  </div>
  <button class="banner__close" aria-label="关闭">×</button>
</div>

<!-- 警告型 -->
<div class="banner banner--warning" role="alert">
  <svg class="banner__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
    <path d="M8 2L14.93 14H1.07L8 2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
    <path d="M8 7v3M8 12v.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  <div class="banner__content">
    <strong class="banner__title">用量接近上限</strong>
    <span class="banner__body">本月 API 调用已使用 87%，请及时升级套餐。</span>
  </div>
  <a href="#" class="btn" style="font-size:var(--text-xs);padding:var(--space-2) var(--space-4);white-space:nowrap">升级套餐</a>
</div>

<!-- 错误型 -->
<div class="banner banner--error" role="alert">
  <svg class="banner__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
    <circle cx="8" cy="8" r="7" stroke="currentColor" stroke-width="1.5"/>
    <path d="M5 5l6 6M11 5l-6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
  </svg>
  <div class="banner__content">
    <strong class="banner__title">请求失败</strong>
    <span class="banner__body">无法连接到 API 服务，请检查网络后重试。</span>
  </div>
  <button class="banner__close" aria-label="关闭">×</button>
</div>
```

```css
.banner {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
  border-radius: var(--radius-lg);
  border: 1px solid transparent;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
}
.banner--info    { background: rgba(90,137,184,0.08);  border-color: rgba(90,137,184,0.25);  color: var(--color-info); }
.banner--success { background: rgba(107,143,71,0.08);  border-color: rgba(107,143,71,0.25);  color: var(--color-success); }
.banner--warning { background: rgba(201,148,58,0.08);  border-color: rgba(201,148,58,0.25);  color: var(--color-warning); }
.banner--error   { background: rgba(192,69,58,0.08);   border-color: rgba(192,69,58,0.25);   color: var(--color-error); }

.banner__icon { flex-shrink: 0; margin-top: 1px; }
.banner__content {
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-1) var(--space-2);
  align-items: baseline;
  color: var(--color-text-primary);
}
.banner__title {
  font-weight: var(--weight-medium);
  color: inherit; /* 继承 banner 类型颜色 */
}
.banner__body { color: var(--color-text-secondary); }
.banner__close {
  flex-shrink: 0;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.1rem;
  line-height: 1;
  color: var(--color-text-muted);
  padding: 0 var(--space-1);
  margin-top: -1px;
  transition: color 0.15s ease;
}
.banner__close:hover { color: var(--color-text-primary); }
```

---

## 25. Step Indicator 步骤条 {#step-indicator}

```html
<!-- 横向步骤条 -->
<nav class="steps" aria-label="创建流程">
  <ol class="steps__list">
    <li class="steps__step steps__step--done" aria-label="第 1 步：基本信息（已完成）">
      <div class="steps__node" aria-hidden="true">
        <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
          <path d="M2 6l3 3 5-5" stroke="white" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <span class="steps__label">基本信息</span>
    </li>
    <li class="steps__connector" aria-hidden="true"></li>

    <li class="steps__step steps__step--active" aria-label="第 2 步：权限配置（进行中）" aria-current="step">
      <div class="steps__node" aria-hidden="true">2</div>
      <span class="steps__label">权限配置</span>
    </li>
    <li class="steps__connector" aria-hidden="true"></li>

    <li class="steps__step" aria-label="第 3 步：环境变量（未开始）">
      <div class="steps__node" aria-hidden="true">3</div>
      <span class="steps__label">环境变量</span>
    </li>
    <li class="steps__connector" aria-hidden="true"></li>

    <li class="steps__step" aria-label="第 4 步：确认部署（未开始）">
      <div class="steps__node" aria-hidden="true">4</div>
      <span class="steps__label">确认部署</span>
    </li>
  </ol>
</nav>

<!-- 进度说明行（可选） -->
<div class="steps__meta">
  <span class="steps__meta-step">第 2 步 / 共 4 步</span>
  <span class="steps__meta-title">权限配置</span>
</div>
```

```css
.steps { width: 100%; }
.steps__list {
  display: flex;
  align-items: center;
  list-style: none;
  padding: 0;
  margin: 0;
}

/* 步骤节点 */
.steps__step {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-2);
  flex-shrink: 0;
}
.steps__node {
  width: 32px;
  height: 32px;
  border-radius: var(--radius-full);
  border: 2px solid var(--color-border-default);
  background: var(--color-bg-base);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  color: var(--color-text-muted);
  transition: all 0.2s ease;
}
.steps__label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  white-space: nowrap;
  transition: color 0.2s ease;
}

/* 完成态 */
.steps__step--done .steps__node {
  background: var(--color-accent-orange);
  border-color: var(--color-accent-orange);
  color: white;
}
.steps__step--done .steps__label { color: var(--color-text-secondary); }

/* 当前态 */
.steps__step--active .steps__node {
  background: var(--color-bg-base);
  border-color: var(--color-accent-orange);
  color: var(--color-accent-orange);
  box-shadow: 0 0 0 4px rgba(217,119,87,0.15);
}
.steps__step--active .steps__label {
  color: var(--color-text-primary);
  font-weight: var(--weight-medium);
}

/* 连接线 */
.steps__connector {
  flex: 1;
  height: 2px;
  background: var(--color-border-subtle);
  margin: 0 var(--space-2);
  margin-bottom: 20px; /* 对齐节点中心 */
  min-width: var(--space-6);
  transition: background 0.3s ease;
}
/* 已完成段落变为橙色 */
.steps__step--done + .steps__connector { background: var(--color-accent-orange); }

/* 说明行 */
.steps__meta {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  margin-top: var(--space-4);
}
.steps__meta-step {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  letter-spacing: 0.05em;
}
.steps__meta-title {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
```

---

## ══════════════════════════════
## 补充组件（高频）
## ══════════════════════════════

