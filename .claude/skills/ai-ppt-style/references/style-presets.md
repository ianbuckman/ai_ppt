# Visual Style Presets

12 curated visual styles for presentations. Each preset defines colors, fonts, layout signature, and mood. Use these instead of generic framework themes to create distinctive, non-generic presentations.

**Adapted from [frontend-slides](https://github.com/zarazhangrui/frontend-slides) with additions for SlideDev and Reveal.js framework compatibility.**

---

## How to Use

1. During Phase 2 (Style Discovery), present 3 style previews based on the article's mood
2. User selects one (or mixes elements)
3. Apply the preset's CSS variables, fonts, and layout patterns to the chosen framework

### Mood → Preset Mapping

| Mood / Audience | Recommended Presets |
|----------------|-------------------|
| 想给人留下深刻印象 (Impress) | Bold Signal, Electric Studio, Dark Botanical |
| 想让人兴奋/激动 (Excite) | Creative Voltage, Neon Cyber, Split Pastel |
| 想让人感到平静/信任 (Calm) | Pastel Geometry, Swiss Modern, Paper & Ink |
| 想激发思考/灵感 (Inspire) | Vintage Editorial, Notebook Tabs, Terminal Green |

---

## Dark Themes

### 1. Bold Signal

**Mood**: Confident, bold, modern, high-impact
**Best for**: Business pitches, product launches, investor decks

```css
:root {
  --bg-primary: #1a1a1a;
  --bg-gradient: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #1a1a1a 100%);
  --card-bg: #FF5722;
  --text-primary: #ffffff;
  --text-on-card: #1a1a1a;
  --accent: #FF5722;
}
```

**Fonts**: Archivo Black (900) for display; Space Grotesk (400/500) for body
```html
<link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Space+Grotesk:wght@400;500&display=swap" rel="stylesheet">
```

**Layout Signature**: Colored accent card on dark gradient background. Large section numbers (top-left), navigation breadcrumbs (top-right), title anchored bottom-left.

**SlideDev Adaptation**:
```yaml
---
theme: default
fonts:
  sans: "Space Grotesk"
  weights: "400,500"
---
```
```html
<style>
:root { --slidev-font-family: "Space Grotesk", sans-serif; }
.slidev-layout { background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #1a1a1a 100%); color: #fff; }
.slidev-layout h1 { font-family: "Archivo Black", sans-serif; color: #FF5722; }
</style>
```

**Reveal.js Adaptation**:
```css
:root {
  --r-background-color: #1a1a1a;
  --r-main-color: #ffffff;
  --r-heading-color: #FF5722;
  --r-main-font: "Space Grotesk", sans-serif;
  --r-heading-font: "Archivo Black", sans-serif;
}
```

---

### 2. Electric Studio

**Mood**: Bold, clean, professional, high contrast
**Best for**: Tech company presentations, product demos

```css
:root {
  --bg-dark: #0a0a0a;
  --bg-white: #ffffff;
  --accent-blue: #4361ee;
  --text-dark: #0a0a0a;
  --text-light: #ffffff;
}
```

**Fonts**: Manrope (400/500/800)
```html
<link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;800&display=swap" rel="stylesheet">
```

**Layout Signature**: Vertical split-panel — white top, blue bottom. Brand marks in corners. Accent bar on panel edge.

---

### 3. Creative Voltage

**Mood**: Bold, creative, energetic, retro-modern
**Best for**: Creative industry, startup culture, design talks

```css
:root {
  --bg-primary: #0066ff;
  --bg-dark: #1a1a2e;
  --accent-neon: #d4ff00;
  --text-light: #ffffff;
}
```

**Fonts**: Syne (700/800) for display; Space Mono (400/700) monospace for body
```html
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
```

**Layout Signature**: Split panels — electric blue left, dark right. Neon yellow badges/callouts. Halftone texture patterns.

---

### 4. Dark Botanical

**Mood**: Elegant, sophisticated, artistic, premium
**Best for**: Luxury brands, art/culture topics, premium product analysis

```css
:root {
  --bg-primary: #0f0f0f;
  --text-primary: #e8e4df;
  --text-secondary: #9a9590;
  --accent-warm: #d4a574;
  --accent-pink: #e8b4b8;
  --accent-gold: #c9b896;
}
```

**Fonts**: Cormorant (400/600) serif for display; IBM Plex Sans (300/400) for body
```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant:wght@400;600&family=IBM+Plex+Sans:wght@300;400&display=swap" rel="stylesheet">
```

**Layout Signature**: Centered content on pure dark background. Abstract soft gradient circles (CSS only, blurred overlapping shapes) in warm tones as corner decorations. Thin vertical accent lines.

---

## Light Themes

### 5. Notebook Tabs

**Mood**: Editorial, organized, elegant, tactile
**Best for**: Educational content, structured analysis, research presentations

```css
:root {
  --bg-outer: #2d2d2d;
  --bg-page: #f8f6f1;
  --text-primary: #1a1a1a;
  --tab-1: #98d4bb; /* Mint */
  --tab-2: #c7b8ea; /* Lavender */
  --tab-3: #f4b8c5; /* Pink */
  --tab-4: #a8d8ea; /* Sky */
  --tab-5: #ffe6a7; /* Cream */
}
```

**Fonts**: Bodoni Moda (400/700) for display; DM Sans (400/500) for body
```html
<link href="https://fonts.googleapis.com/css2?family=Bodoni+Moda:wght@400;700&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
```

**Layout Signature**: Cream paper card on dark outer background with subtle shadow. Colorful section tabs (vertical text) on right edge. Binder hole decorations optional.

---

### 6. Pastel Geometry

**Mood**: Friendly, organized, modern, approachable
**Best for**: Team updates, internal presentations, onboarding

```css
:root {
  --bg-primary: #c8d9e6;
  --card-bg: #faf9f7;
  --pill-pink: #f0b4d4;
  --pill-mint: #a8d4c4;
  --pill-sage: #5a7c6a;
  --pill-lavender: #9b8dc4;
  --pill-violet: #7c6aad;
}
```

**Fonts**: Plus Jakarta Sans (400/500/700/800)
```html
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;700;800&display=swap" rel="stylesheet">
```

**Layout Signature**: White rounded card on pastel background. Vertical colored pills on right edge (varying heights: short → medium → tall).

---

### 7. Split Pastel

**Mood**: Playful, modern, friendly, creative
**Best for**: Marketing, social media content, casual talks

```css
:root {
  --bg-peach: #f5e6dc;
  --bg-lavender: #e4dff0;
  --text-dark: #1a1a1a;
  --badge-mint: #c8f0d8;
  --badge-yellow: #f0f0c8;
  --badge-pink: #f0d4e0;
}
```

**Fonts**: Outfit (400/500/700/800)
```html
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;700;800&display=swap" rel="stylesheet">
```

**Layout Signature**: Two-color vertical split — peach left, lavender right. Playful badge pills with icons. Grid pattern overlay on right panel.

---

### 8. Vintage Editorial

**Mood**: Witty, confident, editorial, personality-driven
**Best for**: Opinion pieces, thought leadership, personal brand

```css
:root {
  --bg-cream: #f5f3ee;
  --text-primary: #1a1a1a;
  --text-secondary: #555;
  --accent-warm: #e8d4c0;
}
```

**Fonts**: Fraunces (700/900) distinctive serif for display; Work Sans (400/500) for body
```html
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@700;900&family=Work+Sans:wght@400;500&display=swap" rel="stylesheet">
```

**Layout Signature**: Centered content on cream. Abstract geometric CSS shapes as accents (circle outline, line, dot). Bold bordered CTA boxes. Conversational copy style.

---

## Specialty Themes

### 9. Neon Cyber

**Mood**: Futuristic, techy, confident
**Best for**: AI/ML topics, tech conferences, developer talks

```css
:root {
  --bg-primary: #0a0f1c;
  --accent-cyan: #00ffcc;
  --accent-magenta: #ff00aa;
  --text-primary: #ffffff;
}
```

**Fonts**: Clash Display + Satoshi (from Fontshare) or Space Grotesk + JetBrains Mono (Google Fonts fallback)

**Layout Signature**: Particle backgrounds, neon glow effects (box-shadow), grid patterns.

---

### 10. Terminal Green

**Mood**: Developer-focused, hacker aesthetic
**Best for**: Developer tools, open source, security topics

```css
:root {
  --bg-primary: #0d1117;
  --accent-green: #39d353;
  --text-primary: #c9d1d9;
  --text-secondary: #8b949e;
}
```

**Fonts**: JetBrains Mono (monospace)
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
```

**Layout Signature**: Scan lines, blinking cursor animation, code syntax styling, terminal-window borders.

---

### 11. Swiss Modern

**Mood**: Clean, precise, Bauhaus-inspired
**Best for**: Design systems, architecture, data visualization

```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #000000;
  --accent-red: #ff3300;
}
```

**Fonts**: Archivo (800) for display; Nunito (400) for body
```html
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@800&family=Nunito:wght@400&display=swap" rel="stylesheet">
```

**Layout Signature**: Visible grid lines, asymmetric layouts, geometric shapes, heavy use of whitespace.

---

### 12. Paper & Ink

**Mood**: Editorial, literary, thoughtful
**Best for**: Book reviews, cultural analysis, long-form content

```css
:root {
  --bg-primary: #faf9f7;
  --text-primary: #1a1a1a;
  --accent-crimson: #c41e3a;
}
```

**Fonts**: Cormorant Garamond + Source Serif 4
```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600&family=Source+Serif+4:wght@400;600&display=swap" rel="stylesheet">
```

**Layout Signature**: Drop caps, pull quotes with accent borders, elegant horizontal rules, generous margins.

---

## CSS Gotcha

When negating CSS functions in calc(), always use multiplication:
```css
/* WRONG — will break in some browsers */
margin-top: calc(-clamp(1rem, 2vw, 2rem));

/* CORRECT */
margin-top: calc(-1 * clamp(1rem, 2vw, 2rem));
```

---

## Chinese Font Pairing

When source content is Chinese, pair the preset's display font with a Chinese font stack:

```css
/* Display/heading: Keep preset display font + add Chinese fallback */
font-family: "Archivo Black", "Noto Sans SC", sans-serif;

/* Body: Keep preset body font + add Chinese fallback */
font-family: "Space Grotesk", "Noto Sans SC", sans-serif;
```

For presets with serif display fonts (Dark Botanical, Notebook Tabs, Paper & Ink), pair with:
```css
font-family: "Cormorant", "Noto Serif SC", serif;
```
