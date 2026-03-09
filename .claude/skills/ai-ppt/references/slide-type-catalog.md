# Slide Type Catalog

15 reusable slide types with concrete implementations for both SlideDev and Reveal.js.

---

## 1. Cover

**Purpose**: Opening slide. Sets the tone and context.
**When to use**: Always the first slide.
**Content limit**: 1 title + 1 subtitle + optional author/date

### SlideDev

```markdown
---
layout: cover
background: "linear-gradient(135deg, #1a1a2e 0%, #16213e 100%)"
---

# 只做代工，凭什么成为全球最重要的公司？

拆解台积电的飞轮、锁定与定价权

<div class="abs-bl m-6 text-sm opacity-50">
2025 · 17号野生分析师
</div>
```

### Reveal.js

```html
<section data-background-gradient="linear-gradient(135deg, #1a1a2e 0%, #16213e 100%)">
  <h1 style="font-size: 2.2em; line-height: 1.3;">只做代工，凭什么成为<br>全球最重要的公司？</h1>
  <p style="font-size: 1.2em; opacity: 0.7; margin-top: 1em;">拆解台积电的飞轮、锁定与定价权</p>
  <p style="font-size: 0.7em; opacity: 0.4; margin-top: 2em;">2025 · 17号野生分析师</p>
</section>
```

**Animation**: None (static entry point)
**Chinese**: Keep title under 20 characters. Split with `<br>` if needed.

---

## 2. Section Divider

**Purpose**: Mark the beginning of a new major section.
**When to use**: Between major sections (maps to H2 headings).
**Content limit**: Section number + title + optional 1-line teaser

### SlideDev

```markdown
---
layout: section
transition: fade
---

# 01
## 飞轮效应
规模如何变成不可逾越的优势
```

### Reveal.js

```html
<section data-background-gradient="linear-gradient(135deg, #667eea 0%, #764ba2 100%)" data-transition="fade">
  <p style="font-size: 4em; font-weight: 900; opacity: 0.3; margin: 0;">01</p>
  <h2 style="font-size: 2.5em; margin-top: -0.3em;">飞轮效应</h2>
  <p style="font-size: 1em; opacity: 0.7;">规模如何变成不可逾越的优势</p>
</section>
```

**Animation**: `fade` transition (distinct from normal `slide`)

---

## 3. Thesis / Statement

**Purpose**: Present one bold claim or core argument.
**When to use**: Key turning points, section conclusions, "so what" moments.
**Content limit**: 1 sentence (max 25 Chinese characters)

### SlideDev

```markdown
---
layout: statement
---

# 不竞争，是最强的竞争策略
```

### Reveal.js

```html
<section data-auto-animate>
  <h2 data-id="thesis" style="font-size: 2.5em; font-weight: 700;">
    不竞争，是最强的竞争策略
  </h2>
</section>
```

**Animation**: `data-auto-animate` or simple `fade`

---

## 4. Bullet List

**Purpose**: Present 3-5 related points.
**When to use**: Supporting details, enumerated arguments.
**Content limit**: 1 heading + 3-5 bullets (each under 20 chars)

### SlideDev

```markdown
# 客户出逃三星的原因

<v-clicks>

- Google Tensor 芯片转投台积电
- 高通取消三星代工线
- 三星自家 Exynos 良率不达标
- Galaxy S25 全线采用高通芯片

</v-clicks>
```

### Reveal.js

```html
<section>
  <h3>客户出逃三星的原因</h3>
  <ul>
    <li class="fragment fade-up">Google Tensor 芯片转投台积电</li>
    <li class="fragment fade-up">高通取消三星代工线</li>
    <li class="fragment fade-up">三星自家 Exynos 良率不达标</li>
    <li class="fragment fade-up">Galaxy S25 全线采用高通芯片</li>
  </ul>
</section>
```

**Animation**: Progressive reveal (one bullet at a time)

---

## 5. Two-Column

**Purpose**: Side-by-side comparison or concept + example.
**When to use**: Comparing two entities, showing before/after, concept with illustration.
**Content limit**: 2 columns × 3-4 items each

### SlideDev

```markdown
---
layout: two-cols-header
---

# 纯代工 vs 混合模式

::left::

### 台积电（纯代工）
- 不设计自己的芯片
- 零利益冲突
- 客户完全信任

::right::

### 三星/英特尔（混合）
- 同时设计和代工
- 与客户存在竞争
- 信任难以建立
```

### Reveal.js

