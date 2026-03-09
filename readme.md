# ai-ppt

一个 Claude Code Skill，将文章、文档、PPT 自动转换为专业演示文稿。

## 功能

- **文章转幻灯片** — 输入 Markdown、纯文本或 URL，自动分析内容结构并生成演示文稿
- **PPT 转网页** — 将 .pptx 文件转换为可在浏览器中展示的网页演示文稿
- **三种输出框架**：
  - **SlideDev** — Markdown 驱动，适合技术演讲、含代码和图表的场景
  - **Reveal.js** — 单 HTML 文件 + CDN，通用演示场景
  - **纯 HTML** — 零依赖，完全自包含，直接浏览器打开
- **12 套视觉风格预设** — 涵盖暗色/亮色/特殊主题，每套包含完整的配色、字体和动画方案
- **15 种幻灯片类型** — Cover、Statement、Timeline、Comparison、Diagram 等
- **中文排版优化** — 专门的 CJK 字体加载、间距和标点规则

## 使用方式

在 Claude Code 中对话即可触发，例如：

```
把这篇文章做成PPT
生成演示文稿
make a presentation from this article
```

Skill 会引导你完成两个决策点：

1. 选择输出框架（SlideDev / Reveal.js / 纯 HTML / 自动推荐）
2. 选择视觉风格（根据内容情绪推荐 3 个选项）

然后自动生成完整的演示文稿文件。

## 示例项目

`projects/tsmc/` 包含一个完整示例 — 将台积电商业分析文章转换为演示文稿：

```bash
# 运行 SlideDev 版本
npm run dev:tsmc

# 构建 SlideDev 版本
npm run build:tsmc

# Reveal.js 版本直接浏览器打开
open projects/tsmc/index.html
```

## 项目结构

```
.claude/skills/ai-ppt/
├── SKILL.md                    # Skill 定义
└── references/
    ├── slide-type-catalog.md   # 15 种幻灯片类型实现参考
    ├── style-presets.md        # 12 套视觉风格预设
    ├── animation-patterns.md   # 6 种动画情绪模式
    ├── slidev-syntax.md        # SlideDev 语法参考
    ├── revealjs-syntax.md      # Reveal.js 语法参考
    ├── html-template.md        # 纯 HTML 模板
    ├── chinese-typography.md   # 中文排版规则
    ├── viewport-base.css       # 响应式视口基础样式
    └── extract-pptx.py         # PPT 内容提取脚本
projects/                       # 生成的演示文稿项目
```

## 依赖

```bash
npm install
```

PPT 提取功能需要 Python 和 `python-pptx`：

```bash
pip install python-pptx
```
