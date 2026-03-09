---
name: ai-ppt-style
description: 演示文稿风格发现与选择子技能。分析文章情绪，推荐视觉风格预设，支持从截图/HTML/CSS 提取自定义风格。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT Style — Style Discovery & Presentation Architecture

This sub-skill handles visual style selection and presentation architecture design. It is invoked by the main `ai-ppt` skill after content analysis (Phase 1) is complete.

## Prerequisites from Orchestrator

Before this sub-skill is called, the orchestrator has completed:
- **Phase 1: Content Analysis** — structured content inventory with tagged content units, key elements, and section breakdown
- The user has already selected a framework (SlideDev / Reveal.js / HTML)

## Phase 2: Style Discovery & Presentation Architecture

### Step 1: Visual Style Selection — MANDATORY USER INTERACTION

**CRITICAL: You MUST present style options to the user and WAIT for their response before generating any slides.** Do NOT skip this step. Do NOT auto-select a style.

**Do NOT ask abstract questions like "深色还是浅色".** Instead, analyze the article's mood and audience, then present curated style options using AskUserQuestion.

#### Determine the Article's Mood

| Article Characteristics | Mood | Recommended Presets |
|------------------------|------|-------------------|
| Business analysis, investor logic, competition | Impress (给人留下深刻印象) | Bold Signal, Electric Studio, Dark Botanical |
| Startup, creative industry, disruption | Excite (让人兴奋) | Creative Voltage, Neon Cyber, Split Pastel |
| Education, process, methodology | Calm (平静/信任) | Pastel Geometry, Swiss Modern, Paper & Ink |
| Thought leadership, cultural analysis | Inspire (激发思考) | Vintage Editorial, Notebook Tabs, Terminal Green |

#### Present 4 Options via AskUserQuestion

After determining the mood category, use AskUserQuestion to present **3 preset options + 1 custom option** from `references/style-presets.md`. Add the recommended one as the first option with "(Recommended)" suffix.

```
Question: "根据文章风格，推荐以下视觉方案，选哪个？"
Options:
  A: "[Preset 1] (Recommended)" — [1-sentence visual description]
  B: "[Preset 2]" — [1-sentence visual description]
  C: "[Preset 3]" — [1-sentence visual description]
  D: "自定义参考风格" — 提供截图、HTML/CSS 或文字描述，我来提取视觉风格
```

Example (for a business analysis article with "Impress" mood):

```
Options:
  A: "Bold Signal (Recommended)" — 深色背景 + 橙色强调卡片，Archivo Black 粗体标题，自信有力
  B: "Dark Botanical" — 纯黑底 + 暖金色渐变装饰，Cormorant 衬线标题，优雅高级
  C: "Electric Studio" — 黑白高对比 + 蓝色强调，Manrope 字体，科技专业感
  D: "自定义参考风格" — 提供截图、HTML/CSS 或文字描述，我来提取视觉风格
```

The user can also select "Other" to describe their own preferences (e.g., "A的配色 + C的字体" or a completely custom style).

**Only proceed AFTER receiving the user's style choice.** If the user says "随便" or "你来选" via "Other", then pick the first recommended option.

### Step 1b: Custom Reference Style Workflow (when user selects option D)

When the user chooses "自定义参考风格", follow the extraction process in `references/custom-style-guide.md`.

**Collect reference material** (AskUserQuestion):

```
Question: "请提供设计参考素材（选择一种方式）："
Options:
  A: "截图/图片" — 提供文件路径，我会分析配色、字体、布局风格
  B: "HTML/CSS 代码" — 粘贴代码片段，我会提取设计系统
  C: "文字描述" — 描述你想要的视觉效果，我来匹配最合适的风格
```

Then follow `references/custom-style-guide.md` to:
1. Extract color palette → CSS variables
2. Identify typography → match to Google Fonts
3. Identify layout pattern
4. Map to animation mood
5. Present extracted preset for user confirmation/adjustment

### Step 2: Determine Animation Mood

Based on the selected style (preset or custom), map to an animation mood from `references/animation-patterns.md`:

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
| Custom preset | Auto-mapped | Based on extraction analysis |

### Step 3: Design Slide Sequence

Map each section from the content inventory to a sequence of slides. For each slide, specify:

```
Slide N: [Slide Type]
  Message: [one sentence]
  Content: [what goes on this slide]
  Source section: [which part of the article]
  Animation: [from animation mood, e.g. "fade-up 0.6s staggered"]
```

Use the Content Type → Slide Type mapping from the main `ai-ppt` skill, referencing `slide-type-catalog.md` for slide type details.

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

## Output

After this sub-skill completes, the main orchestrator has:
1. A chosen style preset (with CSS variables, fonts, layout signature)
2. An animation mood with specific duration/easing settings
3. A confirmed slide sequence with types and messages
4. Everything needed to invoke the appropriate framework generator sub-skill

## Reference Files

| File | Purpose | When to Load |
|------|---------|-------------|
| `references/style-presets.md` | 12 curated visual style presets | Always — needed for presenting options |
| `references/animation-patterns.md` | 6 emotion-driven animation categories | After style selection — for mood mapping |
| `references/custom-style-guide.md` | Guide for extracting style from reference materials | Only when user selects "自定义参考风格" |