```html
<section>
  <h3>纯代工 vs 混合模式</h3>
  <div style="display: flex; gap: 3em; margin-top: 1em;">
    <div style="flex: 1; text-align: left;">
      <h4 style="color: #2ecc71;">台积电（纯代工）</h4>
      <ul>
        <li class="fragment fade-up">不设计自己的芯片</li>
        <li class="fragment fade-up">零利益冲突</li>
        <li class="fragment fade-up">客户完全信任</li>
      </ul>
    </div>
    <div style="flex: 1; text-align: left;">
      <h4 style="color: #e74c3c;">三星/英特尔（混合）</h4>
      <ul>
        <li class="fragment fade-up">同时设计和代工</li>
        <li class="fragment fade-up">与客户存在竞争</li>
        <li class="fragment fade-up">信任难以建立</li>
      </ul>
    </div>
  </div>
</section>
```

**Animation**: Progressive reveal per column

---

## 6. Single Stat

**Purpose**: Make one number dramatic and memorable.
**When to use**: Key data points that deserve emphasis. The more impressive the number, the more it deserves its own slide.
**Content limit**: 1 giant number + 1 context line + optional 1 detail

### SlideDev

```markdown
---
layout: fact
---

# 71%
全球晶圆代工市场份额

<p class="text-sm opacity-50 mt-4">领先第二名 64 个百分点</p>
```

### Reveal.js

```html
<section data-background-color="#0a0a15">
  <div style="font-size: 8em; font-weight: 900; color: #667eea; line-height: 1;">71%</div>
  <p style="font-size: 1.5em; opacity: 0.8; margin-top: 0.5em;">全球晶圆代工市场份额</p>
  <p class="fragment fade-up" style="font-size: 0.9em; opacity: 0.5;">领先第二名 64 个百分点</p>
</section>
```

**Animation**: Number appears instantly; context fades in
**Chinese**: Use Arabic numerals for the stat, Chinese for the description

---

## 7. Comparison Table

**Purpose**: Structured comparison across multiple dimensions.
**When to use**: Comparing 2-3 entities across 3-5 attributes.
**Content limit**: Max 3 columns × 5 rows

### SlideDev

```markdown
# 代工三巨头对比

| | 台积电 | 三星 | 英特尔 |
|---|---|---|---|
| 市占率 | **71%** | 6.8% | ~1% |
| 营收 | 1224亿 | ~200亿 | 178亿 |
| 利润 | 554亿 | 亏损 | 亏损103亿 |
| 3nm良率 | >90% | ~20% | N/A |
```

### Reveal.js

```html
<section>
  <h3>代工三巨头对比</h3>
  <table style="font-size: 0.8em; margin-top: 1em;">
    <thead>
      <tr><th></th><th style="color: #2ecc71;">台积电</th><th>三星</th><th>英特尔</th></tr>
    </thead>
    <tbody>
      <tr class="fragment fade-up"><td>市占率</td><td><strong>71%</strong></td><td>6.8%</td><td>~1%</td></tr>
      <tr class="fragment fade-up"><td>营收</td><td><strong>1224亿</strong></td><td>~200亿</td><td>178亿</td></tr>
      <tr class="fragment fade-up"><td>利润</td><td><strong>554亿</strong></td><td>亏损</td><td>亏损103亿</td></tr>
      <tr class="fragment fade-up"><td>3nm良率</td><td><strong>>90%</strong></td><td>~20%</td><td>N/A</td></tr>
    </tbody>
  </table>
</section>
```

**Animation**: Row-by-row reveal

---

## 8. Timeline

**Purpose**: Show chronological progression of events.
**When to use**: Historical development, process steps, evolution.
**Content limit**: 3-5 time points

### SlideDev

```markdown
# 客户出逃时间线

<v-clicks>

- **2024初** — 三星3nm良率个位数
- **2024中** — Google Tensor 转投台积电
- **2024末** — 高通取消三星代工线
- **2025** — 三星市占率从11%腰斩至6.8%

</v-clicks>
```

### Reveal.js

