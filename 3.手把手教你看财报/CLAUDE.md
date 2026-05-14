# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在本仓库中工作时提供指导。

**全局规则：所有交流、注释、说明一律使用中文。**

## 项目概述

本仓库是一份中文财务教育类电子书——"手把手教你读财报"的 HTML 章节合集。每个章节为独立的单页 HTML 文件，内含内联 CSS 和 JavaScript，无需构建工具或框架。

## 排版与视觉规范（必须遵循）

所有新建章节必须严格参照 `第二章 投资高手关心的表 —— 资产负债表.html` 的风格。以下是核心规范：

### 全局设定

- `lang="zh-CN"`，charset UTF-8
- body 字体：serif 族（`"Noto Serif SC", "Source Han Serif CN", "Songti SC", Georgia, "Times New Roman", serif`），字号 17px，行高 1.9，两端对齐（`text-align: justify`）
- 背景色 `#f7fafc`，正文色 `#1a202c`

### CSS 变量体系

必须使用 `:root` 中定义的语义化变量，不要硬编码颜色：

| 变量 | 用途 |
|------|------|
| `--primary` / `--primary-light` | 主色调深蓝，用于 h2/h3、表头渐变、hero 背景 |
| `--accent` / `--accent-light` | 橙色强调，h3 左边框、行内 code 背景 |
| `--success` / `--success-light` | 绿色，正面评价、成功提示 |
| `--warning` / `--warning-light` | 红色，风险警告、负面评价 |
| `--info` / `--info-light` | 蓝色，知识提示 |
| `--tip` / `--tip-light` | 紫色，阅读技巧 |
| `--example-bg` | 黄色 `#fefcbf`，案例分析背景 |

### 页面布局结构

```
├── .hamburger（移动端菜单按钮，默认隐藏）
├── .overlay（遮罩层）
├── aside.sidebar（固定左侧导航栏 300px，深蓝渐变背景）
│   ├── .logo / .logo-sub
│   └── nav > a（支持 .indent / .indent2 层级缩进）
├── button.scroll-top（回到顶部按钮）
└── main.main
    ├── .hero（章节头图区域，渐变蓝底白字）
    │   ├── .chapter-label（如 "Chapter 02"）
    │   ├── h1（章节标题）
    │   └── .subtitle（副标题）
    └── 章节内容（h2 > h3 > h4 层级）
```

### 标题层级

| 标签 | 字号 | 样式特征 |
|------|------|----------|
| h2 | 28px | 底部 3px 实线 `--primary`，作为大节标题 |
| h3 | 22px | 左侧 4px 实线 `--accent`，作为小节标题 |
| h4 | 19px | 无装饰，正文色 |
| h5 | 17px | 次要色 `--text-secondary` |

### 内容组件（必须使用 class 调用）

**提示框**（5 种语义类型，均使用 `.callout` + 类型 class）：
- `.callout.warning`（红色）：风险警示、造假手法揭露
- `.callout.tip`（紫色）：阅读技巧、实操建议
- `.callout.info`（蓝色）：准则更新、知识补充
- `.callout.example`（黄色）：典型案例、简化数字例子
- `.callout.success`（绿色）：总结、正面结语

每个 callout 内部必须有 `.callout-title`，格式如：
```html
<div class="callout warning">
  <div class="callout-title">⚠ 警告标题</div>
  <p>内容</p>
</div>
```

**表格**：必须包裹在 `.table-wrapper` 中，表头使用 `--primary` 渐变背景，数字列使用 `class="num"`（等宽右对齐），合计行加粗并带浅蓝背景。

**卡片网格** `.card-grid`：用于并列展示 3 个相关概念，每张 `.card` 可通过 `.orange` / `.green` / `.purple` 改变顶部色条。

**对比行** `.compare-row`：2 列布局用于正反对比，每列 `.compare-item` 通过 `style="border-top:4px solid var(--success)"` 或 `var(--warning)` 区分正负面。

**步骤流程** `.step-flow`：用于展示操作步骤或递进关系，内部每步为 `.step` > `.step-marker`（圆形编号）+ `.step-content`。

**公式块** `.formula`：居中、黄色虚线边框、粗体，用于展示计算公式。

**徽章** `.badge`：行内彩色标签，5 种颜色 `.red` `.blue` `.green` `.orange` `.purple`，用于标注要点性质。

**引用** `blockquote`：渐变紫蓝背景、左侧 5px 蓝色边框、斜体，用于名人名言或重要论述。

**分隔线** `.divider`：渐变淡出效果，用于大节之间。

### 侧边栏导航

- 顶级链接无缩进，节标题用 `.indent`，子标题用 `.indent2`
- 节与节之间用 `<div class="divider"></div>` 分隔
- href 使用页面内锚点 `#id`，不要使用外部文件路径

### 响应式断点

- `1024px`：侧边栏隐藏，显示汉堡菜单，主内容去左内边距
- `600px`：字号缩小至 15.5px，卡片网格变单列，表格紧凑

### 交互 JavaScript

- 汉堡菜单切换 `.open` class
- 滚动超过 400px 显示回到顶部按钮
- 滚动时自动高亮当前可见 section 对应的侧边栏链接

## 内容写作规范

- 正文中关键术语用 `<strong>` 加粗
- 数字示例使用简化数字便于理解（如万元、亿元）
- 每个小知识点结构：定义 → 会计处理 → 投资者关注点 → 风险提示
- 使用 `<small>` 标签在卡片中展示补充说明
- 表格中"投资者关注点"或"注意"列单独呈现，不混在定义列中

## 文件命名规范

`第X章 章节标题 —— 副标题.html`（中文命名，空格和破折号分隔）
