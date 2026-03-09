---
name: ai-ppt
description: 将文章/文档/PPT 转换为专业演示文稿。支持 SlideDev、Reveal.js 和零依赖纯 HTML 三种框架输出。当用户说"做成PPT""生成演示文稿""把这篇文章做成slides""转成PPT""把PPT转成网页""article to presentation""make a presentation from this article""convert to slides"等时触发。
---

# AI PPT Skill — Article-to-Presentation Converter

Transform articles, documents, and existing PowerPoint files into professional, well-structured presentations. Supports three output frameworks: SlideDev, Reveal.js, and zero-dependency HTML. This skill combines **deep content analysis** with **distinctive visual design** — understanding article structure, extracting key messages, mapping them to optimal slide types, and applying curated visual styles.

## Core Philosophy

1. **Content-First** — Always analyze the article deeply before designing any slides. The presentation structure should emerge from the content, not be imposed on it.
2. **One Message Per Slide** — Every slide must have exactly ONE clear takeaway. If you can't state it in one sentence, split the slide.
3. **Three Framework Options** — SlideDev (technical talks), Reveal.js (general use with CDN), or zero-dependency HTML (maximum portability and visual freedom).
4. **Presentation Rhythm** — Alternate slide types to maintain audience attention. Never three bullet slides in a row. Add breathing room every 4-5 slides.
5. **Show, Don't Tell** — For style selection, show visual previews rather than asking abstract questions. Let users pick from concrete examples.
6. **Anti-"AI Slop"** — Use curated style presets with distinctive typography, color, and animation. Avoid generic, template-y aesthetics.
7. **Chinese-First Typography** — When the article is in Chinese, apply CJK-specific typography rules throughout. See `references/chinese-typography.md`.
8. **Viewport Fitting** — Every slide must fit within the viewport. No scrolling within slides, ever. Use `clamp()` for responsive scaling.

---

## Execution Flow Summary

The workflow has **two mandatory user interaction points** before generation begins:

1. **Phase 0 Step 2** — Framework selection (Reveal.js / SlideDev / HTML)
2. **Phase 2 Step 1** — Visual style selection (present 3 preset options via AskUserQuestion, WAIT for response)

After content analysis (Phase 1), you MUST ask the user to choose a visual style before generating any slides. Do NOT auto-select a style and proceed directly to generation.

---

## Phase 0: Input Detection & Mode Selection

### Step 1: Detect Input Type

Identify what the user is providing:
- **File path (.md, .txt, etc.)**: Read the file and extract content → **Article-to-PPT mode**
- **File path (.pptx)**: Extract via `references/extract-pptx.py` → **PPT Conversion mode** (see Phase 4)
- **Pasted text**: Use directly → **Article-to-PPT mode**
- **URL**: Fetch the page content → **Article-to-PPT mode**

If no input is provided, ask the user to provide the article, file path, or .pptx file.

### Step 2: Framework Selection

Ask the user to choose their output framework:

> 请选择输出格式：
> 1. **Reveal.js** — 单个 HTML 文件，CDN 依赖，直接浏览器打开（推荐大多数场景）
> 2. **SlideDev** — Markdown 驱动，适合包含代码/图表的技术演讲
> 3. **纯 HTML** — 零依赖，单文件，自带导航/动画，最大视觉自由度
> 4. **帮我选** — 根据文章内容自动推荐

### Decision Matrix for "帮我选"

| Factor | → Reveal.js | → SlideDev | → 纯 HTML |
|--------|-------------|------------|-----------|
| Contains code snippets | | ✓ | |
| Contains diagrams/flowcharts | | ✓ | |
| Technical audience | | ✓ | |
| Quick sharing (email/Slack) | | | ✓ |
| No internet/Node.js needed | | | ✓ |
| Maximum visual customization | | | ✓ |
| Business/general audience | ✓ | | |
| Speaker notes needed | ✓ | ✓ | |
| Contains mostly narrative | ✓ | | |
| Contains data/statistics | ✓ | | |

**Default recommendation**: Reveal.js for most cases. Pure HTML when zero-dependency or distinctive visual design is prioritized.

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

## Phase 2: Style Discovery & Presentation Architecture

### Step 1: Visual Style Selection — MANDATORY USER INTERACTION

**CRITICAL: You MUST present style options to the user and WAIT for their response before generating any slides.** Do NOT skip this step. Do NOT auto-select a style. This is a required interaction point, not an optional step.

**Do NOT ask abstract questions like "深色还是浅色".** Instead, analyze the article's mood and audience, then present 3 curated style options using AskUserQuestion.

#### Determine the Article's Mood

