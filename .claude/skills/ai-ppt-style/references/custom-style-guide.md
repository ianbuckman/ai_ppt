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

## User Confirmation

After generating the custom preset, present it to the user for confirmation:

```
从参考素材中提取了以下风格：

🎨 配色方案：
  背景: [--bg-primary] / 文字: [--text-primary] / 强调: [--accent]

📝 字体搭配：
  标题: [Display font] / 正文: [Body font]

📐 布局风格：[Layout pattern name] — [1句描述]

🎬 动画情绪：[Mood name] — [特点描述]

满意吗？需要调整哪里？（比如换个强调色、换套字体等）
```

Allow the user to fine-tune individual elements (e.g., "强调色换成红色", "字体用更现代的") before finalizing.

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
