# 中文排版规范

英文界面的美感来自「衬线体标题 + 无衬线体 UI」的混排策略。
本文件指导如何将同样的策略移植到简体中文界面，并解决中英混排时的视觉割裂问题。

---

## 目录
1. [问题根源](#problem)
2. [字体选型](#font-selection)
3. [引入方式](#import)
4. [CSS Token 覆盖](#tokens)
5. [混排字体栈](#mixed-stack)
6. [中文排版细节](#details)
7. [各平台系统字体兜底](#fallback)

---

## 1. 问题根源 {#problem}

`base.css` 中的 DM Serif Display / DM Serif Text / DM Sans 均为纯 Latin 字体，
遇到中文字符时浏览器会直接回退到操作系统默认字体：

| 平台 | 默认中文字体 | 风格 |
|------|------------|------|
| macOS / iOS | 苹方（PingFang SC） | 现代无衬线，偏冷 |
| Windows | 微软雅黑 | 现代无衬线，偏商务 |
| Android | 思源黑体 / Noto Sans | 中性 |
| Linux | 文泉驿 / Noto | 不稳定 |

这些系统字体本身并无问题，但风格与 Anthropic 的温暖克制感不匹配，
导致中英文混排时出现明显的**字形割裂**。

**其他连带问题：**
- 行高 1.55 对中文偏紧，阅读疲劳（中文推荐 1.7–1.8）
- 正文 14px 在中文下细节损失严重（推荐 ≥ 16px）
- 未声明标点压缩，全角标点导致行首/行尾出现大量空白
- 中英文之间缺少自动间距（汉字与 Latin 字符之间应有约 0.25em 间隔）

---

## 2. 字体选型 {#font-selection}

### 首选方案：霞鹜文楷 + 思源黑体

| 角色 | 字体 | 理由 |
|------|------|------|
| 大标题（display） | **霞鹜文楷**（LXGW WenKai） | 楷体笔画有手写弧度，与 DM Serif Display 的温度感最接近；OFL 开源可商用 |
| 正文（body） | **霞鹜文楷** | 楷体长文舒适，阅读时有叙事感，契合 Anthropic 的人文调性 |
| UI / 按钮 / 标签 | **思源黑体**（Source Han Sans SC） | Adobe + Google 联合出品，多字重，克制现代；OFL 开源 |
| 代码 | JetBrains Mono（不变） | 与英文版一致 |

**霞鹜文楷为何胜出：**
目前中文免费字体中，宋体类（思源宋体）过于正式，黑体类（思源黑体）缺少温度，
只有楷体在笔画上保留了手写的有机弧度——这与 Anthropic 用 Tiempos（带手写感的衬线体）做正文的逻辑完全吻合。

### 备选方案：思源宋体 + 思源黑体（更产品化）

```css
/* 适合 SaaS 控制台、文档站等偏产品向的界面 */
--font-display-cn: 'Source Han Serif SC', 'Noto Serif SC', serif;
--font-body-cn:    'Source Han Serif SC', 'Noto Serif SC', serif;
--font-heading-cn: 'Source Han Sans SC',  'Noto Sans SC',  sans-serif;
```

---

## 3. 引入方式 {#import}

### 方式 A：CDN 引入（推荐，文件较大不宜内置）

```html
<head>
  <!-- 霞鹜文楷 WebFont（约 1–3MB，按需加载） -->
  <link rel="preconnect" href="https://cdn.jsdelivr.net">
  <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.7.0/style.css">

  <!-- 思源黑体：Google Fonts CDN -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="stylesheet"
        href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700&display=swap">

  <!-- 原有字体（Latin） -->
  <link rel="stylesheet" href="assets/fonts/fonts.css">
  <!-- 基础样式 -->
  <link rel="stylesheet" href="assets/base.css">
</head>
```

### 方式 B：本地子集化（推荐，零网络依赖）

适合完全离线、内网部署、对加载速度敏感的项目。原理是用 `fonttools` 自动扫描项目源码中实际出现的汉字，只裁剪这些字生成极小的 woff2 文件。

```bash
# 安装 fonttools
pip install fonttools brotli

# 第一步：自动提取项目中所有中文字符
find src/ -name "*.vue" -o -name "*.js" -o -name "*.ts" -o -name "*.html" | xargs cat | \
  python3 -c "
import sys, re
text = sys.stdin.read()
chars = set(re.findall(r'[\u4e00-\u9fff\u3000-\u303f\uff00-\uffef\u2018-\u201d]', text))
print(''.join(sorted(chars)))
" > used-chars.txt

# 第二步：裁剪字体（需先下载原始 TTF/OTF 文件）
python3 -m fontTools.subset LXGWWenKai-Regular.ttf \
  --text-file=used-chars.txt --flavor=woff2 \
  --output-file=assets/fonts/LXGWWenKai-subset.woff2

python3 -m fontTools.subset NotoSansSC-Regular.otf \
  --text-file=used-chars.txt --flavor=woff2 \
  --output-file=assets/fonts/NotoSansSC-subset.woff2
```

**典型结果：** 500 汉字 → 霞鹜文楷 ~100KB + 思源黑体 ~80KB，合计约 190KB。

```css
/* 在 assets/fonts/fonts.css 中追加，或单独建 chinese-fonts.css */
@font-face {
  font-family: 'LXGW WenKai';
  font-weight: 400;
  font-display: swap;
  src: url('./LXGWWenKai-subset.woff2') format('woff2');
}
@font-face {
  font-family: 'Noto Sans SC';
  font-weight: 400;
  font-display: swap;
  src: url('./NotoSansSC-subset.woff2') format('woff2');
}
```

**局限性：** 仅覆盖扫描时出现的汉字。如果后续新增中文内容，需重新运行裁剪脚本。动态渲染的中文（如从接口拉取的内容）无法被扫描到，建议对动态内容区域额外追加系统字体兜底（见方式 C）。

### 方式 C：系统字体优先（极简，零加载）

无需任何网络请求，直接使用各平台内置的最佳中文字体。适合对加载速度要求极高、或字体品质要求不高的场景。

```css
:root {
  --font-display-cn:  'Songti SC', 'STSong', 'SimSun', serif;
  --font-heading-cn:  'PingFang SC', 'Hiragino Sans GB',
                      'Microsoft YaHei UI', 'Microsoft YaHei', sans-serif;
  --font-body-cn:     'Songti SC', 'STSong', 'SimSun', serif;
}
```

**缺点：** macOS/iOS 效果好（苹方/宋体），Windows 宋体较老气，各平台视觉一致性差。

### 方式 A 的运行时降级（CDN 加载失败时）

使用方式 A 的项目，如果用户网络无法访问 CDN，浏览器会自动按字体栈顺序降级。`base.css` 中已定义完整的降级链，**无需额外处理**：

```css
/* base.css 已内置，CDN 失败时自动生效 */
--font-display-cn:  'Lora',          /* ① CDN 成功时用 Lora（Latin） */
                    'LXGW WenKai',   /* ② CDN 成功时用霞鹜文楷（中文） */
                    'Songti SC',     /* ③ CDN 失败 → macOS 宋体 */
                    'STSong',        /* ④ macOS 旧版 */
                    'SimSun',        /* ⑤ Windows 宋体 */
                    serif;           /* ⑥ 终极兜底 */
```

唯一需要注意的是 `font-display: swap`（fonts.css 中已设置）——CDN 字体加载失败时，浏览器会立即切换到下一个可用字体而不是显示空白，用户体验不会中断。

---

## 4. CSS Token 覆盖 {#tokens}

以下内容追加到 `base.css` 的 `:root` 中，补充中文专用 Token：

```css
:root {
  /* ── 中文字体族 ── */
  --font-display-cn:  'LXGW WenKai', 'DM Serif Display',
                      'Songti SC', 'STSong', serif;
  --font-body-cn:     'LXGW WenKai', 'DM Serif Text',
                      'Songti SC', 'STSong', serif;
  --font-heading-cn:  'Noto Sans SC', 'Source Han Sans SC', 'DM Sans',
                      'PingFang SC', 'Microsoft YaHei', sans-serif;

  /* ── 中文行高（比英文高 ~15%）── */
  --leading-cn-tight:  1.4;   /* 大标题 */
  --leading-cn-snug:   1.6;   /* 小标题 */
  --leading-cn-normal: 1.75;  /* 正文 */
  --leading-cn-loose:  1.9;   /* 长文阅读 */

  /* ── 中文字号下限提升 ── */
  --text-cn-sm:   0.9375rem; /* 15px，替代原 14px */
  --text-cn-base: 1rem;      /* 16px，中文正文最小值 */
  --text-cn-md:   1.125rem;  /* 18px */
}
```

---

## 5. 混排字体栈 {#mixed-stack}

### 核心原理

`assets/fonts/fonts.css` 中的 DM Sans / DM Serif 已声明 `unicode-range: U+0000-00FF`（只覆盖 Latin 字符）。浏览器遇到中文字符时会自动跳过这些字体，往后找中文字体——这是混排生效的关键。

**字体栈顺序必须是：英文字体在前，中文字体在后。**

```css
/* ── UI 元素（按钮、标签、导航）── */
.ui-element {
  font-family:
    'DM Sans',              /* ① Latin 字符（有 unicode-range，自动跳过中文） */
    'Noto Sans SC',         /* ② 中文字符首选 */
    'PingFang SC',          /* ③ macOS 系统兜底 */
    'Microsoft YaHei',      /* ④ Windows 系统兜底 */
    sans-serif;             /* ⑤ 通用兜底 */
}

/* ── 正文段落 ── */
.body-copy-cn {
  font-family:
    'DM Serif Text',        /* ① Latin 字符（有 unicode-range） */
    'LXGW WenKai',          /* ② 中文字符首选（楷体，温暖有机） */
    'Songti SC',            /* ③ macOS 系统宋体兜底 */
    'SimSun',               /* ④ Windows 系统宋体兜底 */
    serif;
}

/* ── 大标题 ── */
.hero-title-cn {
  font-family:
    'DM Serif Display',     /* ① Latin */
    'LXGW WenKai',          /* ② 中文：楷体大标题有叙事感 */
    'Songti SC',
    serif;
}
```

### 验证混排是否生效

```html
<!-- 在浏览器中检查这行文字的字体渲染 -->
<p style="font-family:'DM Sans','Noto Sans SC',sans-serif">
  Claude AI · 人工智能 · Hello World
</p>
<!-- 
  期望：
  "Claude AI" 和 "Hello World" → DM Sans
  "·"（中点）和 "人工智能" → Noto Sans SC
  开发者工具 → 元素 → 计算样式 → 字体族 可以确认
-->
```

### 中英文之间的自动间距

主流浏览器（Chrome/Safari）会自动在 CJK 字符与 Latin 字符之间插入约 0.25em 的间距。
如需手动控制：

```css
/* 方法：用伪元素在边界插入窄空格 */
/* 实际项目中通常依赖自动行为，无需手动处理 */

/* 如果需要禁用自动间距（罕见需求）： */
.no-auto-spacing {
  font-variant-east-asian: normal;
  text-spacing: normal;
}
```

---

## 6. 中文排版细节 {#details}

### 标点压缩

全角标点（，。！？；：""『』）默认占一个全角宽度，连续出现时行尾/行首会出现大块空白。
用 `text-spacing-trim` 压缩（现代浏览器支持）：

```css
.body-copy-cn {
  /* 标准写法（Chrome 119+，Safari 17.4+） */
  text-spacing-trim: trim-start allow-end;

  /* 旧版兼容写法 */
  text-spacing: ideograph-alpha ideograph-numeric;

  /* 悬挂标点：允许句末标点轻微突出行边界 */
  hanging-punctuation: allow-end;
}
```

### 换行规则

```css
.body-copy-cn {
  /* 中文自动换行（不在字符中间断）*/
  word-break: normal;
  overflow-wrap: break-word;

  /* 禁止孤字（最后一行不能只有一个字）*/
  text-wrap: pretty;          /* 现代浏览器 */
  orphans: 2; widows: 2;      /* 多列/分页兜底 */

  /* 行末标点不悬空 */
  line-break: strict;
}

/* 标题不拆词 */
.heading-cn {
  word-break: keep-all;       /* 中文词组不在中间断行 */
  overflow-wrap: break-word;  /* 但超长时允许强制换行 */
}
```

### 字重选择

中文字体与 Latin 字体的字重感知不同，同样是 `font-weight: 700` 视觉上中文更「粗」：

```css
/* 中文标题：400 即有足够分量，不需要 700 */
.hero-title-cn  { font-weight: 400; } /* 楷体/宋体 400 = 视觉上 semibold */
.section-title-cn { font-weight: 400; }

/* 中文 UI 标签：500 足够，不用 700 */
.ui-label-cn    { font-weight: 500; }

/* 正文：400 */
.body-copy-cn   { font-weight: 400; }

/* 需要强调时：用颜色或衬线变化，而不是加粗 */
.emphasis-cn    { color: var(--color-accent-orange); } /* 而不是 font-weight: 700 */
```

### 字号与行高搭配表

| 用途 | font-size | line-height | font-family |
|------|-----------|-------------|-------------|
| Hero 大标题 | clamp(2rem, 5vw, 3.5rem) | 1.3 | display-cn |
| 章节标题 | clamp(1.5rem, 3vw, 2.25rem) | 1.4 | display-cn |
| 卡片标题 | 1.25rem (20px) | 1.5 | heading-cn |
| 正文 | 1rem (16px) | 1.75 | body-cn |
| 辅助文字 | 0.9375rem (15px) | 1.7 | heading-cn |
| UI 标签 | 0.875rem (14px) | 1.4 | heading-cn |
| 说明文字 | 0.8125rem (13px) | 1.6 | heading-cn |

### 数字与单位的混排

```css
/* 数字、英文单位用 Latin 字体的 tabular-nums */
.stats-value {
  font-family: 'DM Serif Display', serif; /* Latin 字体处理数字 */
  font-variant-numeric: tabular-nums;
  letter-spacing: -0.02em;
}

/* 例：「处理了 1,234,567 个请求」
   数字部分自动用 DM Sans，中文部分用 Noto Sans SC */
```

---

## 7. 各平台系统字体兜底 {#fallback}

当 CDN 不可用、网络超时时，兜底字体决定最终体验下限：

```css
:root {
  --font-display-cn: 
    'LXGW WenKai',           /* 首选：楷体温暖 */
    'DM Serif Display',       /* Latin 字符 */
    'Songti SC',              /* macOS 系统宋体 */
    'STSong',                 /* macOS 旧版 */
    'AR PL UMing CN',         /* Linux */
    'SimSun',                 /* Windows 宋体（兜底，偏老气）*/
    serif;

  --font-heading-cn:
    'Noto Sans SC',           /* 首选 */
    'Source Han Sans SC',     /* 同源不同名 */
    'DM Sans',                /* Latin 字符 */
    'PingFang SC',            /* macOS / iOS */
    'Hiragino Sans GB',       /* macOS 旧版 */
    'Microsoft YaHei UI',     /* Windows 10+ */
    'Microsoft YaHei',        /* Windows 7+ */
    'WenQuanYi Micro Hei',    /* Linux */
    sans-serif;
}
```

### 字体加载优先级建议

```html
<!-- 关键路径：UI 字体优先预加载，正文字体懒加载 -->
<head>
  <!-- 1. 优先加载 UI 无衬线体（影响首屏布局）-->
  <link rel="preload"
        href="https://fonts.gstatic.com/s/notosanssc/v37/k3kXo84MPvpLmixcA63oeALhLOCT-xWNm8Hqd37g1OkDRZe7lR4sg1IzSy-MNbE9VH8V.0.woff2"
        as="font" type="font/woff2" crossorigin>

  <!-- 2. 正文楷体异步加载（不阻塞渲染）-->
  <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.7.0/style.css"
        media="print" onload="this.media='all'">
</head>
```
