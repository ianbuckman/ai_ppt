# Emotion-Driven Animation Patterns

Animations should match the presentation's emotional tone, not be applied generically. This reference maps 6 emotional categories to specific animation implementations.

**Adapted from [frontend-slides](https://github.com/zarazhangrui/frontend-slides).**

---

## Effect-to-Feeling Guide

| Feeling | Animation Style | Duration | Easing |
|---------|----------------|----------|--------|
| Dramatic/Cinematic | Slow fades, large scale | 1–1.5s | ease-out |
| Techy/Futuristic | Neon glow, glitch, grid reveal | 0.4–0.8s | cubic-bezier |
| Playful/Friendly | Bouncy, floating, wobble | 0.5–0.8s | spring physics |
| Professional/Corporate | Subtle, fast, clean | 0.2–0.3s | ease |
| Calm/Minimal | Gentle fades, slow motion | 0.8–1.2s | ease-in-out |
| Editorial/Magazine | Staggered reveals, type animation | 0.6–1s | ease-out-expo |

---

## Core CSS Variables

```css
:root {
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-out-back: cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-in-out-smooth: cubic-bezier(0.4, 0, 0.2, 1);
  --duration-fast: 0.3s;
  --duration-normal: 0.6s;
  --duration-slow: 1s;
  --duration-dramatic: 1.5s;
}
```

---

## Entrance Animation Patterns

### Fade + Slide Up (Most Versatile — default choice)

```css
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s var(--ease-out-expo),
              transform 0.6s var(--ease-out-expo);
}
.slide.visible .reveal {
  opacity: 1;
  transform: translateY(0);
}

/* Staggered children */
.reveal:nth-child(1) { transition-delay: 0.1s; }
.reveal:nth-child(2) { transition-delay: 0.2s; }
.reveal:nth-child(3) { transition-delay: 0.3s; }
.reveal:nth-child(4) { transition-delay: 0.4s; }
```

### Scale In (For stats, key numbers)

```css
.reveal-scale {
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}
.slide.visible .reveal-scale {
  opacity: 1;
  transform: scale(1);
}
```

### Slide from Left (For comparisons, before/after)

```css
.reveal-left {
  opacity: 0;
  transform: translateX(-50px);
  transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}
.slide.visible .reveal-left {
  opacity: 1;
  transform: translateX(0);
}
```

### Blur In (For dramatic/cinematic mood)

```css
.reveal-blur {
  opacity: 0;
  filter: blur(10px);
  transition: opacity 0.8s, filter 0.8s var(--ease-out-expo);
}
.slide.visible .reveal-blur {
  opacity: 1;
  filter: blur(0);
}
```

---

## Background Effects

### Gradient Mesh (Depth/premium feel)

```css
.bg-mesh {
  background:
    radial-gradient(ellipse at 20% 80%, rgba(102, 126, 234, 0.15) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 20%, rgba(118, 75, 162, 0.15) 0%, transparent 50%),
    var(--bg-primary);
}
```

### Grid Pattern (Tech/futuristic)

```css
.bg-grid {
  background-image:
    linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
  background-size: 50px 50px;
}
```

### Noise Texture (Subtle grain)

```css
.bg-noise::after {
  content: '';
  position: absolute;
  inset: 0;
  opacity: 0.03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  pointer-events: none;
}
```

---

## Mood-Specific Implementations

### Dramatic/Cinematic

Use with: Dark Botanical, Bold Signal

```css
/* Slow, grand entrance */
.dramatic .reveal {
  transition-duration: 1.2s;
  transform: translateY(40px) scale(0.95);
}
.dramatic .reveal-number {
  transition-duration: 1.5s;
  transform: scale(0.8);
  filter: blur(5px);
}
.dramatic.visible .reveal-number {
  transform: scale(1);
  filter: blur(0);
}
```

### Techy/Futuristic

Use with: Neon Cyber, Terminal Green

```css
/* Neon glow on hover/focus */
.neon-glow {
  text-shadow:
    0 0 10px var(--accent-cyan),
    0 0 40px rgba(0, 255, 204, 0.3);
}

/* Glitch text effect */
@keyframes glitch {
  0%, 100% { transform: translate(0); }
  20% { transform: translate(-2px, 2px); }
  40% { transform: translate(2px, -2px); }
  60% { transform: translate(-1px, -1px); }
  80% { transform: translate(1px, 1px); }
}
.glitch { animation: glitch 0.3s ease-in-out; }

/* Scan line overlay */
.scanlines::after {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg, transparent, transparent 2px,
    rgba(0, 0, 0, 0.1) 2px, rgba(0, 0, 0, 0.1) 4px
  );
  pointer-events: none;
}
```

### Playful/Friendly

Use with: Split Pastel, Pastel Geometry

```css
/* Bouncy entrance */
.bouncy .reveal {
  transition-timing-function: var(--ease-out-back);
  transform: translateY(40px) scale(0.8);
}
.bouncy.visible .reveal {
  transform: translateY(0) scale(1);
}

/* Floating animation for decorative elements */
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}
.floating { animation: float 3s ease-in-out infinite; }
```

### Professional/Corporate

Use with: Swiss Modern, Electric Studio

```css
/* Fast, subtle — almost invisible */
.professional .reveal {
  transition-duration: 0.25s;
  transition-timing-function: ease;
  transform: translateY(15px);
}
```

### Calm/Minimal

Use with: Paper & Ink, Pastel Geometry

```css
/* Gentle, slow fade */
.calm .reveal {
  transition-duration: 1s;
  transition-timing-function: ease-in-out;
  transform: translateY(10px);
}
```

### Editorial/Magazine

Use with: Vintage Editorial, Notebook Tabs

```css
/* Staggered text reveals */
.editorial .reveal:nth-child(1) { transition-delay: 0s; }
.editorial .reveal:nth-child(2) { transition-delay: 0.15s; }
.editorial .reveal:nth-child(3) { transition-delay: 0.3s; }
.editorial .reveal:nth-child(4) { transition-delay: 0.45s; }
.editorial .reveal:nth-child(5) { transition-delay: 0.6s; }
```

---

## 3D Tilt Effect (Interactive — use sparingly)

```javascript
class TiltEffect {
  constructor(element, maxTilt = 10) {
    this.element = element;
    this.maxTilt = maxTilt;
    element.style.transformStyle = 'preserve-3d';
    element.style.transition = 'transform 0.1s ease';

    element.addEventListener('mousemove', (e) => {
      const rect = element.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width - 0.5;
      const y = (e.clientY - rect.top) / rect.height - 0.5;
      element.style.transform =
        `rotateY(${x * this.maxTilt}deg) rotateX(${-y * this.maxTilt}deg)`;
    });

    element.addEventListener('mouseleave', () => {
      element.style.transform = 'rotateY(0) rotateX(0)';
    });
  }
}
```

---

## Framework Mapping

### SlideDev Animation Equivalents

| Animation Pattern | SlideDev Implementation |
|------------------|------------------------|
| Fade + slide up | `v-click` (built-in) |
| Staggered children | `<v-clicks>` |
| Scale in | `v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }"` |
| Slide from left | `v-motion :initial="{ x: -50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }"` |
| Blur in | Custom CSS in `<style>` block |

### Reveal.js Animation Equivalents

| Animation Pattern | Reveal.js Implementation |
|------------------|-------------------------|
| Fade + slide up | `class="fragment fade-up"` |
| Scale in | `class="fragment grow"` |
| Slide from left | `class="fragment fade-right"` |
| Staggered | Sequential `data-fragment-index` |
| Dramatic slow | `data-auto-animate-duration="1.5"` |

---

## Performance Rules

1. **Only animate `transform` and `opacity`** — avoid animating layout properties (width, height, margin)
2. **Use `will-change: transform`** on animated elements (but remove after animation completes)
3. **Respect `prefers-reduced-motion`**:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.2s !important;
  }
}
```
4. **Limit active animations** — no more than 3 simultaneous animated elements per slide
