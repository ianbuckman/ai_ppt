# AI-PPT 项目规则

将文章/文档转换为专业演示文稿的 Claude Code skill 系统。主 skill `ai-ppt` 编排子 skill（style、image、revealjs、extract）通过 Reveal.js 生成单文件 HTML 演示文稿。

## 关键规则（CRITICAL）

### 用户交互

- **MUST 等用户选风格再生成** — Phase 2 的风格选择是强制等待点，不能跳过、不能自动选。因为风格是主观偏好，自动选择必然不符用户预期。
- **不能跳过 Phase 1 内容分析** — 跳过分析直接生成 slide 是质量最大的单点故障。内容分析的深度直接决定最终演示的质量。
- **用户说"随便"选第一推荐，说"直接做"跳过确认** — 尊重用户效率偏好的同时给出合理默认值。

### 内容处理

- **不要把段落转成 bullet points** — 这是 article-to-PPT 转换的 #1 错误。应该提取 key message 并匹配合适的 slide type。
- **每个 slide 只有一个核心信息** — 如果无法用一句话说明这个 slide 的 takeaway，就需要拆分。
- **Slide count 公式：每 150-250 个中文字（或 80-150 英文词）一个 slide** — 防止过多或过少。

### 中文排版

- **NEVER 对中文使用 italic** — CJK italic 几乎不可读，看起来是坏的。用 bold 或 color 替代。
- **标题最多 15 个中文字，正文每行最多 25 个中文字** — 超出会导致 slide 内容溢出或可读性差。
- **行高 1.8-2.0，字间距 0.05em** — 中文字符密度高，需要更多呼吸空间。
- **使用全角中文标点** — ，。：；"…"——？！。代码和英文中用半角。
- **不要翻译品牌名和技术术语** — 保持原文形式，如"台积电的 CoWoS 封装技术"。

### 图片处理

- **NEVER 自动插入 placeholder 图片或 stock photo URL** — 只使用用户明确提供的图片。自动插图是用户最反感的 AI 行为之一。
- **审计图片，不要盲目包含** — 用 Read tool 读取每张图片理解内容，然后 keep/adapt/drop。
- **不要为 thesis/statement、section divider、stat slide 推荐图片** — 这些 slide 的排版本身就是视觉元素。

### 视口适配

- **所有 slide 内容必须在 viewport 内，不能滚动** — 使用 `clamp()` 做响应式缩放。
- **遵守 Content Density Hard Limits** — 如 bullet 最多 5 条、feature grid 最多 6 张卡片，超出就拆 slide。

## 重要规则（IMPORTANT）

### 节奏与结构

- **不能连续 3 个同类型 slide** — 如果有 3 个 bullet slide 连续，把中间的转成其他类型（stat、quote、two-column）。
- **每 4-5 个 slide 加一个 breathing slide** — 插入 section divider、single stat 或 transition slide。
- **开头高冲击力** — 前 2-3 个 slide：cover → hook stat 或 provocative question。不要用无聊的 agenda slide 开场。
- **结尾回扣开头** — 最后一个 slide 应该呼应或回答开头的问题。
- **每个 section 以 "so what" 结尾** — 最后一个 slide 陈述 thesis/implication。

### 风格与视觉

- **Show, don't tell** — 展示 3 个视觉预览让用户选择，不要问抽象问题如"深色还是浅色?"。
- **Anti-"AI Slop"** — 使用 12 个精选的 style preset，避免通用模板化的美学。每个演示应该有独特的手工感。
- **动画匹配情绪** — 财务分析不应该 bounce，创意 pitch 不应该用 corporate 0.2s fade。
- **数据 slide 要 dramatic** — 当数字令人印象深刻时，给它一个整页 slide 加大字体。不要把"71% 市占率"埋在 bullet list 里。

### Reference 加载

- **生成前必须加载 reference 文件** — revealjs-syntax.md + viewport-base.css。这些包含经过测试的精确代码模式。
- **中文内容加载 chinese-typography.md** — 包含字体栈、间距、标点等完整规则。

### Reveal.js 特定

- **viewport-base.css MUST be included** — 确保所有 slide 在标准视口内正确显示。

## 已知失败模式

从实际使用中积累的教训。每次发现新问题，在这里加一行。

| 日期 | 问题 | 修正规则 |
|------|------|----------|
| - | *(模板 — 从后续使用中积累)* | - |

## 开发约定

- 每个 SKILL.md 控制在 500 行以内，超出拆到 `references/`
- 子 skill 不独立触发，只由主 skill 编排调用
