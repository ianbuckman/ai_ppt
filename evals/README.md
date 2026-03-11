# AI-PPT Eval 机制

用于验证 skill 质量的测试框架。遵循 skill-creator 的 `draft → test → evaluate → rewrite` 循环。

## 目录结构

```
evals/
├── README.md          # 本文件
├── assertions.md      # 每个 prompt 的量化 pass/fail 检查项
└── prompts/           # 测试 prompt
    ├── 01-chinese-business.md   # 中文商业分析（全流程）
    ├── 02-english-tech.md       # 英文技术文章
    ├── 03-no-images.md          # 无图片文章（图片流程）
    ├── 04-custom-style.md       # 自定义参考风格
    └── 05-pptx-input.md         # PPTX/PDF 输入
```

## 如何跑 Eval

### 1. 选择测试 prompt

打开 `prompts/` 下的某个文件，阅读场景描述和预期行为。

### 2. 执行

在 Claude Code 中新开对话，复制 prompt 中的 `Prompt` 部分发送。按"测试交互选择"中的指示回答每个用户交互点。

### 3. 评估

打开 `assertions.md`，找到对应 prompt 的检查项，逐条验证输出是否通过。

### 4. 记录结果

在 assertions.md 中勾选通过的项。对于失败的项，记录具体表现。

## 迭代流程

```
发现 assertion 失败
    ↓
分析根因：是 SKILL.md 指令不够清晰？还是模型的通用行为？
    ↓
如果是指令问题 → 修改对应 SKILL.md
    ↓
将教训加入 CLAUDE.md 的"已知失败模式"表格
    ↓
重跑受影响的 eval prompt 验证修复
    ↓
如果涉及新场景 → 在 prompts/ 中添加新 test case
```

## 如何添加新 Test Case

1. 在 `prompts/` 下创建新文件 `NN-description.md`
2. 包含以下部分：
   - `# Test NN: 标题` — 一句话场景描述
   - `## 场景` — 测试什么、为什么测
   - `## Prompt` — 实际发送给 Claude Code 的内容（代码块格式）
   - `## 预期行为` — 预期的执行步骤
   - `## 测试交互选择` — 每个交互点该怎么回答
   - `## 重点检查` — 这个 case 特别关注什么
3. 在 `assertions.md` 中添加对应的检查项

## 覆盖矩阵

| 场景 | 01 | 02 | 03 | 04 | 05 |
|------|----|----|----|----|-----|
| Phase 1 内容分析 | ✓ | ✓ | ✓ | ✓ | ✓ |
| 中文排版 | ✓ | | ✓ | ✓ | |
| 英文排版 | | ✓ | | | |
| Image = sufficient | ✓ | | | | |
| Image = none | | | ✓ | | |
| 预设风格选择 | ✓ | ✓ | ✓ | | ✓ |
| 自定义风格 | | | | ✓ | |
| Reveal.js 生成 | ✓ | ✓ | ✓ | ✓ | ✓ |
| PPTX 提取 | | | | | ✓ |
| Slide 节奏规则 | ✓ | ✓ | ✓ | ✓ | |
| Stat slide 效果 | ✓ | ✓ | ✓ | | |

### 已知未覆盖场景

- SlideDev 框架生成（目前默认 Reveal.js）
- Pure HTML 框架生成
- Image = "partial"（有一些图但不够）
- 用户提供图片后的整合流程
- 非中非英语言
- URL 输入（WebFetch 抓取）
- 极长文章（10000+ 字）
- 极短内容（< 500 字）