| Article Characteristics | Mood | Recommended Presets |
|------------------------|------|-------------------|
| Business analysis, investor logic, competition | Impress (给人留下深刻印象) | Bold Signal, Electric Studio, Dark Botanical |
| Startup, creative industry, disruption | Excite (让人兴奋) | Creative Voltage, Neon Cyber, Split Pastel |
| Education, process, methodology | Calm (平静/信任) | Pastel Geometry, Swiss Modern, Paper & Ink |
| Thought leadership, cultural analysis | Inspire (激发思考) | Vintage Editorial, Notebook Tabs, Terminal Green |

#### Present 3 Options via AskUserQuestion

After determining the mood category, use the AskUserQuestion tool to present exactly 3 style options from `references/style-presets.md`. Each option should include the preset name and a short description of its look & feel. Add the recommended one as the first option with "(Recommended)" suffix.

Example (for a business analysis article with "Impress" mood):

```
Question: "根据文章风格，推荐以下视觉方案，选哪个？"
Options:
  A: "Bold Signal (Recommended)" — 深色背景 + 橙色强调卡片，Archivo Black 粗体标题，自信有力
  B: "Dark Botanical" — 纯黑底 + 暖金色渐变装饰，Cormorant 衬线标题，优雅高级
  C: "Electric Studio" — 黑白高对比 + 蓝色强调，Manrope 字体，科技专业感
```

The user can also select "Other" to describe their own preferences (e.g., "A的配色 + C的字体" or a completely custom style).

**Only proceed to slide generation AFTER receiving the user's style choice.** If the user says "随便" or "你来选" via the "Other" option, then pick the first recommended option based on mood analysis.

### Step 2: Determine Animation Mood

Based on the selected style, map to an animation mood from `references/animation-patterns.md`:

| Style Preset | Animation Mood | Key Effect |
|-------------|---------------|------------|
| Bold Signal, Electric Studio | Professional | Fast 0.2-0.3s, subtle |
| Dark Botanical | Dramatic | Slow 1-1.5s, scale + blur |
| Creative Voltage, Neon Cyber | Techy | Glow, glitch, grid |
| Split Pastel, Pastel Geometry | Playful | Bouncy, spring physics |
| Paper & Ink, Notebook Tabs | Editorial | Staggered text reveals |
| Swiss Modern | Calm | Gentle 1s fades |
| Vintage Editorial | Editorial | Staggered, type-focused |
| Terminal Green | Techy | Scan lines, cursor blink |

### Step 3: Design Slide Sequence

Map each section from the content inventory to a sequence of slides. For each slide, specify:

```
Slide N: [Slide Type]
  Message: [one sentence]
  Content: [what goes on this slide]
  Source section: [which part of the article]
  Animation: [from animation mood, e.g. "fade-up 0.6s staggered"]
```

### Step 4: Apply Rhythm Rules

Review the sequence and enforce these rules:

1. **No 3 consecutive same-type slides** — If you have 3 bullet slides in a row, convert the middle one to a different type (stat, quote, two-column)
2. **Breathing slide every 4-5 slides** — Insert a section divider, single stat, or transition slide
3. **Open with high impact** — First 2-3 slides: cover → hook stat or provocative question
4. **End each section with "so what"** — The last slide of each section should state the thesis/implication
5. **Alternate data-heavy and narrative** — Don't cluster all data slides together
6. **Close with callback** — Final slide should echo or answer the opening

### Step 5: User Confirmation

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
8. **Apply style preset colors** via scoped `<style>` blocks on individual slides

**Reference**: Load and follow `references/slidev-syntax.md` for exact syntax.

### Theme + Style Preset Integration

The SlideDev theme provides base layout. Style preset provides custom colors and typography:

```yaml
---
theme: default  # or seriph
fonts:
  sans: "Space Grotesk"  # From style preset
  serif: "Noto Serif SC"
  mono: "Fira Code"
  weights: "400,500,700,900"
  provider: google
---
```

Plus a global `<style>` block applying preset colors:

```html
<style>
:root {
  --slidev-font-family: "Space Grotesk", "Noto Sans SC", sans-serif;
}
.slidev-layout {
  background: var(--bg-primary, #1a1a1a);
  color: var(--text-primary, #ffffff);
}
.slidev-layout h1 {
  font-family: "Archivo Black", "Noto Sans SC", sans-serif;
}
</style>
```

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
9. **Apply style preset** via CSS custom properties in `:root`

**Reference**: Load and follow `references/revealjs-syntax.md` for exact syntax.

### Style Preset Integration

Override Reveal.js theme variables with the selected preset:

```css
/* Load preset fonts */
@import url('https://fonts.googleapis.com/css2?family=Archivo+Black&family=Space+Grotesk:wght@400;500&family=Noto+Sans+SC:wght@400;700;900&display=swap');

:root {
  --r-background-color: #1a1a1a;      /* From preset --bg-primary */
  --r-main-color: #ffffff;             /* From preset --text-primary */
  --r-heading-color: #FF5722;          /* From preset --accent */
  --r-main-font: "Space Grotesk", "Noto Sans SC", sans-serif;
  --r-heading-font: "Archivo Black", "Noto Sans SC", sans-serif;
}
```

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

