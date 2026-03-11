# AI-PPT Skill 最佳实践评估报告

基于 Claude skill-creator 官方最佳实践 + Boris (Lenny's Podcast) 的设计原则，对本项目进行系统评估。

## 总体评价

项目整体设计水平很高，在多数最佳实践上已经做到优秀甚至标杆级别。核心短板在于缺少 test/eval 机制和 CLAUDE.md。

---

## 逐条评估

### 一、结构规范 ⭐⭐⭐⭐⭐

| 实践 | 状态 | 评估 |
|------|------|------|
| SKILL.md + YAML frontmatter | 完全遵循 | 7 个 skill 都有规范的 name + description frontmatter |
| SKILL.md < 500 行 | 完全遵循 | 最大 297 行（主 skill），其余 60-233 行 |
| references/ 按需加载 | 完全遵循 | 每个 skill 有独立 references/，且有 "When to Load" 表格 |
| scripts/ 确定性脚本 | 部分遵循 | extract-pptx.py 放在 references/ 而非 scripts/ |
| assets/ 模板资源 | 变体实现 | preview HTML 放在 references/previews/，语义上更接近 assets |

小建议：把 `extract-pptx.py` 移到 `scripts/`，preview HTML 和 CSS 移到 `assets/`。低优先级。

---

### 二、触发 Description ⭐⭐⭐⭐

主 skill description 做得很好——列出了多种中英文触发关键词，符合"pushy"原则：

```
"做成PPT""生成演示文稿""把这篇文章做成slides""转成PPT"
"article to presentation""make a presentation from this article""convert to slides"
```

子 skill description 都标注了"由 ai-ppt 主技能调用，不独立触发"，设计合理。

可补充的边缘触发词："帮我做个汇报""prepare a deck""make slides from this""把这个内容变成演讲"。

---

### 三、Progressive Disclosure ⭐⭐⭐⭐⭐

教科书级别的三层加载：

1. **Metadata 层**：7 个 skill 的 name + description 始终在 context（~200 词）
2. **SKILL.md 主体层**：只有被触发的 skill 才加载
3. **References 层**：按需加载，有明确条件表格

```markdown
| File | Purpose | When to Load |
|------|---------|-------------|
| references/chinese-typography.md | CJK 排版规则 | When source is Chinese |
| references/custom-style-guide.md | 自定义风格提取 | Only when user selects "自定义参考风格" |
```

---

### 四、Separation of Concerns ⭐⭐⭐⭐⭐

7 个 skill 职责边界清晰无重叠：

```
ai-ppt          → 编排协调（不生成 HTML）
ai-ppt-style    → 风格选择（不涉及内容分析）
ai-ppt-image    → 图片审计（不涉及风格）
ai-ppt-revealjs → Reveal.js 生成
ai-ppt-slidev   → SlideDev 生成
ai-ppt-html     → 纯 HTML 生成
ai-ppt-extract  → PPTX 提取
```

---

### 五、Least Surprise ⭐⭐⭐⭐

- 用户交互点用 "MANDATORY USER INTERACTION" 和 "conditional" 明确标注
- 多处强调 "Do NOT skip this step. Do NOT auto-select a style."
- 每个框架 skill 有精确的输出结构说明
- **缺失**：异常情况处理指引（无法抓取的 URL、空文件、非中非英语言等）

---

### 六、Sensible Defaults ⭐⭐⭐⭐⭐

- 框架默认 Reveal.js，不让用户选
- 用户说"随便"时选第一推荐
- 用户说"直接做"跳过确认
- Slide count 公式 150-250 字/slide 作为默认

Convention over Configuration 做得很到位。

---

### 七、Verify, Don't Trust ⭐ （最大短板）

Phase 4 Quality Checklist 有 6 大类 20+ 条检查项，但这只是模型自查，没有自动化验证。

**完全缺失**：
- 没有 test prompts
- 没有 assertions
- 没有 eval scripts
- 没有 A/B 比较机制
- 没有 description optimization loop

skill-creator 最强调的 `draft → test → evaluate → rewrite` 循环在本项目中不存在。

---

### 八、Every Mistake → Rule ⭐⭐

- **没有 CLAUDE.md**
- 防错规则分散在各 SKILL.md 中（如 "NEVER use italic for Chinese"、"Don't convert paragraphs into bullet points"）
- 这些规则看起来来自真实使用经验，但缺少集中维护和跨 skill 共享

---

### 九、Latent Demand ⭐⭐⭐⭐

好的体现：
- 自定义风格提取（从截图/HTML/CSS）= 用户会说"我想要像 XX 那样的风格"
- 图片审计而非自动插图 = AI 默认行为（插 stock photo）是用户不想要的
- "Anti-AI Slop" 作为核心原则 = 理解了用户对 AI 生成内容同质化的反感

---

### 十、Build for Future Model ⭐⭐⭐⭐

- 架构面向扩展：加新框架只需加新子 skill
- 多模态图片审计充分利用模型能力
- 部分规则（如反 bullet point 转换）是针对当前模型倾向的 workaround，未来可能不需要

---

### 十一、精炼不臃肿 ⭐⭐⭐⭐⭐

- 每个 SKILL.md 都很精炼（主 skill 297 行，子 skill 平均 130 行）
- 没有冗余重复
- 通用能力留给模型 general intelligence

---

### 十二、写作质量 ⭐⭐⭐⭐

- 祈使句：✓ "Identify all headings"、"Tag each unit"
- 解释"为什么"：部分——有的好（"The quality depends entirely on how well you understand the article"），有的只有 MUST/NEVER
- Input/Output 示例：✓ Content Inventory 模板、AskUserQuestion 完整示例
- Theory of mind 泛化：部分——示例用了台积电文章，有一定过拟合风险

---

## 评分汇总

| 最佳实践 | 得分 |
|----------|------|
| 结构规范 | ⭐⭐⭐⭐⭐ |
| 触发 Description | ⭐⭐⭐⭐ |
| Progressive Disclosure | ⭐⭐⭐⭐⭐ |
| Separation of Concerns | ⭐⭐⭐⭐⭐ |
| Least Surprise | ⭐⭐⭐⭐ |
| Sensible Defaults | ⭐⭐⭐⭐⭐ |
| **Verify, Don't Trust** | **⭐** |
| **Every Mistake → Rule** | **⭐⭐** |
| Latent Demand | ⭐⭐⭐⭐ |
| Build for Future Model | ⭐⭐⭐⭐ |
| 精炼不臃肿 | ⭐⭐⭐⭐⭐ |
| 写作质量 | ⭐⭐⭐⭐ |

---

## 优先改进建议

1. **[高优] 建立 eval 机制**：写 3-5 个真实测试 prompt，定义 assertions，建立 draft → test → evaluate → rewrite 循环
2. **[高优] 创建 CLAUDE.md**：收集跨 skill 的通用规则、已知失败模式和修正规则
3. **[中优] 补充 description 触发词**：加入更多边缘表述
4. **[低优] 目录语义整理**：scripts/ 放脚本，assets/ 放模板/预览文件
