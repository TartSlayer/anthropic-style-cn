---
name: anthropic-style-frontend-cn
description: |
  生成 Anthropic 风格的高质量前端界面——有温度、有个性、绝不「AI 通用」。
  包含官方品牌字体（Poppins + Lora）、43 个完整组件、4 种场景模式。

  每当用户要构建任何前端界面，无论是 landing page、SaaS 产品、AI 工具、
  Chat UI、Dashboard、后台管理，都应使用本 skill——尤其是当他们说「做个界面」
  「设计一个页面」「帮我写 UI」「Anthropic 风格」「Claude 风格」「好看一点」
  「避免 AI 通用感」「有温度感」时，立即触发本 skill，不要自行发挥。

  本 skill 强制要求：动手前先确定大胆的美学方向；每次输出必须有差异化，
  不得收敛到同一套模板；实现复杂度必须匹配美学强度。
---

# Anthropic 风格前端设计规范

*基于官方 brand-guidelines + frontend-design skill*

---

## 文件索引（先看这里）

| 文件 | 内容 | 何时读取 |
|------|------|---------|
| `assets/base.css` | 完整 CSS Token（颜色/字体/间距/动效/Z-index）| 每次都要引入 |
| `assets/fonts/fonts.css` | 离线字体声明（Poppins + Lora + DM 系列）| 每次都要引入 |
| `references/components/index.md` | 43 个组件索引，含快速查找表 | 需要具体组件时 |
| `references/systems.md` | 11 条系统规范（响应式/暗色模式/焦点陷阱等）| 构建完整项目时 |
| `references/dashboard.md` | Dashboard 专项规范（KPI/图表/实时数据）| 数据密集场景 |
| `references/logo.md` | Logo 绘制 + Favicon 规范 | 需要图标/Logo 时 |
| `references/typography-cn.md` | 中文排版（霞鹜文楷/混排/子集化）| 中文界面 |
| `references/design-rules.md` | 操作规则详细版（自检/模式隔离/修复规则）| 需要完整规则时 |
| `references/design-patterns.md` | 背景纹理 CSS、动效规范、微交互、可访问性、反模式 | 需要具体视觉实现参考时 |
| `SKILL-lite.md` | 精简版（~700 token）| 简单组件/单页面/快速原型，先读此文件 |

---

## 开始前：选择模式

收到任务后，先按以下关键词判断模式，这决定了后续所有决策：

| 任务包含这些词 | 模式 | 核心调整 |
|-------------|------|---------|
| dashboard / 监控 / 数据看板 / metrics / 图表 / 运维 / 报表 | **数据密集** | 压缩留白、提高对比度、参考 dashboard.md |
| admin / 后台 / 管理系统 / 配置 / 权限 / CRM / ERP / OA | **工具优先** | 密度优先、功能完整、可突破留白底线 |
| landing page / 落地页 / marketing / 品牌 / 官网首页 / 游戏 | **品牌增强** | 允许受控渐变、更强视觉张力 |
| 其他 | **默认** | 完整执行本 skill，留白克制 |

模式判断优先于「觉得这应该属于哪种」——关键词匹配是机械判断，不是语义猜测。非默认模式仍使用同一套 Token，只在密度、对比度、克制程度上做有限调整。模式隔离细则见 `design-rules.md`。

---

## 设计哲学

Anthropic 的视觉语言建立在一个核心矛盾上：**技术性 + 人文温度**。它刻意与冷蓝色调的「AI 科技风」划清界限：

- 暖米色大地调（`#ECE9E0`）而非冰冷白底
- 衬线体（Lora）+ 无衬线体（Poppins）混排，而非全无衬线
- 克制的橙色（`#D97757`）作为唯一强调色
- 叙事感排版，而非功能性列举
- 大留白，或极致密度——拒绝中间状态

> 做完设计后问自己：**「把 Logo 遮住，还能看出这是一家专注人类价值观的 AI 公司吗？」**

