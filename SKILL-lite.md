---
name: anthropic-style-lite
description: |
  Anthropic 风格前端设计规范【精简版】。
  适用于：简单组件、单页面、快速原型、不需要完整系统约束的任务。
  完整版见 SKILL.md（含多模式、自检清单、系统规范）。
---

# Anthropic 风格 · 精简规则

## ① 选模式（先做这一步）

| 任务包含这些词 | 用哪个模式 |
|-------------|----------|
| dashboard / 监控 / 图表 / 数据看板 | **数据密集**：压缩留白，提高对比度 |
| admin / 后台 / 管理系统 / 配置 | **工具优先**：密度优先，功能完整 |
| landing / 官网 / 品牌 / 营销 | **品牌增强**：允许渐变，视觉张力 |
| 其他 | **默认**：留白克制，衬线混排 |

## ② 用 Token（不要硬编码）

```css
/* 颜色 */
--color-bg-base: #ECE9E0        /* 页面底色，不用纯白 */
--color-accent-orange: #D97757  /* 主 CTA，唯一强调色 */
--color-text-primary: #141413   /* 主文字 */
--color-text-secondary: #6B6860 /* 次要文字 */
--color-error: #C0453A          /* 危险/错误 */

/* 字体 */
--font-display: 'Lora', serif           /* 大标题 */
--font-heading: 'Poppins', sans-serif   /* UI/按钮/标签 */
--font-body:    'Lora', serif           /* 正文 */

/* 间距（4px 网格）*/
--space-4: 16px  --space-6: 24px  --space-8: 32px
--space-10: 40px  --space-16: 64px
```

## ③ 用已有组件（不要重新发明）

查 `references/components/index.md`，按需加载对应分类文件。

已有：Button、Card、Form、Modal、Table、Sidebar、Toast、Empty State、Chat UI 等共 43 个。

## ④ 三条铁律

```
1. 页面底色用 var(--color-bg-base)，不用 #FFFFFF
2. 主 CTA 用橙色，不用蓝色/紫色
3. 危险操作（删除/覆盖）用 var(--color-error) 强调，不用克制灰色
```

## ⑤ 输出前检查（30 秒）

- [ ] 有没有硬编码颜色（#xxxxxx）？
- [ ] 有没有重新手写已有组件？
- [ ] 危险按钮是否用了红色？

---

**需要完整规范时（多模式隔离、自检清单、系统规范、Dashboard 专项）→ 读 SKILL.md**
