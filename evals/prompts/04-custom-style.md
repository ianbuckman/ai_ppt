# Test 04: 自定义参考风格 → 风格提取流程

## 场景

用户选择"自定义参考风格"，提供文字描述作为参考。测试 ai-ppt-style 的自定义风格提取 + preview 生成 + 对比选择流程。

## Prompt

```
把以下内容做成PPT：

# AI Agent 的三种架构模式

当我们谈论 AI Agent 时，其实在说三种截然不同的东西。

## 模式一：ReAct（推理+行动）

最简单的 Agent 架构。模型在每一步都做两件事：先推理当前状态，再决定下一步行动。

核心循环：思考 → 行动 → 观察 → 思考 → 行动 → ...

优点是简单直观，缺点是容易陷入循环，且无法并行。

## 模式二：Plan & Execute

先制定完整计划，再逐步执行。

步骤：分析任务 → 制定计划 → 逐步执行 → 验证结果

优点是全局视野更好，适合复杂任务。缺点是计划可能过时，需要动态调整。

## 模式三：Multi-Agent

多个专业化的 Agent 协作完成任务。

架构：Orchestrator → [Researcher, Coder, Reviewer]

优点是可以并行、专业化程度高。缺点是协调成本高，通信开销大。

## 如何选择

没有最好的架构，只有最适合的。简单任务用 ReAct，复杂但线性的用 Plan & Execute，需要多种专业能力的用 Multi-Agent。
```

## 测试交互选择

- 风格选择：选 D（"自定义参考风格"）
- 自定义方式：选 C（"文字描述"），输入："科技感强，深色背景，用蓝色和紫色渐变作为强调色，类似 Vercel 或 Linear 的设计风格"
- 自定义预览确认：选 A（使用自定义风格）
- Slide 计划确认：说"可以"

## 预期行为

1. 用户选 D 后，触发 Step 1b 的自定义流程
2. 用 AskUserQuestion 问参考素材类型
3. 用户选"文字描述"后，从描述中提取：配色（深色+蓝紫渐变）、字体（现代 sans-serif）、布局、动画 mood（Techy）
4. 生成 `style-preview.html` 预览文件
5. 在浏览器打开自定义预览 + 3 个近似预设（可能是 Electric Studio、Neon Cyber、Terminal Green）
6. 用 AskUserQuestion 让用户在自定义 vs 3 个预设之间选
7. 用户选自定义后，继续 slide sequence 设计

## 重点检查

- 自定义风格 CSS variables 合理反映"Vercel/Linear 风格"（深色底、蓝紫渐变）
- style-preview.html 被生成且包含 5 个示例 slide
- 打开了 4 个 browser tab（1 自定义 + 3 预设）
- 最终生成的 HTML 使用了自定义 CSS variables
- 动画 mood 映射合理（应为 Techy）
- 内容（三种架构模式）被正确拆分成多个 slide，不是 3 个大 bullet list
