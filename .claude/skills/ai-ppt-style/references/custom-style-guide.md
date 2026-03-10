# Custom Style Guide — Extracting Visual DNA from Reference Materials

When the user provides a design reference (screenshot, HTML/CSS export, or text description), follow this guide to extract a complete style preset compatible with `style-presets.md` format.

## Supported Input Types

| Input | How to Process |
|-------|---------------|
| **Screenshot / Image file** | Use Read tool to view the image (multimodal). Analyze visual elements directly. |
| **HTML/CSS code** | Parse CSS variables, font-family declarations, color values, layout properties. |
| **Text description** | Match to closest existing preset, then customize based on described preferences. |
| **URL** | Fetch and extract HTML/CSS, or take a screenshot if CSS extraction fails. |

---

## Extraction Process

### Step 1: Color Palette Extraction

**From screenshots:**
1. Identify the **background color** (dominant area) → `--bg-primary`
2. Identify **secondary background** (cards, panels) → `--bg-secondary`
3. Identify **primary text color** → `--text-primary`
4. Identify **secondary/muted text** → `--text-secondary`
5. Identify **accent color** (buttons, highlights, links) → `--accent`
6. Identify any **gradient** patterns → `--bg-gradient`
7. Note color temperature: warm (amber/orange/red) vs cool (blue/cyan/green)

**From HTML/CSS:**
- Priority: CSS custom properties (`--var`) > class-level styles > inline styles
- Extract: `background`, `color`, `border-color`, `box-shadow` color values
- Convert named colors to hex values

**Output format:**
```css
:root {
  --bg-primary: #0a1628;
  --bg-secondary: #1a2a4a;
  --text-primary: #ffffff;
  --text-secondary: #8899aa;
  --accent: #00d4ff;
  --accent-glow: rgba(0, 212, 255, 0.3);
}
```

### Step 2: Typography Extraction

**From screenshots:**
1. Identify heading style: serif / sans-serif / monospace / display
2. Estimate weight: thin (100-300) / regular (400) / bold (700) / black (900)
3. Note size hierarchy: how much bigger are headings vs body text
4. Check letter-spacing: tight / normal / wide / very wide

**From HTML/CSS:**
- Extract `font-family`, `font-weight`, `font-size`, `letter-spacing`, `line-height`
- Note any `@import` or `@font-face` declarations

**Google Fonts Matching Table:**

| Detected Style | Google Fonts Match |
|---------------|-------------------|
| Geometric sans | Space Grotesk, Inter, Plus Jakarta Sans |
| Humanist sans | Nunito, DM Sans, Work Sans |
| Neo-grotesque | Manrope, Outfit, Archivo |
| Modern serif | Fraunces, Cormorant, Bodoni Moda |
| Classical serif | Cormorant Garamond, Source Serif 4, Playfair Display |
| Monospace | JetBrains Mono, Space Mono, Fira Code |
| Display/decorative | Archivo Black, Syne, Clash Display (→ fallback to Space Grotesk) |
| Brutalist | Archivo Black, Anton, Bebas Neue |
| Editorial | Fraunces + Work Sans, Playfair Display + Source Sans 3 |

**Output format:**
```
Fonts: [Display Font] ([weight]) for headings; [Body Font] ([weight]) for body
```
```html
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Inter:wght@400;500&display=swap" rel="stylesheet">
```

### Step 3: Layout Pattern Identification

Identify the dominant layout signature:

| Pattern | Description | Characteristics |
|---------|-------------|-----------------|
| **Centered** | Content centered vertically and horizontally | Large whitespace, single focal point |
| **Split-panel** | Two vertical halves with different colors/content | Left: visual/color, Right: text (or vice versa) |
| **Card-based** | Content in rounded-corner containers | Shadow/border, padding, background contrast |
| **Grid** | Multiple equal cells | 2x2, 3x2, or masonry layout |
| **Editorial** | Magazine-style with varied element sizes | Pull quotes, drop caps, asymmetric margins |
| **Full-bleed** | Content extends to edges | Minimal padding, immersive feel |
| **Brutalist** | Intentionally raw, blocky | Thick borders, high contrast, monospace type |
| **Terminal** | Code/CLI aesthetic | Dark background, monospace, green/cyan text |

**Output format:**
```
Layout Signature: [Pattern name] — [2-sentence description of the visual approach]
```

### Step 4: Animation & Motion Mood

Map the overall design feeling to one of the 6 animation moods:

| Design Feeling | → Animation Mood | Duration | Easing |
|---------------|-----------------|----------|--------|
| Clean, professional, corporate | Professional | 0.2–0.3s | ease |
| Dark, cinematic, luxury | Dramatic | 1–1.5s | ease-out |
| Tech, futuristic, glowing | Techy | 0.4–0.8s | cubic-bezier |
| Fun, colorful, rounded shapes | Playful | 0.5–0.8s | spring |
| Calm, soft colors, generous spacing | Calm | 0.8–1.2s | ease-in-out |
| Magazine, editorial, type-focused | Editorial | 0.6–1s | ease-out-expo |

### Step 5: Spacing & Density

Note the whitespace style:

| Density | Characteristics | CSS Approach |
|---------|----------------|-------------|
| **Airy** | Lots of breathing room, few elements per screen | Large padding (clamp(2rem, 5vw, 5rem)), generous gaps |
| **Balanced** | Moderate whitespace, clear visual hierarchy | Standard padding (clamp(1rem, 3vw, 3rem)) |
| **Dense** | Information-rich, compact | Tight padding (clamp(0.5rem, 2vw, 2rem)), smaller gaps |

---

## Output: Complete Custom Preset

Assemble all extracted elements into a complete preset:

```markdown
### Custom: [Name based on aesthetic]

**Mood**: [2-3 adjective description]
**Best for**: [Content types this style suits]
**Extracted from**: [Source description]

\`\`\`css
:root {
  --bg-primary: [value];
  --bg-secondary: [value];
  --text-primary: [value];
  --text-secondary: [value];
  --accent: [value];
  --accent-glow: [rgba version of accent];
}
\`\`\`

**Fonts**: [Display] ([weight]) for headings; [Body] ([weight]) for body
\`\`\`html
<link href="[Google Fonts URL]" rel="stylesheet">
\`\`\`

**Layout Signature**: [Pattern description]

**Animation Mood**: [Mood name] — [Duration range], [easing]
```

---

## Step 6: Generate Style Preview HTML

**After extracting the custom preset, ALWAYS generate a live preview HTML file** so the user can see the style applied to sample slides before committing to it.

Write the preview to: `[project-dir]/style-preview.html`

The preview is a **self-contained single HTML file** with 5 representative sample slides that showcase the extracted style's colors, typography, layout, and animations. It uses the same base structure as `html-template.md` but with minimal, style-focused content.