### 开始前的四个问题（前置思考框架）

来自官方 frontend-design skill：

1. **Purpose（目的）**：这个界面解决什么问题？谁在用它？
2. **Tone（调性）**：选一个明确方向并坚定执行——Anthropic 风格是「有机自然 × 精致克制 × 编辑叙事」的交叉点。可选方向：极简克制 / 有机自然 / 精致奢华 / 编辑叙事 / 几何硬朗 / 温暖柔和 / 工业实用
3. **Constraints（约束）**：框架要求？多语言？可访问性？
4. **Differentiation（差异化）**：这个设计里有什么让人过目不忘的元素？

### 实现复杂度必须匹配美学强度

来自官方 frontend-design skill——这是最容易被忽略的原则：

- **极简设计**需要精准的间距、克制的排版、每个细节都经过深思；不能用「代码少」来合理化「设计随意」
- **极繁设计**需要大量动效、复杂背景处理、精心编排的视觉层次；代码量和视觉丰富度必须匹配
- **不可接受**：介于两者之间的「普通」——既不够克制，又不够大胆

### 强制差异化（防止模式坍缩）

来自官方 frontend-design skill：**不同任务的输出必须有视觉差异，绝不收敛到同一套模板。**

每次生成时主动改变其中一项：字体配对 / 配色权重 / 布局结构 / 背景处理 / 动效风格。

在默认模式下，每次还需要加入一个「非标准元素」：
- 非对称分栏（而非全部居中）
- 突破容器边界的装饰图形
- 极大标题 + 极小说明的字号对比
- 不寻常的负空间分配

### 信息密度规则（具体数字）

```
单屏视觉层级：最多 3 个（主标题 / 内容 / 辅助信息）
单区块信息块：最多 2 个，超出拆分成新区块
行宽控制：正文最大 65ch，标题最大 28ch
留白底线（默认模式）：区块间距最小 64px，卡片内距最小 24px
导航项数：顶部导航最多 6 个
```

### 决策优先级（规则冲突时）

信息清晰 > 交互可用 > 视觉一致 > 极简美学。

美感是最低优先级——如果为了「好看」牺牲了信息或可用性，那就是设计失败。

### 必须放弃极简的场景

危险操作（删除/不可逆操作）、紧急报警、系统崩溃、不可回退节点——这些场景必须用强烈视觉强调，不能克制。详细实现见 `systems.md` 第 11.4 节和 `design-rules.md`。

---

## Token 系统

所有实现细节在 `assets/base.css`，下面是关键值速查：

**颜色**
```css
--color-bg-base: #ECE9E0       /* 页面底色，不用纯白 */
--color-bg-raised: #F5F3EC     /* 卡片/面板 */
--color-bg-inverted: #141413   /* 深色区块 */
--color-accent-orange: #D97757 /* 唯一强调色，主 CTA */
--color-text-primary: #141413  /* 主文字 */
--color-text-secondary: #6B6860
--color-error: #C0453A         /* 危险操作用这个，不用克制灰色 */
```

**字体**（官方品牌字体，已内置离线文件）
```css
--font-display: 'Lora', 'DM Serif Display', serif    /* 大标题 */
--font-heading: 'Poppins', 'DM Sans', sans-serif      /* UI/按钮/导航 */
--font-body:    'Lora', 'DM Serif Text', serif         /* 正文 */
--font-mono:    'JetBrains Mono', monospace
```

**间距**（4px 网格，常用值）
```css
--space-4: 16px  --space-6: 24px  --space-8: 32px
--space-10: 40px  --space-16: 64px  --space-32: 128px
```

引入方式：在 HTML `<head>` 中先 `fonts.css` 后 `assets/base.css`，顺序不能颠倒。中文项目在两者之前额外引入中文字体 CDN，见 `typography-cn.md`。

---

## 组件系统

**43 个完整组件已内置，使用已有的，不要重新发明。**

