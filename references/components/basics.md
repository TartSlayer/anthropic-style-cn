# 组件完整示例库

## 目录
1. [Hero 区块](#hero)
2. [特性卡片网格](#feature-grid)
3. [统计数字展示](#stats)
4. [引用块 Blockquote](#blockquote)
5. [价格卡片](#pricing)
6. [深色 CTA 区块](#cta-dark)
7. [页脚 Footer](#footer)
8. [代码块](#code-block)
9. [通知/提示条 Toast](#toast)
10. [加载骨架屏](#skeleton)

---

## 1. Hero 区块 {#hero}

```html
<section class="hero">
  <div class="page-container">
    <div class="hero__inner">
      <div class="badge badge-orange reveal">
        <span>新功能</span>
        <span>Claude 3.7 已上线</span>
      </div>
      <h1 class="hero__title reveal">
        为人类价值观而生的<br>
        <em>人工智能</em>
      </h1>
      <p class="hero__body reveal">
        我们相信 AI 的未来应当是安全的、可解释的，
        并且真正为所有人带来益处。
      </p>
      <div class="hero__actions reveal">
        <a href="#" class="btn btn-primary">开始使用</a>
        <a href="#" class="btn btn-secondary">了解我们</a>
      </div>
    </div>
  </div>
</section>
```

```css
.hero {
  padding-block: clamp(80px, 12vw, 160px);
  background:
    radial-gradient(ellipse 80% 50% at 30% 0%, rgba(217,119,87,0.07) 0%, transparent 55%),
    var(--color-bg-base);
  overflow: hidden;
}
.hero__inner {
  max-width: 720px;
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}
.hero__title {
  font-family: var(--font-display);
  font-size: clamp(2.75rem, 5.5vw, 4.5rem);
  font-weight: 300;
  line-height: 1.1;
  letter-spacing: -0.025em;
  color: var(--color-text-primary);
  text-wrap: balance;
}
.hero__title em {
  font-style: italic;
  color: var(--color-accent-orange);
}
.hero__body {
  font-family: var(--font-body);
  font-size: clamp(1.05rem, 1.8vw, 1.2rem);
  line-height: 1.65;
  color: var(--color-text-secondary);
  max-width: 54ch;
}
.hero__actions {
  display: flex;
  gap: var(--space-3);
  flex-wrap: wrap;
}
```

---

## 2. 特性卡片网格 {#feature-grid}

```html
<section class="features">
  <div class="page-container">
    <div class="section-label">核心能力</div>
    <h2 class="section-title">专为可靠而设计</h2>
    <div class="features__grid">
      <article class="feature-card scroll-reveal">
        <div class="feature-card__icon">
          <svg><!-- 图标 --></svg>
        </div>
        <h3 class="feature-card__title">宪法 AI 训练</h3>
        <p class="feature-card__body">通过原则导向的训练方法，使模型天然倾向于安全和诚实的回答。</p>
        <a class="feature-card__link" href="#">了解详情 →</a>
      </article>
      <!-- 重复 2–5 个 -->
    </div>
  </div>
</section>
```

```css
.features { padding-block: var(--space-32); }

.section-label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--color-accent-orange);
  margin-bottom: var(--space-4);
}
.section-title {
  font-family: var(--font-display);
  font-size: clamp(1.75rem, 3vw, 2.75rem);
  font-weight: 400;
  color: var(--color-text-primary);
  max-width: 22ch;
  margin-bottom: var(--space-12);
  text-wrap: balance;
}

.features__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: var(--space-6);
}
.feature-card {
  padding: var(--space-8);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
  display: flex;
  flex-direction: column;
  gap: var(--space-4);
  transition: transform 0.25s var(--ease-default), box-shadow 0.25s ease;
}
.feature-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 40px rgba(20,20,19,0.07);
}
.feature-card__icon {
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(217,119,87,0.1);
  color: var(--color-accent-orange);
  border-radius: var(--radius-md);
}
.feature-card__title {
  font-family: var(--font-heading);
  font-size: var(--text-lg);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
}
.feature-card__body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  color: var(--color-text-secondary);
  flex: 1;
}
.feature-card__link {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-accent-orange);
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
}
```

---

## 3. 统计数字展示 {#stats}

```html
<section class="stats-bar">
  <div class="page-container">
    <dl class="stats-bar__grid">
      <div class="stat-item">
        <dt class="stat-item__label">融资规模</dt>
        <dd class="stat-item__value">$7.3B+</dd>
      </div>
      <div class="stat-divider"></div>
      <div class="stat-item">
        <dt class="stat-item__label">研究团队</dt>
        <dd class="stat-item__value">800+</dd>
      </div>
      <div class="stat-divider"></div>
      <div class="stat-item">
        <dt class="stat-item__label">月活用户</dt>
        <dd class="stat-item__value">数千万</dd>
      </div>
    </dl>
  </div>
</section>
```

```css
.stats-bar {
  padding-block: var(--space-16);
  border-top: 1px solid var(--color-border-subtle);
  border-bottom: 1px solid var(--color-border-subtle);
}
.stats-bar__grid {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-12);
  flex-wrap: wrap;
}
.stat-item {
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
.stat-item__label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}
.stat-item__value {
  font-family: var(--font-display);
  font-size: clamp(2rem, 3.5vw, 3rem);
  font-weight: 300;
  color: var(--color-text-primary);
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.02em;
}
.stat-divider {
  width: 1px;
  height: 48px;
  background: var(--color-border-default);
}
```

---

## 4. 引用块 Blockquote {#blockquote}

```html
<figure class="blockquote-card">
  <blockquote>
    <p>我们相信，了解 AI 风险并努力解决它的人，
    才是最有可能构建出安全 AI 系统的人。</p>
  </blockquote>
  <figcaption class="blockquote-card__attribution">
    <img src="avatar.jpg" alt="Dario Amodei" class="blockquote-card__avatar">
    <div>
      <div class="blockquote-card__name">Dario Amodei</div>
      <div class="blockquote-card__role">CEO & 联合创始人</div>
    </div>
  </figcaption>
</figure>
```

```css
.blockquote-card {
  position: relative;
  padding: var(--space-10) var(--space-10) var(--space-8);
  background: var(--color-bg-inverted);
  border-radius: var(--radius-xl);
  overflow: hidden;
}
.blockquote-card::before {
  content: '\201C';
  position: absolute;
  top: -20px;
  left: 32px;
  font-family: var(--font-display);
  font-size: 160px;
  color: rgba(217,119,87,0.15);
  line-height: 1;
  pointer-events: none;
}
.blockquote-card blockquote p {
  font-family: var(--font-display);
  font-size: clamp(1.2rem, 2vw, 1.6rem);
  font-weight: 300;
  line-height: var(--leading-snug);
  color: var(--color-text-inverted);
  margin: 0 0 var(--space-8);
  text-wrap: balance;
}
.blockquote-card__attribution {
  display: flex;
  align-items: center;
  gap: var(--space-4);
}
.blockquote-card__avatar {
  width: 44px;
  height: 44px;
  border-radius: var(--radius-full);
  border: 2px solid rgba(217,119,87,0.4);
  object-fit: cover;
}
.blockquote-card__name {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  font-weight: var(--weight-medium);
  color: var(--color-text-inverted);
}
.blockquote-card__role {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: rgba(250,249,245,0.5);
}
```

---

## 5. 价格卡片 {#pricing}

```html
<div class="pricing-card pricing-card--featured">
  <div class="pricing-card__header">
    <span class="badge badge-orange">最受欢迎</span>
    <h3 class="pricing-card__name">Pro</h3>
    <div class="pricing-card__price">
      <span class="pricing-card__amount">$20</span>
      <span class="pricing-card__period">/ 月</span>
    </div>
    <p class="pricing-card__desc">适合个人专业用户和创作者</p>
  </div>
  <ul class="pricing-card__features">
    <li><span class="check-icon">✓</span> 无限制使用 Claude Sonnet</li>
    <li><span class="check-icon">✓</span> 优先访问新功能</li>
    <li><span class="check-icon">✓</span> 100K token 上下文窗口</li>
  </ul>
  <a href="#" class="btn btn-primary" style="width:100%;justify-content:center">开始 14 天免费试用</a>
</div>
```

```css
.pricing-card {
  padding: var(--space-8);
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-xl);
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}
.pricing-card--featured {
  background: var(--color-bg-inverted);
  border-color: rgba(217,119,87,0.3);
  color: var(--color-text-inverted);
  box-shadow: 0 20px 60px rgba(20,20,19,0.2);
  transform: scale(1.02);
}
.pricing-card__name {
  font-family: var(--font-heading);
  font-size: var(--text-xl);
  font-weight: var(--weight-medium);
}
.pricing-card__amount {
  font-family: var(--font-display);
  font-size: 3rem;
  font-weight: 300;
  letter-spacing: -0.03em;
  font-variant-numeric: tabular-nums;
}
.pricing-card__period {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  color: var(--color-text-muted);
}
.pricing-card__features {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  flex: 1;
}
.pricing-card__features li {
  display: flex;
  gap: var(--space-3);
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
}
.pricing-card--featured .pricing-card__features li {
  color: rgba(250,249,245,0.7);
}
.check-icon { color: var(--color-accent-orange); flex-shrink: 0; }
```

---

## 6. 深色 CTA 区块 {#cta-dark}

```html
<section class="cta-dark">
  <div class="page-container">
    <div class="cta-dark__inner">
      <h2 class="cta-dark__title">准备好了吗？</h2>
      <p class="cta-dark__body">加入数百万用户，体验不一样的 AI 对话。</p>
      <div class="cta-dark__actions">
        <a href="#" class="btn btn-primary">免费开始使用</a>
        <a href="#" class="btn" style="color:var(--color-text-inverted);border-color:rgba(250,249,245,0.3)">联系销售</a>
      </div>
    </div>
  </div>
</section>
```

```css
.cta-dark {
  padding-block: var(--space-32);
  background:
    radial-gradient(ellipse 80% 60% at 50% 100%, rgba(217,119,87,0.1) 0%, transparent 60%),
    var(--color-bg-inverted);
}
.cta-dark__inner {
  text-align: center;
  max-width: 600px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-6);
}
.cta-dark__title {
  font-family: var(--font-display);
  font-size: clamp(2rem, 4vw, 3.5rem);
  font-weight: 300;
  line-height: var(--leading-tight);
  color: var(--color-text-inverted);
  text-wrap: balance;
}
.cta-dark__body {
  font-family: var(--font-body);
  font-size: var(--text-md);
  color: rgba(250,249,245,0.6);
  line-height: var(--leading-normal);
}
.cta-dark__actions {
  display: flex;
  gap: var(--space-3);
  flex-wrap: wrap;
  justify-content: center;
}
```

---

## 7. 代码块 {#code-block}

```html
<div class="code-block">
  <div class="code-block__header">
    <span class="code-block__lang">Python</span>
    <button class="code-block__copy" onclick="copyCode(this)">复制</button>
  </div>
  <pre class="code-block__pre"><code class="code-block__code">import anthropic

client = anthropic.Anthropic()
message = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello, Claude"}]
)
print(message.content)</code></pre>
</div>
```

```css
.code-block {
  border-radius: var(--radius-lg);
  overflow: hidden;
  border: 1px solid var(--color-border-default);
  background: #1E1D19;
}
.code-block__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-5);
  background: #252420;
  border-bottom: 1px solid rgba(255,255,255,0.07);
}
.code-block__lang {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-accent-orange);
  letter-spacing: 0.05em;
}
.code-block__copy {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: rgba(250,249,245,0.5);
  background: transparent;
  border: 1px solid rgba(250,249,245,0.15);
  border-radius: var(--radius-sm);
  padding: 2px 10px;
  cursor: pointer;
  transition: color 0.15s ease, border-color 0.15s ease;
}
.code-block__copy:hover {
  color: var(--color-text-inverted);
  border-color: rgba(250,249,245,0.4);
}
.code-block__pre {
  padding: var(--space-5);
  margin: 0;
  overflow-x: auto;
}
.code-block__code {
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: 1.7;
  color: #C9C5B8;
}
```

---

## 8. Toast 通知 {#toast}

```html
<div class="toast toast--success" role="alert" aria-live="polite">
  <svg class="toast__icon"><!-- 图标 --></svg>
  <p class="toast__message">操作成功完成</p>
  <button class="toast__close" aria-label="关闭">×</button>
</div>
```

```css
.toast {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-4) var(--space-5);
  background: var(--color-bg-overlay);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  box-shadow: 0 8px 32px rgba(20,20,19,0.12);
  min-width: 280px;
  max-width: 420px;
  animation: slideInRight 0.3s var(--ease-default);
}
@keyframes slideInRight {
  from { opacity: 0; transform: translateX(24px); }
  to   { opacity: 1; transform: translateX(0); }
}
.toast--success { border-left: 3px solid var(--color-success); }
.toast--error   { border-left: 3px solid var(--color-error); }
.toast--warning { border-left: 3px solid var(--color-warning); }
.toast__message {
  font-family: var(--font-heading);
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  flex: 1;
}
.toast__close {
  background: none;
  border: none;
  color: var(--color-text-muted);
  font-size: 1.2rem;
  cursor: pointer;
  padding: 0 var(--space-1);
  line-height: 1;
}
```

---

## 9. 加载骨架屏 {#skeleton}

```html
<div class="skeleton-card" aria-busy="true" aria-label="加载中">
  <div class="skeleton skeleton--image"></div>
  <div class="skeleton skeleton--title"></div>
  <div class="skeleton skeleton--text"></div>
  <div class="skeleton skeleton--text" style="width:70%"></div>
</div>
```

```css
@keyframes shimmer {
  0%   { background-position: -600px 0; }
  100% { background-position:  600px 0; }
}
.skeleton {
  border-radius: var(--radius-md);
  background:
    linear-gradient(
      90deg,
      var(--color-border-subtle) 25%,
      var(--color-bg-raised) 50%,
      var(--color-border-subtle) 75%
    );
  background-size: 600px 100%;
  animation: shimmer 1.5s infinite linear;
}
.skeleton--image  { height: 180px; margin-bottom: var(--space-4); border-radius: var(--radius-lg); }
.skeleton--title  { height: 24px; width: 60%; margin-bottom: var(--space-3); }
.skeleton--text   { height: 16px; margin-bottom: var(--space-2); }
```

---

## ══════════════════════════════
## 导航与结构类
## ══════════════════════════════