```html
<section>
  <h3>客户出逃时间线</h3>
  <div style="text-align: left; max-width: 700px; margin: 1em auto;">
    <div class="fragment fade-up" style="padding: 0.5em 0; border-left: 3px solid #667eea; padding-left: 1em; margin-bottom: 0.8em;">
      <strong style="color: #667eea;">2024初</strong><br>三星3nm良率个位数
    </div>
    <div class="fragment fade-up" style="padding: 0.5em 0; border-left: 3px solid #667eea; padding-left: 1em; margin-bottom: 0.8em;">
      <strong style="color: #667eea;">2024中</strong><br>Google Tensor 转投台积电
    </div>
    <div class="fragment fade-up" style="padding: 0.5em 0; border-left: 3px solid #667eea; padding-left: 1em; margin-bottom: 0.8em;">
      <strong style="color: #667eea;">2024末</strong><br>高通取消三星代工线
    </div>
    <div class="fragment fade-up" style="padding: 0.5em 0; border-left: 3px solid #667eea; padding-left: 1em;">
      <strong style="color: #667eea;">2025</strong><br>三星市占率从11%腰斩至6.8%
    </div>
  </div>
</section>
```

**Animation**: Step-by-step reveal

---

## 9. Quote

**Purpose**: Feature a memorable quotation.
**When to use**: Direct quotes from people, memorable phrases from the article.
**Content limit**: 1 quote (max 3 lines) + attribution

### SlideDev

```markdown
---
layout: quote
---

# "产能卖光了，一直到2026年都是满的"

— 魏哲家，台积电CEO
```

### Reveal.js

```html
<section>
  <blockquote style="font-size: 1.4em; border-left: 4px solid #667eea; padding: 0.5em 1em; text-align: left; font-style: normal; line-height: 1.8;">
    "产能卖光了，一直到2026年都是满的"
  </blockquote>
  <p style="text-align: right; opacity: 0.6; font-size: 0.8em; margin-top: 1em;">
    — 魏哲家，台积电CEO
  </p>
</section>
```

**Animation**: Quote fades in, attribution appears after
**Chinese**: Use `font-style: normal` — never italic for Chinese

---

## 10. Analogy

**Purpose**: "X is like Y" explanation to make abstract concepts tangible.
**When to use**: When the article uses a metaphor or analogy.
**Content limit**: 2 panels (the thing + the analogy)

### SlideDev

```markdown
---
layout: two-cols
---

### 台积电的生态锁定

换代工厂 = 重做整个设计
六到十八个月的迁移成本
巨大的良率和上市时间风险

::right::

### 就像苹果全家桶

换安卓 ≠ 只换手机
App、照片、iCloud、AirDrop…
迁移成本高到你放弃换
```

### Reveal.js

```html
<section>
  <h3>生态锁定</h3>
  <div style="display: flex; gap: 2em; align-items: center; margin-top: 1em;">
    <div class="fragment fade-right" style="flex: 1; padding: 1.5em; background: rgba(102,126,234,0.1); border-radius: 12px; text-align: left;">
      <h4>台积电的客户</h4>
      <p style="font-size: 0.8em;">换代工厂 = 重做整个设计<br>耗时6-18个月<br>巨大的良率风险</p>
    </div>
    <div style="font-size: 2em; opacity: 0.5;">≈</div>
    <div class="fragment fade-left" style="flex: 1; padding: 1.5em; background: rgba(231,76,60,0.1); border-radius: 12px; text-align: left;">
      <h4>苹果全家桶用户</h4>
      <p style="font-size: 0.8em;">换安卓 ≠ 只换手机<br>App、照片、iCloud…<br>迁移成本高到放弃</p>
    </div>
  </div>
</section>
```

**Animation**: Left panel slides in, then right panel

---

## 11. Story / Evidence

**Purpose**: Present narrative evidence or a cause-effect chain.
**When to use**: Anecdotes, case studies, proof points.
**Content limit**: 1 heading + 2-3 progressive points

### SlideDev

```markdown
# 三星的困境

<v-clicks>

**起因**：同时做芯片设计和代工 → 与客户存在竞争

**结果**：客户用脚投票

**证据**：Galaxy S25 被迫全线用高通芯片——三星代工连自家芯片的良率都搞不定

</v-clicks>
```

### Reveal.js

```html
<section>
  <h3>三星的困境</h3>
  <div style="text-align: left; max-width: 800px; margin: 1em auto;">
    <p class="fragment fade-up"><strong style="color: #e74c3c;">起因：</strong>同时做芯片设计和代工 → 与客户存在竞争</p>
    <p class="fragment fade-up"><strong style="color: #f39c12;">结果：</strong>客户用脚投票</p>
    <p class="fragment fade-up"><strong style="color: #2ecc71;">证据：</strong>Galaxy S25 被迫全线用高通芯片</p>
  </div>
</section>
```

**Animation**: Progressive reveal to build the narrative

