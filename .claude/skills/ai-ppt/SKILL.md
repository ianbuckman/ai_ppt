---
name: ai-ppt
description: 将文章/文档转换为专业演示文稿。使用 Reveal.js 框架输出单文件 HTML。当用户说"做成PPT""生成演示文稿""把这篇文章做成slides""转成PPT""article to presentation""make a presentation from this article""convert to slides"等时触发。
---

# AI PPT Skill — Article-to-Presentation Orchestrator

Transform articles and documents into professional, well-structured presentations. This is the **main orchestrator** that coordinates specialized sub-skills for style selection and framework-specific generation.

## Core Philosophy

1. **Content-First** — Always analyze the article deeply before designing any slides. The presentation structure should emerge from the content, not be imposed on it.
2. **One Message Per Slide** — Every slide must have exactly ONE clear takeaway. If you can't state it in one sentence, split the slide.
3. **Reveal.js Output** — Single-file HTML with CDN dependencies, suitable for all presentation scenarios.
4. **Presentation Rhythm** — Alternate slide types to maintain audience attention. Never three bullet slides in a row. Add breathing room every 4-5 slides.
5. **Show, Don't Tell** — For style selection, show visual previews rather than asking abstract questions. Let users pick from concrete examples.
6. **Anti-"AI Slop"** — Use curated style presets with distinctive typography, color, and animation. Avoid generic, template-y aesthetics.
7. **Chinese-First Typography** — When the article is in Chinese, apply CJK-specific typography rules. See `references/chinese-typography.md`.
8. **Viewport Fitting** — Every slide must fit within the viewport. No scrolling within slides, ever. Use `clamp()` for responsive scaling.

---

## Sub-Skill Architecture

This skill orchestrates 6 specialized sub-skills:

| Sub-Skill | Path | Responsibility |
|-----------|------|---------------|
| **ai-ppt-style** | `.claude/skills/ai-ppt-style/SKILL.md` | Style discovery, preset/custom style selection, slide sequence design |
| **ai-ppt-revealjs** | `.claude/skills/ai-ppt-revealjs/SKILL.md` | Reveal.js (single HTML with CDN) generation |
| **ai-ppt-image** | `.claude/skills/ai-ppt-image/SKILL.md` | Image audit, sufficiency analysis, gap recommendations |
| **ai-ppt-extract** | `.claude/skills/ai-ppt-extract/SKILL.md` | PPTX content extraction |

---

## Execution Flow

```
Phase 0: Input Detection (this skill)
    ↓
Phase 1: Deep Content Analysis (this skill) + Load ai-ppt-image for image audit
    ↓
[If .pptx input] → Load ai-ppt-extract → extract content → return here
    ↓
[If images insufficient] → ai-ppt-image handles recommendations + user interaction
    ↓
Phase 2: Load ai-ppt-style → style selection + slide architecture
    ↓
Phase 3: Load ai-ppt-revealjs → generate Reveal.js presentation
    ↓
Phase 4: Quality Checklist & Delivery (this skill)
```

**One mandatory + one conditional user interaction points:**
1. **Phase 1.5 Step 3** — *(conditional)* Image recommendations — triggered when the Image Sufficiency Analysis (Phase 1 Step 3) returns `partial` or `none`. Present a table of image gaps with priorities, and ask the user if they want to provide additional images. Accounts for images already kept from the source.
2. **Phase 2 Step 1** — Visual style selection (handled by ai-ppt-style)

After content analysis (Phase 1), audit all source images (keep/adapt/drop), then run the Image Sufficiency Analysis. If images are sufficient for the planned slides, proceed directly to style selection. If images are insufficient (`partial` or `none`), trigger Phase 1.5 to recommend additional images with a clear table and ask the user. Then proceed to style selection. Do NOT auto-select a style and proceed directly to generation.

---

## Phase 0: Input Detection

### Step 1: Detect Input Type

| Input | Action |
|-------|--------|
| File path (.md, .txt, etc.) | Read file → Article-to-PPT mode |
| Pasted text | Use directly → Article-to-PPT mode |
| URL | Fetch page content → Article-to-PPT mode |

If no input is provided, ask the user.

**Framework**: Always use **Reveal.js** — single HTML file with CDN dependencies, directly opens in browser. No framework selection needed.

---

## Phase 1: Deep Content Analysis