## Phase 3C: Generate — Zero-Dependency HTML Path

When the user chooses pure HTML, generate a single self-contained HTML file with no external dependencies (fonts via Google Fonts are the only external request).

### Generation Rules

1. **Single HTML file** with all CSS and JS inline
2. **Follow the template** in `references/html-template.md` exactly
3. **Include viewport-base CSS** from `references/viewport-base.css` (mandatory)
4. **Apply style preset** via CSS custom properties
5. **Apply animation patterns** from `references/animation-patterns.md` matching the selected mood
6. **Include navigation**: keyboard (arrows, space, page up/down), touch/swipe, mouse wheel, progress bar, nav dots
7. **Inline editing** (optional): Include if user opted in during Phase 2; uses JS hover system with 400ms grace period
8. **Use `.reveal` class** on elements for entrance animations triggered by Intersection Observer
9. **Stagger child animations** via `transition-delay` on nth-child
10. **Respect `prefers-reduced-motion`** media query

### Slide Type Implementation

Use the HTML patterns from `references/html-template.md` for each slide type. Each slide is a `<section class="slide">` with a `.slide-content` container.

### Content Density Hard Limits

These are **maximum** values — exceeding any requires splitting into multiple slides:

| Slide Type | Maximum Content |
|-----------|----------------|
| Cover | 1 heading + 1 subtitle |
| Content/Bullet | 1 heading + 5 bullets |
| Feature Grid | 6 cards |
| Stat | 1 number + 1 context line + 1 detail |
| Quote | 3 lines + attribution |
| Two-Column | 2 × 3-4 items |
| Timeline | 3-5 time points |

### Run Instructions

```
直接在浏览器中打开 index.html 即可。无需任何安装。
支持键盘方向键、触控滑动、鼠标滚轮导航。
```

---

## Phase 4: PPT Conversion Mode

When the user provides a `.pptx` file, follow this conversion flow instead of the article-to-PPT flow.

### Step 1: Extract Content

Run the extraction script:

```bash
python references/extract-pptx.py input.pptx [output_dir]
```

This produces:
- `extracted-slides.json` — Structured content (titles, text, images, notes)
- `assets/` — Extracted images

If Python/python-pptx is not available, manually parse the user's description of their PPT content.

### Step 2: Review & Confirm

Present the extracted content to the user:

```
提取了 N 张幻灯片：
  1. [title] (X 张图片)
  2. [title]
  ...

需要调整内容吗？还是直接用这些内容生成？
```

### Step 3: Apply Style & Generate

1. Run Phase 2 (Style Discovery) to select a visual style
2. Map extracted slides to appropriate slide types from `references/slide-type-catalog.md`
3. Generate output using the chosen framework (Phase 3A/3B/3C)
4. Preserve: text content, images (as file paths), speaker notes

---

## Phase 5: Chinese Typography Rules

**Apply these rules whenever the source article is in Chinese.**

### Font Configuration

For style presets, pair the preset's display font with Chinese fallbacks:

