## 43. Chat UI 对话界面 {#chat-ui}

Anthropic 产品核心场景。区分用户消息与 AI 消息的视觉层级，同时保持克制——
不用气泡，用左右分列 + 背景色区分，保留大量呼吸空间。

```html
<div class="chat-layout">

  <!-- 侧边栏：历史对话 -->
  <aside class="chat-sidebar">
    <div class="chat-sidebar__header">
      <button class="btn btn-primary" style="width:100%;justify-content:center">
        <svg width="14" height="14" viewBox="0 0 14 14" fill="none" aria-hidden="true">
          <path d="M7 2v10M2 7h10" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        新建对话
      </button>
    </div>
    <nav class="chat-history" aria-label="历史对话">
      <div class="chat-history__section-label">今天</div>
      <a href="#" class="chat-history__item chat-history__item--active" aria-current="page">
        <span class="chat-history__title">宪法 AI 的设计原则</span>
        <span class="chat-history__time">14:32</span>
      </a>
      <a href="#" class="chat-history__item">
        <span class="chat-history__title">帮我写一份 API 文档</span>
        <span class="chat-history__time">10:15</span>
      </a>
      <div class="chat-history__section-label">昨天</div>
      <a href="#" class="chat-history__item">
        <span class="chat-history__title">Python 代码审查</span>
        <span class="chat-history__time">周五</span>
      </a>
    </nav>
  </aside>

  <!-- 主区域：对话内容 -->
  <main class="chat-main" aria-label="对话内容">

    <!-- 消息流 -->
    <div class="chat-messages" role="log" aria-live="polite" aria-label="对话消息">

      <!-- 用户消息 -->
      <div class="chat-msg chat-msg--user">
        <div class="chat-msg__content">
          <p>宪法 AI 是如何工作的？它和 RLHF 有什么区别？</p>
        </div>
        <div class="chat-msg__meta">
          <div class="avatar avatar--sm avatar--orange" aria-hidden="true">
            <span class="avatar__initials">你</span>
          </div>
          <time class="chat-msg__time" datetime="2025-03-17T14:32">14:32</time>
        </div>
      </div>

      <!-- AI 消息 -->
      <div class="chat-msg chat-msg--ai">
        <div class="chat-msg__meta">
          <div class="chat-msg__ai-avatar" aria-hidden="true">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M8 2L14 12H2L8 2Z" fill="var(--color-accent-orange)" opacity=".9"/>
            </svg>
          </div>
          <span class="chat-msg__sender">Claude</span>
          <time class="chat-msg__time" datetime="2025-03-17T14:32">14:32</time>
        </div>
        <div class="chat-msg__content">
          <p>宪法 AI（Constitutional AI）是 Anthropic 提出的一种训练方法，
             核心思路是让模型通过一组明确的原则（"宪法"）来自我评判和修正输出。</p>
          <p>和 RLHF 的主要区别在于：</p>
          <ul>
            <li><strong>监督信号来源</strong>：RLHF 依赖大量人工标注；宪法 AI 用模型自身评判减少人工标注量</li>
            <li><strong>可解释性</strong>：宪法 AI 的评判标准是显式的原则列表，更透明</li>
            <li><strong>扩展性</strong>：减少对人工标注的依赖，更容易规模化</li>
          </ul>
        </div>
        <!-- 工具调用展示（可选） -->
        <div class="chat-tool-use" aria-label="工具调用">
          <div class="chat-tool-use__header">
            <svg width="12" height="12" viewBox="0 0 12 12" fill="none" aria-hidden="true">
              <path d="M2 6h8M6 2l4 4-4 4" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
            <span>搜索了论文数据库</span>
          </div>
          <p class="chat-tool-use__result">找到 3 篇相关论文：Constitutional AI (Bai et al., 2022)…</p>
        </div>
        <!-- 消息操作 -->
        <div class="chat-msg__actions" role="group" aria-label="消息操作">
          <button class="chat-msg__action-btn" aria-label="复制">
            <svg width="13" height="13" viewBox="0 0 13 13" fill="none"><rect x="4" y="4" width="7" height="8" rx="1.5" stroke="currentColor" stroke-width="1.2"/><path d="M2 9V2h7" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </button>
          <button class="chat-msg__action-btn" aria-label="点赞">
            <svg width="13" height="13" viewBox="0 0 13 13" fill="none"><path d="M2 6l3-4 2 2 3-3v5l1 3H5L4 7H2V6z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
          </button>
          <button class="chat-msg__action-btn" aria-label="点踩">
            <svg width="13" height="13" viewBox="0 0 13 13" fill="none"><path d="M11 7l-3 4-2-2-3 3V7l-1-3h6l1 2h2v1z" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round"/></svg>
          </button>
          <button class="chat-msg__action-btn" aria-label="重新生成">
            <svg width="13" height="13" viewBox="0 0 13 13" fill="none"><path d="M2 6.5A4.5 4.5 0 0110 3.5M11 6.5A4.5 4.5 0 013 9.5" stroke="currentColor" stroke-width="1.2" stroke-linecap="round"/><path d="M10 1.5v2h-2M3 11.5v-2h2" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </button>
        </div>
      </div>

      <!-- 加载中（流式输出）-->
      <div class="chat-msg chat-msg--ai chat-msg--streaming" aria-label="AI 正在回复" aria-busy="true">
        <div class="chat-msg__meta">
          <div class="chat-msg__ai-avatar" aria-hidden="true">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M8 2L14 12H2L8 2Z" fill="var(--color-accent-orange)" opacity=".9"/>
            </svg>
          </div>
          <span class="chat-msg__sender">Claude</span>
        </div>
        <div class="chat-msg__content">
          <span class="chat-typing-indicator" aria-hidden="true">
            <span></span><span></span><span></span>
          </span>
        </div>
      </div>

    </div><!-- /chat-messages -->

    <!-- 输入区 -->
    <div class="chat-input-area">
      <div class="chat-input-wrap">
        <!-- 附件按钮 -->
        <button class="chat-input-btn" aria-label="上传附件">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
            <path d="M14 8l-5.5 5.5a4 4 0 01-5.66-5.66L9 2.34a2.5 2.5 0 013.54 3.54L6.41 12a1 1 0 01-1.41-1.41L11 4.5" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <!-- 文本输入 -->
        <textarea
          class="chat-textarea"
          placeholder="给 Claude 发送消息…"
          rows="1"
          aria-label="消息输入框"
          aria-multiline="true"
        ></textarea>
        <!-- 发送按钮 -->
        <button class="chat-send-btn" aria-label="发送消息" disabled>
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
            <path d="M14 8H2M14 8L8 2M14 8L8 14" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
      <p class="chat-input-hint">Claude 可能会出错，请核实重要信息</p>
    </div>

  </main><!-- /chat-main -->
</div>
```

