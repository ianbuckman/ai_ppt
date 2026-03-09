# Reveal.js 5.x Syntax Reference

Complete syntax reference for Reveal.js presentations. Use this when generating Reveal.js output.

## Full HTML Boilerplate

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>演示文稿标题</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reset.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/theme/black.css">
  <!-- Syntax highlighting -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/highlight/monokai.css">
  <!-- Chinese fonts -->
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;700;900&display=swap');
    :root {
      --r-main-font: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
      --r-heading-font: "Noto Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;
      --r-main-font-size: 38px;
      --r-heading-line-height: 1.4;
      --r-block-margin: 20px;
    }
    .reveal { line-height: 1.8; letter-spacing: 0.05em; }
    .reveal h1, .reveal h2, .reveal h3 { letter-spacing: 0.08em; font-weight: 700; }
  </style>
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <!-- Slides go here -->
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/reveal.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/highlight/highlight.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/notes/notes.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/math/math.js"></script>
  <script>
    Reveal.initialize({
      hash: true,
      transition: 'slide',
      transitionSpeed: 'default',
      backgroundTransition: 'fade',
      autoAnimateDuration: 0.7,
      autoAnimateEasing: 'ease',
      width: 1920,
      height: 1080,
      margin: 0.04,
      plugins: [RevealHighlight, RevealNotes, RevealMath.KaTeX]
    });
  </script>
</body>
</html>
```

## Slide Structure

### Basic horizontal slide

```html
<section>
  <h2>标题</h2>
  <p>内容</p>
</section>
```

### Vertical slides (sub-topics within a section)

```html
<section>
  <!-- Vertical stack -->
  <section>
    <h2>主题概览</h2>
  </section>
  <section>
    <h2>细节一</h2>
  </section>
  <section>
    <h2>细节二</h2>
  </section>
</section>
```

## Built-in Themes (12 total)

| Theme | Style | Best For |
|-------|-------|----------|
| `black` | Dark, high contrast | Business, formal |
| `white` | Clean, minimal | General purpose |
| `league` | Dark, gray tones | Tech presentations |
| `beige` | Warm, paper-like | Academic |
| `sky` | Light blue gradient | Casual, friendly |
| `night` | Dark blue | Evening events |
| `serif` | Traditional, serif fonts | Formal, literary |
| `simple` | White, no distractions | Content-focused |
| `solarized` | Solarized color scheme | Developer talks |
| `blood` | Dark red accents | Dramatic |
| `moon` | Dark, muted | Elegant, understated |
| `dracula` | Dracula color scheme | Developer talks |

Change theme by swapping the CSS link:
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5/dist/theme/moon.css">
```

## Transitions

### Global (in Reveal.initialize)

```javascript
transition: 'slide',      // none | fade | slide | convex | concave | zoom
transitionSpeed: 'default' // default | fast | slow
```

### Per-slide override

```html
<section data-transition="fade">
  <h2>使用 fade 过渡</h2>
</section>

<section data-transition="zoom-in fade-out">
  <h2>进入时 zoom，离开时 fade</h2>
</section>
```

### Transition combinations

```
slide-in fade-out     # Slide in, fade out
zoom-in slide-out     # Zoom in, slide out
fade-in none-out      # Fade in, no exit animation
convex-in concave-out # Convex in, concave out
```

## Backgrounds

### Solid color

```html
<section data-background-color="#1a1a2e">
  <h2>深色背景</h2>
</section>
```

### Gradient

```html
<section data-background-gradient="linear-gradient(135deg, #667eea 0%, #764ba2 100%)">
  <h2>渐变背景</h2>
</section>

<section data-background-gradient="radial-gradient(circle, #2c3e50 0%, #000 100%)">
  <h2>径向渐变</h2>
</section>
```

### Image

```html
<section data-background-image="url.jpg" data-background-size="cover" data-background-opacity="0.3">
  <h2>图片背景（半透明）</h2>
</section>
```

### Video

```html
<section data-background-video="video.mp4" data-background-video-loop data-background-video-muted>
  <h2>视频背景</h2>
</section>
```

