# Chinese (CJK) Typography Rules for Presentations

## Font Stacks

### Sans-serif (primary — use for most slides)

```css
font-family: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", "Hiragino Sans GB", sans-serif;
```

### Serif (for formal/literary contexts)

```css
font-family: "Noto Serif SC", "Songti SC", "SimSun", serif;
```

### Monospace (for code)

```css
font-family: "Fira Code", "Source Code Pro", "Menlo", monospace;
```

## Web Font Loading

### Google Fonts (recommended)

```html
<!-- In <head> or via @import -->
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;700;900&display=swap');
</style>
```

### SlideDev (automatic)

```yaml
# In global headmatter — SlideDev loads from Google Fonts automatically
fonts:
  sans: "Noto Sans SC"
  serif: "Noto Serif SC"
  mono: "Fira Code"
  weights: "400,700,900"
  provider: google
```

### Reveal.js (manual)

```css
:root {
  --r-main-font: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
  --r-heading-font: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
}
```

## Spacing & Line Height

| Property | Value | Reason |
|----------|-------|--------|
| `line-height` | 1.8–2.0 | Chinese characters are denser; needs more breathing room |
| `letter-spacing` | 0.05em | Slightly loose tracking improves readability |
| Heading `letter-spacing` | 0.08em | More spacing for large headings |
| `max-width` for body text | ~25 characters | Prevent overly long lines |

```css
.reveal {
  line-height: 1.8;
  letter-spacing: 0.05em;
}
.reveal h1, .reveal h2, .reveal h3 {
  letter-spacing: 0.08em;
  line-height: 1.4;
}
```

## Content Density

**Chinese slides need 20-30% less content than English equivalents.**

| Element | English | Chinese |
|---------|---------|---------|
| Heading max length | ~8 words | ~15 characters |
| Body text per line | ~12 words | ~25 characters |
| Bullets per slide | 5-6 | 3-5 |
| Paragraph lines | 3-4 | 2-3 |

## Emphasis Rules

### DO

- Use **bold** (`font-weight: 700`) for emphasis
- Use **color** (accent color, contrasting hue) for emphasis
- Use **size** (larger font) for emphasis

### DON'T

- **NEVER use italic for Chinese** — CJK italic is nearly unreadable and looks broken
- Don't use underline for emphasis (can be confused with links)

```css
/* Remove italic from blockquotes — critical for Chinese */
.reveal blockquote { font-style: normal; }
em { font-style: normal; font-weight: 700; color: #667eea; }
```

## Punctuation

### Use full-width Chinese punctuation

| Use | Don't Use |
|-----|-----------|
| ，（逗号） | , |
| 。（句号） | . |
| ：（冒号） | : |
| ；（分号） | ; |
| "..."（引号） | "..." |
| ——（破折号） | -- |
| ？（问号） | ? |
| ！（感叹号） | ! |

Exception: In code, data, and English text, use half-width punctuation.

## Mixed CJK-English Spacing

When Chinese and English/numbers appear in the same line:

- Add a thin space (or rely on `text-spacing` CSS) between CJK and Latin characters
- Keep English terms in original form: "台积电的 CoWoS 封装技术"
- Don't translate brand names or well-known technical terms

## Numbers and Units

| Context | Format | Example |
|---------|--------|---------|
| Statistics | Arabic numerals | 71%、60%、1224亿 |
| Large numbers | Arabic + Chinese unit | 234亿美元、103亿美元 |
| Currency | Arabic + Chinese | 三万美元 or 30,000美元 |
| Ordinal | Chinese | 第一、第二 |
| Year | Arabic | 2025年 |
| Ratios | Arabic | 20-30% |

**On stat slides**: Always use Arabic numerals in giant font for visual impact. Use Chinese for the descriptive context below.
