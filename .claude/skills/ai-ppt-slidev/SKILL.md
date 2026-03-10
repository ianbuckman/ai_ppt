---
name: ai-ppt-slidev
description: SlideDev 演示文稿生成子技能。生成 Markdown 驱动的 SlideDev 演示文稿，适合包含代码/图表的技术演讲。由 ai-ppt 主技能调用，不独立触发。
---

# AI PPT SlideDev — SlideDev Presentation Generator

This sub-skill generates SlideDev presentations. It is invoked by the main `ai-ppt` skill when the user selects SlideDev as their output framework.

## Prerequisites from Orchestrator

Before this sub-skill is called, the orchestrator has completed:
- **Content analysis** (Phase 1) — slide sequence with types and messages
- **Style selection** (via ai-ppt-style) — a chosen style preset with CSS variables, fonts, animation mood
- **Slide plan** — confirmed by the user

## File Structure to Generate

```
project-name/
├── package.json
├── slides.md          # Main presentation
└── components/        # Optional custom components
    └── Counter.vue    # Only if needed
```

## package.json

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

## slides.md Generation Rules

1. **Global headmatter** at top of file — set theme, title, transition, fonts
2. **Slide separators**: `---` between slides, with optional frontmatter per slide
3. **Use built-in layouts**: cover, section, center, statement, fact, quote, two-cols, two-cols-header, image-right, full
4. **Progressive reveal**: Use `<v-clicks>` for lists, `v-click` for individual elements
5. **Transitions**: `slide-left` for normal flow, `fade` for section changes
6. **Speaker notes**: `<!-- notes -->` at bottom of each slide
7. **Chinese fonts**: Configure via headmatter `fonts` field
8. **Apply style preset colors** via scoped `<style>` blocks on individual slides

## Theme + Style Preset Integration

The SlideDev theme provides base layout. The style preset (from ai-ppt-style) provides custom colors and typography:

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

## Image Placement (SlideDev)

When images were kept in the Phase 1 image audit:

- **Background image**: Use per-slide frontmatter `background: "/path/to/image.jpg"` with `data-background-opacity` for dimming
- **Side-by-side with text**: Use `layout: image-right` with `image: "/path/to/image.jpg"`
- **Inline image**: Use standard Markdown `![alt](path)` with sizing classes (e.g., `<img src="path" class="w-80 mx-auto" />`)
- **Image storage**: Place images in a `public/` folder so SlideDev can serve them

## Content Type → SlideDev Layout Mapping

| Content Type | SlideDev Layout | Notes |
|-------------|----------------|-------|
| `thesis` | `center` or `statement` | Bold, centered text |
| `data_point` | `fact` | Giant number display |
| `comparison` | `two-cols` | Side-by-side |
| `quote` | `quote` | Built-in blockquote style |
| `narrative` | `default` with `v-clicks` | Progressive reveal |
| `concept` | `center` | Centered heading |
| `analogy` | `two-cols` | X vs Y side-by-side |
| `list` | `default` with `<v-clicks>` | Progressive bullets |
| `transition` | `section` | Section divider |
| `evidence` | `default` or `two-cols` | Facts revealed progressively |

## Chinese Typography (when source is Chinese)

- Pair preset fonts with Chinese fallbacks: `"Archivo Black", "Noto Sans SC", sans-serif`
- For serif presets: `"Cormorant", "Noto Serif SC", serif`
- Never use italic for Chinese text — use bold or color instead
- Maximum 15 Chinese characters per heading
- Maximum 25 Chinese characters per body line
- Line height: 1.8–2.0
- See main skill's `references/chinese-typography.md` for full rules

## Run Instructions

```bash
cd project-name
npm install
npm run dev
# Opens at http://localhost:3030
```

## Reference Files

| File | Purpose |
|------|---------|
| `references/slidev-syntax.md` | Complete SlideDev syntax reference (layouts, directives, code blocks, Mermaid diagrams) |

**Load `references/slidev-syntax.md` before generating slides.md** — it contains exact syntax patterns for all SlideDev features.
