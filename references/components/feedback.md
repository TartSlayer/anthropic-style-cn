# 反馈与输入组件

## 目录
1. [Number Stepper 数字步进器](#number-stepper)
2. [Radio Group 单选按钮组](#radio-group)
3. [File Upload/Dropzone 拖放上传](#dropzone)
4. [Segmented Control 分段控制](#segmented-control)
5. [Status Indicator 状态指示器](#status-indicator)
6. [Rating 星级评分](#rating)
7. [Notification Dropdown 通知中心](#notification)

---

## 36. Number Stepper 数字步进器 {#number-stepper}

```html
<!-- 独立数字步进器 -->
<div class="stepper" role="group" aria-labelledby="stepper-label-1">
  <label class="stepper__label" id="stepper-label-1">请求并发数</label>
  <div class="stepper__control">
    <button class="stepper__btn" aria-label="减少" data-stepper-action="dec">
      <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
        <path d="M3 7h8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
    <input
      class="stepper__input"
      type="number"
      value="4"
      min="1"
      max="100"
      step="1"
      aria-live="polite"
      aria-atomic="true"
    >
    <button class="stepper__btn" aria-label="增加" data-stepper-action="inc">
      <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
        <path d="M7 3v8M3 7h8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
    </button>
  </div>
  <p class="stepper__hint">范围 1–100</p>
</div>

<!-- 紧凑行内变体（用于表格、表单行内） -->
<div class="stepper stepper--inline">
  <button class="stepper__btn" aria-label="减少" data-stepper-action="dec">−</button>
  <input class="stepper__input" type="number" value="10" min="1" max="999">
  <button class="stepper__btn" aria-label="增加" data-stepper-action="inc">+</button>
</div>
```

```css
.stepper { display: flex; flex-direction: column; gap: var(--space-2); }
.stepper__label {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.stepper__control {
  display: inline-flex;
  align-items: center;
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-md);
  overflow: hidden;
  background: var(--color-bg-overlay);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.stepper__control:focus-within {
  border-color: var(--color-accent-orange);
  box-shadow: 0 0 0 3px rgba(217,119,87,0.15);
}
.stepper__btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  background: var(--color-bg-raised);
  color: var(--color-text-secondary);
  flex-shrink: 0;
  cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
  user-select: none;
}
.stepper__btn:hover:not(:disabled) {
  background: var(--color-bg-base);
  color: var(--color-text-primary);
}
.stepper__btn:active { background: var(--color-border-subtle); }
.stepper__btn:disabled { opacity: 0.35; cursor: not-allowed; }
.stepper__btn:first-child { border-right: 1px solid var(--color-border-subtle); }
.stepper__btn:last-child  { border-left:  1px solid var(--color-border-subtle); }

.stepper__input {
  width: 60px;
  height: 36px;
  text-align: center;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  font-variant-numeric: tabular-nums;
  color: var(--color-text-primary);
  background: transparent;
  border: none;
  outline: none;
  -moz-appearance: textfield;
}
.stepper__input::-webkit-outer-spin-button,
.stepper__input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }

.stepper__hint {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin: 0;
}

/* 行内紧凑变体 */
.stepper--inline { flex-direction: row; align-items: center; gap: 0; }
.stepper--inline .stepper__input { width: 48px; }
```

```js
document.querySelectorAll('[data-stepper-action]').forEach(btn => {
  btn.addEventListener('click', () => {
    const input = btn.closest('.stepper__control').querySelector('.stepper__input');
    const step  = Number(input.step)  || 1;
    const min   = Number(input.min)   ?? -Infinity;
    const max   = Number(input.max)   ?? Infinity;
    const val   = Number(input.value) || 0;
    const isInc = btn.dataset.stepperAction === 'inc';
    const next  = Math.min(max, Math.max(min, val + (isInc ? step : -step)));
    input.value = next;
    input.dispatchEvent(new Event('change', { bubbles: true }));
    // 边界时禁用按钮
    const decBtn = btn.closest('.stepper__control').querySelector('[data-stepper-action="dec"]');
    const incBtn = btn.closest('.stepper__control').querySelector('[data-stepper-action="inc"]');
    if (decBtn) decBtn.disabled = next <= min;
    if (incBtn) incBtn.disabled = next >= max;
  });
});
```

---

## 37. Radio Group 单选按钮组 {#radio-group}

```html
<!-- 竖向单选组 -->
<fieldset class="radio-group">
  <legend class="radio-group__legend">
    选择套餐
    <span class="form__required" aria-hidden="true">*</span>
  </legend>
  <div class="radio-group__list">
    <label class="radio-item">
      <input type="radio" name="plan" value="free" class="radio-item__input">
      <span class="radio-item__circle" aria-hidden="true"></span>
      <span class="radio-item__content">
        <span class="radio-item__label">免费版</span>
        <span class="radio-item__desc">每月 100 次 API 调用，适合体验</span>
      </span>
    </label>
    <label class="radio-item radio-item--checked">
      <input type="radio" name="plan" value="pro" class="radio-item__input" checked>
      <span class="radio-item__circle" aria-hidden="true"></span>
      <span class="radio-item__content">
        <span class="radio-item__label">Pro <span class="badge badge-orange">推荐</span></span>
        <span class="radio-item__desc">无限调用，优先访问新模型，$20/月</span>
      </span>
    </label>
    <label class="radio-item">
      <input type="radio" name="plan" value="enterprise" class="radio-item__input">
      <span class="radio-item__circle" aria-hidden="true"></span>
      <span class="radio-item__content">
        <span class="radio-item__label">企业版</span>
        <span class="radio-item__desc">定制合同，专属支持，联系销售</span>
      </span>
    </label>
  </div>
</fieldset>

<!-- 横向紧凑单选（如性别、频率选择） -->
<fieldset class="radio-group radio-group--horizontal">
  <legend class="radio-group__legend">通知频率</legend>
  <div class="radio-group__list">
    <label class="radio-chip">
      <input type="radio" name="freq" value="realtime" class="radio-chip__input">
      <span class="radio-chip__label">实时</span>
    </label>
    <label class="radio-chip">
      <input type="radio" name="freq" value="daily" class="radio-chip__input" checked>
      <span class="radio-chip__label">每日</span>
    </label>
    <label class="radio-chip">
      <input type="radio" name="freq" value="weekly" class="radio-chip__input">
      <span class="radio-chip__label">每周</span>
    </label>
    <label class="radio-chip">
      <input type="radio" name="freq" value="never" class="radio-chip__input">
      <span class="radio-chip__label">关闭</span>
    </label>
  </div>
</fieldset>
```

```css
/* ── 字段集基础 ── */
.radio-group { border: none; padding: 0; margin: 0; }
.radio-group__legend {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  margin-bottom: var(--space-3);
  display: flex;
  align-items: center;
  gap: var(--space-1);
}
.radio-group__list { display: flex; flex-direction: column; gap: var(--space-2); }
.radio-group--horizontal .radio-group__list {
  flex-direction: row;
  flex-wrap: wrap;
  gap: var(--space-2);
}

/* ── 卡片式单选项 ── */
.radio-item {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
  background: var(--color-bg-raised);
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  cursor: pointer;
  transition: border-color 0.2s ease, background 0.2s ease;
}
.radio-item:hover { border-color: var(--color-border-strong); }
.radio-item__input {
  position: absolute;
  opacity: 0;
  width: 0; height: 0;
}
.radio-item__circle {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  margin-top: 2px;
  border-radius: var(--radius-full);
  border: 2px solid var(--color-border-default);
  background: var(--color-bg-overlay);
  transition: all 0.18s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}
.radio-item__input:checked ~ .radio-item__circle {
  border-color: var(--color-accent-orange);
  background: var(--color-accent-orange);
  box-shadow: inset 0 0 0 3px var(--color-bg-overlay);
}
.radio-item__input:checked ~ .radio-item__circle::after {
  content: '';
  width: 6px;
  height: 6px;
  border-radius: var(--radius-full);
  background: white;
}
.radio-item__input:focus-visible ~ .radio-item__circle {
  outline: 2px solid var(--color-accent-orange);
  outline-offset: 2px;
}
.radio-item--checked,
.radio-item:has(.radio-item__input:checked) {
  border-color: var(--color-accent-orange);
  background: rgba(217,119,87,0.04);
}
.radio-item__label {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  display: flex;
  align-items: center;
  gap: var(--space-2);
}
.radio-item__desc {
  display: block;
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin-top: var(--space-1);
}

/* ── Chip 式单选 ── */
.radio-chip { display: inline-flex; cursor: pointer; }
.radio-chip__input {
  position: absolute;
  opacity: 0;
  width: 0; height: 0;
}
.radio-chip__label {
  display: inline-flex;
  align-items: center;
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-secondary);
  background: var(--color-bg-raised);
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-full);
  transition: all 0.15s ease;
  user-select: none;
}
.radio-chip__label:hover { border-color: var(--color-border-strong); color: var(--color-text-primary); }
.radio-chip__input:checked + .radio-chip__label {
  background: rgba(217,119,87,0.1);
  border-color: var(--color-accent-orange);
  color: var(--color-accent-warm);
}
.radio-chip__input:focus-visible + .radio-chip__label {
  outline: 2px solid var(--color-accent-orange);
  outline-offset: 2px;
}
```

---

## 38. File Upload / Dropzone 拖放上传 {#dropzone}

```html
<div class="dropzone" data-dropzone aria-label="文件上传区域">
  <input
    type="file"
    class="dropzone__input"
    id="file-upload"
    multiple
    accept=".pdf,.docx,.txt,.png,.jpg"
    aria-label="选择文件"
  >
  <label class="dropzone__area" for="file-upload">
    <div class="dropzone__icon" aria-hidden="true">
      <svg width="40" height="40" viewBox="0 0 40 40" fill="none">
        <rect x="2" y="2" width="36" height="36" rx="10" stroke="currentColor" stroke-width="1.5" stroke-dasharray="4 3"/>
        <path d="M20 26V14M14 20l6-6 6 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <p class="dropzone__title">拖放文件到此处</p>
    <p class="dropzone__subtitle">或 <span class="dropzone__browse">点击浏览</span></p>
    <p class="dropzone__hint">支持 PDF、Word、图片，单文件最大 20MB</p>
  </label>

  <!-- 已上传文件列表 -->
  <ul class="dropzone__file-list" aria-label="已选文件" aria-live="polite"></ul>
</div>
```

```css
.dropzone { display: flex; flex-direction: column; gap: var(--space-3); }

.dropzone__input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.dropzone__area {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-3);
  padding: var(--space-12) var(--space-8);
  text-align: center;
  border: 2px dashed var(--color-border-default);
  border-radius: var(--radius-xl);
  background: var(--color-bg-raised);
  cursor: pointer;
  transition: all 0.2s var(--ease-default);
}
.dropzone__area:hover,
.dropzone--dragging .dropzone__area {
  border-color: var(--color-accent-orange);
  background: rgba(217,119,87,0.04);
}
.dropzone--dragging .dropzone__area {
  box-shadow: 0 0 0 4px rgba(217,119,87,0.15);
}

.dropzone__icon { color: var(--color-accent-sand); }
.dropzone--dragging .dropzone__icon { color: var(--color-accent-orange); }

.dropzone__title {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  margin: 0;
}
.dropzone__subtitle {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  margin: 0;
}
.dropzone__browse {
  color: var(--color-text-link);
  text-decoration: underline;
  text-decoration-color: rgba(201,100,66,0.4);
}
.dropzone__hint {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin: 0;
}

/* 文件列表 */
.dropzone__file-list { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: var(--space-2); }
.dropzone__file-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-md);
  animation: fadeUp var(--duration-normal) var(--ease-default) both;
}
.dropzone__file-name {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.dropzone__file-size {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  flex-shrink: 0;
}
.dropzone__file-remove {
  flex-shrink: 0;
  color: var(--color-text-muted);
  cursor: pointer;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-sm);
  transition: background 0.15s ease, color 0.15s ease;
}
.dropzone__file-remove:hover { background: var(--color-bg-raised); color: var(--color-error); }
```

```js
(function() {
  document.querySelectorAll('[data-dropzone]').forEach(zone => {
    const input    = zone.querySelector('.dropzone__input');
    const area     = zone.querySelector('.dropzone__area');
    const fileList = zone.querySelector('.dropzone__file-list');
    let files = [];

    function formatSize(bytes) {
      if (bytes < 1024)       return bytes + ' B';
      if (bytes < 1024*1024)  return (bytes/1024).toFixed(1) + ' KB';
      return (bytes/1024/1024).toFixed(1) + ' MB';
    }

    function renderFiles() {
      fileList.innerHTML = files.map((f, i) => `
        <li class="dropzone__file-item">
          <span class="dropzone__file-name">${f.name}</span>
          <span class="dropzone__file-size">${formatSize(f.size)}</span>
          <button class="dropzone__file-remove" aria-label="移除 ${f.name}" data-index="${i}">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
              <path d="M2 2l8 8M10 2l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </button>
        </li>`).join('');
    }

    function addFiles(newFiles) {
      files = [...files, ...Array.from(newFiles)];
      renderFiles();
    }

    input.addEventListener('change', () => addFiles(input.files));

    fileList.addEventListener('click', e => {
      const btn = e.target.closest('[data-index]');
      if (btn) {
        files.splice(Number(btn.dataset.index), 1);
        renderFiles();
      }
    });

    ['dragenter','dragover'].forEach(ev => area.addEventListener(ev, e => {
      e.preventDefault(); zone.classList.add('dropzone--dragging');
    }));
    ['dragleave','drop'].forEach(ev => area.addEventListener(ev, e => {
      e.preventDefault(); zone.classList.remove('dropzone--dragging');
    }));
    area.addEventListener('drop', e => addFiles(e.dataTransfer.files));
  });
})();
```

---

## 39. Segmented Control 分段控制 {#segmented-control}

```html
<!-- 视图切换（List / Grid / Card）-->
<div class="segmented" role="group" aria-label="视图切换">
  <button class="segmented__btn" aria-pressed="false" data-view="list">
    <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
      <path d="M2 4h10M2 7h10M2 10h10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
    列表
  </button>
  <button class="segmented__btn segmented__btn--active" aria-pressed="true" data-view="grid">
    <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
      <rect x="2" y="2" width="4" height="4" rx="1" stroke="currentColor" stroke-width="1.3"/>
      <rect x="8" y="2" width="4" height="4" rx="1" stroke="currentColor" stroke-width="1.3"/>
      <rect x="2" y="8" width="4" height="4" rx="1" stroke="currentColor" stroke-width="1.3"/>
      <rect x="8" y="8" width="4" height="4" rx="1" stroke="currentColor" stroke-width="1.3"/>
    </svg>
    网格
  </button>
  <button class="segmented__btn" aria-pressed="false" data-view="card">
    <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
      <rect x="1" y="3" width="12" height="8" rx="2" stroke="currentColor" stroke-width="1.3"/>
      <path d="M4 6h6M4 9h4" stroke="currentColor" stroke-width="1.3" stroke-linecap="round"/>
    </svg>
    卡片
  </button>
</div>

<!-- 纯文字变体（无图标）-->
<div class="segmented segmented--sm" role="group" aria-label="时间范围">
  <button class="segmented__btn" aria-pressed="false">今天</button>
  <button class="segmented__btn segmented__btn--active" aria-pressed="true">本周</button>
  <button class="segmented__btn" aria-pressed="false">本月</button>
  <button class="segmented__btn" aria-pressed="false">全部</button>
</div>
```

```css
.segmented {
  display: inline-flex;
  align-items: center;
  gap: 2px;
  padding: 3px;
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
}
.segmented__btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-muted);
  background: transparent;
  border: none;
  border-radius: calc(var(--radius-lg) - 3px);
  cursor: pointer;
  white-space: nowrap;
  transition: color 0.15s ease, background 0.2s var(--ease-default), box-shadow 0.2s ease;
  user-select: none;
}
.segmented__btn:hover:not(.segmented__btn--active) {
  color: var(--color-text-secondary);
  background: rgba(0,0,0,0.04);
}
.segmented__btn--active {
  background: var(--color-bg-overlay);
  color: var(--color-text-primary);
  box-shadow: 0 1px 4px rgba(20,20,19,0.1), 0 0 0 1px var(--color-border-default);
}

/* 小尺寸变体 */
.segmented--sm .segmented__btn {
  padding: var(--space-1) var(--space-3);
  font-size: var(--text-xs);
}
```

```js
document.querySelectorAll('.segmented').forEach(seg => {
  seg.querySelectorAll('.segmented__btn').forEach(btn => {
    btn.addEventListener('click', () => {
      seg.querySelectorAll('.segmented__btn').forEach(b => {
        b.classList.remove('segmented__btn--active');
        b.setAttribute('aria-pressed', 'false');
      });
      btn.classList.add('segmented__btn--active');
      btn.setAttribute('aria-pressed', 'true');
    });
  });
});
```

---

## 40. Status Indicator 状态指示器 {#status-indicator}

```html
<!-- 独立状态点（用于用户在线状态、服务状态） -->
<span class="status status--online" aria-label="在线">
  <span class="status__dot" aria-hidden="true"></span>
  在线
</span>
<span class="status status--busy" aria-label="忙碌">
  <span class="status__dot" aria-hidden="true"></span>
  忙碌
</span>
<span class="status status--offline" aria-label="离线">
  <span class="status__dot" aria-hidden="true"></span>
  离线
</span>
<span class="status status--pending" aria-label="等待中">
  <span class="status__dot" aria-hidden="true"></span>
  处理中
</span>

<!-- 头像上的状态徽标 -->
<div class="avatar-status-wrap">
  <div class="avatar avatar--md avatar--orange">
    <span class="avatar__initials">DA</span>
  </div>
  <span class="status-badge status-badge--online" aria-label="在线状态"></span>
</div>

<!-- 服务状态横幅 -->
<div class="status-banner" role="status">
  <span class="status__dot status__dot--lg status__dot--online" aria-hidden="true"></span>
  <span class="status-banner__text">所有系统正常运行</span>
  <a href="#" class="status-banner__link">查看状态页 →</a>
</div>
```

```css
/* ── 行内状态标签 ── */
.status {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
}
.status--online  { color: var(--color-success); }
.status--busy    { color: var(--color-warning); }
.status--offline { color: var(--color-text-muted); }
.status--pending { color: var(--color-info); }

/* ── 状态圆点 ── */
.status__dot {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  flex-shrink: 0;
  position: relative;
}
.status--online  .status__dot,
.status__dot--online  { background: var(--color-success); }
.status--busy    .status__dot,
.status__dot--busy    { background: var(--color-warning); }
.status--offline .status__dot,
.status__dot--offline { background: var(--color-text-muted); }
.status--pending .status__dot,
.status__dot--pending { background: var(--color-info); }

/* 在线状态：呼吸动画 */
.status--online .status__dot::after,
.status__dot--online::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: var(--radius-full);
  background: inherit;
  animation: pulse-status 2s ease-in-out infinite;
}
@keyframes pulse-status {
  0%, 100% { transform: scale(1);   opacity: 0.7; }
  50%       { transform: scale(2.2); opacity: 0; }
}

/* 大尺寸点 */
.status__dot--lg { width: 12px; height: 12px; }

/* ── 头像状态徽标 ── */
.avatar-status-wrap { position: relative; display: inline-flex; }
.status-badge {
  position: absolute;
  bottom: 1px;
  right: 1px;
  width: 11px;
  height: 11px;
  border-radius: var(--radius-full);
  border: 2px solid var(--color-bg-base);
}
.status-badge--online  { background: var(--color-success); }
.status-badge--busy    { background: var(--color-warning); }
.status-badge--offline { background: var(--color-text-muted); }

/* ── 服务状态横幅 ── */
.status-banner {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-5);
  background: rgba(107,143,71,0.08);
  border: 1px solid rgba(107,143,71,0.2);
  border-radius: var(--radius-full);
  width: fit-content;
}
.status-banner__text {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
}
.status-banner__link {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  text-decoration: none;
  transition: color 0.15s ease;
}
.status-banner__link:hover { color: var(--color-text-primary); }
```

---

## 41. Rating 星级评分 {#rating}

```html
<!-- 只读展示 -->
<div class="rating" aria-label="评分 4.5 分（满分 5 分）">
  <span class="rating__star rating__star--full"  aria-hidden="true">★</span>
  <span class="rating__star rating__star--full"  aria-hidden="true">★</span>
  <span class="rating__star rating__star--full"  aria-hidden="true">★</span>
  <span class="rating__star rating__star--full"  aria-hidden="true">★</span>
  <span class="rating__star rating__star--half"  aria-hidden="true">★</span>
  <span class="rating__value">4.5</span>
  <span class="rating__count">（128 条评价）</span>
</div>

<!-- 可交互评分 -->
<fieldset class="rating-input" data-rating>
  <legend class="sr-only">选择评分</legend>
  <label class="rating-input__item" aria-label="1 星">
    <input type="radio" name="score" value="1" class="sr-only">
    <svg class="rating-input__star" viewBox="0 0 20 20" aria-hidden="true">
      <path d="M10 2l2.4 4.9 5.4.8-3.9 3.8.9 5.4L10 14.4l-4.8 2.5.9-5.4L2.2 7.7l5.4-.8L10 2z"
            stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/>
    </svg>
  </label>
  <label class="rating-input__item" aria-label="2 星">
    <input type="radio" name="score" value="2" class="sr-only">
    <svg class="rating-input__star" viewBox="0 0 20 20" aria-hidden="true"><path d="M10 2l2.4 4.9 5.4.8-3.9 3.8.9 5.4L10 14.4l-4.8 2.5.9-5.4L2.2 7.7l5.4-.8L10 2z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
  </label>
  <label class="rating-input__item" aria-label="3 星">
    <input type="radio" name="score" value="3" class="sr-only">
    <svg class="rating-input__star" viewBox="0 0 20 20" aria-hidden="true"><path d="M10 2l2.4 4.9 5.4.8-3.9 3.8.9 5.4L10 14.4l-4.8 2.5.9-5.4L2.2 7.7l5.4-.8L10 2z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
  </label>
  <label class="rating-input__item" aria-label="4 星">
    <input type="radio" name="score" value="4" class="sr-only">
    <svg class="rating-input__star" viewBox="0 0 20 20" aria-hidden="true"><path d="M10 2l2.4 4.9 5.4.8-3.9 3.8.9 5.4L10 14.4l-4.8 2.5.9-5.4L2.2 7.7l5.4-.8L10 2z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
  </label>
  <label class="rating-input__item" aria-label="5 星">
    <input type="radio" name="score" value="5" class="sr-only" checked>
    <svg class="rating-input__star" viewBox="0 0 20 20" aria-hidden="true"><path d="M10 2l2.4 4.9 5.4.8-3.9 3.8.9 5.4L10 14.4l-4.8 2.5.9-5.4L2.2 7.7l5.4-.8L10 2z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
  </label>
</fieldset>
```

```css
/* ── 只读展示 ── */
.rating { display: inline-flex; align-items: center; gap: var(--space-1); }
.rating__star {
  font-size: 1rem;
  line-height: 1;
  color: var(--color-border-default);
}
.rating__star--full  { color: var(--color-accent-orange); }
.rating__star--half  {
  position: relative;
  color: var(--color-border-default);
}
.rating__star--half::before {
  content: '★';
  position: absolute;
  left: 0;
  width: 50%;
  overflow: hidden;
  color: var(--color-accent-orange);
}
.rating__value {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  margin-left: var(--space-1);
  font-variant-numeric: tabular-nums;
}
.rating__count {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}

/* ── 可交互评分 ── */
.rating-input {
  display: inline-flex;
  flex-direction: row-reverse; /* 反向，CSS 兄弟选择器技巧 */
  gap: 2px;
  border: none;
  padding: 0;
  margin: 0;
}
.rating-input__item { cursor: pointer; }
.rating-input__star {
  width: 24px;
  height: 24px;
  fill: none;
  color: var(--color-border-default);
  transition: color 0.15s ease, transform 0.15s var(--ease-bounce);
}
/* 悬停：当前及之后（视觉上是之前）全部高亮 */
.rating-input__item:hover .rating-input__star,
.rating-input__item:hover ~ .rating-input__item .rating-input__star {
  color: var(--color-accent-orange);
  fill: rgba(217,119,87,0.15);
  transform: scale(1.15);
}
/* 已选中状态 */
.rating-input__item:has(input:checked) .rating-input__star,
.rating-input__item:has(input:checked) ~ .rating-input__item .rating-input__star {
  color: var(--color-accent-orange);
  fill: rgba(217,119,87,0.2);
}
```

---

## 42. Notification Dropdown 通知中心 {#notification}

```html
<!-- 触发按钮（含未读角标）-->
<div class="notif-wrap" data-notif>
  <button class="notif-trigger" aria-label="通知（3 条未读）" aria-haspopup="true" aria-expanded="false">
    <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
      <path d="M9 2a5 5 0 00-5 5v3l-1.5 2.5h13L14 10V7a5 5 0 00-5-5z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
      <path d="M7 14.5a2 2 0 004 0" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
    <span class="notif-badge" aria-hidden="true">3</span>
  </button>

  <!-- 通知面板 -->
  <div class="notif-panel" role="dialog" aria-label="通知中心" aria-hidden="true">
    <div class="notif-panel__header">
      <h4 class="notif-panel__title">通知</h4>
      <button class="btn btn-ghost" style="font-size:var(--text-xs)">全部标为已读</button>
    </div>
    <div class="notif-panel__body">
      <!-- 未读 -->
      <div class="notif-item notif-item--unread">
        <div class="notif-item__dot" aria-hidden="true"></div>
        <div class="notif-item__content">
          <p class="notif-item__title">API 用量达到 90%</p>
          <p class="notif-item__body">本月剩余额度不足，请及时升级套餐以免服务中断。</p>
          <time class="notif-item__time" datetime="2025-03-16T10:30">10 分钟前</time>
        </div>
        <button class="notif-item__action btn btn-ghost" style="font-size:var(--text-xs);white-space:nowrap">升级</button>
      </div>
      <div class="notif-item notif-item--unread">
        <div class="notif-item__dot" aria-hidden="true"></div>
        <div class="notif-item__content">
          <p class="notif-item__title">Claude 3.7 Sonnet 已发布</p>
          <p class="notif-item__body">新模型已可用，扩展思考模式支持 200K 上下文。</p>
          <time class="notif-item__time" datetime="2025-03-16T08:00">2 小时前</time>
        </div>
      </div>
      <div class="notif-panel__divider" role="separator"><span>更早</span></div>
      <!-- 已读 -->
      <div class="notif-item">
        <div class="notif-item__content">
          <p class="notif-item__title">密钥 prod-key-01 已创建</p>
          <time class="notif-item__time" datetime="2025-03-15T14:00">昨天 14:00</time>
        </div>
      </div>
    </div>
    <div class="notif-panel__footer">
      <a href="#" class="btn btn-ghost" style="font-size:var(--text-xs)">查看全部通知</a>
    </div>
  </div>
</div>
```

```css
.notif-wrap { position: relative; display: inline-flex; }

/* 触发按钮 */
.notif-trigger {
  position: relative;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-md);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
}
.notif-trigger:hover { background: var(--color-bg-raised); color: var(--color-text-primary); }

.notif-badge {
  position: absolute;
  top: 4px;
  right: 4px;
  min-width: 16px;
  height: 16px;
  padding: 0 4px;
  background: var(--color-accent-orange);
  color: white;
  font-family: var(--font-heading);
  font-size: 10px;
  font-weight: var(--weight-bold);
  line-height: 16px;
  text-align: center;
  border-radius: var(--radius-full);
  border: 2px solid var(--color-bg-base);
}

/* 通知面板 */
.notif-panel {
  position: absolute;
  top: calc(100% + var(--space-2));
  right: 0;
  width: 360px;
  max-height: 480px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-xl);
  box-shadow: 0 12px 48px rgba(20,20,19,0.14);
  z-index: var(--z-dropdown);
  display: flex;
  flex-direction: column;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-8px) scale(0.98);
  transform-origin: top right;
  transition: opacity 0.2s var(--ease-default), transform 0.2s var(--ease-default), visibility 0.2s;
}
.notif-wrap.is-open .notif-panel {
  opacity: 1;
  visibility: visible;
  transform: none;
}
.notif-panel__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4) var(--space-5);
  border-bottom: 1px solid var(--color-border-subtle);
  flex-shrink: 0;
}
.notif-panel__title {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.notif-panel__body {
  flex: 1;
  overflow-y: auto;
  overscroll-behavior: contain;
}
.notif-panel__footer {
  padding: var(--space-3) var(--space-5);
  border-top: 1px solid var(--color-border-subtle);
  text-align: center;
  flex-shrink: 0;
}

/* 通知项 */
.notif-item {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
  transition: background 0.12s ease;
  position: relative;
}
.notif-item:hover { background: var(--color-bg-raised); }
.notif-item--unread { background: rgba(217,119,87,0.03); }

.notif-item__dot {
  width: 8px;
  height: 8px;
  border-radius: var(--radius-full);
  background: var(--color-accent-orange);
  flex-shrink: 0;
  margin-top: 5px;
}
.notif-item__content { flex: 1; min-width: 0; }
.notif-item__title {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  margin: 0 0 var(--space-1);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.notif-item__body {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
  margin: 0 0 var(--space-1);
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.notif-item__time {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}
.notif-panel__divider {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-5);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}
.notif-panel__divider::before,
.notif-panel__divider::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--color-border-subtle);
}
```

```js
document.querySelectorAll('[data-notif]').forEach(wrap => {
  const trigger = wrap.querySelector('.notif-trigger');
  trigger.addEventListener('click', e => {
    e.stopPropagation();
    const open = wrap.classList.toggle('is-open');
    trigger.setAttribute('aria-expanded', open);
    wrap.querySelector('.notif-panel').setAttribute('aria-hidden', !open);
  });
  document.addEventListener('click', () => {
    wrap.classList.remove('is-open');
    trigger.setAttribute('aria-expanded', 'false');
  });
  document.addEventListener('keydown', e => {
    if (e.key === 'Escape') wrap.classList.remove('is-open');
  });
});
```

---

## ══════════════════════════════
## Chat UI 对话界面（v3 补充）
## ══════════════════════════════

