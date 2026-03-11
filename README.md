# ai-ppt

一个 Claude Code Skill，将文章、文档、PPT 自动转换为专业演示文稿。

## 亮点功能

### 🎨 12 套精选视觉风格 + 实时浏览器预览

不是抽象地问"深色还是浅色"，而是根据文章情绪自动推荐 3 个匹配风格，**在浏览器中打开预览 HTML**，让你切换标签页直观对比后再选择。

| 情绪 | 推荐预设 |
|------|---------|
| 想给人留下深刻印象 | Bold Signal · Electric Studio · Dark Botanical |
| 想让人兴奋/激动 | Creative Voltage · Neon Cyber · Split Pastel |
| 想让人感到平静/信任 | Pastel Geometry · Swiss Modern · Paper & Ink |
| 想激发思考/灵感 | Vintage Editorial · Notebook Tabs · Terminal Green |

### 🖌️ 自定义参考风格提取

提供截图、HTML/CSS 代码或文字描述，自动提取视觉 DNA（配色 / 字体 / 布局 / 动画），生成预览 HTML 后与 3 个相近预设一起在浏览器中打开，方便对比微调。

### 📐 15 种幻灯片类型

Cover · Single Stat · Thesis · Bullet Points · Two-Column · Timeline · Quote · Comparison · Diagram · Image Showcase · Code Block · Section Divider · Summary · CTA · Closing — 根据内容自动选型，保证演示节奏。

### 🎬 6 种动画情绪模式

Professional（快速精准）· Dramatic（慢速宏大）· Techy（发光网格）· Playful（弹性回弹）· Editorial（错落文字）· Calm（柔和渐现）— 自动匹配风格预设。

### 🔧 三种输出框架

- **SlideDev** — Markdown 驱动，适合包含代码和图表的技术演讲
- **Reveal.js** — 单 HTML + CDN，通用演示场景
- **纯 HTML** — 零依赖，完全自包含，直接浏览器打开

### 🀄 中文排版优化

专门的 CJK 字体加载策略、中英文混排间距、标点挤压规则，告别 AI 生成的"洋味排版"。

## 使用方式

在 Claude Code 中对话即可触发：

```
把这篇文章做成PPT
生成演示文稿
make a presentation from this article
```

## 执行流程

```mermaid
flowchart TD
    Start([用户触发 ai-ppt]) --> P0S1[Phase 0.1: 检测输入类型]

    P0S1 --> P0S2

    P0S2{{"💬 Phase 0.2: 框架选择<br/>Reveal.js / SlideDev / 纯HTML / 帮我选"}}

    P0S2 --> P1[Phase 1: 深度内容分析]

    P1 --> P1S1["Step 1: 解析结构<br/>标题层级 → 章节边界<br/>估算 slide 数量"]
    P1S1 --> P1S2["Step 2: 标注内容单元<br/>thesis / data_point / comparison<br/>quote / narrative / concept..."]
    P1S2 --> P1S3["🔧 ai-ppt-image<br/>Step 3: 图片审计 + 充足性分析<br/>Keep / Adapt / Drop → 覆盖率评估"]
    P1S3 -->|充足| P1S4
    P1S3 -->|不足 / 无图片| P15{{"💬 图片建议<br/>表格展示缺口 + 优先级<br/>补充图片 / 用已有 / 部分采纳"}}
    P15 -->|用户提供图片| P1S3b[审计用户提供的图片<br/>与已有图片合并]
    P1S3b --> P1S4
    P15 -->|不补充图片| P1S4
    P1S4["Step 4: 提取关键要素<br/>数据点 / 金句 / 核心论点"]
    P1S4 --> P1S5[Step 5: 构建内容清单]

    P1S5 --> P2

    P2["🔧 ai-ppt-style<br/>Phase 2: 风格发现"]
    P2 --> P2S1["分析文章情绪<br/>Impress / Excite / Calm / Inspire"]
    P2S1 --> P2S1a{{"💬 Phase 2.1: 风格选择<br/>3个预设 + 自定义参考风格"}}

    P2S1a -->|选择预设| P2Preview
    P2S1a -->|自定义参考风格| P2Custom{{"💬 收集参考素材<br/>截图 / HTML·CSS / 文字描述"}}
    P2Custom --> P2Extract["提取视觉 DNA<br/>配色 / 字体 / 布局 / 动画"]
    P2Extract --> P2GenPreview["生成自定义风格预览<br/>style-preview.html"]
    P2GenPreview --> P2PreviewCustom

    P2Preview["🌐 自动打开浏览器<br/>打开 3 个预设风格预览 HTML"]
    P2Preview --> P2Pick{{"💬 对比预览，选择最终风格<br/>切换标签页查看"}}

    P2PreviewCustom["🌐 自动打开浏览器<br/>自定义预览 + 3 个相近预设预览"]
    P2PreviewCustom --> P2PickCustom{{"💬 选择最终风格<br/>自定义 / 预设A / 预设B / 预设C"}}

    P2Pick --> P2S2
    P2PickCustom --> P2S2

    P2S2[确定动画情绪]
    P2S2 --> P2S3["设计 Slide 序列<br/>应用节奏规则"]
    P2S3 --> P2S5{{"💬 Phase 2.5: 确认 Slide Plan<br/>这个结构可以吗？"}}

    P2S5 --> P3{根据框架选择}

    P3 -->|SlideDev| SlideDev["🔧 ai-ppt-slidev<br/>生成 slides.md + package.json"]
    P3 -->|Reveal.js| Revealjs["🔧 ai-ppt-revealjs<br/>生成 index.html（CDN）"]
    P3 -->|纯 HTML| Html["🔧 ai-ppt-html<br/>生成零依赖 HTML"]

    SlideDev --> P4
    Revealjs --> P4
    Html --> P4

    P4["Phase 4: 质量检查<br/>内容 / 节奏 / 视觉 / 图片 / 框架"]
    P4 --> Deliver[交付 + 运行说明]
    Deliver --> Iterate{{"💬 需要调整吗？<br/>增减幻灯片 / 换风格 / 调动画"}}

    style P0S2 fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P15 fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P2S1a fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P2Custom fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P2Pick fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P2PickCustom fill:#FFE4B5,stroke:#FF8C00,color:#000
    style P2S5 fill:#FFE4B5,stroke:#FF8C00,color:#000
    style Iterate fill:#FFE4B5,stroke:#FF8C00,color:#000

    style P1S3 fill:#E0F0FF,stroke:#4361EE,color:#000
    style P2 fill:#E0F0FF,stroke:#4361EE,color:#000
    style SlideDev fill:#E0F0FF,stroke:#4361EE,color:#000
    style Revealjs fill:#E0F0FF,stroke:#4361EE,color:#000
    style Html fill:#E0F0FF,stroke:#4361EE,color:#000

    style P2Preview fill:#D4EDDA,stroke:#28A745,color:#000
    style P2PreviewCustom fill:#D4EDDA,stroke:#28A745,color:#000
    style P2GenPreview fill:#D4EDDA,stroke:#28A745,color:#000
```

