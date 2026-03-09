---
name: ai-ppt
description: 将文章/文档转换为专业演示文稿（PPT）。支持 SlideDev 和 Reveal.js 两种框架输出。当用户说"做成PPT""生成演示文稿""把这篇文章做成slides""转成PPT""article to presentation""make a presentation from this article""convert to slides"等时触发。注意：本 skill 专门用于从已有文章/文本生成演示文稿，不用于从零开始设计幻灯片（那个请使用 frontend-slides skill）。
---

# AI PPT Skill — Article-to-Presentation Converter

Transform articles and text documents into professional, well-structured presentations using SlideDev or Reveal.js. This skill focuses on **content analysis and intelligent arrangement** — understanding article structure, extracting key messages, and mapping them to optimal slide types.

## Core Philosophy

1. **Content-First** — Always analyze the article deeply before designing any slides. The presentation structure should emerge from the content, not be imposed on it.
2. **One Message Per Slide** — Every slide must have exactly ONE clear takeaway. If you can't state it in one sentence, split the slide.
3. **Framework-Based Output** — Generate using established frameworks (SlideDev or Reveal.js), not custom HTML from scratch.
4. **Presentation Rhythm** — Alternate slide types to maintain audience attention. Never three bullet slides in a row. Add breathing room every 4-5 slides.
5. **Chinese-First Typography** — When the article is in Chinese, apply CJK-specific typography rules throughout. See `references/chinese-typography.md`.
6. **Viewport Fitting** — Every slide must fit within the viewport. No scrolling within slides, ever.

---

## Phase 0: Input & Framework Selection

### Step 1: Detect Input

Identify the input type:
- **File path**: Read the file and extract content
- **Pasted text**: Use directly
- **URL**: Fetch the page content (if possible)

If no input is provided, ask the user to provide the article or file path.

### Step 2: Framework Selection

Ask the user to choose their output framework:

> 请选择演示文稿框架：
> 1. **Reveal.js** — 单个 HTML 文件，零依赖，直接浏览器打开（推荐大多数场景）
> 2. **SlideDev** — Markdown 驱动，适合包含代码/图表的技术演讲
> 3. **帮我选** — 根据文章内容自动推荐

### Decision Matrix for "帮我选"

| Factor | → Reveal.js | → SlideDev |
|--------|-------------|------------|
| Contains code snippets | | ✓ |
| Contains diagrams/flowcharts | | ✓ |
| Technical audience | | ✓ |
| Quick sharing needed | ✓ | |
| No Node.js environment | ✓ | |
| Business/general audience | ✓ | |
| Contains mostly narrative | ✓ | |
| Contains data/statistics | ✓ | |

**Default recommendation**: Reveal.js (zero setup, single HTML file, works everywhere).

### Step 3: Style Preference

Ask briefly:

> 风格偏好？深色/浅色/自动（根据内容选择）

Map to framework themes:
- Dark → Reveal.js: `black` or `moon` / SlideDev: `seriph` or `default`
- Light → Reveal.js: `white` or `simple` / SlideDev: `apple-basic` or `bricks`
- Auto → Analyze article tone (formal → dark, casual → light)

---

## Phase 1: Deep Content Analysis

**This is the most critical phase.** Do not skip or rush it. The quality of the final presentation depends entirely on how well you understand the article.

### Step 1: Parse Structure

- Identify all headings (H1, H2, H3) → these become section boundaries
- Count total characters/words to estimate slide count
- **Slide count formula**: ~1 slide per 150-250 Chinese characters (or 80-150 English words)
- Typical range: 15-35 slides for a 3000-5000 character article

### Step 2: Tag Content Units

Go through the article paragraph by paragraph and tag each content unit with its type:

| Content Type | Description | Example |
|-------------|-------------|---------|
| `thesis` | Core argument or claim | "不竞争是最强的竞争策略" |
| `data_point` | Specific number or statistic | "市占率71%，领先第二名64个百分点" |
| `comparison` | Two or more things contrasted | "台积电 vs 三星 vs 英特尔" |
| `quote` | Direct quotation | CEO魏哲家的原话 |
| `narrative` | Story, anecdote, or example | Google/高通从三星转投台积电 |
| `concept` | Named framework or model | "飞轮效应""TSMC Brake" |
| `analogy` | "X is like Y" explanation | "像苹果全家桶的锁定效应" |
| `list` | Enumerated items | 飞轮的三圈 |
| `transition` | Bridge between sections | "回到开头的问题" |
| `evidence` | Supporting proof for a claim | 三星良率数据 vs 台积电良率数据 |

### Step 3: Extract Key Elements

Pull out and list:
- **Key data points**: All numbers, percentages, dollar amounts
- **Quotable phrases**: Memorable statements that work as standalone slides
- **Named concepts**: Frameworks, models, coined terms
- **Comparisons**: Any side-by-side contrasts
- **The central thesis**: The one sentence that captures the entire article

### Step 4: Build Content Inventory

Create a structured inventory:

