## 16. Form 完整表单 {#form}

```html
<form class="form" novalidate>
  <div class="form__header">
    <h2 class="form__title">创建 API 密钥</h2>
    <p class="form__subtitle">填写以下信息以生成新的访问密钥</p>
  </div>

  <div class="form__body">
    <!-- 单行输入 -->
    <div class="form__field">
      <label class="form__label" for="key-name">
        密钥名称
        <span class="form__required" aria-hidden="true">*</span>
      </label>
      <input
        class="input"
        type="text"
        id="key-name"
        name="keyName"
        placeholder="如：生产环境主密钥"
        required
        autocomplete="off"
      >
      <p class="form__hint">仅用于标识，不影响权限</p>
    </div>

    <!-- 下拉选择 -->
    <div class="form__field">
      <label class="form__label" for="permission">权限级别</label>
      <div class="form__select-wrapper">
        <select class="input form__select" id="permission" name="permission">
          <option value="">请选择权限</option>
          <option value="read">只读</option>
          <option value="write">读写</option>
          <option value="admin">管理员</option>
        </select>
        <svg class="form__select-arrow" width="12" height="12" viewBox="0 0 12 12" fill="none" aria-hidden="true">
          <path d="M2 4l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
    </div>

    <!-- 多行文本 -->
    <div class="form__field">
      <label class="form__label" for="description">备注说明</label>
      <textarea class="input form__textarea" id="description" name="description" rows="3" placeholder="可选，记录用途或注意事项"></textarea>
    </div>

    <!-- 复选框 -->
    <div class="form__field">
      <label class="form__checkbox-label">
        <input type="checkbox" class="form__checkbox" name="agree" required>
        <span class="form__checkbox-custom" aria-hidden="true"></span>
        我已阅读并同意 <a href="#" class="form__link">API 使用条款</a>
      </label>
    </div>

    <!-- 错误提示（有错误时显示） -->
    <div class="form__error-banner" role="alert" hidden>
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
        <circle cx="8" cy="8" r="7" stroke="currentColor" stroke-width="1.5"/>
        <path d="M8 5v4M8 11v.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      请检查并修正上方标红的字段
    </div>
  </div>

  <div class="form__footer">
    <button type="button" class="btn btn-secondary">取消</button>
    <button type="submit" class="btn btn-primary">生成密钥</button>
  </div>
</form>
```

```css
.form {
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-xl);
  overflow: hidden;
  max-width: 520px;
}
.form__header {
  padding: var(--space-8) var(--space-8) 0;
}
.form__title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 400;
  color: var(--color-text-primary);
  margin-bottom: var(--space-2);
}
.form__subtitle {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
}
.form__body {
  padding: var(--space-6) var(--space-8);
  display: flex;
  flex-direction: column;
  gap: var(--space-5);
}
.form__field { display: flex; flex-direction: column; gap: var(--space-2); }

.form__label {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  display: flex;
  align-items: center;
  gap: var(--space-1);
}
.form__required { color: var(--color-accent-orange); }
.form__hint {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin: 0;
}

/* 输入框错误态 */
.input--error {
  border-color: var(--color-error);
  box-shadow: 0 0 0 3px rgba(192,69,58,0.12);
}
.form__field-error {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-error);
}

/* Select */
.form__select-wrapper {
  position: relative;
}
.form__select {
  cursor: pointer;
  padding-right: var(--space-8);
  appearance: none;
}
.form__select-arrow {
  position: absolute;
  right: var(--space-4);
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  color: var(--color-text-muted);
}

/* Textarea */
.form__textarea {
  resize: vertical;
  min-height: 88px;
  font-family: var(--font-heading);
  line-height: var(--leading-normal);
}

/* Checkbox */
.form__checkbox-label {
  display: flex;
  align-items: flex-start;
  gap: var(--space-3);
  cursor: pointer;
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
}
.form__checkbox { position: absolute; opacity: 0; width: 0; height: 0; }
.form__checkbox-custom {
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  margin-top: 1px;
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-sm);
  background: var(--color-bg-overlay);
  transition: all 0.15s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}
.form__checkbox:checked + .form__checkbox-custom {
  background: var(--color-accent-orange);
  border-color: var(--color-accent-orange);
}
.form__checkbox:checked + .form__checkbox-custom::after {
  content: '';
  width: 9px;
  height: 5px;
  border-left: 2px solid white;
  border-bottom: 2px solid white;
  transform: rotate(-45deg) translateY(-1px);
}
.form__link {
  color: var(--color-text-link);
  text-decoration: underline;
  text-decoration-color: rgba(201,100,66,0.4);
}

/* 错误横幅 */
.form__error-banner {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: rgba(192,69,58,0.08);
  border: 1px solid rgba(192,69,58,0.25);
  border-radius: var(--radius-md);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-error);
}

/* Footer */
.form__footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: var(--space-5) var(--space-8);
  border-top: 1px solid var(--color-border-subtle);
  background: var(--color-bg-base);
}
```

---