**This is the most critical phase.** Do not skip or rush it. The quality of the final presentation depends entirely on how well you understand the article.

### Step 1: Parse Structure

- Identify all headings (H1, H2, H3) → section boundaries
- Count total characters/words → estimate slide count
- **Slide count formula**: ~1 slide per 150-250 Chinese characters (or 80-150 English words)
- Typical range: 15-35 slides for a 3000-5000 character article

### Step 2: Tag Content Units

Go through the article paragraph by paragraph and tag each unit:

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

### Step 3: Image Audit & Sufficiency Analysis

**Load and follow `.claude/skills/ai-ppt-image/SKILL.md`** for this step.

Pass the source content and estimated slide sequence to the image sub-skill. It will:
1. Audit all source images (classify, keep/adapt/drop)
2. Run Image Sufficiency Analysis (coverage ratio, category gaps, key slide coverage)
3. Return a sufficiency verdict: `sufficient`, `partial`, or `none`
4. If `partial` or `none`: generate a recommendation table with priorities and ask the user via AskUserQuestion

The image sub-skill returns the final image inventory (including any user-provided images) to use in subsequent steps.

### Step 4: Extract Key Elements

Pull out and list:
- **Key data points**: All numbers, percentages, dollar amounts
- **Quotable phrases**: Memorable statements that work as standalone slides
- **Named concepts**: Frameworks, models, coined terms
- **Comparisons**: Any side-by-side contrasts
- **The central thesis**: The one sentence that captures the entire article

### Step 5: Build Content Inventory

Create a structured inventory (include image assignments from Step 3):

```
Section: [H2 heading]
  Key message: [one sentence]
  Content units:
    - [type]: [brief description]
    - [type]: [brief description]
  Images: [filename → placement] (if any)
  Best slide types: [from slide-type-catalog.md]
```

---

## Phase 2: Style Discovery & Slide Architecture

**Load and follow `.claude/skills/ai-ppt-style/SKILL.md`** for this entire phase.

Pass the content inventory from Phase 1 to the style sub-skill. It will:
1. Analyze article mood → recommend 3 presets + 1 custom option
2. Open 3 preset preview HTMLs in browser, then present style options to the user (MANDATORY wait point — single selection, no second confirmation)
3. If user selects "自定义参考风格", guide them through the custom extraction workflow → **generate a style preview HTML** (`style-preview.html`) → open alongside 3 comparison presets → user selects final style
4. Determine animation mood
5. Design slide sequence with rhythm rules
6. Get user confirmation on the slide plan

---

## Phase 3: Generate Presentation

**Load and follow `.claude/skills/ai-ppt-revealjs/SKILL.md`** to generate the Reveal.js presentation.

Pass to the Reveal.js sub-skill:
- The content inventory (Phase 1), including image inventory
- The chosen style preset with CSS variables, fonts, layout signature (Phase 2)
- The animation mood (Phase 2)
- The confirmed slide sequence (Phase 2)

---

## Phase 4: Quality Checklist & Delivery

Before delivering the final output, verify every item:

### Content Quality
- [ ] Every slide has ONE clear message
- [ ] No slide tries to convey two unrelated ideas
- [ ] Key data points from the article are prominently featured
- [ ] The narrative arc of the original article is preserved
- [ ] Nothing important from the article is missing

### Rhythm & Flow
- [ ] No 3+ consecutive slides of the same type
- [ ] Breathing room every 4-5 slides
- [ ] Opening is high-impact (not a boring agenda slide)
- [ ] Each section ends with a "so what" takeaway
- [ ] Closing echoes or answers the opening
- [ ] Total slide count is appropriate for article length

### Visual & Style
- [ ] Selected style preset is consistently applied
- [ ] Chinese fonts are properly loaded (web font, not just system fallback)
- [ ] All slides fit within the viewport (no scrolling)
- [ ] Animations match the mood
- [ ] Color contrast is sufficient for readability
- [ ] Design feels distinctive, not generic

### Images
- [ ] All kept images are properly sized and positioned for slide scale (not overflowing, not tiny)
- [ ] No placeholder images or stock photo URLs were inserted without user consent
- [ ] Text-heavy source images were adapted (data extracted into clean slide elements) rather than embedded as-is
- [ ] Decorative/redundant images from the source were dropped, not blindly included
- [ ] If user provided images after recommendation, they are integrated into the correct slides with proper placement