```
Section: [H2 heading]
  Key message: [one sentence]
  Content units:
    - [type]: [brief description]
    - [type]: [brief description]
  Best slide types: [from slide-type-catalog.md]
```

---

## Phase 2: Presentation Architecture

### Step 1: Design Slide Sequence

Map each section from the content inventory to a sequence of slides. For each slide, specify:

```
Slide N: [Slide Type]
  Message: [one sentence]
  Content: [what goes on this slide]
  Source section: [which part of the article]
  Animation: [what animation effect, if any]
```

### Step 2: Apply Rhythm Rules

Review the sequence and enforce these rules:

1. **No 3 consecutive same-type slides** — If you have 3 bullet slides in a row, convert the middle one to a different type (stat, quote, two-column)
2. **Breathing slide every 4-5 slides** — Insert a section divider, single stat, or transition slide
3. **Open with high impact** — First 2-3 slides: cover → hook stat or provocative question
4. **End each section with "so what"** — The last slide of each section should state the thesis/implication
5. **Alternate data-heavy and narrative** — Don't cluster all data slides together
6. **Close with callback** — Final slide should echo or answer the opening

### Step 3: User Confirmation

Present the slide plan to the user in a compact table:

```
| # | Type | Message |
|---|------|---------|
| 1 | Cover | [title] |
| 2 | Single Stat | [hook number] |
| 3 | Thesis | [opening claim] |
| ... | ... | ... |
```

Ask: "这个结构可以吗？需要调整哪里？" (Unless the user said "直接做" or "just do it", in which case skip confirmation.)

---

## Phase 3A: Generate — SlideDev Path

When the user chooses SlideDev, generate the following files:

### File Structure

```
project-name/
├── package.json
├── slides.md          # Main presentation
└── components/        # Optional custom components
    └── Counter.vue    # Only if needed
```

### package.json

```json
{
  "name": "presentation-name",
  "private": true,
  "scripts": {
    "dev": "slidev",
    "build": "slidev build",
    "export": "slidev export"
  },
  "dependencies": {
    "@slidev/cli": "latest",
    "@slidev/theme-default": "latest"
  }
}
```

### slides.md Generation Rules

1. **Global headmatter** at top of file — set theme, title, transition, fonts
2. **Slide separators**: `---` between slides, with optional frontmatter per slide
3. **Use built-in layouts**: cover, section, center, statement, fact, quote, two-cols, two-cols-header, image-right, full
4. **Progressive reveal**: Use `<v-clicks>` for lists, `v-click` for individual elements
5. **Transitions**: `slide-left` for normal flow, `fade` for section changes
6. **Speaker notes**: `<!-- notes -->` at bottom of each slide
7. **Chinese fonts**: Configure via headmatter `fonts` field

**Reference**: Load and follow `references/slidev-syntax.md` for exact syntax.

### Theme Selection

| Article Tone | SlideDev Theme |
|-------------|---------------|
| Business/formal | `seriph` |
| Technical | `default` |
| Casual/startup | `apple-basic` |
| Academic | `bricks` |

### Run Instructions

```bash
cd project-name
npm install
npm run dev
# Opens at http://localhost:3030
```

---

## Phase 3B: Generate — Reveal.js Path

When the user chooses Reveal.js, generate a single `index.html` file.

### Generation Rules

1. **Single self-contained HTML file** with CDN links for Reveal.js
2. **Use semantic slide structure**: `.reveal > .slides > section`
3. **Vertical slides** for sub-topics within a section (nested `<section>`)
4. **Fragment classes** for progressive reveal: `fade-up` for lists, `highlight-red` for comparisons, `grow` for stats
5. **Auto-animate** for smooth transitions: `data-auto-animate` with `data-id` matching
6. **Backgrounds**: gradient for section dividers, solid color for emphasis slides
7. **Speaker notes**: `<aside class="notes">` inside each section
8. **Chinese fonts**: Load via Google Fonts `@import` in `<style>`

**Reference**: Load and follow `references/revealjs-syntax.md` for exact syntax.

### Theme Selection

| Article Tone | Reveal.js Theme | Background |
|-------------|----------------|------------|
| Business/formal | `black` | Dark gradient |
| Technical | `moon` | Dark solid |
| Casual | `white` | Light |
| Academic | `simple` | Light |

### Transition Strategy

- Normal slide progression: `slide`
- Section changes: `fade`
- Key moments / big reveals: `zoom`
- Data/stat slides: `none` (instant, let the number speak)

### Run Instructions

```
直接在浏览器中打开 index.html 即可。
或用 live server: npx serve .
```

---

## Phase 4: Chinese Typography Rules

**Apply these rules whenever the source article is in Chinese.**

### Font Configuration

```css
font-family: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", "Hiragino Sans GB", sans-serif;
```

For both frameworks, ensure Chinese web fonts are loaded (not just system fonts). See `references/chinese-typography.md` for loading snippets.

### Content Density

- **Heading**: Maximum 15 Chinese characters
- **Body text per line**: Maximum 25 Chinese characters
- **Line height**: 1.8–2.0 (wider than English default)
- **Letter spacing**: 0.05em
- **Content per slide**: 20-30% less than English equivalent