### iframe

```html
<section data-background-iframe="https://example.com" data-background-interactive>
  <h2>嵌入网页</h2>
</section>
```

## Fragments (Progressive Reveal)

### Basic fragment

```html
<p class="fragment">第一次点击显示</p>
<p class="fragment">第二次点击显示</p>
```

### Fragment classes

| Class | Effect |
|-------|--------|
| `fade-in` | Default, fade in |
| `fade-out` | Fade out |
| `fade-up` | Slide up while fading in |
| `fade-down` | Slide down while fading in |
| `fade-left` | Slide left while fading in |
| `fade-right` | Slide right while fading in |
| `fade-in-then-out` | Fade in, then out on next step |
| `fade-in-then-semi-out` | Fade in, then semi-transparent |
| `grow` | Scale up |
| `shrink` | Scale down |
| `strike` | Strikethrough |
| `highlight-red` | Text turns red |
| `highlight-green` | Text turns green |
| `highlight-blue` | Text turns blue |
| `highlight-current-red` | Red only when active |
| `highlight-current-green` | Green only when active |
| `highlight-current-blue` | Blue only when active |

### Usage

```html
<ul>
  <li class="fragment fade-up">要点一</li>
  <li class="fragment fade-up">要点二</li>
  <li class="fragment fade-up">要点三</li>
</ul>
```

### Fragment ordering

```html
<p class="fragment" data-fragment-index="2">显示第二</p>
<p class="fragment" data-fragment-index="1">显示第一</p>
<p class="fragment" data-fragment-index="3">显示第三</p>
```

### Nested fragments

```html
<span class="fragment fade-in">
  <span class="fragment highlight-red">先出现，再变红</span>
</span>
```

## Auto-Animate

Smooth automatic animation between slides with matching elements.

### Basic usage

```html
<section data-auto-animate>
  <h2>标题</h2>
</section>
<section data-auto-animate>
  <h2 style="color: #e74c3c;">标题</h2>
  <p>新增的内容</p>
</section>
```

### With data-id matching

```html
<section data-auto-animate>
  <div data-id="box" style="width: 100px; height: 100px; background: #3498db;"></div>
</section>
<section data-auto-animate>
  <div data-id="box" style="width: 300px; height: 300px; background: #e74c3c; border-radius: 50%;"></div>
</section>
```

### Auto-animate for lists (growing content)

```html
<section data-auto-animate>
  <h3>飞轮效应</h3>
  <ul>
    <li>最好的客户带来最大的收入</li>
  </ul>
</section>
<section data-auto-animate>
  <h3>飞轮效应</h3>
  <ul>
    <li>最好的客户带来最大的收入</li>
    <li>收入碾压转化为投入碾压</li>
  </ul>
</section>
<section data-auto-animate>
  <h3>飞轮效应</h3>
  <ul>
    <li>最好的客户带来最大的收入</li>
    <li>收入碾压转化为投入碾压</li>
    <li>技术领先带来更多客户……</li>
  </ul>
</section>
```

### Auto-animate options

```html
<section data-auto-animate
         data-auto-animate-duration="0.5"
         data-auto-animate-easing="ease-in-out">
```

## Speaker Notes

```html
<section>
  <h2>标题</h2>
  <p>内容</p>
  <aside class="notes">
    这里是演讲者备注。
    按 S 键打开演讲者视图。
    支持 HTML 标签。
  </aside>
</section>
```

## Code Highlighting

```html
<section>
  <pre><code class="language-python" data-trim data-line-numbers="2-3|5">
def process():
    step_one()   # highlighted first
    step_two()   # highlighted first

    step_three() # highlighted second
  </code></pre>
</section>
```

## Custom CSS Patterns

### Giant stat slide

```html
<section data-background-gradient="radial-gradient(circle, #1a1a2e 0%, #0a0a15 100%)">
  <div style="font-size: 8em; font-weight: 900; color: #667eea; line-height: 1;">
    71%
  </div>
  <p style="font-size: 1.5em; opacity: 0.8; margin-top: 0.5em;">
    全球晶圆代工市场份额
  </p>
  <p class="fragment fade-up" style="font-size: 0.9em; opacity: 0.5;">
    领先第二名 64 个百分点
  </p>
</section>
```

