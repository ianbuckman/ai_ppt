# Test 05: PPTX 输入 → 提取 + 重新生成

## 场景

用户提供 .pptx 文件，测试 ai-ppt-extract 子 skill 触发、内容提取、后续流程。

## Prompt

```
把这个PPT转成网页版：projects/mu/以美光科技为例看存储芯片的AI重构.pdf
```

> **注意**：当前 projects/mu/ 下是 PDF 文件，不是 .pptx。如需测试 PPTX 提取流程，需准备一个 .pptx 测试文件。本 prompt 可用于测试 PDF 输入的处理（应作为普通文档读取而非走 PPTX 提取流程）。

## 替代 Prompt（如果有 .pptx 文件）

```
把这个PPT转成网页版：[path-to-test.pptx]
```

## 预期行为（PPTX 路径）

1. 识别 .pptx 扩展名 → 触发 ai-ppt-extract
2. 运行 `extract-pptx.py` 提取内容
3. 生成 `extracted-slides.json` + `assets/` 目录
4. 向用户展示提取结果并确认
5. 进入正常流程：风格选择 → 生成

## 预期行为（PDF 路径）

1. 识别为文档输入（非 PPTX）
2. 读取 PDF 内容
3. 按常规 article-to-PPT 流程处理

## 测试交互选择

- 提取确认：说"直接用这些内容"
- 风格选择：选 A
- Slide 计划确认：说"可以"

## 重点检查

- 正确区分 .pptx vs 其他文件格式
- PPTX 提取保留了原始内容（文本、图片、notes）
- 提取的图片被后续 image audit 处理
- 原始 slide 顺序被保留（除非用户要求重组）
