---
name: ai-ppt-revealjs
description: Reveal.js 演示文稿生成子技能。生成基于 CDN 的单文件 HTML 演示文稿，适合大多数通用演讲场景。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT Reveal.js — Reveal.js Presentation Generator

This sub-skill generates Reveal.js 5.x presentations as a single HTML file with CDN dependencies. It is invoked by the main `ai-ppt` skill when the user selects Reveal.js as their output framework.

## Prerequisites from Orchestrator

Before this sub-skill is called, the orchestrator has completed:
- **Content analysis** (Phase 1) — slide sequence with types and messages
- **Style selection** (via ai-ppt-style) — a chosen style preset with CSS variables, fonts, animation mood
- **Slide plan** — confirmed by the user

## Generation Rules

1. **Single self-contained HTML file** with CDN links for Reveal.js 5.x
2. **Use semantic slide structure**: `.reveal > .slides > section`
3. **Vertical slides** for sub-topics within a section (nested `<section>`)
4. **Fragment classes** for progressive reveal: `fade-up` for lists, `highlight-red` for comparisons, `grow` for stats
5. **Auto-animate** for smooth transitions: `data-auto-animate` with `data-id` matching
6. **Backgrounds**: gradient for section dividers, solid color for emphasis slides
7. **Speaker notes**: `<aside class="notes">` inside each section
8. **Chinese fonts**: Load via Google Fonts `@import` in `<style>`
9. **Apply style preset** via CSS custom properties in `:root`

## Style Preset Integration

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

## Signature CSS Integration (Custom Styles)

When the style preset includes Signature CSS blocks (from custom style extraction), inject them into the `<style>` tag after the `:root` variables. These blocks carry the design's visual personality that cannot be expressed through variables alone.

**Selector mapping** — preview selectors must be adapted to Reveal.js structure:

| Preview Selector | Reveal.js Equivalent |
|-----------------|---------------------|
| `.slide` background | `section` background or `data-background-*` attribute |
| `.glass-card`, `.neon-card` | Custom `<div>` classes inside `<section>` |
| `h1`, `h2` text effects | `.reveal .slides h1`, `.reveal .slides h2` |
| `.bg-grid::before` | `section::before` with `position: absolute; inset: 0; pointer-events: none; z-index: 0` |
| `@keyframes` | Global `<style>` block (no scoping needed) |

**Scoping rule**: Prefix signature CSS selectors with `.reveal .slides` for specificity over the base Reveal.js theme. Example: `.reveal .slides .glass-card { ... }`.

**Decorative elements**: Use HTML `<div>` children with `position: absolute` inside `<section>` rather than pseudo-elements, since slides may need multiple decorative layers and pseudo-elements are limited to `::before`/`::after`.

**Background patterns**: Apply via inline `style` on `<section>` or via `section::before` pseudo-element with `position: absolute; inset: 0; pointer-events: none` to avoid interfering with content interaction.

**Content z-index**: Ensure slide content sits above decorative layers — wrap content in a `<div style="position: relative; z-index: 1">`.

## Image Placement (Reveal.js)

When images were kept in the Phase 1 image audit:

- **Background image**: `<section data-background-image="path" data-background-size="cover" data-background-opacity="0.3">`
- **Side-by-side with text**: Use `.r-hstack` or CSS grid with `<img>` + text column
- **Inline image**: `<img src="path" style="max-height: 60vh; object-fit: contain;" />`
- **Image storage**: Place images alongside the HTML file or use relative paths

## Content Type → Reveal.js Pattern Mapping

| Content Type | Reveal.js Pattern | Notes |
|-------------|-------------------|-------|
| `thesis` | Centered `<h2>` with `data-auto-animate` | Bold statement |
| `data_point` | Giant `<span>` + context `<p>` | Make the number dramatic |
| `comparison` | `<table>` or side-by-side `<div>` | Use `.r-hstack` |
| `quote` | `<blockquote>` with attribution | Built-in style |
| `narrative` | Fragments with `fade-up` | Progressive reveal |
| `concept` | Centered heading + auto-animate | Named framework |
| `analogy` | Split layout with `.r-hstack` | X vs Y |
| `list` | `<ul>` with fragment classes | Progressive bullets |
| `transition` | Background gradient + centered text | Section divider |
| `evidence` | Fragments revealing proof points | Build the case |

## Transition Strategy

- Normal slide progression: `slide`
- Section changes: `fade`
- Key moments / big reveals: `zoom`
- Data/stat slides: `none` (instant, let the number speak)

## Chinese Typography (when source is Chinese)

- Pair preset fonts with Chinese fallbacks in `--r-main-font` and `--r-heading-font`
- For serif presets: `"Cormorant", "Noto Serif SC", serif`
- Never use italic for Chinese text — use bold or color
- Maximum 15 Chinese characters per heading
- Maximum 25 Chinese characters per body line
- Line height: 1.8–2.0
- See main skill's `references/chinese-typography.md` for full rules

## Run Instructions

```
直接在浏览器中打开 index.html 即可。
或用 live server: npx serve .
快捷键: S(演讲者视图), F(全屏), ESC(总览), ?(帮助)
```

## Reference Files

| File | Purpose |
|------|---------|
| `references/revealjs-syntax.md` | Complete Reveal.js 5.x syntax reference (sections, fragments, auto-animate, plugins, themes) |

**Load `references/revealjs-syntax.md` before generating index.html** — it contains exact syntax patterns for all Reveal.js features.
