---
name: ai-ppt-html
description: 零依赖纯 HTML 演示文稿生成子技能。生成单个自包含 HTML 文件，自带导航/动画，无需任何外部框架。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT HTML — Zero-Dependency HTML Presentation Generator

This sub-skill generates self-contained HTML presentations with no external framework dependencies (only Google Fonts). It is invoked by the main `ai-ppt` skill when the user selects pure HTML as their output framework.

## Prerequisites from Orchestrator

Before this sub-skill is called, the orchestrator has completed:
- **Content analysis** (Phase 1) — slide sequence with types and messages
- **Style selection** (via ai-ppt-style) — a chosen style preset with CSS variables, fonts, animation mood
- **Slide plan** — confirmed by the user

## Generation Rules

1. **Single HTML file** with all CSS and JS inline
2. **Follow the template** in `references/html-template.md` exactly
3. **Include viewport-base CSS** from `references/viewport-base.css` (mandatory)
4. **Apply style preset** via CSS custom properties
5. **Apply animation patterns** from the style selection (animation mood determined by ai-ppt-style)
6. **Include navigation**: keyboard (arrows, space, page up/down), touch/swipe, mouse wheel, progress bar, nav dots
7. **Inline editing** (optional): Include if user opted in; uses JS hover system with 400ms grace period
8. **Use `.reveal` class** on elements for entrance animations triggered by Intersection Observer
9. **Stagger child animations** via `transition-delay` on nth-child
10. **Respect `prefers-reduced-motion`** media query

## Slide Structure

Each slide is a `<section class="slide">` with a `.slide-content` container:

```html
<section class="slide" id="slide-1">
  <div class="slide-content">
    <!-- Content here -->
  </div>
</section>
```

## Image Placement (Pure HTML)

When images were kept in the Phase 1 image audit:

- **Background image**: `style="background-image: url('path'); background-size: cover; background-position: center;"` on the `<section>` element, with a semi-transparent overlay `<div>` for text readability
- **Side-by-side with text**: Use `.two-cols` layout with `<img>` in one column
- **Inline image**: `<img src="path" style="max-height: 55vh; width: auto; object-fit: contain; border-radius: 8px;" />`
- **Image storage**: Place images alongside the HTML file; use relative paths; do NOT base64-encode large images inline

## Content Density Hard Limits

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

## Content Type → HTML Pattern Mapping

| Content Type | HTML Pattern | Notes |
|-------------|-------------|-------|
| `thesis` | `.slide-content` centered `<h2>` | Bold statement |
| `data_point` | `.big-number` + `<p>` | Giant number, make it dramatic |
| `comparison` | `.two-cols` | Side-by-side divs |
| `quote` | `<blockquote>` + `.attribution` | Styled quote block |
| `narrative` | `.reveal` staggered | Progressive entrance |
| `concept` | Centered `<h2>` | Named framework |
| `analogy` | `.two-cols` | X vs Y |
| `list` | `<ul>` with `.reveal` | Progressive bullets |
| `transition` | Gradient bg + centered | Section divider |
| `evidence` | `.reveal` staggered | Build the proof |

## Navigation System (Built-in)

The generated HTML includes:
- **Keyboard**: → / Space / Page Down (next); ← / Page Up (prev); E (edit mode)
- **Touch**: Swipe up/down or left/right
- **Mouse**: Scroll wheel
- **UI**: Progress bar (top), nav dots (right side with click navigation)

## Chinese Typography (when source is Chinese)

- Pair preset fonts with Chinese fallbacks
- Never use italic for Chinese text
- Maximum 15 Chinese characters per heading
- Maximum 25 Chinese characters per body line
- Line height: 1.8–2.0
- See main skill's `references/chinese-typography.md` for full rules

## Run Instructions

```
直接在浏览器中打开 index.html 即可。无需任何安装。
支持键盘方向键、触控滑动、鼠标滚轮导航。
```

## Reference Files

| File | Purpose |
|------|---------|
| `references/html-template.md` | Complete HTML template with slide type patterns, navigation JS, animation CSS |
| `references/viewport-base.css` | Responsive viewport CSS with clamp() scaling — MUST be included |

**Load both reference files before generating** — they contain the exact HTML structure, CSS, and JS patterns to follow.