### Emphasis Rules

- **NEVER use italic for Chinese text** — it's unreadable
- Use **bold** or **color** for emphasis instead
- Use Chinese quotation marks：「...」or "..."
- Use Arabic numerals for data (71%), Chinese for units (亿美元)

### Mixed Language

- Add thin space between Chinese and English/numbers
- Keep English terms in their original form (don't translate brand names, technical terms)

---

## Phase 5: Quality Checklist & Delivery

Before delivering the final output, verify every item:

### Content Quality
- [ ] Every slide has ONE clear message (can be stated in one sentence)
- [ ] No slide tries to convey two unrelated ideas
- [ ] Key data points from the article are prominently featured
- [ ] The narrative arc of the original article is preserved
- [ ] Nothing important from the article is missing

### Rhythm & Flow
- [ ] No 3+ consecutive slides of the same type
- [ ] Breathing room (section divider, stat, or transition) every 4-5 slides
- [ ] Opening is high-impact (not a boring agenda slide)
- [ ] Each section ends with a "so what" takeaway
- [ ] Closing echoes or answers the opening
- [ ] Total slide count is appropriate for the article length

### Visual & Technical
- [ ] Chinese fonts are properly loaded (web font, not just system fallback)
- [ ] All slides fit within the viewport (no scrolling)
- [ ] Animations serve a purpose (reveal information progressively, not decorative)
- [ ] Code/diagrams render correctly (if present)
- [ ] Color contrast is sufficient for readability
- [ ] Font sizes are large enough for projection

### Framework-Specific
- [ ] SlideDev: `npm run dev` works without errors
- [ ] SlideDev: All layouts are valid built-in layouts
- [ ] Reveal.js: HTML is valid, CDN links are correct
- [ ] Reveal.js: All plugins are properly initialized

### Speaker Support
- [ ] Speaker notes included for key slides (at minimum: cover, stat slides, thesis slides, closing)
- [ ] Notes contain talking points, not just slide content repeated

### Delivery

1. Write all files to the appropriate location
2. Provide run/open instructions
3. Summarize what was created:
   - Total slide count
   - Section breakdown
   - Key design choices made
4. Offer iteration: "需要调整哪些地方？比如增减幻灯片、修改配色、调整动画效果等"

---

## Content Type → Slide Type Mapping

This is the central reference table. Use it when deciding which slide type to use for each content unit.

| Content Type | Primary Slide Type | Alternative | SlideDev Layout | Reveal.js Pattern |
|-------------|-------------------|-------------|-----------------|-------------------|
| `thesis` | Thesis/Statement | Cover (if opening) | `center` or `statement` | Centered `<h2>` with `data-auto-animate` |
| `data_point` | Single Stat | Bullet List (if minor) | `fact` | Giant `<span>` + context `<p>` |
| `comparison` | Comparison Table | Two-Column | `two-cols` | `<table>` or side-by-side `<div>` |
| `quote` | Quote | Highlight | `quote` | `<blockquote>` with attribution |
| `narrative` | Story/Evidence | Bullet List | `default` with `v-clicks` | Fragments with `fade-up` |
| `concept` | Thesis/Statement | Diagram | `center` | Centered heading + auto-animate |
| `analogy` | Analogy | Two-Column | `two-cols` | Split layout with `.r-hstack` |
| `list` | Bullet List | Timeline (if chronological) | `default` with `<v-clicks>` | `<ul>` with fragment classes |
| `transition` | Transition | Section Divider | `section` | Background gradient + centered text |
| `evidence` | Story/Evidence | Comparison Table | `default` or `two-cols` | Fragments revealing proof points |

### Slide Type Details

For complete implementation details of each slide type (purpose, content limits, concrete code examples for both frameworks), load `references/slide-type-catalog.md`.

---

## Important Reminders

1. **Always analyze before generating.** Never skip Phase 1. The temptation to jump straight to slide generation produces mediocre results.

2. **Respect the article's logic.** The presentation should follow the article's argumentative structure, not impose a generic template. If the article builds an argument in 5 steps, the presentation should too.

3. **Less is more.** A 3000-character article does NOT need 50 slides. Aim for the slide count formula (1 per 150-250 Chinese chars) and err on the side of fewer, more impactful slides.

4. **Data slides should be dramatic.** When a number is impressive, give it a full slide with a giant font. Don't bury "71% market share" in a bullet list.

5. **The "so what" test.** For every slide, ask: "If the audience remembers only this slide, what do they take away?" If you can't answer, the slide needs redesign.

6. **Don't convert paragraphs into bullet points.** This is the #1 mistake in article-to-PPT conversion. Instead, extract the key message and find the right slide type for it.

7. **Use reference files.** Before generating any code, load the appropriate syntax reference (`references/slidev-syntax.md` or `references/revealjs-syntax.md`) and the slide type catalog (`references/slide-type-catalog.md`). These contain exact, tested code patterns.