## 17. Toggle / Switch 开关 {#toggle}

```html
<!-- 独立 Toggle -->
<label class="toggle" aria-label="启用深色模式">
  <input type="checkbox" class="toggle__input" role="switch" aria-checked="false">
  <span class="toggle__track" aria-hidden="true">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label">深色模式</span>
</label>

<!-- 带描述的 Toggle 行 -->
<div class="toggle-row">
  <div class="toggle-row__info">
    <div class="toggle-row__title">邮件通知</div>
    <div class="toggle-row__desc">接收模型状态和账单的邮件提醒</div>
  </div>
  <label class="toggle" aria-label="邮件通知">
    <input type="checkbox" class="toggle__input" role="switch" checked aria-checked="true">
    <span class="toggle__track" aria-hidden="true">
      <span class="toggle__thumb"></span>
    </span>
  </label>
</div>
```

```css
.toggle {
  display: inline-flex;
  align-items: center;
  gap: var(--space-3);
  cursor: pointer;
  user-select: none;
}
.toggle__input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}
.toggle__track {
  position: relative;
  width: 40px;
  height: 22px;
  background: var(--color-border-default);
  border-radius: var(--radius-full);
  flex-shrink: 0;
  transition: background 0.2s ease;
}
.toggle__input:checked ~ .toggle__track {
  background: var(--color-accent-orange);
}
.toggle__thumb {
  position: absolute;
  top: 3px;
  left: 3px;
  width: 16px;
  height: 16px;
  background: white;
  border-radius: var(--radius-full);
  box-shadow: 0 1px 4px rgba(20,20,19,0.2);
  transition: transform 0.2s var(--ease-bounce);
}
.toggle__input:checked ~ .toggle__track .toggle__thumb {
  transform: translateX(18px);
}
.toggle__input:focus-visible ~ .toggle__track {
  outline: 2px solid var(--color-accent-orange);
  outline-offset: 2px;
}
.toggle__label {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
}

/* 整行 Toggle 布局 */
.toggle-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-6);
  padding: var(--space-4) 0;
  border-bottom: 1px solid var(--color-border-subtle);
}
.toggle-row__title {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.toggle-row__desc {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin-top: var(--space-1);
}
```

---

## 18. Tooltip 气泡提示 {#tooltip}

```html
<!-- CSS-only Tooltip（简单场景，无需 JS） -->
<span class="tooltip-wrap">
  <button class="btn btn-secondary">
    高级选项
    <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
      <circle cx="7" cy="7" r="6" stroke="currentColor" stroke-width="1.5"/>
      <path d="M7 6v4M7 4.5v.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
    </svg>
  </button>
  <span class="tooltip" role="tooltip">
    包含实验性参数，修改前请阅读文档
  </span>
</span>

<!-- Tooltip 方向变体：只需加 modifier class -->
<!-- .tooltip--top（默认）/ .tooltip--bottom / .tooltip--left / .tooltip--right -->
```

```css
.tooltip-wrap {
  position: relative;
  display: inline-flex;
}
.tooltip {
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%) translateY(4px);
  z-index: 300;
  min-width: 160px;
  max-width: 240px;
  padding: var(--space-2) var(--space-3);
  background: var(--color-bg-inverted);
  color: var(--color-text-inverted);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  line-height: var(--leading-normal);
  border-radius: var(--radius-md);
  white-space: normal;
  text-align: center;
  pointer-events: none;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.18s ease, transform 0.18s var(--ease-default), visibility 0.18s;
  box-shadow: 0 4px 16px rgba(20,20,19,0.2);
}
/* 小箭头 */
.tooltip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 5px solid transparent;
  border-top-color: var(--color-bg-inverted);
}
.tooltip-wrap:hover .tooltip,
.tooltip-wrap:focus-within .tooltip {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(0);
}

/* 底部变体 */
.tooltip--bottom {
  bottom: auto;
  top: calc(100% + 8px);
  transform: translateX(-50%) translateY(-4px);
}
.tooltip--bottom::after {
  top: auto;
  bottom: 100%;
  border-top-color: transparent;
  border-bottom-color: var(--color-bg-inverted);
}
.tooltip-wrap:hover .tooltip--bottom { transform: translateX(-50%) translateY(0); }
```

---

## 19. Modal 弹窗 {#modal}

