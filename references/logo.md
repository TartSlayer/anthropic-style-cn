# Logo 绘制指南

本文件指导 agent 在 Anthropic 风格体系下构思、绘制、适配 SVG Logo。
不要凭空发明，要按这里的原则推导出每一条路径。

---

## 目录
1. [Logo 构成原则](#principles)
2. [SVG 代码绘制思路](#svg-drawing)
3. [品牌色在 Logo 中的用法](#brand-color)
4. [单色 / 反色 / 浅色背景变体](#variants)
5. [最小尺寸 & 安全间距规则](#sizing)
6. [Favicon 与 App Icon 适配](#favicon)

---

## 1. Logo 构成原则 {#principles}

### 设计哲学

Anthropic 的 Logo 设计哲学与其品牌一脉相承：**几何克制 + 有机温度**。
不追求复杂图形，而是通过精确的比例关系传递可信感。

### 六大构成原则

**原则 1：基于网格，从简单几何形出发**

所有图形元素都从圆形、正方形、等边三角形等基础形状导出，再做圆角或局部裁切。
不要自由曲线起步——先画网格，再画形状，最后微调节点。

```
推荐构图网格：
┌─────────────────────┐
│  8×8 或 12×12 单元格  │
│  每个主要元素占 ≥ 2 格 │
│  留白不少于 1 格      │
└─────────────────────┘
```

**原则 2：黄金比例 / 根号比例**

元素比例优先选择：
- 1 : 1.618（黄金比例）
- 1 : √2 = 1 : 1.414
- 1 : 2 / 1 : 3（整数比）

```
不好的比例：宽 47px，高 63px（任意值）
好的比例：宽 40px，高 64.7px（1 : 1.618）
      或：宽 40px，高 40px（正圆/正方形）
```

**原则 3：奇数对称 + 刻意不对称**

- 纯对称 Logo 显得呆板，Anthropic 的图标有轻微的动态感
- 允许一侧轻微偏移（2–4px），但不能随意——要有逻辑依据（如视觉重心补偿）

**原则 4：笔画粗细统一**

图标内所有 stroke 使用同一粗细值，通常为画布宽度的 1/12 到 1/8：
- 16×16 画布 → stroke-width: 1.5
- 24×24 画布 → stroke-width: 1.5–2
- 32×32 画布 → stroke-width: 2
- 48×48 画布 → stroke-width: 2.5

**原则 5：有机圆角**

```
纯几何感：border-radius = 0，硬角
有机温度感：border-radius = 元素宽度的 15–25%

Anthropic 风格选有机温度感。
正方形图标：rx/ry = 宽度 × 0.2
圆角三角形：path 的角点换成弧线
```

**原则 6：负空间有意义**

好的 Logo 的空白区域本身也构成可识别的形状。
设计时问：「把颜色反过来，形状是否依然好看？」

---

## 2. SVG 代码绘制思路 {#svg-drawing}

### SVG 画布标准设置

```svg
<!-- 标准 Logo SVG 模板 -->
<svg
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 40 40"      <!-- viewBox 与设计稿尺寸一致 -->
  width="40"
  height="40"
  role="img"
  aria-labelledby="logo-title"
>
  <title id="logo-title">公司名称</title>
  <!-- 图形内容 -->
</svg>
```

### 常用几何基元

```svg
<!-- 正圆 -->
<circle cx="20" cy="20" r="16" fill="currentColor"/>

<!-- 圆角矩形 -->
<rect x="4" y="4" width="32" height="32" rx="8" ry="8" fill="currentColor"/>

<!-- 正三角形（中心 20,20，外接圆半径 16） -->
<polygon points="20,5 33.86,27.5 6.14,27.5" fill="currentColor"/>

<!-- 六边形 -->
<polygon points="20,4 34,12 34,28 20,36 6,28 6,12" fill="currentColor"/>

<!-- 路径：带圆角的自定义形状 -->
<path
  d="M 8 20
     C 8 13.37 13.37 8 20 8
     C 26.63 8 32 13.37 32 20"
  stroke="currentColor"
  stroke-width="2"
  fill="none"
  stroke-linecap="round"
/>
```

### 路径命令速查

```
M x,y       移动到 (x,y)，不画线
L x,y       直线到 (x,y)
H x         水平线到 x
V y         垂直线到 y
C cx1,cy1 cx2,cy2 x,y   三次贝塞尔曲线
Q cx,cy x,y             二次贝塞尔曲线
A rx,ry rot,large,sweep x,y  弧线（最常用于圆角）
Z           闭合路径

小写字母 = 相对坐标，大写 = 绝对坐标
```

### 绘制圆角三角形（步骤示例）

```svg
<!-- 目标：40×40 画布内的圆角等边三角形 -->
<!--
  三角形顶点（外接圆 r=16，中心 20,20）：
  顶：(20, 4)
  右下：(33.86, 28)
  左下：(6.14, 28)

  用 A 命令在每个顶点加圆角（r=4）
-->
<path
  d="M 20 8
     L 31 26
     Q 33.86 28 31.5 29.5
     L 8.5 29.5
     Q 6.14 28 9 26
     Z"
  fill="#D97757"
/>
<!-- 微调每个 Q 控制点直到圆角自然 -->
```

### 组合图形的层次逻辑

```svg
<svg viewBox="0 0 40 40">
  <!-- 1. 背景形状（最底层） -->
  <rect x="2" y="2" width="36" height="36" rx="10" fill="#D97757"/>

  <!-- 2. 主图形（中层） -->
  <path d="M 12 20 L 20 12 L 28 20 L 20 28 Z"
        fill="none" stroke="white" stroke-width="2"/>

  <!-- 3. 细节/高光（最顶层） -->
  <circle cx="20" cy="20" r="2" fill="white"/>
</svg>
```

### 让形状更有「温度」的技巧

```svg
<!-- 技巧 1：让线条末端轻微突出（stroke-linecap="round"）-->
<line x1="10" y1="20" x2="30" y2="20"
      stroke="currentColor" stroke-width="2" stroke-linecap="round"/>

<!-- 技巧 2：转角圆滑（stroke-linejoin="round"）-->
<polyline points="10,28 20,12 30,28"
          fill="none" stroke="currentColor" stroke-width="2"
          stroke-linecap="round" stroke-linejoin="round"/>

<!-- 技巧 3：轻微不规则（节点偏移 1–2px，模拟手绘感）-->
<!-- 不要完全对称的路径，稍微错开让形状有生命感 -->

<!-- 技巧 4：微妙的粗细变化（用 path 的宽度渐变） -->
<!-- 通过两条间距渐变的路径模拟笔刷感 -->
```

---

## 3. 品牌色在 Logo 中的用法 {#brand-color}

### 标准色值

```
主品牌橙：#D97757
深橙（悬停）：#C96442
近黑（正色）：#141413
暖白（反色）：#FAF9F5
米色（浅底）：#ECE9E0
沙棕（辅助）：#C4B99A
```

### 配色方案

**方案 A：橙色图标 + 深色文字（最常用）**
```
图标颜色：#D97757
文字颜色：#141413
背景：透明 / #ECE9E0
```

**方案 B：深色背景 + 橙色图标 + 白色文字**
```
背景：#141413
图标：#D97757
文字：#FAF9F5
```

**方案 C：纯深色（印刷/黑白场景）**
```
图标 + 文字：均用 #141413
```

**方案 D：全橙色（活泼/营销场景）**
```
图标背景：#D97757
图标内容：#FAF9F5（白）
文字：#D97757
```

### 颜色使用铁律

1. **图标本体不超过 2 色**，辅助细节可加第 3 色但需极克制
2. **橙色永远是视觉焦点**，不要让其他颜色在面积上超过橙色
3. **渐变慎用**：如果要用，只在图标内的填充区用从橙到深橙（#D97757 → #C96442），不用多色渐变
4. **不用纯黑 #000000**，用 #141413（近黑，更有温度）

---

## 4. 单色 / 反色 / 浅色背景变体 {#variants}

每个 Logo 至少需要准备以下 4 个变体：

### 变体 1：彩色主版本（默认）

```svg
<!-- 橙色图标 + 深色文字，用于白/米色背景 -->
<svg viewBox="0 0 120 40">
  <!-- 图标 -->
  <g fill="#D97757"><!-- 图标路径 --></g>
  <!-- 文字 -->
  <text x="50" y="26" font-family="'DM Serif Display'" font-size="20"
        fill="#141413">Anthropic</text>
</svg>
```

### 变体 2：深色背景版本（反色）

```svg
<!-- 图标改白色，文字改暖白，用于 #141413 背景 -->
<svg viewBox="0 0 120 40">
  <g fill="#FAF9F5"><!-- 图标路径 --></g>
  <text x="50" y="26" font-family="'DM Serif Display'" font-size="20"
        fill="#FAF9F5">Anthropic</text>
</svg>
```

### 变体 3：单色版本（印刷 / 刻印 / 黑白打印）

```svg
<!-- 一切用 currentColor，或硬编码 #141413 -->
<svg viewBox="0 0 120 40" fill="currentColor">
  <!-- 所有图形和文字统一颜色 -->
</svg>
```

```css
/* 使用时靠 CSS 设置颜色 */
.logo-mono { color: #141413; }
.logo-mono-white { color: white; }
```

### 变体 4：仅图标版（Favicon / App Icon）

```svg
<!-- 去掉文字，只保留图标，viewBox 改为正方形 -->
<svg viewBox="0 0 40 40">
  <g fill="#D97757"><!-- 图标路径 --></g>
</svg>
```

### CSS 变体切换

```css
/* 通过 CSS 变量切换，只需一个 SVG 文件 */
.logo { --logo-icon: #D97757; --logo-text: #141413; }
.logo--dark { --logo-icon: #FAF9F5; --logo-text: #FAF9F5; }
.logo--mono { --logo-icon: currentColor; --logo-text: currentColor; }

/* SVG 内部使用变量 */
.logo-icon-path { fill: var(--logo-icon); }
.logo-text-path { fill: var(--logo-text); }
```

---

## 5. 最小尺寸 & 安全间距规则 {#sizing}

### 最小尺寸

| 使用场景 | 最小宽度 | 要求 |
|---------|---------|------|
| 数字屏幕（网页/App） | 80px（横版）/ 24px（图标） | 文字清晰可读 |
| 移动端导航栏 | 60px 宽 / 32px 高 | |
| 印刷（名片/文档） | 15mm（横版）| 300dpi 以上 |
| 刺绣/雕刻 | 25mm | 细节需简化 |

低于最小尺寸时，切换到**仅图标版本**，不要强行缩小带文字的横版。

### 安全间距（保护区）

**规则：Logo 四周留出图标高度的 50% 作为保护区，任何其他元素不得进入此区域。**

```
图标高度 = H

    ┌──────────────────────────────────────────┐
    │                  0.5H                    │
    │  ┌───────────────────────────────────┐   │
    │  │                                   │   │
 0.5H  │    [图标]      [品牌文字]          │ 0.5H
    │  │                                   │   │
    │  └───────────────────────────────────┘   │
    │                  0.5H                    │
    └──────────────────────────────────────────┘
```

```css
/* 在代码中实现安全间距 */
.logo-container {
  padding: calc(var(--logo-height) * 0.5);
  /* 或者固定值 */
  padding: var(--space-4) var(--space-6);
}
```

### 横版 vs 竖版布局

```svg
<!-- 横版（默认）：图标左，文字右 -->
<svg viewBox="0 0 160 40">
  <g transform="translate(0, 0)"><!-- 40×40 图标 --></g>
  <text x="52" y="26" ...>Brand Name</text>
</svg>

<!-- 竖版（居中）：图标上，文字下 -->
<svg viewBox="0 0 80 80">
  <g transform="translate(20, 4)"><!-- 40×40 图标 --></g>
  <text x="40" y="72" text-anchor="middle" ...>Brand</text>
</svg>
```

---

## 6. Favicon 与 App Icon 适配 {#favicon}

### Favicon 多尺寸方案

```html
<!-- HTML <head> 中的完整 Favicon 声明 -->

<!-- 传统 ICO（浏览器标签页，最高优先级） -->
<link rel="icon" href="/favicon.ico" sizes="any">

<!-- SVG Favicon（现代浏览器，支持深色模式自适应） -->
<link rel="icon" href="/favicon.svg" type="image/svg+xml">

<!-- Apple Touch Icon（iOS 主屏图标） -->
<link rel="apple-touch-icon" href="/apple-touch-icon.png"> <!-- 180×180 -->

<!-- PWA Manifest（Android / 桌面 PWA） -->
<link rel="manifest" href="/manifest.json">
```

### SVG Favicon（推荐，自动适配深色模式）

```svg
<!-- favicon.svg -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <!-- 深色模式下自动切换颜色 -->
  <style>
    .icon-bg   { fill: #D97757; }
    .icon-mark { fill: #FAF9F5; }

    @media (prefers-color-scheme: dark) {
      .icon-bg   { fill: #2C2B26; }
      .icon-mark { fill: #D97757; }
    }
  </style>

  <!-- 背景圆角矩形 -->
  <rect class="icon-bg" x="0" y="0" width="32" height="32" rx="7"/>

  <!-- 图标主体（示例：简化的几何标记） -->
  <path class="icon-mark"
        d="M 16 8 L 24 22 L 8 22 Z"
        stroke="none"/>
</svg>
```

### PWA manifest.json

```json
{
  "name": "Anthropic",
  "short_name": "Anthropic",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" },
    { "src": "/icon-maskable-512.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ],
  "theme_color": "#D97757",
  "background_color": "#ECE9E0",
  "display": "standalone"
}
```

### App Icon 安全区域（Maskable Icon）

Android 自适应图标会裁切成圆形/水滴形/方形，需要在中心留出安全区：

```
512×512 画布
├── 外圈：0–512px（可能被裁切）
├── 出血区：40–472px（不同形状可能裁切）
└── 安全区：102–410px（中心 308×308，保证内容可见）
         ⬆ 所有关键内容必须在此范围内
```

```svg
<!-- maskable icon 示例：内容缩到 60% 中心 -->
<svg viewBox="0 0 512 512">
  <!-- 全出血背景色（覆盖整个画布） -->
  <rect width="512" height="512" fill="#D97757"/>

  <!-- 图标主体缩放到 60%，居中放置（308×308 区域） -->
  <g transform="translate(102, 102) scale(0.6)">
    <!-- 原始 40×40 图标放大到 512 -->
    <!-- ... -->
  </g>
</svg>
```

### 各尺寸生成规范

| 文件 | 尺寸 | 用途 |
|------|------|------|
| `favicon.ico` | 16×16 + 32×32（多尺寸 ICO）| 浏览器标签页 |
| `favicon.svg` | 矢量 | 现代浏览器，支持暗色模式 |
| `apple-touch-icon.png` | 180×180 | iOS 主屏图标 |
| `icon-192.png` | 192×192 | Android PWA |
| `icon-512.png` | 512×512 | Android PWA / 启动屏 |
| `icon-maskable-512.png` | 512×512（带出血背景） | Android 自适应图标 |
| `og-image.jpg` | 1200×630 | 社交分享预览图（非图标但同属品牌素材）|

### OG 社交图（og-image.jpg）规范

```html
<meta property="og:image" content="https://example.com/og-image.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:image" content="https://example.com/og-image.jpg">
```

```
OG 图设计规范（1200×630）：
- 背景：var(--color-bg-inverted) = #141413
- 中心区域（800×400）放 Logo + 一行标语
- 四周留 100px 安全边距
- 文字不小于 32px（缩略图时依然可读）
- 避免在边缘区域放重要信息
```