### Quote slide

```html
<section>
  <blockquote style="font-size: 1.3em; border-left: 4px solid #667eea; padding-left: 1em; text-align: left; font-style: normal;">
    "产能卖光了，一直到2026年都是满的。"
  </blockquote>
  <p style="text-align: right; opacity: 0.6; font-size: 0.8em;">
    — 魏哲家，台积电CEO
  </p>
</section>
```

### Two-column comparison

```html
<section>
  <h3>对比</h3>
  <div style="display: flex; gap: 2em; margin-top: 1em;">
    <div style="flex: 1; text-align: left;">
      <h4 style="color: #2ecc71;">台积电</h4>
      <ul>
        <li class="fragment fade-up">市占率 71%</li>
        <li class="fragment fade-up">毛利率 ~60%</li>
      </ul>
    </div>
    <div style="flex: 1; text-align: left;">
      <h4 style="color: #e74c3c;">三星</h4>
      <ul>
        <li class="fragment fade-up">市占率 6.8%</li>
        <li class="fragment fade-up">代工亏损</li>
      </ul>
    </div>
  </div>
</section>
```

### Section divider

```html
<section data-background-gradient="linear-gradient(135deg, #667eea 0%, #764ba2 100%)" data-transition="fade">
  <h2 style="font-size: 2.5em;">第二章</h2>
  <p style="font-size: 1.2em; opacity: 0.8;">飞轮效应</p>
</section>
```

## Theme Overrides (Custom CSS)

```html
<style>
  /* Override theme colors */
  :root {
    --r-background-color: #0a0a15;
    --r-main-color: #e0e0e0;
    --r-heading-color: #ffffff;
    --r-link-color: #667eea;
    --r-selection-background-color: #667eea;
  }

  /* Remove italic from blockquote (important for Chinese) */
  .reveal blockquote {
    font-style: normal;
  }

  /* Better list spacing for Chinese */
  .reveal ul li, .reveal ol li {
    margin-bottom: 0.5em;
    line-height: 1.8;
  }

  /* Accent color utility */
  .accent { color: #667eea; }
  .accent-green { color: #2ecc71; }
  .accent-red { color: #e74c3c; }
  .big-number {
    font-size: 6em;
    font-weight: 900;
    line-height: 1;
  }
</style>
```

## Initialization Config Options

```javascript
Reveal.initialize({
  // Navigation
  hash: true,              // Enable URL hash for each slide
  history: false,          // Push each slide change to browser history
  controls: true,          // Show navigation arrows
  progress: true,          // Show progress bar

  // Presentation
  transition: 'slide',     // none|fade|slide|convex|concave|zoom
  transitionSpeed: 'default', // default|fast|slow
  backgroundTransition: 'fade',

  // Auto-animate
  autoAnimateDuration: 0.7,
  autoAnimateEasing: 'ease',
  autoAnimateUnmatched: true,

  // Display
  width: 1920,
  height: 1080,
  margin: 0.04,
  center: true,            // Vertical centering

  // Plugins
  plugins: [RevealHighlight, RevealNotes, RevealMath.KaTeX]
});
```

## Plugin Loading

### Highlight (code syntax highlighting)

```html
<script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/highlight/highlight.js"></script>
```

### Notes (speaker notes)

```html
<script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/notes/notes.js"></script>
```

### Math (LaTeX/KaTeX)

```html
<script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/math/math.js"></script>
```

### Markdown (write slides in markdown)

```html
<script src="https://cdn.jsdelivr.net/npm/reveal.js@5/plugin/markdown/markdown.js"></script>
```

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `S` | Open speaker notes view |
| `F` | Fullscreen |
| `O` / `Esc` | Slide overview |
| `B` / `.` | Black screen (pause) |
| `?` | Show keyboard shortcuts |
| `→` / `Space` | Next slide |
| `←` | Previous slide |
| `↑` / `↓` | Navigate vertical slides |