```html
<!-- 触发按钮 -->
<button class="btn btn-primary" onclick="openModal('confirm-modal')">删除模型</button>

<!-- 弹窗 -->
<div class="modal-overlay" id="confirm-modal" role="dialog" aria-modal="true" aria-labelledby="modal-title" hidden>
  <div class="modal">
    <div class="modal__header">
      <h3 class="modal__title" id="modal-title">确认删除</h3>
      <button class="modal__close" aria-label="关闭" onclick="closeModal('confirm-modal')">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <path d="M3 3l10 10M13 3L3 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
    </div>
    <div class="modal__body">
      <p>你即将删除 <strong>Claude 3.7 Sonnet</strong>，此操作不可撤销，所有关联的 API 调用记录将被清除。</p>
    </div>
    <div class="modal__footer">
      <button class="btn btn-secondary" onclick="closeModal('confirm-modal')">取消</button>
      <button class="btn" style="background:var(--color-error);color:white;border-color:var(--color-error)">确认删除</button>
    </div>
  </div>
</div>
```

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: 400;
  background: rgba(20, 20, 19, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-6);
  animation: fadeIn 0.2s ease;
}
.modal-overlay[hidden] { display: none; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

.modal {
  width: 100%;
  max-width: 480px;
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-xl);
  box-shadow: 0 24px 80px rgba(20,20,19,0.25);
  animation: modalUp 0.25s var(--ease-default);
}
@keyframes modalUp {
  from { opacity: 0; transform: translateY(20px) scale(0.97); }
  to   { opacity: 1; transform: none; }
}

.modal__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-6) var(--space-6) 0;
}
.modal__title {
  font-family: var(--font-display);
  font-size: var(--text-xl);
  font-weight: 400;
  color: var(--color-text-primary);
}
.modal__close {
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
.modal__close:hover {
  background: var(--color-bg-raised);
  color: var(--color-text-primary);
}
.modal__body {
  padding: var(--space-5) var(--space-6);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
}
.modal__body strong { font-weight: var(--weight-medium); color: var(--color-text-primary); }
.modal__footer {
  display: flex;
  justify-content: flex-end;
  gap: var(--space-3);
  padding: 0 var(--space-6) var(--space-6);
}
```

```js
function openModal(id) {
  const overlay = document.getElementById(id);
  overlay.removeAttribute('hidden');
  document.body.style.overflow = 'hidden';
  // 焦点管理
  overlay.querySelector('button')?.focus();
}
function closeModal(id) {
  document.getElementById(id).setAttribute('hidden', '');
  document.body.style.overflow = '';
}
// Esc 键关闭
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    document.querySelectorAll('.modal-overlay:not([hidden])').forEach(m => {
      m.setAttribute('hidden', '');
      document.body.style.overflow = '';
    });
  }
});
// 点击遮罩关闭
document.querySelectorAll('.modal-overlay').forEach(overlay => {
  overlay.addEventListener('click', e => {
    if (e.target === overlay) closeModal(overlay.id);
  });
});
```

---

## 20. Accordion 折叠面板 {#accordion}

```html
<div class="accordion" role="list">
  <div class="accordion__item" role="listitem">
    <button class="accordion__trigger" aria-expanded="true" aria-controls="acc-1">
      Claude 如何保护用户隐私？
      <svg class="accordion__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
        <path d="M4 6l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <div class="accordion__panel" id="acc-1" role="region">
      <div class="accordion__body">
        我们不会将用户对话用于训练模型，所有数据传输均经过加密处理。
        你可以在隐私设置中查看和删除历史记录。
      </div>
    </div>
  </div>

  <div class="accordion__item" role="listitem">
    <button class="accordion__trigger" aria-expanded="false" aria-controls="acc-2">
      API 速率限制是多少？
      <svg class="accordion__icon" width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
        <path d="M4 6l4 4 4-4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <div class="accordion__panel accordion__panel--collapsed" id="acc-2" role="region">
      <div class="accordion__body">
        免费层每分钟 5 次请求，Pro 层每分钟 50 次，企业层可联系我们定制。
      </div>
    </div>
  </div>
</div>
```

```css
.accordion {
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
  overflow: hidden;
}
.accordion__item + .accordion__item {
  border-top: 1px solid var(--color-border-subtle);
}
.accordion__trigger {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-4);
  padding: var(--space-5) var(--space-6);
  background: var(--color-bg-raised);
  text-align: left;
  font-family: var(--font-heading);
  font-size: var(--text-base);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  cursor: pointer;
  transition: background 0.15s ease;
}
.accordion__trigger:hover { background: var(--color-bg-overlay); }
.accordion__icon {
  flex-shrink: 0;
  color: var(--color-text-muted);
  transition: transform 0.25s var(--ease-default);
}
.accordion__trigger[aria-expanded="true"] .accordion__icon {
  transform: rotate(180deg);
}
.accordion__panel {
  overflow: hidden;
  max-height: 400px;
  transition: max-height 0.35s var(--ease-default);
}
.accordion__panel--collapsed {
  max-height: 0;
}
.accordion__body {
  padding: 0 var(--space-6) var(--space-5);
  font-family: var(--font-body);
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  line-height: var(--leading-normal);
}
```

```js
document.querySelectorAll('.accordion__trigger').forEach(trigger => {
  trigger.addEventListener('click', () => {
    const expanded = trigger.getAttribute('aria-expanded') === 'true';
    trigger.setAttribute('aria-expanded', !expanded);
    const panel = document.getElementById(trigger.getAttribute('aria-controls'));
    panel.classList.toggle('accordion__panel--collapsed', expanded);
  });
});
```

---

## ══════════════════════════════
## 内容展示类
## ══════════════════════════════