---

## 12. Diagram

**Purpose**: Show relationships, processes, or cycles visually.
**When to use**: Flowcharts, cycles, organizational structures.
**Content limit**: Keep diagrams simple (5-7 nodes max)

### SlideDev (Mermaid)

````markdown
# 飞轮效应

```mermaid
graph LR
  A[最好的客户] --> B[最大的收入]
  B --> C[最大的投入]
  C --> D[技术领先]
  D --> A
  style A fill:#667eea,color:#fff
  style B fill:#764ba2,color:#fff
  style C fill:#e74c3c,color:#fff
  style D fill:#2ecc71,color:#fff
```
````

### Reveal.js (CSS-based)

```html
<section>
  <h3>飞轮效应</h3>
  <div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 1em; margin-top: 1.5em;">
    <div class="fragment" style="padding: 1em 1.5em; background: #667eea; border-radius: 8px; color: #fff;">最好的客户</div>
    <div class="fragment" style="font-size: 2em; align-self: center;">→</div>
    <div class="fragment" style="padding: 1em 1.5em; background: #764ba2; border-radius: 8px; color: #fff;">最大的收入</div>
    <div class="fragment" style="font-size: 2em; align-self: center;">→</div>
    <div class="fragment" style="padding: 1em 1.5em; background: #e74c3c; border-radius: 8px; color: #fff;">最大的投入</div>
    <div class="fragment" style="font-size: 2em; align-self: center;">→</div>
    <div class="fragment" style="padding: 1em 1.5em; background: #2ecc71; border-radius: 8px; color: #fff;">技术领先</div>
    <div class="fragment" style="font-size: 2em; align-self: center;">→ 🔄</div>
  </div>
</section>
```

**Animation**: Nodes appear one by one to show the cycle

---

## 13. Highlight

**Purpose**: Pull-quote or key phrase from the article, enlarged for impact.
**When to use**: Memorable phrases that aren't direct quotes from a person.
**Content limit**: 1 phrase (max 20 Chinese characters) + optional context

### SlideDev

```markdown
---
layout: center
class: text-center
---

<div class="text-4xl font-bold leading-relaxed">
这不是信任问题。<br>
<span class="text-6xl text-blue-400">这是数学题。</span>
</div>
```

### Reveal.js

```html
<section data-background-color="#1a1a2e">
  <p style="font-size: 1.5em; opacity: 0.8;">这不是信任问题。</p>
  <p class="fragment grow" style="font-size: 3em; font-weight: 900; color: #667eea; margin-top: 0.3em;">
    这是数学题。
  </p>
</section>
```

**Animation**: Second line grows or appears dramatically

---

## 14. Transition

**Purpose**: Breathing room between sections. Rhetorical question or pause.
**When to use**: Every 4-5 slides, between major sections.
**Content limit**: 1 question or 1 short sentence

### SlideDev

```markdown
---
layout: center
transition: fade
---

# 就算客户不满意——他们能换吗？
```

### Reveal.js

```html
<section data-transition="fade" data-background-color="#16213e">
  <h2 style="font-size: 2em; font-weight: 400; opacity: 0.9;">
    就算客户不满意——<br>他们能换吗？
  </h2>
</section>
```

**Animation**: Gentle `fade` transition, no fragments

---

## 15. Closing

**Purpose**: Final slide. Summarize and callback to opening.
**When to use**: Always the last slide.
**Content limit**: 1 summary sentence + callback + optional CTA

### SlideDev

```markdown
---
layout: center
class: text-center
---

# 飞轮一旦转起来
## 追赶者面对的不是一个强大的对手
## 而是一整个系统

<div class="mt-8 text-sm opacity-50">
但没有任何护城河是永恒的——下一篇：台积电的命门
</div>
```

### Reveal.js

```html
<section data-background-gradient="linear-gradient(135deg, #1a1a2e 0%, #16213e 100%)">
  <h2 style="font-size: 1.8em; line-height: 1.6;">
    飞轮一旦转起来<br>
    <span style="opacity: 0.7;">追赶者面对的不是一个强大的对手</span><br>
    <span class="fragment" style="color: #667eea; font-weight: 900;">而是一整个系统</span>
  </h2>
  <p class="fragment fade-up" style="font-size: 0.8em; opacity: 0.4; margin-top: 2em;">
    但没有任何护城河是永恒的——下一篇：台积电的命门
  </p>
</section>
```

**Animation**: Key phrase revealed last for emphasis