查找流程：`components/index.md` → 找对应分类文件 → 复制代码按需修改。

快速查找：

| 需要 | 去哪里 |
|------|-------|
| 按钮 / 卡片 / 代码块 / 骨架屏 | `components/basics.md` |
| 侧边栏 / 标签页 / 下拉菜单 | `components/navigation.md` |
| 表单 / 开关 / 弹窗 / 抽屉 | `components/forms.md` |
| 表格 / 时间线 / 空状态 / 步骤条 | `components/display.md` |
| 搜索 / ⌘K / 进度条 / 轮播 / FAB | `components/overlay.md` |
| 步进器 / 单选 / 上传 / 评分 / 通知 | `components/feedback.md` |
| 对话界面 / 消息流 / 输入框 | `components/chat.md` |
| KPI 卡片 / 图表 / 实时数据 | `dashboard.md` |

---

## 核心设计规范

### 颜色用法

- 页面底色用 `--color-bg-base`，不用 `#FFFFFF`
- 主 CTA 用橙色，不用蓝色/紫色
- 单页最多 5 种颜色（含黑白）
- 禁止：蓝紫渐变（`#6366F1→#8B5CF6`）、荧光色、高饱和彩色

### 字体用法

- 标题：Lora（`var(--font-display)`）
- UI/按钮：Poppins（`var(--font-heading)`）
- 正文：Lora（`var(--font-body)`）
- 禁止：Inter、Roboto、Open Sans、Space Grotesk（官方明确禁用）

### 动效约束

- 只动 `transform` 和 `opacity`，不动 `width`/`height`/`top`/`left`
- 时长：`--duration-fast` (150ms) / `--duration-normal` (250ms) / `--duration-slow` (400ms)
- easing：`--ease-default: cubic-bezier(0.16, 1, 0.3, 1)`
- **HTML/CSS 项目**：优先 CSS 动画，用 `animation-delay` 实现 Stagger（错落延迟 80ms/条）
- **React 项目**：使用 Motion library（`import { motion } from 'motion/react'`），比 CSS 更精确，支持 `useInView` 和 `whileHover`
- 标志效果：内容自下而上淡入（`translateY(24px) → 0`），各元素 80ms 错落

### 背景与视觉深度

纯色背景显得廉价——背景应该**创造氛围和深度**（来自官方 frontend-design skill）。

**技法菜单**（按需选用，同一项目选 1-2 种）：

```css
/* A. 径向渐变晕染（最常用）*/
background:
  radial-gradient(ellipse 80% 60% at 20% 10%, rgba(217,119,87,0.06) 0%, transparent 60%),
  var(--color-bg-base);

/* B. Gradient Mesh（多点晕染，更丰富）*/
background-image:
  radial-gradient(at 20% 20%, rgba(217,119,87,0.08) 0px, transparent 50%),
  radial-gradient(at 80% 10%, rgba(106,155,204,0.06) 0px, transparent 45%),
  radial-gradient(at 50% 80%, rgba(120,140,93,0.05) 0px, transparent 50%);

/* C. 噪点纹理 / Grain Overlay */
background-image: url("data:image/svg+xml,...feTurbulence...");

/* D. 几何图案（Geometric Pattern）*/
background-image: url("data:image/svg+xml,...stroke='%23D8D5CC'...");

/* E. 装饰边框（Decorative Border）*/
.section { border: 1px solid var(--color-border-subtle); border-radius: var(--radius-xl); }

/* F. 自定义光标（Custom Cursor，品牌增强模式）*/
body { cursor: url('cursor.svg'), auto; }
```

**深度分层**：用多层叠透明度（layered transparencies）制造 Z 轴感——背景层 opacity 0.06、中间层 0.4、前景层 1.0。

**区块节奏**：`[米色 Hero] → [浅色内容] → [深色区块] → [米色内容] → [深色 Footer]`

### 空间构图（来自官方 frontend-design skill）

