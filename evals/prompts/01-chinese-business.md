# Test 01: 中文商业分析文章 → 全流程

## 场景

中文商业分析文章，包含数据点、引用、比较、叙事等多种内容类型，且有图片。
测试全流程：内容分析 → 图片审计 → 风格选择 → Reveal.js 生成 → 中文排版。

## Prompt

```
把这篇文章做成PPT：projects/tsmc/张忠谋和他的台积电，如何改变了整个芯片世界.md
```

## 预期行为

1. 触发 ai-ppt 主 skill
2. Phase 1：深度内容分析，输出 content inventory
3. Phase 1 Step 3：加载 ai-ppt-image，审计 3 张图片（img.png, img_1.png, img_2.png）
4. Phase 2：加载 ai-ppt-style，分析 mood（应为 Impress），打开 3 个预设预览，用 AskUserQuestion 让用户选
5. Phase 2 后续：设计 slide sequence，呈现确认表格
6. Phase 3：加载 ai-ppt-revealjs，生成 index.html
7. Phase 4：质量检查 + 交付

## 测试交互选择

- 风格选择：选 A（第一推荐）
- Slide 计划确认：说"可以"
- 图片推荐（如果触发）：选 B（用已有图片）

## 重点检查

- 中文排版规则全部应用（无 italic、全角标点、字符限制）
- 数据点（如 71% 市占率）有独立的 stat slide，用大字体
- 文章的论证结构被保留（飞轮效应等概念有对应 slide）
- 图片被合理审计（keep/adapt/drop），非盲目包含
