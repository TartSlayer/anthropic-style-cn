# Dashboard 数据密集界面设计规范

数据密集场景是 Anthropic 风格最容易「失守」的地方——
因为数据本身会带来视觉复杂度，稍不注意就会滑向 Ant Design 或 Material 风格。
本文件定义在高信息密度场景下如何守住品牌调性。

---

## 目录
1. [核心原则：数据也要有温度](#principles)
2. [布局结构](#layout)
3. [数字与指标展示](#metrics)
4. [图表配色约束](#chart-colors)
5. [表格进阶规范](#table-advanced)
6. [状态与实时数据](#realtime)
7. [空状态与加载态](#empty-loading)
8. [信息层级控制](#hierarchy)
9. [反模式](#anti-patterns)

---

## 1. 核心原则：数据也要有温度 {#principles}

Dashboard 不等于「把数据塞满屏幕」。Anthropic 风格的数据界面遵循：

```
克制展示优于穷举展示：
  一个屏幕内最多 3 个关键指标在视觉上突出
  其余数据降级为辅助信息，用字号/颜色/位置区分

颜色只传递语义，不传递美感：
  绿色 = 正向/增长，红色 = 负向/警告，灰色 = 中性
  禁止用多彩配色装饰数据——颜色不够用时用形状和位置

数字用字体说话：
  所有数字使用 font-variant-numeric: tabular-nums（等宽数字）
  关键指标用 var(--font-display)，辅助数字用 var(--font-mono)
```

---

## 2. 布局结构 {#layout}

### 2.1 标准 Dashboard 骨架

```html
<div class="dashboard">

  <!-- 顶部 KPI 条 -->
  <section class="dashboard-kpi-bar" aria-label="核心指标">
    <div class="kpi-card">
      <span class="kpi-card__label">本月 API 调用</span>
      <span class="kpi-card__value">1,247,832</span>
      <span class="kpi-card__delta kpi-card__delta--up">+12.4%</span>
    </div>
    <div class="kpi-card">
      <span class="kpi-card__label">平均响应时间</span>
      <span class="kpi-card__value">284<span class="kpi-card__unit">ms</span></span>
      <span class="kpi-card__delta kpi-card__delta--down">-8ms</span>
    </div>
    <div class="kpi-card">
      <span class="kpi-card__label">错误率</span>
      <span class="kpi-card__value">0.12<span class="kpi-card__unit">%</span></span>
      <span class="kpi-card__delta kpi-card__delta--neutral">持平</span>
    </div>
    <div class="kpi-card">
      <span class="kpi-card__label">活跃 API 密钥</span>
      <span class="kpi-card__value">47</span>
      <span class="kpi-card__delta kpi-card__delta--up">+3 本周</span>
    </div>
  </section>

  <!-- 主内容区：左大右小 -->
  <div class="dashboard-body">

    <!-- 主图表区 -->
    <section class="dashboard-main" aria-label="用量趋势">
      <div class="chart-card">
        <div class="chart-card__header">
          <div>
            <h2 class="chart-card__title">API 调用趋势</h2>
            <p class="chart-card__subtitle">过去 30 天，按模型分组</p>
          </div>
          <div class="chart-card__controls">
            <div class="segmented segmented--sm" role="group" aria-label="时间范围">
              <button class="segmented__btn" aria-pressed="false">7天</button>
              <button class="segmented__btn segmented__btn--active" aria-pressed="true">30天</button>
              <button class="segmented__btn" aria-pressed="false">90天</button>
            </div>
          </div>
        </div>
        <div class="chart-card__body">
          <!-- 图表区域，具体渲染由 Chart.js / D3 等完成 -->
          <canvas id="usage-chart" aria-label="API 调用折线图" role="img"></canvas>
        </div>
      </div>
    </section>

    <!-- 侧边信息列 -->
    <aside class="dashboard-aside">
      <!-- 模型分布饼图 -->
      <div class="chart-card chart-card--sm">
        <div class="chart-card__header">
          <h3 class="chart-card__title">模型使用分布</h3>
        </div>
        <div class="chart-card__body">
          <canvas id="model-pie" aria-label="模型使用占比" role="img"></canvas>
          <!-- 图例 -->
          <ul class="chart-legend">
            <li class="chart-legend__item">
              <span class="chart-legend__dot" style="background: var(--chart-color-1)"></span>
              <span class="chart-legend__label">Claude Sonnet</span>
              <span class="chart-legend__value">68%</span>
            </li>
            <li class="chart-legend__item">
              <span class="chart-legend__dot" style="background: var(--chart-color-2)"></span>
              <span class="chart-legend__label">Claude Haiku</span>
              <span class="chart-legend__value">24%</span>
            </li>
            <li class="chart-legend__item">
              <span class="chart-legend__dot" style="background: var(--chart-color-3)"></span>
              <span class="chart-legend__label">Claude Opus</span>
              <span class="chart-legend__value">8%</span>
            </li>
          </ul>
        </div>
      </div>

      <!-- 快速数据列表 -->
      <div class="chart-card chart-card--sm">
        <div class="chart-card__header">
          <h3 class="chart-card__title">Top 5 密钥用量</h3>
          <a href="#" class="btn btn-ghost" style="font-size:var(--text-xs)">全部 →</a>
        </div>
        <ul class="data-list">
          <li class="data-list__item">
            <span class="data-list__label">prod-key-01</span>
            <div class="data-list__bar-wrap">
              <div class="data-list__bar" style="width: 82%"></div>
            </div>
            <span class="data-list__value">82%</span>
          </li>
          <li class="data-list__item">
            <span class="data-list__label">staging-key</span>
            <div class="data-list__bar-wrap">
              <div class="data-list__bar" style="width: 11%"></div>
            </div>
            <span class="data-list__value">11%</span>
          </li>
          <li class="data-list__item">
            <span class="data-list__label">dev-local</span>
            <div class="data-list__bar-wrap">
              <div class="data-list__bar" style="width: 5%"></div>
            </div>
            <span class="data-list__value">5%</span>
          </li>
        </ul>
      </div>
    </aside>

  </div><!-- /dashboard-body -->

  <!-- 底部详细数据表 -->
  <section class="dashboard-table-section" aria-label="详细调用记录">
    <div class="chart-card">
      <div class="chart-card__header">
        <h2 class="chart-card__title">调用记录</h2>
        <div class="chart-card__controls">
          <div class="search" style="max-width:240px">
            <div class="search__input-wrap">
              <svg class="search__icon" width="14" height="14" viewBox="0 0 16 16" fill="none">
                <circle cx="7" cy="7" r="5" stroke="currentColor" stroke-width="1.5"/>
                <path d="M11 11l3 3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
              </svg>
              <input class="search__input" type="search" placeholder="搜索密钥、模型…">
            </div>
          </div>
          <button class="btn btn-secondary" style="font-size:var(--text-xs)">导出 CSV</button>
        </div>
      </div>
      <!-- 使用 components.md 的 Table 组件 -->
      <div class="table-wrap">
        <!-- ... 见 components.md #table ... -->
      </div>
    </div>
  </section>

</div>
```

### 2.2 布局 CSS

```css
.dashboard {
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
  padding: var(--space-8);
  background: var(--color-bg-base);
  min-height: 100vh;
}

/* KPI 横条：等宽四格 */
.dashboard-kpi-bar {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--space-4);
}
@media (max-width: 1024px) {
  .dashboard-kpi-bar { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 640px) {
  .dashboard-kpi-bar { grid-template-columns: 1fr; }
}

/* 主体：左大右小，7:3 */
.dashboard-body {
  display: grid;
  grid-template-columns: 1fr 320px;
  gap: var(--space-6);
  align-items: start;
}
@media (max-width: 1200px) {
  .dashboard-body { grid-template-columns: 1fr; }
  .dashboard-aside { display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-6); }
}
@media (max-width: 640px) {
  .dashboard-aside { grid-template-columns: 1fr; }
}

/* 图表卡片 */
.chart-card {
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-xl);
  overflow: hidden;
}
.chart-card--sm { /* 侧边小卡片 */ }

.chart-card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-4);
  padding: var(--space-5) var(--space-6);
  border-bottom: 1px solid var(--color-border-subtle);
  flex-wrap: wrap;
}
.chart-card__title {
  font-family: var(--font-heading);
  font-size: var(--text-base);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  margin: 0;
}
.chart-card__subtitle {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin: var(--space-1) 0 0;
}
.chart-card__controls {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  flex-shrink: 0;
}
.chart-card__body {
  padding: var(--space-5) var(--space-6);
}
```

---

## 3. 数字与指标展示 {#metrics}

```css
/* KPI 卡片 */
.kpi-card {
  background: var(--color-bg-raised);
  border: 1px solid var(--color-border-subtle);
  border-radius: var(--radius-lg);
  padding: var(--space-5) var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  transition: box-shadow 0.2s ease;
}
.kpi-card:hover {
  box-shadow: 0 4px 20px rgba(20,20,19,0.06);
}

.kpi-card__label {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}

/* 关键数字：用显示字体 + tabular-nums */
.kpi-card__value {
  font-family: var(--font-display);
  font-size: clamp(1.75rem, 2.5vw, 2.25rem);
  font-weight: 300;
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: var(--color-text-primary);
  font-variant-numeric: tabular-nums;
}
.kpi-card__unit {
  font-size: 0.55em;
  font-weight: 400;
  color: var(--color-text-muted);
  margin-left: 2px;
}

/* 变化量：颜色传递语义 */
.kpi-card__delta {
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  font-variant-numeric: tabular-nums;
  display: inline-flex;
  align-items: center;
  gap: 3px;
}
.kpi-card__delta--up      { color: var(--color-success); }
.kpi-card__delta--down    { color: var(--color-error); }
.kpi-card__delta--neutral { color: var(--color-text-muted); }

/* 向上箭头 */
.kpi-card__delta--up::before   { content: '↑'; }
/* 向下箭头 */
.kpi-card__delta--down::before { content: '↓'; }

/* 行内数据对比（用于折叠行或 tooltip） */
.metric-compare {
  display: flex;
  align-items: baseline;
  gap: var(--space-3);
}
.metric-compare__primary {
  font-family: var(--font-mono);
  font-size: var(--text-lg);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  font-variant-numeric: tabular-nums;
}
.metric-compare__secondary {
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  font-variant-numeric: tabular-nums;
  text-decoration: line-through;
}
```

---

## 4. 图表配色约束 {#chart-colors}

**最重要的规则：图表颜色必须从品牌调色板派生，不允许引入外部颜色。**

```css
:root {
  /* 图表专用色板（从品牌色派生，保持大地调性）*/
  --chart-color-1: #D97757;  /* 橙：主系列，最重要的数据 */
  --chart-color-2: #6A9BCC;  /* 灰蓝：第二系列 */
  --chart-color-3: #788C5D;  /* 橄榄绿：第三系列 */
  --chart-color-4: #C4B99A;  /* 沙棕：第四系列 */
  --chart-color-5: #9B9890;  /* 中灰：第五系列（弱化） */

  /* 禁止使用鲜艳颜色：#FF0000 / #00FF00 / #0000FF 等 */

  /* 图表背景与网格 */
  --chart-bg:          transparent;
  --chart-grid:        var(--color-border-subtle);
  --chart-axis-label:  var(--color-text-muted);
  --chart-tooltip-bg:  var(--color-bg-inverted);
  --chart-tooltip-text: var(--color-text-inverted);
}
```

### 4.1 Chart.js 配置模板

```js
// Anthropic 风格的 Chart.js 全局配置
Chart.defaults.font.family = "'Poppins', 'DM Sans', sans-serif";
Chart.defaults.font.size = 11;
Chart.defaults.color = getComputedStyle(document.documentElement)
  .getPropertyValue('--color-text-muted').trim();

// 折线图标准配置
const lineChartConfig = {
  type: 'line',
  data: {
    labels: [...],
    datasets: [{
      label: 'Claude Sonnet',
      data: [...],
      borderColor: 'var(--chart-color-1)',  /* 注意：Chart.js 不支持 CSS 变量，需用 getComputedStyle 取值 */
      backgroundColor: 'rgba(217, 119, 87, 0.08)',
      borderWidth: 2,
      pointRadius: 0,           /* 无数据点，线条更干净 */
      pointHoverRadius: 4,
      tension: 0.3,             /* 轻微曲线，比折线更有机 */
      fill: true,
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    interaction: { mode: 'index', intersect: false },
    plugins: {
      legend: {
        display: false,         /* 用自定义图例替代 Chart.js 自带的 */
      },
      tooltip: {
        backgroundColor: '#141413',
        titleColor: '#FAF9F5',
        bodyColor: '#B0AEA5',
        borderColor: 'rgba(250,249,245,0.1)',
        borderWidth: 1,
        padding: 12,
        cornerRadius: 8,
        titleFont: { family: "'Poppins'", size: 12, weight: '500' },
        bodyFont:  { family: "'Poppins'", size: 11 },
        callbacks: {
          label: (ctx) => ` ${ctx.dataset.label}: ${ctx.parsed.y.toLocaleString()}`
        }
      }
    },
    scales: {
      x: {
        grid: { display: false },
        border: { display: false },
        ticks: { maxTicksLimit: 6 }
      },
      y: {
        grid: {
          color: 'rgba(216, 213, 204, 0.5)',  /* --color-border-subtle 半透明 */
          drawTicks: false,
        },
        border: { display: false, dash: [4, 4] },
        ticks: {
          maxTicksLimit: 5,
          callback: (v) => v >= 1000 ? (v/1000).toFixed(0) + 'k' : v
        }
      }
    }
  }
};
```

### 4.2 图表使用规则

```
何时用颜色区分数据：
  ✅ 多条折线（不同模型/密钥）→ 用 --chart-color-1/2/3
  ✅ 饼图/甜甜圈图 → 用全部 5 色
  ✅ 状态区分（正常/警告/错误）→ 用语义色（success/warning/error）
  ❌ 同一系列的柱状图 → 全部用 --chart-color-1，不要每根柱子不同色
  ❌ 热力图 → 用单色渐变（橙色系），不用彩虹色

系列数量上限：
  折线图：最多 4 条线，超出合并为「其他」
  饼图：最多 5 个分片，超出合并为「其他」
  柱状图：最多 6 组，超出用滚动/分页

Y 轴从零开始：
  所有柱状图 Y 轴必须从 0 开始，禁止截断坐标轴误导数据
  折线图可以不从 0 开始，但需明确标注起点值
```

---

## 5. 表格进阶规范 {#table-advanced}

基础表格见 `components.md #table`，此处补充数据密集场景的额外规则。

```css
/* 数字列：必须右对齐 + 等宽数字 */
.table__td--numeric,
.table__th--numeric {
  text-align: right;
  font-family: var(--font-mono);
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.01em;
}

/* 状态列：宽度固定，防止文字换行 */
.table__td--status { white-space: nowrap; }

/* 进度条列（用于展示百分比）*/
.table-progress {
  display: flex;
  align-items: center;
  gap: var(--space-3);
}
.table-progress__bar-wrap {
  flex: 1;
  height: 4px;
  background: var(--color-border-subtle);
  border-radius: var(--radius-full);
  overflow: hidden;
}
.table-progress__bar {
  height: 100%;
  background: var(--color-accent-orange);
  border-radius: var(--radius-full);
  transition: width 0.4s var(--ease-default);
}
.table-progress__value {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  min-width: 36px;
  text-align: right;
  font-variant-numeric: tabular-nums;
}

/* 行内 Sparkline（微型趋势图）*/
.table-sparkline {
  display: inline-flex;
  align-items: flex-end;
  gap: 2px;
  height: 20px;
}
.table-sparkline__bar {
  width: 3px;
  background: var(--color-accent-sand);
  border-radius: 1px;
  transition: background 0.15s ease;
}
.table-sparkline__bar--highlight { background: var(--color-accent-orange); }

/* 固定列（横向滚动时）*/
.table__td--sticky,
.table__th--sticky {
  position: sticky;
  left: 0;
  background: inherit;
  z-index: var(--z-raised);
  box-shadow: 2px 0 6px rgba(20,20,19,0.06);
}

/* 展开行 */
.table__row--expandable { cursor: pointer; }
.table__row--expandable:hover { background: var(--color-bg-raised); }
.table__expand-icon {
  transition: transform 0.2s var(--ease-default);
  color: var(--color-text-muted);
}
.table__row--expanded .table__expand-icon { transform: rotate(90deg); }
.table__row--detail {
  display: none;
  background: var(--color-bg-overlay);
}
.table__row--expanded + .table__row--detail { display: table-row; }
.table__detail-cell {
  padding: var(--space-4) var(--space-6);
  border-top: 1px solid var(--color-border-subtle);
}
```

### 5.1 多列信息的视觉层级处理

```
列宽度分配原则：
  主标识列（名称/ID）→ 最宽，min-width: 200px，允许截断
  数字列（数量/金额）→ 固定宽度，右对齐
  状态列 → 固定宽度，居中
  操作列 → 最小宽度，右对齐，只放图标或极短文字

列重要性降级：
  最重要 → 加粗 + 深色（var(--color-text-primary)）
  次要   → 正常 + 中灰（var(--color-text-secondary)）
  辅助   → 小字 + 浅灰（var(--color-text-muted)）
  技术性（ID/时间戳）→ 等宽字体 + 最浅色

禁止在表格中使用超过 3 种颜色区分数据，颜色冗余会淹没语义。
```

---

## 6. 状态与实时数据 {#realtime}

```html
<!-- 实时更新指示器 -->
<div class="realtime-indicator" aria-live="polite" aria-label="数据实时更新中">
  <span class="realtime-dot" aria-hidden="true"></span>
  <span class="realtime-label">实时</span>
  <span class="realtime-timestamp">更新于 <time id="last-updated">刚刚</time></span>
</div>

<!-- 数据刷新进度条（顶部细线）-->
<div class="refresh-bar" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="60" aria-label="下次刷新进度" hidden>
  <div class="refresh-bar__fill" style="width: 60%"></div>
</div>
```

```css
/* 实时指示器 */
.realtime-indicator {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-heading);
  font-size: var(--text-xs);
  color: var(--color-text-muted);
}
.realtime-dot {
  width: 6px;
  height: 6px;
  border-radius: var(--radius-full);
  background: var(--color-success);
  position: relative;
}
.realtime-dot::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: var(--radius-full);
  background: var(--color-success);
  animation: pulse-status 2s ease-in-out infinite;
}

/* 顶部刷新进度条 */
.refresh-bar {
  position: fixed;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: var(--color-border-subtle);
  z-index: var(--z-progress);
}
.refresh-bar__fill {
  height: 100%;
  background: var(--color-accent-orange);
  transition: width 1s linear;
}

/* 数字变化动画（数值刷新时）*/
@keyframes valueFlash {
  0%   { color: var(--color-text-primary); }
  30%  { color: var(--color-accent-orange); }
  100% { color: var(--color-text-primary); }
}
.value-updated {
  animation: valueFlash 0.6s ease;
}
```

```js
// 数字变化时触发闪烁
function updateValue(el, newValue) {
  const old = el.textContent;
  if (old !== String(newValue)) {
    el.textContent = newValue;
    el.classList.remove('value-updated');
    void el.offsetWidth; // 强制重绘
    el.classList.add('value-updated');
  }
}

// 相对时间更新
function updateTimestamp(el) {
  const seconds = Math.floor((Date.now() - lastUpdated) / 1000);
  el.textContent = seconds < 5 ? '刚刚' :
                   seconds < 60 ? `${seconds} 秒前` :
                   `${Math.floor(seconds/60)} 分钟前`;
}
```

---

## 7. 空状态与加载态 {#empty-loading}

Dashboard 的空/加载态必须和整体风格一致，不能退化成通用 spinner。

```html
<!-- 图表加载中：用 Skeleton 替代空白 -->
<div class="chart-card">
  <div class="chart-card__header">
    <div class="skeleton skeleton--title" style="width: 120px"></div>
  </div>
  <div class="chart-card__body" style="height: 200px; display:flex; flex-direction:column; gap:8px; padding-top:var(--space-5)">
    <div class="skeleton" style="height: 160px; border-radius: var(--radius-md)"></div>
  </div>
</div>

<!-- KPI 卡片加载中 -->
<div class="kpi-card" aria-busy="true" aria-label="加载中">
  <div class="skeleton skeleton--title" style="width: 80px"></div>
  <div class="skeleton" style="height: 36px; width: 140px; border-radius: var(--radius-md)"></div>
  <div class="skeleton skeleton--text" style="width: 60px"></div>
</div>

<!-- 无数据：使用 components.md 的 Empty State，但配 Dashboard 语境文字 -->
<div class="chart-card">
  <div class="chart-card__body">
    <div class="empty-state" style="padding-block: var(--space-16)">
      <div class="empty-state__icon">
        <svg width="40" height="40" viewBox="0 0 40 40" fill="none">
          <path d="M8 32L12 20l8 6 6-12 6 16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" opacity=".3"/>
          <circle cx="20" cy="20" r="18" stroke="currentColor" stroke-width="1.5" opacity=".2"/>
        </svg>
      </div>
      <h3 class="empty-state__title">暂无数据</h3>
      <p class="empty-state__body">该时间段内没有 API 调用记录。<br>尝试调整时间范围或检查 API 密钥配置。</p>
    </div>
  </div>
</div>
```

---

## 8. 信息层级控制 {#hierarchy}

Dashboard 特有的信息密度规则（补充 SKILL.md 0.4 节）：

```
单屏最多 4 个 KPI 卡片（超出用「更多指标」折叠）

图表优先级：
  第一优先：时序趋势（折线/面积）→ 放最大区域
  第二优先：构成比例（饼/甜甜圈）→ 放侧边
  第三优先：排名（横向柱状）→ 放侧边或折叠
  不允许：同屏超过 3 个图表

颜色层级（从强到弱）：
  橙色  → 最关键指标、需要关注的异常
  深色  → 正常数值
  灰色  → 历史/对比/次要数据
  虚线  → 目标值/基准线

数字对齐铁律：
  同一列的数字必须小数点对齐
  使用 font-variant-numeric: tabular-nums 保证等宽
  千位分隔符：中文界面用逗号（1,234,567）
```

---

## 9. 反模式 {#anti-patterns}

| 禁止行为 | 原因 | 替代方案 |
|---------|------|---------|
| 彩虹色图表（每系列随机色）| 破坏品牌调性 | 只用 --chart-color-1 到 5 |
| Y 轴不从零开始的柱状图 | 误导数据比例 | 始终从零，用折线图展示趋势 |
| 同屏超过 4 个图表 | 认知过载 | 用 Tab 或折叠面板分组 |
| 表格行数超过 25 没有分页 | 性能和可读性 | 加 Pagination 或虚拟滚动 |
| 实时数据每秒全量刷新 DOM | 闪烁和性能 | 只更新变化的数值节点 |
| 在图表上直接用 `#FF0000` 标错误 | 颜色太刺激 | 用 var(--color-error) 配合图标 |
| 数字列左对齐 | 无法快速比较大小 | 数字列始终右对齐 |
| 图表没有空状态处理 | 空白区域显得 broken | 用 Skeleton 或 Empty State |

---

## 附：数据列表组件（行内横向进度）

```css
.data-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
}
.data-list__item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) 0;
  border-bottom: 1px solid var(--color-border-subtle);
}
.data-list__item:last-child { border-bottom: none; }
.data-list__label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  width: 90px;
  flex-shrink: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.data-list__bar-wrap {
  flex: 1;
  height: 4px;
  background: var(--color-border-subtle);
  border-radius: var(--radius-full);
  overflow: hidden;
}
.data-list__bar {
  height: 100%;
  background: var(--color-accent-orange);
  border-radius: var(--radius-full);
  transition: width 0.6s var(--ease-default);
}
.data-list__value {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: var(--weight-medium);
  color: var(--color-text-primary);
  font-variant-numeric: tabular-nums;
  width: 32px;
  text-align: right;
  flex-shrink: 0;
}
```
