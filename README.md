# anthropic-style-cn

Anthropic 风格中文前端设计规范 — AgentSkill

## 特性

- 🎨 **完整 Token 系统**：颜色/字体/间距/动效/阴影，开箱即用
- 🧩 **42 个组件**：HTML+CSS+JS，按需分类加载
- 🎯 **4 种场景模式**：数据密集 / 工具优先 / 品牌增强 / 默认
- 🔤 **离线字体**：Poppins（Heading）+ Lora（Body）+ DM 系列
- 🇨🇳 **中文排版**：霞鹜文楷 + 思源黑体混排方案
- 📊 **Dashboard 规范**：KPI 卡片/图表配色/表格进阶
- 💬 **Chat UI**：完整对话界面实现

## 使用

将 `anthropic-style-cn/` 目录放入你的 Agent skills 目录：

```
your-workspace/
└── skills/
    └── anthropic-style-cn/
        ├── SKILL.md          ← 主入口
        ├── SKILL-lite.md     ← 精简版（简单任务）
        ├── assets/
        │   ├── base.css      ← CSS Token
        │   └── fonts/        ← 离线字体
        └── references/
            ├── components/   ← 42 个组件（7 个分类）
            ├── design-rules.md
            ├── design-patterns.md
            ├── systems.md
            ├── dashboard.md
            ├── logo.md
            └── typography-cn.md
```

## 组件索引

| 分类 | 组件 |
|------|------|
| basics | Hero、Feature Grid、Stats、Blockquote、Pricing、CTA Dark、Code Block、Toast、Skeleton |
| navigation | Sidebar、Tabs、Breadcrumb、Pagination、Dropdown |
| forms | Form、Toggle/Switch、Tooltip、Modal、Accordion |
| display | Table、Timeline、Empty State、Banner/Alert、Step Indicator |
| overlay | Avatar、Progress、Search、Command Palette、Drawer、Chip/Tag、Popover、Carousel、Context Menu、FAB |
| feedback | Number Stepper、Radio Group、File Dropzone、Segmented Control、Status Indicator、Rating、Notification |
| chat | 完整 Chat UI |

## 设计模式

收到任务后按关键词选择模式：

| 关键词 | 模式 |
|--------|------|
| dashboard / 监控 / 图表 | 数据密集 |
| admin / 后台 / 管理系统 | 工具优先 |
| landing / 落地页 / 品牌 | 品牌增强 |
| 其他 | 默认 |

## License

MIT