> 🟧 橙色节点 = 用户交互点 &nbsp; 🔵 蓝色节点 = 子 skill 调用 &nbsp; 🟩 绿色节点 = 浏览器预览

### 用户交互点一览

| # | 阶段 | 问什么 | 是否必须 |
|---|------|--------|---------|
| 1 | Phase 0.2 | 选框架（Reveal.js / SlideDev / HTML / 帮我选） | 必须 |
| 2 | Phase 1.5 | 图片不足时，展示缺口表格，要不要补充图片 | 条件触发 |
| 3 | Phase 2.1 | 选视觉风格（3 预设 + 自定义参考） | 必须 |
| 4 | Phase 2.1b | 自定义时：提供参考素材方式 | 条件触发 |
| 5 | Phase 2.1c | 🌐 **浏览器预览** → 对比选择最终风格 | 必须 |
| 6 | Phase 2.5 | 确认 slide plan 表格 | 必须 |
| 7 | 交付后 | 需要调整吗？ | 可选 |

### 风格预览机制

Phase 2.1c 会自动在浏览器中打开预览 HTML 文件：

- **选预设**：打开 3 个推荐的预设风格预览（每个含 5 页示例幻灯片：封面、数据、列表、引言、对比）
- **选自定义**：打开自定义风格预览 + 3 个与 mood 匹配的预设预览（共 4 个标签页）

用户在浏览器标签页之间切换对比，然后回到 Claude Code 告诉我选哪个。

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
.claude/skills/
├── ai-ppt/                        # 主编排 skill
│   ├── SKILL.md                   # 输入检测 → 内容分析 → 编排子skill → 质量检查
│   └── references/
│       ├── slide-type-catalog.md  # 15 种幻灯片类型
│       └── chinese-typography.md  # 中文排版规则
├── ai-ppt-style/                  # 风格发现 + 参考风格提取
│   ├── SKILL.md
│   └── references/
│       ├── style-presets.md       # 12 套视觉风格预设
│       ├── previews/              # 12 个预设风格预览 HTML
│       ├── animation-patterns.md  # 6 种动画情绪模式
│       └── custom-style-guide.md  # 从截图/HTML/CSS 提取自定义风格
├── ai-ppt-slidev/                 # SlideDev 生成器
│   ├── SKILL.md
│   └── references/
│       └── slidev-syntax.md
├── ai-ppt-revealjs/               # Reveal.js 生成器
│   ├── SKILL.md
│   └── references/
│       └── revealjs-syntax.md
├── ai-ppt-html/                   # 零依赖 HTML 生成器
│   ├── SKILL.md
│   └── references/
│       ├── html-template.md
│       └── viewport-base.css
├── ai-ppt-image/                  # 图片审计 + 充足性分析
│   └── SKILL.md
└── ai-ppt-extract/                # PPT 内容提取
    ├── SKILL.md
    └── references/
        └── extract-pptx.py
projects/                          # 生成的演示文稿项目
```

## 依赖

```bash
npm install
```

PPT 提取功能需要 Python 和 `python-pptx`：

```bash
pip install python-pptx
```