```css
/* Display: Preset font + Chinese fallback */
font-family: "Archivo Black", "Noto Sans SC", sans-serif;
/* Body: Preset font + Chinese fallback */
font-family: "Space Grotesk", "Noto Sans SC", sans-serif;
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
- Use Chinese quotation marks："..."
- Use Arabic numerals for data (71%), Chinese for units (亿美元)

### Mixed Language

- Add thin space between Chinese and English/numbers
- Keep English terms in their original form (don't translate brand names, technical terms)

---

## Phase 6: Quality Checklist & Delivery

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

### Visual & Style
- [ ] Selected style preset is consistently applied (fonts, colors, layout patterns)
- [ ] Chinese fonts are properly loaded (web font, not just system fallback)
- [ ] All slides fit within the viewport (no scrolling)
- [ ] Animations match the mood (dramatic → slow, professional → fast, etc.)
- [ ] Animations serve a purpose (reveal information progressively, not decorative)
- [ ] Color contrast is sufficient for readability
- [ ] Font sizes are large enough for projection
- [ ] Design feels distinctive, not generic "AI slop"

### Framework-Specific
- [ ] SlideDev: `npm run dev` works without errors
- [ ] SlideDev: All layouts are valid built-in layouts
- [ ] Reveal.js: HTML is valid, CDN links are correct
- [ ] Reveal.js: All plugins are properly initialized
- [ ] Pure HTML: Navigation (keyboard, touch, wheel) works correctly
- [ ] Pure HTML: Intersection Observer triggers animations on scroll
- [ ] Pure HTML: Progress bar and nav dots update correctly

### Speaker Support
- [ ] Speaker notes included for key slides (at minimum: cover, stat slides, thesis slides, closing)
- [ ] Notes contain talking points, not just slide content repeated

### Delivery

1. Write all files to the appropriate location
2. Provide run/open instructions
3. Summarize what was created:
   - Total slide count
   - Section breakdown
   - Style preset used and why
   - Key design choices made
4. Offer iteration: "需要调整哪些地方？比如增减幻灯片、换一套风格、调整动画效果等"

---

## Content Type → Slide Type Mapping

This is the central reference table. Use it when deciding which slide type to use for each content unit.

| Content Type | Primary Slide Type | Alternative | SlideDev Layout | Reveal.js Pattern | HTML Pattern |
|-------------|-------------------|-------------|-----------------|-------------------|-------------|
| `thesis` | Thesis/Statement | Cover (if opening) | `center` or `statement` | Centered `<h2>` with `data-auto-animate` | `.slide-content` centered `<h2>` |
| `data_point` | Single Stat | Bullet List (if minor) | `fact` | Giant `<span>` + context `<p>` | `.big-number` + `<p>` |
| `comparison` | Comparison Table | Two-Column | `two-cols` | `<table>` or side-by-side `<div>` | `.two-cols` |
| `quote` | Quote | Highlight | `quote` | `<blockquote>` with attribution | `<blockquote>` + `.attribution` |
| `narrative` | Story/Evidence | Bullet List | `default` with `v-clicks` | Fragments with `fade-up` | `.reveal` staggered |
| `concept` | Thesis/Statement | Diagram | `center` | Centered heading + auto-animate | Centered `<h2>` |
| `analogy` | Analogy | Two-Column | `two-cols` | Split layout with `.r-hstack` | `.two-cols` |
| `list` | Bullet List | Timeline (if chronological) | `default` with `<v-clicks>` | `<ul>` with fragment classes | `<ul>` with `.reveal` |
| `transition` | Transition | Section Divider | `section` | Background gradient + centered text | Gradient bg + centered |
| `evidence` | Story/Evidence | Comparison Table | `default` or `two-cols` | Fragments revealing proof points | `.reveal` staggered |

### Slide Type Details

For complete implementation details of each slide type (purpose, content limits, concrete code examples for all three frameworks), load `references/slide-type-catalog.md` and `references/html-template.md`.

---

## Reference Files

Load the appropriate reference files before generating:

| File | Purpose | When to Load |
|------|---------|-------------|
| `references/slide-type-catalog.md` | 15 slide types with SlideDev/Reveal.js code | Always |
| `references/style-presets.md` | 12 visual style presets | During Phase 2 style selection |
| `references/animation-patterns.md` | 6 emotion-driven animation categories | During Phase 2 & generation |
| `references/slidev-syntax.md` | SlideDev syntax reference | When generating SlideDev |
| `references/revealjs-syntax.md` | Reveal.js syntax reference | When generating Reveal.js |
| `references/html-template.md` | Zero-dep HTML template + patterns | When generating pure HTML |
| `references/viewport-base.css` | Responsive viewport CSS | When generating pure HTML |
| `references/chinese-typography.md` | CJK typography rules | When source is Chinese |
| `references/extract-pptx.py` | PowerPoint content extractor | When input is .pptx |

---

## Important Reminders

1. **Always analyze before generating.** Never skip Phase 1. The temptation to jump straight to slide generation produces mediocre results.

2. **Respect the article's logic.** The presentation should follow the article's argumentative structure, not impose a generic template. If the article builds an argument in 5 steps, the presentation should too.

3. **Less is more.** A 3000-character article does NOT need 50 slides. Aim for the slide count formula (1 per 150-250 Chinese chars) and err on the side of fewer, more impactful slides.

4. **Data slides should be dramatic.** When a number is impressive, give it a full slide with a giant font. Don't bury "71% market share" in a bullet list.

5. **The "so what" test.** For every slide, ask: "If the audience remembers only this slide, what do they take away?" If you can't answer, the slide needs redesign.

6. **Don't convert paragraphs into bullet points.** This is the #1 mistake in article-to-PPT conversion. Instead, extract the key message and find the right slide type for it.

7. **Use reference files.** Before generating any code, load the appropriate references. These contain exact, tested code patterns.

8. **Show style, don't ask abstractly.** Present 3 visual previews based on article mood. Never ask "深色还是浅色?" without context.

9. **Match animation to mood.** A financial analysis shouldn't bounce; a creative pitch shouldn't use corporate 0.2s fades. See `references/animation-patterns.md`.

10. **Distinctive over generic.** Every presentation should feel custom-crafted. Use the curated presets from `references/style-presets.md` instead of framework defaults.
