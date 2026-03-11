---
name: ai-ppt
description: 将文章/文档转换为专业演示文稿。支持 SlideDev、Reveal.js 和零依赖纯 HTML 三种框架输出。当用户说"做成PPT""生成演示文稿""把这篇文章做成slides""转成PPT""article to presentation""make a presentation from this article""convert to slides"等时触发。
---

# AI PPT Skill — Article-to-Presentation Orchestrator

Transform articles and documents into professional, well-structured presentations. This is the **main orchestrator** that coordinates specialized sub-skills for style selection and framework-specific generation.

## Core Philosophy

1. **Content-First** — Always analyze the article deeply before designing any slides. The presentation structure should emerge from the content, not be imposed on it.
2. **One Message Per Slide** — Every slide must have exactly ONE clear takeaway. If you can't state it in one sentence, split the slide.
3. **Three Framework Options** — SlideDev (technical talks), Reveal.js (general use with CDN), or zero-dependency HTML (maximum portability and visual freedom).
4. **Presentation Rhythm** — Alternate slide types to maintain audience attention. Never three bullet slides in a row. Add breathing room every 4-5 slides.
5. **Show, Don't Tell** — For style selection, show visual previews rather than asking abstract questions. Let users pick from concrete examples.
6. **Anti-"AI Slop"** — Use curated style presets with distinctive typography, color, and animation. Avoid generic, template-y aesthetics.
7. **Chinese-First Typography** — When the article is in Chinese, apply CJK-specific typography rules. See `references/chinese-typography.md`.
8. **Viewport Fitting** — Every slide must fit within the viewport. No scrolling within slides, ever. Use `clamp()` for responsive scaling.

---

## Sub-Skill Architecture

This skill orchestrates 5 specialized sub-skills:

| Sub-Skill | Path | Responsibility |
|-----------|------|---------------|
| **ai-ppt-style** | `.claude/skills/ai-ppt-style/SKILL.md` | Style discovery, preset/custom style selection, slide sequence design |
| **ai-ppt-slidev** | `.claude/skills/ai-ppt-slidev/SKILL.md` | SlideDev (Markdown) presentation generation |
| **ai-ppt-revealjs** | `.claude/skills/ai-ppt-revealjs/SKILL.md` | Reveal.js (single HTML with CDN) generation |
| **ai-ppt-html** | `.claude/skills/ai-ppt-html/SKILL.md` | Zero-dependency HTML generation |
| **ai-ppt-extract** | `.claude/skills/ai-ppt-extract/SKILL.md` | PPTX content extraction |

---

## Execution Flow

```
Phase 0: Input Detection & Framework Selection (this skill)
    ↓
Phase 1: Deep Content Analysis + Image Audit (this skill)
    ↓
[If .pptx input] → Load ai-ppt-extract → extract content → return here
    ↓
[If images insufficient] → Phase 1.5: Image Recommendations (this skill, conditional)
    ↓
Phase 2: Load ai-ppt-style → style selection + slide architecture
    ↓
Phase 3: Load framework sub-skill based on user choice:
    → ai-ppt-slidev  (if SlideDev)
    → ai-ppt-revealjs (if Reveal.js)
    → ai-ppt-html     (if pure HTML)
    ↓
Phase 4: Quality Checklist & Delivery (this skill)
```

**Two mandatory + one conditional user interaction points:**
1. **Phase 0 Step 2** — Framework selection
2. **Phase 1.5 Step 3** — *(conditional)* Image recommendations — triggered when the Image Sufficiency Analysis (Phase 1 Step 3) returns `partial` or `none`. Present a table of image gaps with priorities, and ask the user if they want to provide additional images. Accounts for images already kept from the source.
3. **Phase 2 Step 1** — Visual style selection (handled by ai-ppt-style)

After content analysis (Phase 1), audit all source images (keep/adapt/drop), then run the Image Sufficiency Analysis. If images are sufficient for the planned slides, proceed directly to style selection. If images are insufficient (`partial` or `none`), trigger Phase 1.5 to recommend additional images with a clear table and ask the user. Then proceed to style selection. Do NOT auto-select a style and proceed directly to generation.

---

## Phase 0: Input Detection & Mode Selection

### Step 1: Detect Input Type