### Preview Template

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>风格预览 — [Custom Preset Name]</title>

  <!-- Fonts: Use the extracted Google Fonts link -->
  <link href="[EXTRACTED_GOOGLE_FONTS_URL]" rel="stylesheet">

  <style>
    /* ===== Extracted CSS Variables ===== */
    :root {
      --bg-primary: [extracted];
      --bg-secondary: [extracted or slightly lighter/darker variant];
      --text-primary: [extracted];
      --text-secondary: [extracted];
      --accent: [extracted];
      --accent-glow: [extracted rgba];

      --font-display: "[Display Font]", "Noto Sans SC", sans-serif;
      --font-body: "[Body Font]", "Noto Sans SC", sans-serif;

      --title-size: clamp(1.5rem, 5vw, 4rem);
      --h2-size: clamp(1.25rem, 3.5vw, 2.5rem);
      --h3-size: clamp(1rem, 2.5vw, 1.75rem);
      --body-size: clamp(0.75rem, 1.5vw, 1.125rem);
      --small-size: clamp(0.65rem, 1vw, 0.875rem);

      --slide-padding: clamp(1rem, 4vw, 4rem);
      --content-gap: clamp(0.5rem, 2vw, 2rem);

      --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
      --duration-normal: [extracted animation duration, e.g. 0.6s];
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }
    html, body { height: 100%; overflow-x: hidden; }
    html { scroll-snap-type: y mandatory; scroll-behavior: smooth; }

    body {
      background-color: var(--bg-primary);
      color: var(--text-primary);
      font-family: var(--font-body);
      font-size: var(--body-size);
      line-height: 1.8;
      letter-spacing: 0.05em;
    }

    .slide {
      width: 100vw; height: 100vh; height: 100dvh;
      overflow: hidden; scroll-snap-align: start;
      display: flex; flex-direction: column; position: relative;
    }

    .slide-content {
      flex: 1; display: flex; flex-direction: column;
      justify-content: center; align-items: center; text-align: center;
      max-height: 100%; overflow: hidden; padding: var(--slide-padding);
    }
    .slide-content.left-align { align-items: flex-start; text-align: left; }

    h1 { font-family: var(--font-display); font-size: var(--title-size); font-weight: 700; letter-spacing: 0.08em; line-height: 1.3; }
    h2 { font-family: var(--font-display); font-size: var(--h2-size); font-weight: 700; letter-spacing: 0.08em; margin-bottom: var(--content-gap); }
    h3 { font-family: var(--font-display); font-size: var(--h3-size); font-weight: 700; }
    p  { font-size: var(--body-size); color: var(--text-secondary); line-height: 1.8; max-width: 800px; }
    em { font-style: normal; font-weight: 700; color: var(--accent); }

    .reveal {
      opacity: 0; transform: translateY(30px);
      transition: opacity var(--duration-normal) var(--ease-out-expo),
                  transform var(--duration-normal) var(--ease-out-expo);
    }
    .slide.visible .reveal { opacity: 1; transform: translateY(0); }
    .reveal:nth-child(1) { transition-delay: 0.1s; }
    .reveal:nth-child(2) { transition-delay: 0.2s; }
    .reveal:nth-child(3) { transition-delay: 0.3s; }
    .reveal:nth-child(4) { transition-delay: 0.4s; }

    .big-number {
      font-family: var(--font-display); font-size: clamp(4rem, 15vw, 10rem);
      font-weight: 900; color: var(--accent); line-height: 1;
    }

    blockquote {
      font-size: var(--h3-size); font-style: normal;
      border-left: 4px solid var(--accent);
      padding: 0.5em 1em; text-align: left; max-width: 800px; line-height: 1.8;
    }
    .attribution { text-align: right; opacity: 0.6; font-size: var(--small-size); margin-top: 1em; }

    .two-cols { display: flex; gap: clamp(1rem, 3vw, 3rem); width: 100%; max-width: 1000px; }
    .two-cols > div { flex: 1; text-align: left; }

    ul { text-align: left; max-width: 800px; list-style: none; }
    ul li { padding: 0.3em 0; }
    ul li::before { content: "—"; color: var(--accent); margin-right: 0.5em; }

    /* ===== Layout Signature Decorations ===== */
    /* [Insert extracted layout-specific CSS here: card styles, split panels, decorative shapes, etc.] */

    /* ===== Animation Mood Overrides ===== */
    /* [Insert mood-specific animation CSS from animation-patterns.md] */

    .progress-bar {
      position: fixed; top: 0; left: 0; height: 4px;
      background: linear-gradient(90deg, var(--accent), var(--accent-glow));
      width: 0%; z-index: 1000; transition: width 0.3s ease;
    }

    /* Preview badge */
    .preview-badge {
      position: fixed; bottom: 1.5rem; left: 50%; transform: translateX(-50%);
      background: var(--accent); color: var(--bg-primary);
      padding: 0.4rem 1.2rem; border-radius: 2rem;
      font-size: var(--small-size); font-weight: 600; z-index: 1000;
      opacity: 0.85; pointer-events: none;
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.2s !important; }
      html { scroll-behavior: auto; }
    }
  </style>
</head>
<body>

  <div class="progress-bar"></div>
  <div class="preview-badge">风格预览 · Style Preview</div>

  <!-- Slide 1: Cover — showcases title font, bg color, accent -->
  <section class="slide" style="background: linear-gradient(135deg, var(--bg-primary), var(--bg-secondary));">
    <div class="slide-content">
      <h1 class="reveal">演示文稿标题</h1>
      <p class="reveal">Presentation Subtitle — 风格预览</p>
    </div>
  </section>

  <!-- Slide 2: Stat — showcases accent color, display font at large scale -->
  <section class="slide">
    <div class="slide-content">
      <div class="big-number reveal">42%</div>
      <p class="reveal" style="font-size: var(--h3-size);">关键数据展示效果</p>
      <p class="reveal" style="font-size: var(--small-size); opacity: 0.5;">Data Visualization Sample</p>
    </div>
  </section>

  <!-- Slide 3: Content — showcases body font, text hierarchy, list styling -->
  <section class="slide">
    <div class="slide-content left-align">
      <h2 class="reveal">内容排版预览</h2>
      <ul>
        <li class="reveal">第一个要点 — 展示<em>强调色</em>效果</li>
        <li class="reveal">第二个要点 — 展示正文字体和行间距</li>
        <li class="reveal">第三个要点 — 展示列表项的前缀装饰</li>
      </ul>
    </div>
  </section>

  <!-- Slide 4: Quote — showcases blockquote styling -->
  <section class="slide">
    <div class="slide-content">
      <blockquote class="reveal">"好的设计是尽可能少的设计。"</blockquote>
      <p class="attribution reveal">— Dieter Rams</p>
    </div>
  </section>

  <!-- Slide 5: Two-Column — showcases layout capabilities -->
  <section class="slide">
    <div class="slide-content">
      <h2 class="reveal">对比布局预览</h2>
      <div class="two-cols reveal">
        <div>
          <h3 style="color: var(--accent);">方案 A</h3>
          <ul>
            <li>优势一</li>
            <li>优势二</li>
          </ul>
        </div>
        <div>
          <h3 style="color: var(--text-secondary);">方案 B</h3>
          <ul>
            <li>优势一</li>
            <li>优势二</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <script>
    // Minimal navigation for preview
    const slides = document.querySelectorAll('.slide');
    let current = 0;
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          current = Array.from(slides).indexOf(entry.target);
          document.querySelector('.progress-bar').style.width =
            ((current + 1) / slides.length * 100) + '%';
        }
      });
    }, { threshold: 0.5 });
    slides.forEach(s => observer.observe(s));

    let lastScroll = 0;
    const go = (dir) => {
      current = Math.max(0, Math.min(slides.length - 1, current + dir));
      slides[current].scrollIntoView({ behavior: 'smooth' });
    };
    document.addEventListener('keydown', (e) => {
      if (['ArrowRight', ' ', 'PageDown'].includes(e.key)) { e.preventDefault(); go(1); }
      if (['ArrowLeft', 'PageUp'].includes(e.key)) { e.preventDefault(); go(-1); }
    });
    document.addEventListener('wheel', (e) => {
      e.preventDefault();
      const now = Date.now();
      if (now - lastScroll < 800) return;
      lastScroll = now;
      go(e.deltaY > 0 ? 1 : -1);
    }, { passive: false });
  </script>
