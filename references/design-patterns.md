# 设计模式与视觉深度

## 背景与视觉深度

### 米色纹理背景

```css
/* 方法 A：CSS 噪点纹理 */
.bg-texture {
  background-color: var(--color-bg-base);
  background-image:
    url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
}

/* 方法 B：径向渐变晕染 */
.bg-vignette {
  background:
    radial-gradient(ellipse 80% 60% at 20% 10%, rgba(217,119,87,0.06) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 80% 90%, rgba(106,155,204,0.05) 0%, transparent 60%),
    var(--color-bg-base);
}

/* 方法 C：深色 Hero 区块 */
.bg-dark-hero {
  background:
    radial-gradient(ellipse 100% 80% at 50% 0%, rgba(217,119,87,0.12) 0%, transparent 55%),
    var(--color-bg-inverted);
}
```

### 分隔区块的视觉节奏

页面用深浅背景交替制造节奏感：
```
[米色 Hero] → [浅色内容] → [深色 Feature] → [米色内容] → [深色 Footer]
```

### 空间构图原则

来自官方 frontend-design skill：

**留白是最强大的设计工具。** 不用装饰也能传达品质感。

```
核心手法：
  ① 不对称留白 → 右侧或底部留大量空白，制造呼吸感
  ② 意外的负空间 → 文字只占容器 40%，其余放空
  ③ 打断网格 → 一个元素突破对齐边界，制造张力
  ④ 极端对比 → 超大标题 + 超小正文，或反之
```

**信息密度量化规则：**

| 密度级别 | 元素数/屏 | 适用场景 |
|---------|----------|---------|
| 低密度（默认） | ≤ 5 | Landing page、品牌展示 |
| 中密度 | 6-10 | 功能页面、表单流程 |
| 高密度 | 11-15 | Dashboard、数据表格 |
| 极高密度 | 16+ | 专业工具（需额外视觉降噪） |

## 动效与交互

### 过渡规范

```css
:root {
  --ease-default: cubic-bezier(0.16, 1, 0.3, 1);   /* 快进慢出 */
  --ease-bounce:  cubic-bezier(0.34, 1.56, 0.64, 1); /* 轻微弹性 */
  --ease-gentle:  cubic-bezier(0.4, 0, 0.2, 1);      /* 温和 */
}
```

### 页面进入动画（Stagger Reveal）

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  opacity: 0;
  animation: fadeUp var(--duration-slow) var(--ease-default) forwards;
}
.reveal:nth-child(1) { animation-delay: 0ms; }
.reveal:nth-child(2) { animation-delay: 80ms; }
.reveal:nth-child(3) { animation-delay: 160ms; }
.reveal:nth-child(4) { animation-delay: 240ms; }
```

### 微交互规范

- **悬停提升**：卡片 hover 上移 2px + 浅阴影，不要超过 4px
- **按钮按下**：`scale(0.97)` 轻微缩小
- **焦点环**：橙色 `box-shadow: 0 0 0 3px rgba(217,119,87,0.35)`

## 可访问性规范

- **对比度**：正文 ≥ 4.5:1（WCAG AA），大标题 ≥ 3:1
- **焦点样式**：所有可交互元素必须有可见焦点环，用橙色
- **点击区域**：最小 44×44px（移动端），最小 32×32px（桌面）
- **动画尊重用户偏好**：
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      transition-duration: 0.01ms !important;
    }
  }
  ```
- **语义 HTML**：用 `<button>` 不用 `<div>` 做按钮，用 `<nav>` 包导航

## 反模式 ❌（绝对禁止）

| 禁止行为 | 原因 |
|---------|------|
| 纯白 `#FFFFFF` 页面底色 | 缺乏温度感 |
| 蓝紫渐变 Hero 背景 | 科技 SaaS 陈词滥调 |
| 使用 Inter/Roboto/Open Sans | 最泛滥的 AI 产品字体 |
| 橙色以外的颜色做主 CTA | 破坏品牌一致性 |
| 超过 5 个主色 | 调色板失控 |
| 方角卡片（`border-radius: 0`） | 缺乏有机感 |
| 所有标题都用无衬线 | 丧失叙事温度 |
| 高饱和度荧光点缀色 | 与大地调色盘冲突 |
| 滥用阴影（多层、高扩散） | 视觉过重，显廉价 |