```css
/* ── 整体布局 ── */
.chat-layout {
  display: flex;
  height: 100vh;
  background: var(--color-bg-base);
  overflow: hidden;
}

/* ── 左侧边栏 ── */
.chat-sidebar {
  width: 260px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  background: var(--color-bg-raised);
  border-right: 1px solid var(--color-border-subtle);
  padding: var(--space-4);
  overflow-y: auto;
}
.chat-sidebar__header {
  padding-bottom: var(--space-4);
  border-bottom: 1px solid var(--color-border-subtle);
  margin-bottom: var(--space-4);
}

/* 历史记录 */
.chat-history { display: flex; flex-direction: column; gap: 2px; }
.chat-history__section-label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--color-text-muted);
  padding: var(--space-4) var(--space-2) var(--space-2);
}
.chat-history__item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-md);
  text-decoration: none;
  transition: background 0.12s ease;
}
.chat-history__item:hover { background: var(--color-bg-overlay); }
.chat-history__item--active { background: rgba(217,119,87,0.1); }
.chat-history__title {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  flex: 1;
}
.chat-history__item--active .chat-history__title {
  color: var(--color-accent-warm);
  font-weight: var(--weight-medium);
}
.chat-history__time {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  flex-shrink: 0;
}

/* ── 主对话区 ── */
.chat-main {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

/* 消息流容器 */
.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: var(--space-8) var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-8);
  overscroll-behavior: contain;
  scroll-behavior: smooth;
  /* 自定义滚动条 */
  scrollbar-width: thin;
  scrollbar-color: var(--color-border-default) transparent;
}

/* ── 消息气泡（无气泡风格）── */
.chat-msg {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  max-width: 720px;
  width: 100%;
}

/* 用户消息：右对齐 */
.chat-msg--user {
  align-self: flex-end;
  align-items: flex-end;
}
.chat-msg--user .chat-msg__content {
  background: var(--color-bg-inverted);
  color: var(--color-text-inverted);
  border-radius: var(--radius-xl) var(--radius-xl) var(--radius-sm) var(--radius-xl);
  padding: var(--space-4) var(--space-5);
  font-family: var(--font-heading);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  max-width: 560px;
}
.chat-msg--user .chat-msg__meta {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  flex-direction: row-reverse;
}

/* AI 消息：左对齐 */
.chat-msg--ai {
  align-self: flex-start;
}
.chat-msg--ai .chat-msg__content {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-loose);
  color: var(--color-text-primary);
}
.chat-msg--ai .chat-msg__content p { margin: 0 0 var(--space-4); }
.chat-msg--ai .chat-msg__content p:last-child { margin-bottom: 0; }
.chat-msg--ai .chat-msg__content ul,
.chat-msg--ai .chat-msg__content ol {
  padding-left: var(--space-5);
  margin: 0 0 var(--space-4);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
.chat-msg--ai .chat-msg__content li {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  color: var(--color-text-secondary);
  list-style: disc;
}
.chat-msg--ai .chat-msg__content strong {
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.chat-msg--ai .chat-msg__meta {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  margin-bottom: var(--space-3);
}

/* 共用元数据 */
.chat-msg__ai-avatar {
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(217,119,87,0.1);
  border-radius: var(--radius-sm);
  flex-shrink: 0;
}
.chat-msg__sender {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.chat-msg__time {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}

/* 工具调用展示 */
.chat-tool-use {
  margin-top: var(--space-3);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-md);
  border-left: 3px solid var(--color-accent-sand);
}
.chat-tool-use__header {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  color: var(--color-text-muted);
  margin-bottom: var(--space-2);
}
.chat-tool-use__result {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  margin: 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* 消息操作按钮 */
.chat-msg__actions {
  display: flex;
  gap: var(--space-1);
  opacity: 0;
  transition: opacity 0.15s ease;
  margin-top: var(--space-2);
}
.chat-msg:hover .chat-msg__actions { opacity: 1; }
.chat-msg__action-btn {
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-sm);
  color: var(--color-text-muted);
  cursor: pointer;
  transition: background 0.12s ease, color 0.12s ease;
}
.chat-msg__action-btn:hover {
  background: var(--color-bg-raised);
  color: var(--color-text-primary);
}

/* 流式输出等待动画 */
.chat-typing-indicator {
  display: inline-flex;
  gap: 4px;
  align-items: center;
  height: 20px;
}
.chat-typing-indicator span {
  width: 6px;
  height: 6px;
  border-radius: var(--radius-full);
  background: var(--color-accent-orange);
  animation: typing-bounce 1.2s ease-in-out infinite;
}
.chat-typing-indicator span:nth-child(2) { animation-delay: 0.2s; }
.chat-typing-indicator span:nth-child(3) { animation-delay: 0.4s; }
@keyframes typing-bounce {
  0%, 60%, 100% { transform: translateY(0);    opacity: 0.5; }
  30%            { transform: translateY(-5px); opacity: 1; }
}

/* ── 输入区 ── */
.chat-input-area {
  flex-shrink: 0;
  padding: var(--space-4) var(--space-6) var(--space-6);
  border-top: 1px solid var(--color-border-subtle);
  background: var(--color-bg-base);
}
.chat-input-wrap {
  display: flex;
  align-items: flex-end;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-4);
  background: var(--color-bg-overlay);
  border: 1.5px solid var(--color-border-default);
  border-radius: var(--radius-xl);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.chat-input-wrap:focus-within {
  border-color: var(--color-accent-orange);
  box-shadow: 0 0 0 3px rgba(217,119,87,0.12);
}
.chat-input-btn {
  flex-shrink: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-md);
  color: var(--color-text-muted);
  cursor: pointer;
  transition: background 0.12s ease, color 0.12s ease;
}
.chat-input-btn:hover { background: var(--color-bg-raised); color: var(--color-text-primary); }

.chat-textarea {
  flex: 1;
  font-family: var(--font-heading);
  font-size: var(--text-base);
  color: var(--color-text-primary);
  background: transparent;
  border: none;
  outline: none;
  resize: none;
  line-height: var(--leading-normal);
  max-height: 200px;
  overflow-y: auto;
  padding: var(--space-1) 0;
  scrollbar-width: thin;
}
.chat-textarea::placeholder { color: var(--color-text-muted); }

.chat-send-btn {
  flex-shrink: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-md);
  background: var(--color-accent-orange);
  color: white;
  cursor: pointer;
  transition: background 0.15s ease, transform 0.15s var(--ease-bounce), opacity 0.15s ease;
}
.chat-send-btn:hover:not(:disabled) {
  background: var(--color-accent-warm);
  transform: scale(1.05);
}
.chat-send-btn:disabled {
  background: var(--color-border-default);
  cursor: not-allowed;
  opacity: 0.5;
}

.chat-input-hint {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  text-align: center;
  margin: var(--space-2) 0 0;
}

/* ── 移动端适配 ── */
@media (max-width: 768px) {
  .chat-sidebar { display: none; } /* 移动端隐藏侧边栏，改为 Drawer */
  .chat-messages { padding: var(--space-4); gap: var(--space-6); }
  .chat-msg--user .chat-msg__content { max-width: 100%; }
  .chat-input-area { padding: var(--space-3) var(--space-4) var(--space-4); }
}
```

```js
// 自动撑高 textarea
document.querySelector('.chat-textarea')?.addEventListener('input', function() {
  this.style.height = 'auto';
  this.style.height = Math.min(this.scrollHeight, 200) + 'px';
  // 有内容时激活发送按钮
  document.querySelector('.chat-send-btn').disabled = !this.value.trim();
});

// 消息流自动滚动到底部
function scrollToBottom() {
  const messages = document.querySelector('.chat-messages');
  if (messages) messages.scrollTop = messages.scrollHeight;
}
```