</body>
</html>
```

### What to Customize in the Preview

When generating the preview, replace the template placeholders with the extracted values:

| Placeholder | Source |
|------------|--------|
| `[Custom Preset Name]` | The name from Step 5 output |
| `[EXTRACTED_GOOGLE_FONTS_URL]` | The Google Fonts link from Step 2 |
| All `[extracted]` CSS variable values | The color palette from Step 1 |
| `[Display Font]` / `[Body Font]` | Typography from Step 2 |
| Layout signature CSS comment block | Layout-specific decorations from Step 3 |
| Animation mood CSS comment block | Mood-specific overrides from Step 4 |
| `[extracted animation duration]` | Duration from Step 4 |

### Layout Signature Application

Apply the extracted layout signature to the preview slides. Examples:

- **Card-based**: Wrap `.slide-content` children in a card div with `background: var(--bg-secondary); border-radius: 12px; padding: 2rem; box-shadow: ...`
- **Split-panel**: Use CSS grid on `.slide` with two color zones
- **Full-bleed**: Reduce padding, add gradient backgrounds that extend to edges
- **Terminal**: Add scanline overlay, monospace body font, cursor blink animation
- **Editorial**: Add pull-quote styling, drop caps on first paragraph

### Animation Mood Application

Apply the extracted animation mood to override the default reveal animations:

- **Dramatic**: Increase `--duration-normal` to `1.2s`, add `scale(0.95)` to initial state
- **Techy**: Add glow `text-shadow` to headings, grid background pattern
- **Playful**: Use `var(--ease-out-back)` easing, add `scale(0.8)` bounce
- **Professional**: Reduce `--duration-normal` to `0.25s`, minimal transform
- **Calm**: Set `--duration-normal` to `1s`, ease-in-out timing
- **Editorial**: Add staggered delays with shorter intervals

---

## User Confirmation (with Preview)

After generating the preview HTML, **do NOT ask the user to manually open it**. Instead, proceed to **SKILL.md Step 1c** which will:

1. **Auto-open** `style-preview.html` in the browser via `xdg-open` / `open`
2. **Also auto-open** the 3 mood-matching preset previews from `references/previews/` for side-by-side comparison
3. Present the extracted style summary alongside the preset alternatives
4. Ask the user to choose between their custom style and the preset options

If the user requests adjustments to the custom style:
1. Update the extracted preset values
2. **Regenerate the preview HTML** with the updated values
3. **Re-open** in browser and ask again

---

## Variant.com Integration Tips

When the user references [variant.com](https://variant.com/) or its community gallery:

1. **Ask for a screenshot** — variant.com blocks programmatic access, so screenshots are the best way to capture designs
2. **Ask user to export HTML** — Variant supports HTML export; the exported code contains extractable CSS
3. **Style Dropper context** — If the user describes a Variant design verbally (e.g., "brutalist dark theme with dot-matrix grid"), use the Google Fonts matching table above to find the closest fonts and build a preset from the description

Variant.com community designs tend to feature:
- High visual density with distinctive typography
- Unconventional color palettes (neon on dark, muted earth tones, acid colors)
- Strong layout personalities (brutalist grids, terminal aesthetics, magazine layouts)
- Bold use of whitespace or deliberate lack thereof

These characteristics map well to the **Techy**, **Editorial**, and **Dramatic** animation moods.