Anthropic 风格拒绝平庸的居中对齐，通过以下手段制造视觉张力：

- **非对称**：主内容与辅助内容用黄金比例（1:1.618），而非 50/50
- **重叠**：允许元素轻微溢出容器边界，`margin-inline: calc(var(--space-16) * -1)`
- **对角流**：倾斜分隔线用 `clip-path: polygon(0 0, 100% 5%, 100% 100%, 0 95%)`
- **突破网格**（Grid-breaking）：装饰图形故意超出内容区
- **大留白 OR 高密度**：二选一，拒绝中间状态

### 可访问性

- 正文对比度 ≥ 4.5:1（WCAG AA）
- 焦点环：橙色 `box-shadow: 0 0 0 3px rgba(217,119,87,0.35)`，不用默认蓝色
- 触摸区域：移动端最小 44×44px
- 语义 HTML：`<button>` 做按钮，`<nav>` 包导航，`<main>` 标主内容
- 减少动画：`@media (prefers-reduced-motion: reduce)` 覆盖所有动效

---

## 反模式（禁止）

| 禁止 | 原因 |
|------|------|
| 纯白 `#FFFFFF` 底色 | 缺乏温度感 |
| 蓝紫渐变 Hero | AI SaaS 陈词滥调（官方明确禁止）|
| Inter / Roboto / Space Grotesk | 官方明确禁用，overused font families |
| 橙色以外的颜色做主 CTA | 破坏品牌一致性 |
| 所有标题用无衬线 | 丧失叙事温度 |
| 危险操作用克制灰色 | 误导用户感知风险 |
| **每次生成相同布局结构** | 官方明确禁止：「No design should be the same」|
| **收敛到 Space Grotesk + 紫色渐变** | 官方明确点名的「generic AI aesthetics」|
| 为了「好看」牺牲信息密度 | 美感优先级最低 |
| 极简设计用「代码少」来合理化 | 实现复杂度必须匹配美学强度 |

---

## 系统规范索引

详细规范见 `references/systems.md`（11 节）：

| 节 | 内容 |
|----|------|
| 1 | Z-index 分层（`--z-*` Token，禁止硬编码）|
| 2 | 响应式断点 + 移动端（Mobile First，44px 触摸区）|
| 3 | 暗色模式（`[data-theme]` + localStorage + 防 FOUC）|
| 4 | 动画性能（only transform/opacity，will-change 边界）|
| 5 | 焦点陷阱（Modal/Drawer/⌘K 必须实现）|
| 6 | SVG 图标系统（currentColor + unicode-range）|
| 7 | 字体加载策略（preload + font-display:swap + 防闪）|
| 8 | 滚动行为（背景锁定无跳动 + overscroll-behavior）|
| 9 | 表单验证（blur 触发 + submit 全量 + 聚焦首错）|
| 10 | 图片优化（WebP + srcset + LCP 禁 lazy）|
| 11 | 上下文感知 Token + 流程连续性 + 报警场景 |

---

## 中文界面

中文项目需额外引入中文字体，字体栈中英文字体必须写在中文字体前（利用 unicode-range 自动分流）。详细方案见 `typography-cn.md`，包含 CDN/本地子集化/系统字体三种选择。

中文排版关键点：行高用 `--leading-cn-normal: 1.75`（不是英文的 1.55）；标题字重用 400（不是 700）；标点用 `text-spacing-trim` 压缩。

---

## 输出质量检查

**简单任务判断**：独立组件 / 只改文字 / 只调颜色 → 读 `SKILL-lite.md` 即可，无需读全文档。

完整任务输出前，快速核对（简单任务可跳过）：

- 颜色、字体、间距、z-index 都用了 Token，没有硬编码
- 所有可识别的组件都来自 `components/`，没有手写重复实现
- 模式已判断，非默认模式已应用对应调整
- 危险操作用了 `--color-error` 强调
- 单屏视觉层级 ≤ 3

完整自检清单见 `design-rules.md`。
