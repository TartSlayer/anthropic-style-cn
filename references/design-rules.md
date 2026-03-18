# 设计操作规则

SKILL.md 的决策机制详细版。当需要完整规则参考时读取本文件。

---

## 目录
1. [组件强制使用清单](#components)
2. [模式隔离细则](#mode-isolation)
3. [生成后自检清单](#self-check)
4. [局部修复规则](#local-fix)
5. [风格冲突处理](#conflict)
6. [受控随机性](#randomness)
7. [上下文感知 Token 调整](#contextual-tokens)

---

## 1. 组件强制使用清单 {#components}

以下组件已有完整实现，直接从对应分类文件复制代码，不要重新创建：

| 分类文件 | 组件 |
|---------|------|
| `components/basics.md` | Hero、Feature Grid、Stats、Blockquote、Pricing、CTA Dark、Code Block、Toast、Skeleton |
| `components/navigation.md` | Sidebar、Tabs、Breadcrumb、Pagination、Dropdown |
| `components/forms.md` | Form、Toggle/Switch、Tooltip、Modal、Accordion |
| `components/display.md` | Table、Timeline、Empty State、Banner/Alert、Step Indicator |
| `components/overlay.md` | Avatar、Progress Bar/Ring、Search、Command Palette、Drawer、Chip/Tag、Popover、Carousel、Context Menu、FAB |
| `components/feedback.md` | Number Stepper、Radio Group、File Dropzone、Segmented Control、Status Indicator、Rating、Notification |
| `components/chat.md` | Chat UI（完整对话界面） |

**唯一例外**：用户明确要求全新设计某个组件，且说明原有版本不适用。

---

## 2. 模式隔离细则 {#mode-isolation}

进入非默认模式后，对应的默认规则调整如下——这样做是为了避免「默认模式的审美惯性」污染其他模式的输出。

### 数据密集模式下的调整

| 规则 | 默认值 | 数据密集值 | 原因 |
|------|--------|-----------|------|
| 区块间距 | min 64px | min 40px | 信息密度优先 |
| 卡片内距 | min 24px | min 16px | 节省空间 |
| 单区块信息块 | max 2 个 | 不限 | 数据展示需要 |
| 背景处理 | 渐变+噪点 | 纯色分层 | 减少视觉干扰 |

### 工具优先模式下的调整

- 留白底线：不适用，密度优先
- 受控随机性要求：暂停（功能一致性优先）
- 过渡动画：默认关闭（除非用户明确要求）
- 标题字体：全用无衬线（扫读优先，衬线体不利于快速浏览）

### 品牌增强模式下新增许可

- 受控渐变：允许（限 2 色，不超过 3 个渐变区域）
- 强调色饱和度：可提高（保持在品牌色系内）
- 动效：可以更丰富（仍遵守 GPU 动画规范）
- 仍然禁止：霓虹色、蓝紫渐变白底、Inter/Roboto/Space Grotesk 字体

---

## 3. 生成后自检清单 {#self-check}

完整任务输出前逐条确认（简单任务可跳过）：

**Token 合规**
- [ ] 颜色全部用 `var(--color-*)`，无硬编码十六进制
- [ ] 字体全部用 `var(--font-*)`，无写死字体名
- [ ] 间距全部用 `var(--space-*)`，无写死 px 值
- [ ] z-index 全部用 `var(--z-*)`，无硬编码数字

**组件合规**
- [ ] 可识别的 UI 元素都用了上方清单中的版本
- [ ] 没有用 `<div class="card">` 手写自制卡片
- [ ] 没有用裸 `<button>` 手写无样式按钮

**模式合规**
- [ ] 已按关键词判断了模式
- [ ] 非默认模式下已应用了对应的调整
- [ ] 危险操作（删除/覆盖）用了 `.btn-danger`，不是克制灰色

**信息层级**
- [ ] 单屏视觉层级 ≤ 3
- [ ] 相邻区块间距符合当前模式的留白规则

任意一项不通过 → 修改后再输出。

---

## 4. 局部修复规则 {#local-fix}

用户要求修改时，先定位问题范围，再做最小改动——这样做是为了避免「改一处、坏全局」。

**识别修改范围**
- 只改文字内容 → 只替换文字，不动结构和样式
- 只改某个组件 → 只替换该组件，其余不变
- 布局整体问题 → 重新判断模式，只调整布局层，保留组件代码

**定位失败规则**

说出是哪条规则没执行到位，例如：「间距用了硬编码 px」「没有用 Modal 组件而是手写的」「模式判断错误」。

**只改不合规的部分**

```
用户说「按钮颜色不对」→ 只改按钮的颜色 Token
用户说「留白太多」    → 只改那个间距变量，不重写布局
用户说「风格不对」   → 重新走模式判断，调整整体密度和色调
```

---

## 5. 风格冲突处理 {#conflict}

用户需求与 Anthropic 风格冲突时（如「炫酷科技风」「高对比霓虹」）：

品牌色和字体系统是底线，不妥协。但用户想要的「视觉张力」可以通过其他手段表达：背景质感、几何图案、动效、大字号对比、非对称布局。

当完全无法使用标准 Token 时（如嵌入第三方深色界面），保持「设计意图」而不是死守数值：
- 无法用 `#ECE9E0` → 找最接近的暖米色，保持「温暖有机」气质
- 无法用 Poppins → 找等宽感接近的无衬线体，保持「克制现代」气质
- 无法用橙色 CTA → 用同色系暖色，保持「不冷不炫」气质

**形散神不散。**

---

## 6. 受控随机性 {#randomness}

每次生成的界面加入一个「非标准元素」，防止所有输出趋同。这个原则来自官方 frontend-design skill——重复的布局模式会让 AI 输出失去辨识度。

可以是：
- 非对角分割的布局（而非全部居中对齐）
- 突破容器边界的装饰图形
- 意外的字号对比（极大标题 + 极小说明）
- 不寻常的负空间分配

工具优先模式下暂停此要求（功能一致性更重要）。

---

## 7. 上下文感知 Token 调整 {#contextual-tokens}

同一个 Token 在不同场景下的「视觉权重」不同，根据模式做相应调整：

| Token | 默认模式 | 数据密集 | 工具优先 |
|-------|---------|---------|---------|
| 边框 | `--color-border-subtle` | `--color-border-default` | `--color-border-default` |
| 卡片间距 | `--space-8` (32px) | `--space-5` (20px) | `--space-4` (16px) |
| 区块间距 | `--space-16` (64px) | `--space-10` (40px) | `--space-8` (32px) |
| 辅助文字 | `--text-sm` (14px) | `--text-xs` (12px) | `--text-xs` (12px) |
| 卡片阴影 | hover 时显示 | 常态显示 | 无阴影（用边框替代）|
