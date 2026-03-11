# Eval Assertions

每个 test prompt 的量化 pass/fail 检查项。跑完 eval 后逐条勾选。

---

## Prompt 01: 中文商业分析（全流程）

### Phase 1 — 内容分析
- [ ] 输出了 content inventory（含 section、key message、content units）
- [ ] 识别了至少 4 种不同的 content type tag（thesis、data_point、comparison、narrative 等）
- [ ] 提取了 key data points（如 71%、1224亿美元等）
- [ ] 给出了 slide count 估算（在合理范围内，通常 15-35）
- [ ] 识别了 central thesis

### Phase 1 — 图片审计
- [ ] 对 3 张源图片执行了 image audit
- [ ] 每张图片有 keep/adapt/drop 判定
- [ ] 给出了 sufficiency verdict（sufficient/partial/none）
- [ ] 未自动插入任何 placeholder 图片

### Phase 2 — 风格选择
- [ ] 分析了 mood 类别（应为 Impress）
- [ ] 在浏览器中打开了 3 个预设预览 HTML
- [ ] 使用 AskUserQuestion 呈现了 3 个预设 + 1 个自定义选项
- [ ] 等待了用户选择，未自动跳过

### Phase 2 — Slide 计划
- [ ] 设计了 slide sequence 并呈现确认表格
- [ ] 没有连续 3 个同类型 slide
- [ ] 有 breathing slide（section divider / stat / transition）
- [ ] 开头是高冲击力 slide（非 agenda）
- [ ] 结尾回扣开头

### Phase 3 — Reveal.js 生成
- [ ] 输出了有效的 HTML 文件
- [ ] Reveal.js CDN 链接正确（5.x 版本）
- [ ] 应用了选中的 style preset CSS variables
- [ ] 中文字体通过 Google Fonts 加载

### Phase 4 — 中文排版
- [ ] 无 italic 中文文本
- [ ] 使用全角中文标点
- [ ] 标题 ≤ 15 个中文字
- [ ] 正文每行 ≤ 25 个中文字
- [ ] line-height 在 1.8-2.0 范围

### Phase 4 — 内容质量
- [ ] 每个 slide 只有一个核心信息
- [ ] 关键数据点（71% 等）有独立 stat slide + 大字体
- [ ] 文章论证结构被保留
- [ ] 有 speaker notes
- [ ] 所有 slide 内容在 viewport 内

---

## Prompt 02: 英文技术文章

### Phase 1 — 内容分析
- [ ] 识别为英文内容
- [ ] 输出了 content inventory
- [ ] 提取了数据点（1 trillion+、4708:1、711.2M 行等）
- [ ] Slide count 估算合理（约 8-12 slides）

### Phase 2 — 风格选择
- [ ] Mood 分析为 Calm 或 Inspire（教育/思想领导力）
- [ ] 推荐的 3 个预设与 mood 匹配
- [ ] 使用 AskUserQuestion 让用户选择

### Phase 3 — 生成
- [ ] 英文字体正确（无中文字体作为主字体）
- [ ] 未应用中文排版规则（如 1.8-2.0 行高）
- [ ] "1 trillion+" 有 dramatic stat slide
- [ ] "4,708:1" test ratio 有 stat slide
- [ ] aviation safety 类比有合适的 slide type

### Phase 4 — 结构
- [ ] 结尾 "Would SQLite add this?" 作为 closing callback
- [ ] 没有把段落转成 bullet points
- [ ] 节奏合理，类型交替

---

## Prompt 03: 无图片文章

### 图片处理（核心测试点）
- [ ] Image audit 正确发现零图片
- [ ] Sufficiency verdict = "none"
- [ ] 生成了推荐表格（含优先级：必要/推荐/加分项）
- [ ] 推荐表格不包括 thesis/stat slide 的图片建议
- [ ] 推荐了合理的图片类型（城市照片、商业地产等）
- [ ] 使用 AskUserQuestion 询问是否提供图片
- [ ] 用户选"不需要"后，零 placeholder 图片出现在最终输出
- [ ] 最终 HTML 中没有任何 unsplash/pexels URL

### 内容处理
- [ ] "三个结构性变化"拆分成多个 slide（非 1 个 bullet list）
- [ ] 数据点（7.5%、22.7%、40-60%）有 stat slide
- [ ] 纯排版方案视觉质量不输有图片版本

---

## Prompt 04: 自定义参考风格

### 自定义流程（核心测试点）
- [ ] 用户选 D 后触发自定义流程
- [ ] 使用 AskUserQuestion 询问参考素材类型
- [ ] 从文字描述中提取了配色（深色 + 蓝紫渐变）
- [ ] 从文字描述中推断了字体（现代 sans-serif）
- [ ] 动画 mood 映射为 Techy
- [ ] 生成了 style-preview.html 文件
- [ ] style-preview.html 包含 5 个示例 slide
- [ ] 在浏览器中打开了 4 个 tab（1 自定义 + 3 近似预设）
- [ ] 使用 AskUserQuestion 让用户在自定义 vs 预设之间选
- [ ] 用户选自定义后，CSS variables 应用到最终输出

### 内容处理
- [ ] 三种架构模式没有被做成 3 个 bullet list
- [ ] ReAct 的"思考→行动→观察"循环有合适的 slide 表现
- [ ] Multi-Agent 的架构关系有合适的 slide 表现

---

## Prompt 05: PPTX / PDF 输入

### 输入检测
- [ ] 正确识别文件类型（.pptx → extract flow / .pdf → document flow）
- [ ] PPTX：触发 ai-ppt-extract 子 skill
- [ ] PPTX：运行提取脚本并向用户展示结果
- [ ] PDF：作为普通文档读取

### 后续流程
- [ ] 提取/读取后正常进入风格选择
- [ ] 内容被完整保留
- [ ] 最终输出质量与其他 prompt 一致