| Input | Action |
|-------|--------|
| File path (.md, .txt, etc.) | Read file → Article-to-PPT mode |
| Pasted text | Use directly → Article-to-PPT mode |
| URL | Fetch page content → Article-to-PPT mode |

If no input is provided, ask the user.

### Step 2: Framework Selection

Ask the user via AskUserQuestion:

```
请选择输出格式：
A: "Reveal.js (Recommended)" — 单个 HTML 文件，CDN 依赖，直接浏览器打开
B: "SlideDev" — Markdown 驱动，适合包含代码/图表的技术演讲
C: "纯 HTML" — 零依赖，单文件，自带导航/动画，最大视觉自由度
D: "帮我选" — 根据文章内容自动推荐
```

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

**Default**: Reveal.js for most cases. Pure HTML when zero-dependency or distinctive visual design is prioritized.

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

### Step 3: Image Audit

Scan the source content for all images (`![](...)`, `<img>`, or embedded media). For each image found:

1. **Read the image** using the Read tool to understand its visual content
2. **Classify** the image into one of these categories:

| Category | Description | Slide Suitability |
|----------|-------------|-------------------|
| `chart` | Data visualization, graph, pie chart | High — use on stat or comparison slides |
| `diagram` | Flowchart, architecture, process | High — use on diagram or concept slides |
| `screenshot` | UI, product, app interface | Medium — use if relevant to the narrative, crop/resize for slide |
| `photo` | People, places, real-world scenes | Medium — use as background or on story/evidence slides |
| `decorative` | Stock art, generic illustration, clip art | Low — usually omit, presentation has its own visual system |
| `logo` | Brand mark, icon | Low — only keep if directly relevant (e.g., company being discussed) |
| `text-heavy` | Scanned document, dense table screenshot, infographic with small text | Low — unreadable at slide scale, extract the data instead |

3. **Decide** for each image:
   - **Keep** — assign it to a specific slide and specify placement (background, inline, side-by-side)
   - **Adapt** — the data is useful but the image format isn't suitable for slides; extract key info and recreate as a slide element (e.g., turn a complex table screenshot into a clean comparison slide)
   - **Drop** — the image adds no value to the presentation (decorative, redundant, or unreadable at slide scale)

4. **Build an image inventory**:

```
Images found: N
  ✓ Keep: [filename] → Slide N ([slide type], [placement])
  ↻ Adapt: [filename] → Extract [what] for Slide N
  ✗ Drop: [filename] — Reason: [decorative/redundant/unreadable]
```

5. **Image Sufficiency Analysis**:

After building the image inventory, evaluate whether the kept images are sufficient for a high-quality presentation. Compare against the [Image-Enhanced Slide Types](#image-enhanced-slide-types) table to identify gaps.

| Factor | How to Assess |
|--------|--------------|
| Coverage ratio | Count kept images vs. slides that would benefit from images (Cover, Two-Column, Story/Evidence, Quote, Analogy). If ratio < 0.5, images are **insufficient**. |
| Category gaps | Do the kept images cover the needed categories? E.g., all charts but no product photos for a product showcase slide. List uncovered categories. |
| Key slide coverage | Are high-impact slides (cover, company/product showcase, closing) served? These carry disproportionate visual weight. |

Produce a sufficiency verdict:

| Verdict | Condition | Next Step |
|---------|-----------|-----------|
| `sufficient` | Kept images cover all high-impact slides AND coverage ratio >= 0.7 | Skip Phase 1.5, proceed to Phase 2 |
| `partial` | Some kept images but coverage ratio < 0.7 OR key slides lack images | Trigger Phase 1.5 with partial inventory |
| `none` | Zero kept images (includes case where all images were dropped) | Trigger Phase 1.5 with empty inventory |

Append the verdict to the image inventory:

```
Image Sufficiency: [sufficient / partial / none]
  Kept images: N
  Slides needing images: M (list slide numbers)
  Coverage ratio: N/M
  Gaps: [list specific unserved slide types/topics]
```

Proceed to Step 4.

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

## Phase 1.5: Image Recommendations (When Source Images Are Insufficient)

**Trigger**: Phase 1 Step 3 Image Sufficiency Analysis returned `partial` or `none`.

**Purpose**: Identify slides that would benefit significantly from visual content beyond what the source already provides, and suggest specific additional images the user could provide.

This phase is NOT about adding generic stock photos to every slide. Most slides work well with typography, color, and layout alone (that's what the style presets are for). Only recommend images where they would **meaningfully enhance comprehension or impact**. When the source already has some kept images (`partial`), focus recommendations on the **gaps** — slides that need images but don't have any from the source.

### Step 1: Identify Image Gaps

Reference the Image Sufficiency Analysis from Phase 1 Step 3. Note which slides already have kept images assigned. Then review the **remaining slides** (those without images) and flag any where an image would deliver clear value:

| Slide Scenario | Why Image Helps | Suggested Image Type |
|---------------|-----------------|---------------------|
| Discussing a specific product/company | Audience needs a visual anchor | Product photo or logo |
| Before/after or physical comparison | Words alone can't convey the difference | Side-by-side photos |
| Geographic or spatial argument | Locations matter to the story | Map or satellite view |
| Describing a physical process | Abstract text → concrete visual | Process photo or diagram |
| Person quoted or profiled | Humanizes the narrative | Headshot or candid photo |
| Data without context | Number needs scale reference | Contextual photo (e.g., "the size of 3 football fields") |

Do NOT recommend images for:
- Thesis/statement slides (typography IS the visual)
- Section dividers (style preset handles these)
- Data/stat slides (the number IS the visual)
- Abstract concepts that would require generic stock photos

### Step 2: Generate Recommendation Table

Build a clear recommendation table with priorities. The format adapts based on the sufficiency verdict:

**Priority definitions**:

| Priority | Label | Meaning |
|----------|-------|---------|
| Must-have | 必要 | This slide loses significant impact without an image (e.g., cover of a product launch, company profile) |
| Recommended | 推荐 | Image would noticeably improve the slide but it can work without one |
| Nice-to-have | 加分项 | Image would be a bonus but the slide works fine with typography alone |

**When sufficiency = `partial`** (source has some images):

```
📷 源内容已有以下可用图片：
  ✓ [filename] → Slide N ([slide type], [placement])
  ✓ [filename] → Slide N ([slide type], [placement])

📷 以下幻灯片补充图片会显著提升效果：

| # | 幻灯片 | 需要什么图片 | 用途/位置 | 优先级 | 建议来源 |
|---|--------|------------|----------|--------|---------|
| 1 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 必要 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 2 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 推荐 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 3 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 加分项 | [用户自有照片 / 产品截图 / 建议搜索词] |

其余幻灯片通过排版和配色已有足够视觉表现力，无需额外图片。
如果没有合适的图片也完全没问题，已有图片 + 纯排版方案一样能生成高质量演示。
```

**When sufficiency = `none`** (no images at all):

```
📷 源内容不包含图片。以下幻灯片补充图片会显著提升效果：

| # | 幻灯片 | 需要什么图片 | 用途/位置 | 优先级 | 建议来源 |
|---|--------|------------|----------|--------|---------|
| 1 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 必要 | [用户自有照片 / 产品截图 / 建议搜索词] |
| 2 | Slide N: [title] | [specific image description] | background / inline / side-by-side | 推荐 | [用户自有照片 / 产品截图 / 建议搜索词] |
| ...

其余幻灯片通过排版和配色已有足够视觉表现力，无需额外图片。
如果没有合适的图片也完全没问题，我会用纯排版方案生成。
```

### Step 3: Ask User

Present the recommendation table (from Step 2) first, then ask via AskUserQuestion. The question adapts based on sufficiency:

**When sufficiency = `partial`**:
```
Question: "源内容有一些图片已保留，但以下幻灯片补充图片效果会更好（见上表）。要提供额外图片吗？"
Options:
  A: "我来补充图片" — 用户将提供额外图片文件或 URL
  B: "用已有图片就好，直接生成" — 保留源内容图片，其余用纯排版
  C: "部分采纳" — 用户选择性提供部分推荐图片
```

**When sufficiency = `none`**:
```
Question: "源内容没有图片。以下幻灯片加上图片会更好（见上表），要提供图片吗？"
Options:
  A: "我来提供图片" — 用户将提供图片文件或 URL
  B: "不需要图片，直接生成" — 用纯排版方案，跳过图片
  C: "部分采纳" — 用户选择性提供部分图片
```

- If the user provides images, run Phase 1 Step 3 (Image Audit) on the newly provided images and integrate them into the slide sequence **alongside any already-kept source images**
- If the user declines, proceed with whatever images are already kept (if `partial`) or pure typography-driven slides (if `none`)
- **NEVER insert placeholder images or unsplash/pexels URLs on your own** — only use images the user explicitly provides

---

## Phase 2: Style Discovery & Slide Architecture

**Load and follow `.claude/skills/ai-ppt-style/SKILL.md`** for this entire phase.

Pass the content inventory from Phase 1 to the style sub-skill. It will:
1. Analyze article mood → recommend 3 presets + 1 custom option
2. Present style options to the user (MANDATORY wait point)
3. If user selects "自定义参考风格", guide them through the custom extraction workflow → **generate a style preview HTML** (`style-preview.html`) for the user to see the extracted style applied to sample slides → iterate until the user is satisfied
4. Determine animation mood
5. Design slide sequence with rhythm rules
6. Get user confirmation on the slide plan

---

## Phase 3: Generate Presentation

Based on the user's framework choice from Phase 0, **load and follow** the corresponding sub-skill:

| Framework | Sub-Skill to Load |
|-----------|------------------|
| SlideDev | **`.claude/skills/ai-ppt-slidev/SKILL.md`** |
| Reveal.js | **`.claude/skills/ai-ppt-revealjs/SKILL.md`** |
| 纯 HTML | **`.claude/skills/ai-ppt-html/SKILL.md`** |

Pass to the framework sub-skill:
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
- [ ] SlideDev: `npm run dev` works without errors; layouts are valid
- [ ] Reveal.js: HTML is valid; CDN links correct; plugins initialized
- [ ] Pure HTML: Navigation (keyboard, touch, wheel) works; animations trigger on scroll

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

| Content Type | Primary Slide Type | Alternative | SlideDev Layout | Reveal.js Pattern | HTML Pattern |
|-------------|-------------------|-------------|-----------------|-------------------|-------------|
| `thesis` | Thesis/Statement | Cover (if opening) | `center` / `statement` | Centered `<h2>` + auto-animate | Centered `<h2>` |
| `data_point` | Single Stat | Bullet List (if minor) | `fact` | Giant `<span>` + `<p>` | `.big-number` + `<p>` |
| `comparison` | Comparison Table | Two-Column | `two-cols` | `<table>` or `<div>` | `.two-cols` |
| `quote` | Quote | Highlight | `quote` | `<blockquote>` | `<blockquote>` + `.attribution` |
| `narrative` | Story/Evidence | Bullet List | `default` + `v-clicks` | Fragments `fade-up` | `.reveal` staggered |
| `concept` | Thesis/Statement | Diagram | `center` | Centered heading | Centered `<h2>` |
| `analogy` | Analogy | Two-Column | `two-cols` | `.r-hstack` | `.two-cols` |
| `list` | Bullet List | Timeline | `default` + `<v-clicks>` | `<ul>` fragments | `<ul>` + `.reveal` |
| `transition` | Transition | Section Divider | `section` | Gradient bg + centered | Gradient bg |
| `evidence` | Story/Evidence | Comparison Table | `default` / `two-cols` | Fragments | `.reveal` staggered |

### Image-Enhanced Slide Types

When images are available (from audit or user-provided), these slide types benefit most:

| Slide Type | Image Usage | Placement |
|-----------|------------|-----------|
| Cover | Hero image as background (dimmed) | `background-image` with 0.2-0.4 opacity overlay |
| Two-Column | Photo/screenshot on one side, text on the other | `image-right` layout or CSS grid |
| Story/Evidence | Contextual photo supporting the narrative | Inline, below heading |
| Quote | Portrait of the quoted person | Small circular image next to attribution |
| Analogy | Visual representation of the "Y" in "X is like Y" | Inline or side-by-side |

Slide types that should generally NOT have images:
- **Thesis/Statement** — typography is the visual
- **Single Stat** — the number is the visual
- **Section Divider** — style preset handles the visual
- **Transition** — breathing room, keep clean

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