### Framework-Specific
- [ ] Reveal.js: HTML is valid; CDN links correct; plugins initialized

### Speaker Support
- [ ] Speaker notes for key slides (cover, stats, thesis, closing)
- [ ] Notes contain talking points, not repeated slide content

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

Central reference table for deciding which slide type to use for each content unit.

| Content Type | Primary Slide Type | Alternative | Reveal.js Pattern |
|-------------|-------------------|-------------|-------------------|
| `thesis` | Thesis/Statement | Cover (if opening) | Centered `<h2>` + auto-animate |
| `data_point` | Single Stat | Bullet List (if minor) | Giant `<span>` + `<p>` |
| `comparison` | Comparison Table | Two-Column | `<table>` or `<div>` |
| `quote` | Quote | Highlight | `<blockquote>` |
| `narrative` | Story/Evidence | Bullet List | Fragments `fade-up` |
| `concept` | Thesis/Statement | Diagram | Centered heading |
| `analogy` | Analogy | Two-Column | `.r-hstack` |
| `list` | Bullet List | Timeline | `<ul>` fragments |
| `transition` | Transition | Section Divider | Gradient bg + centered |
| `evidence` | Story/Evidence | Comparison Table | Fragments |

### Image-Enhanced Slide Types

See `.claude/skills/ai-ppt-image/SKILL.md` for the full image-enhanced slide types reference table and image placement guidance.

For complete slide type implementation details, load `references/slide-type-catalog.md`.

---

## Chinese Typography Rules

**Apply whenever the source article is in Chinese.** Full rules in `references/chinese-typography.md`.

Key rules:
- **NEVER** use italic for Chinese text — use bold or color
- Heading max: 15 Chinese characters
- Body line max: 25 Chinese characters
- Line height: 1.8–2.0
- Letter spacing: 0.05em
- Pair preset fonts with Chinese fallbacks: `"[Preset Font]", "Noto Sans SC", sans-serif`
- Full-width punctuation: ，。：；"…"——？！
- Arabic numerals for data (71%, 1224亿美元)

---

## Reference Files (this skill)

| File | Purpose | When to Load |
|------|---------|-------------|
| `references/slide-type-catalog.md` | 15 slide types with code examples (includes image placement guidance) | Always — needed for content mapping |
| `references/chinese-typography.md` | CJK typography rules | When source is Chinese |

---

## Important Reminders

1. **Always analyze before generating.** Never skip Phase 1. The temptation to jump straight to slide generation produces mediocre results.

2. **Respect the article's logic.** The presentation should follow the article's argumentative structure, not impose a generic template. If the article builds an argument in 5 steps, the presentation should too.

3. **Less is more.** A 3000-character article does NOT need 50 slides. Aim for the slide count formula (1 per 150-250 Chinese chars) and err on the side of fewer, more impactful slides.

4. **Data slides should be dramatic.** When a number is impressive, give it a full slide with a giant font. Don't bury "71% market share" in a bullet list.

5. **The "so what" test.** For every slide, ask: "If the audience remembers only this slide, what do they take away?" If you can't answer, the slide needs redesign.

6. **Don't convert paragraphs into bullet points.** This is the #1 mistake in article-to-PPT conversion. Instead, extract the key message and find the right slide type for it.

7. **Load sub-skill references before generating.** Each sub-skill lists its required references. These contain exact, tested code patterns.

8. **Show style, don't ask abstractly.** Present 3 visual previews based on article mood. Never ask "深色还是浅色?" without context.

9. **Match animation to mood.** A financial analysis shouldn't bounce; a creative pitch shouldn't use corporate 0.2s fades.

10. **Distinctive over generic.** Every presentation should feel custom-crafted. Use the curated presets instead of framework defaults.

11. **Audit images, don't blindly include.** Read every image in the source to understand its content. Drop decorative/redundant images. Adapt text-heavy screenshots into clean slide elements. Only keep images that genuinely enhance comprehension.

12. **Recommend images thoughtfully, not generically.** When source images are insufficient (`partial` or `none`), only suggest visuals for slides where they would meaningfully help (product photos, comparison visuals, maps, portraits). Account for images already kept from the source — don't recommend what's already covered. Present gaps as a clear table with priorities. Most slides work fine with typography and color alone. Never insert placeholder images or stock photo URLs.
