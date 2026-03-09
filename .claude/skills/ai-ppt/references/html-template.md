# Zero-Dependency HTML Presentation Template

Self-contained HTML file with inline CSS/JS. No npm, no CDN dependencies, no build tools. Just open in browser.

**Adapted from [frontend-slides](https://github.com/zarazhangrui/frontend-slides) with integration into ai-ppt workflow.**

---

## When to Use

Choose this over SlideDev/Reveal.js when:
- Quick sharing is the top priority (single file, email/Slack it)
- Zero setup environment (no Node.js, no internet needed after generation)
- Maximum visual customization (no framework constraints)
- User explicitly requests "纯 HTML" or "零依赖"

---

## Complete Template

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>演示文稿标题</title>

  <!-- Fonts: Replace with preset-specific fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;700;900&display=swap" rel="stylesheet">

  <style>
    /* ===== CSS Variables (from style preset) ===== */
    :root {
      --bg-primary: #0a0a15;
      --bg-secondary: #111827;
      --text-primary: #ffffff;
      --text-secondary: #9ca3af;
      --accent: #667eea;
      --accent-glow: rgba(102, 126, 234, 0.3);

      --font-display: "Noto Sans SC", sans-serif;
      --font-body: "Noto Sans SC", sans-serif;

      --title-size: clamp(1.5rem, 5vw, 4rem);
      --h2-size: clamp(1.25rem, 3.5vw, 2.5rem);
      --h3-size: clamp(1rem, 2.5vw, 1.75rem);
      --body-size: clamp(0.75rem, 1.5vw, 1.125rem);
      --small-size: clamp(0.65rem, 1vw, 0.875rem);

      --slide-padding: clamp(1rem, 4vw, 4rem);
      --content-gap: clamp(0.5rem, 2vw, 2rem);
      --element-gap: clamp(0.25rem, 1vw, 1rem);

      --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
      --duration-normal: 0.6s;
    }

    /* ===== Reset ===== */
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

    /* ===== Slide Base ===== */
    .slide {
      width: 100vw;
      height: 100vh;
      height: 100dvh;
      overflow: hidden;
      scroll-snap-align: start;
      display: flex;
      flex-direction: column;
      position: relative;
    }

    .slide-content {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      max-height: 100%;
      overflow: hidden;
      padding: var(--slide-padding);
    }

    .slide-content.left-align {
      align-items: flex-start;
      text-align: left;
    }

    /* ===== Typography ===== */
    h1 {
      font-family: var(--font-display);
      font-size: var(--title-size);
      font-weight: 700;
      letter-spacing: 0.08em;
      line-height: 1.3;
    }

    h2 {
      font-family: var(--font-display);
      font-size: var(--h2-size);
      font-weight: 700;
      letter-spacing: 0.08em;
      margin-bottom: var(--content-gap);
    }

    h3 {
      font-family: var(--font-display);
      font-size: var(--h3-size);
      font-weight: 700;
    }

    p {
      font-size: var(--body-size);
      color: var(--text-secondary);
      line-height: 1.8;
      max-width: 800px;
    }

    /* Chinese: never use italic */
    blockquote { font-style: normal; }
    em { font-style: normal; font-weight: 700; color: var(--accent); }

    /* ===== Entrance Animations ===== */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity var(--duration-normal) var(--ease-out-expo),
                  transform var(--duration-normal) var(--ease-out-expo);
    }
    .slide.visible .reveal {
      opacity: 1;
      transform: translateY(0);
    }
    .reveal:nth-child(1) { transition-delay: 0.1s; }
    .reveal:nth-child(2) { transition-delay: 0.2s; }
    .reveal:nth-child(3) { transition-delay: 0.3s; }
    .reveal:nth-child(4) { transition-delay: 0.4s; }
    .reveal:nth-child(5) { transition-delay: 0.5s; }

    /* ===== Layout Utilities ===== */
    .two-cols {
      display: flex;
      gap: clamp(1rem, 3vw, 3rem);
      width: 100%;
      max-width: 1000px;
    }
    .two-cols > div { flex: 1; text-align: left; }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(min(100%, 250px), 1fr));
      gap: clamp(0.5rem, 1.5vw, 1rem);
      width: 100%;
      max-width: 1000px;
    }

    ul, ol {
      text-align: left;
      max-width: 800px;
      list-style: none;
    }
    ul li { padding: 0.3em 0; }
    ul li::before {
      content: "—";
      color: var(--accent);
      margin-right: 0.5em;
    }

    /* ===== Stat Slide ===== */
    .big-number {
      font-family: var(--font-display);
      font-size: clamp(4rem, 15vw, 10rem);
      font-weight: 900;
      color: var(--accent);
      line-height: 1;
    }

    /* ===== Quote Slide ===== */
    blockquote {
      font-size: var(--h3-size);
      border-left: 4px solid var(--accent);
      padding: 0.5em 1em;
      text-align: left;
      max-width: 800px;
      line-height: 1.8;
    }
    .attribution {
      text-align: right;
      opacity: 0.6;
      font-size: var(--small-size);
      margin-top: 1em;
    }

    /* ===== Navigation ===== */
    .progress-bar {
      position: fixed;
      top: 0;
      left: 0;
      height: 4px;
      background: linear-gradient(90deg, var(--accent), var(--accent-glow));
      width: 0%;
      z-index: 1000;
      transition: width 0.3s ease;
    }

    .nav-dots {
      position: fixed;
      right: 2rem;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      gap: 1rem;
      z-index: 100;
    }

    .nav-dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background-color: rgba(255, 255, 255, 0.3);
      cursor: pointer;
      transition: all 0.3s ease;
    }
    .nav-dot.active {
      background-color: var(--accent);
      transform: scale(1.3);
    }

    /* ===== Inline Editing (opt-in) ===== */
    .edit-hotzone {
      position: fixed;
      top: 0;
      left: 0;
      width: 80px;
      height: 80px;
      z-index: 10000;
      cursor: pointer;
    }
    .edit-toggle {
      position: fixed;
      top: 1rem;
      left: 1rem;
      background: var(--accent);
      color: var(--bg-primary);
      border: none;
      border-radius: 8px;
      padding: 0.5rem 1rem;
      font-size: 1rem;
      cursor: pointer;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.3s ease;
      z-index: 10001;
    }
    .edit-toggle.show,
    .edit-toggle.active { opacity: 1; pointer-events: auto; }
    [contenteditable] {
      outline: 2px dashed var(--accent);
      padding: 0.25rem;
      border-radius: 4px;
    }

    /* ===== Responsive ===== */
    @media (max-height: 700px) {
      :root {
        --slide-padding: clamp(0.75rem, 3vw, 2rem);
        --content-gap: clamp(0.4rem, 1.5vw, 1rem);
        --title-size: clamp(1.25rem, 4.5vw, 2.5rem);
      }
    }
    @media (max-height: 600px) {
      :root {
        --slide-padding: clamp(0.5rem, 2.5vw, 1.5rem);
        --title-size: clamp(1.1rem, 4vw, 2rem);
        --body-size: clamp(0.7rem, 1.2vw, 0.95rem);
      }
      .nav-dots, .decorative { display: none; }
    }
    @media (max-height: 500px) {
      :root {
        --title-size: clamp(1rem, 3.5vw, 1.5rem);
        --body-size: clamp(0.65rem, 1vw, 0.85rem);
      }
    }
    @media (max-width: 600px) {
      .two-cols { flex-direction: column; }
      .grid { grid-template-columns: 1fr; }
    }
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.2s !important;
      }
      html { scroll-behavior: auto; }
    }
  </style>
</head>
<body>

  <div class="progress-bar"></div>
  <nav class="nav-dots"></nav>
  <!-- Inline editing elements (include only if user opted in) -->
  <div class="edit-hotzone"></div>
  <button class="edit-toggle" id="editToggle">编辑</button>

  <!-- ===== SLIDES START ===== -->

  <!-- Cover Slide -->
  <section class="slide" style="background: linear-gradient(135deg, var(--bg-primary), var(--bg-secondary));">
    <div class="slide-content">
      <h1 class="reveal">演示文稿标题</h1>
      <p class="reveal">副标题或作者</p>
    </div>
  </section>

  <!-- Content Slide -->
  <section class="slide">
    <div class="slide-content">
      <h2 class="reveal">章节标题</h2>
      <p class="reveal">内容在这里...</p>
    </div>
  </section>

  <!-- Stat Slide -->
  <section class="slide">
    <div class="slide-content">
      <div class="big-number reveal">71%</div>
      <p class="reveal">全球晶圆代工市场份额</p>
    </div>
  </section>

  <!-- Two-Column Slide -->
  <section class="slide">
    <div class="slide-content">
      <h2 class="reveal">对比标题</h2>
      <div class="two-cols reveal">
        <div>
          <h3 style="color: var(--accent);">左侧</h3>
          <ul>
            <li>要点一</li>
            <li>要点二</li>
          </ul>
        </div>
        <div>
          <h3 style="color: #e74c3c;">右侧</h3>
          <ul>
            <li>要点一</li>
            <li>要点二</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- Quote Slide -->
  <section class="slide">
    <div class="slide-content">
      <blockquote class="reveal">"引用内容放在这里"</blockquote>
      <p class="attribution reveal">— 出处</p>
    </div>
  </section>

  <!-- ===== SLIDES END ===== -->

  <script>
    class SlidePresentation {
      constructor() {
        this.slides = document.querySelectorAll('.slide');
        this.currentSlide = 0;
        this.isEditing = false;
        this.isScrolling = false;
        this.setupIntersectionObserver();
        this.setupKeyboardNav();
        this.setupTouchNav();
        this.setupMouseWheelNav();
        this.setupProgressBar();
        this.setupNavDots();
        this.setupInlineEditing();
      }

      setupIntersectionObserver() {
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              entry.target.classList.add('visible');
              const index = Array.from(this.slides).indexOf(entry.target);
              if (index !== -1) {
                this.currentSlide = index;
                this.updateProgressBar();
                this.updateNavDots();
              }
            }
          });
        }, { threshold: 0.5 });
        this.slides.forEach(slide => observer.observe(slide));
      }

      setupKeyboardNav() {
        document.addEventListener('keydown', (e) => {
          if (this.isEditing && e.target.getAttribute('contenteditable')) return;
          switch (e.key) {
            case 'ArrowRight': case ' ': case 'PageDown':
              e.preventDefault(); this.nextSlide(); break;
            case 'ArrowLeft': case 'PageUp':
              e.preventDefault(); this.prevSlide(); break;
            case 'e': case 'E':
              if (!e.target.getAttribute('contenteditable')) this.toggleEditMode();
              break;
          }
        });
      }

      setupTouchNav() {
        let startX = 0, startY = 0;
        document.addEventListener('touchstart', (e) => {
          startX = e.changedTouches[0].screenX;
          startY = e.changedTouches[0].screenY;
        });
        document.addEventListener('touchend', (e) => {
          const dx = startX - e.changedTouches[0].screenX;
          const dy = startY - e.changedTouches[0].screenY;
          if (Math.abs(dy) > Math.abs(dx) && Math.abs(dy) > 50) {
            if (dy > 0) this.nextSlide(); else this.prevSlide();
          } else if (Math.abs(dx) > 50) {
            if (dx > 0) this.nextSlide(); else this.prevSlide();
          }
        });
      }

      setupMouseWheelNav() {
        let lastScroll = 0;
        document.addEventListener('wheel', (e) => {
          if (this.isEditing) return;
          e.preventDefault();
          const now = Date.now();
          if (now - lastScroll < 800) return; // Debounce
          lastScroll = now;
          if (e.deltaY > 0) this.nextSlide(); else this.prevSlide();
        }, { passive: false });
      }

      nextSlide() {
        if (this.currentSlide < this.slides.length - 1) {
          this.currentSlide++;
          this.goToSlide(this.currentSlide);
        }
      }

      prevSlide() {
        if (this.currentSlide > 0) {
          this.currentSlide--;
          this.goToSlide(this.currentSlide);
        }
      }

      goToSlide(index) {
        this.currentSlide = index;
        this.slides[index].scrollIntoView({ behavior: 'smooth' });
        this.updateProgressBar();
        this.updateNavDots();
      }

      setupProgressBar() { this.updateProgressBar(); }

      updateProgressBar() {
        const pct = ((this.currentSlide + 1) / this.slides.length) * 100;
        const bar = document.querySelector('.progress-bar');
        if (bar) bar.style.width = pct + '%';
      }

      setupNavDots() {
        const nav = document.querySelector('.nav-dots');
        if (!nav) return;
        this.slides.forEach((_, i) => {
          const dot = document.createElement('div');
          dot.className = 'nav-dot' + (i === 0 ? ' active' : '');
          dot.addEventListener('click', () => this.goToSlide(i));
          nav.appendChild(dot);
        });
      }

      updateNavDots() {
        document.querySelectorAll('.nav-dot').forEach((dot, i) => {
          dot.classList.toggle('active', i === this.currentSlide);
        });
      }

      setupInlineEditing() {
        const hotzone = document.querySelector('.edit-hotzone');
        const btn = document.getElementById('editToggle');
        if (!hotzone || !btn) return;
        let hideTimeout = null;
        const show = () => { clearTimeout(hideTimeout); btn.classList.add('show'); };
        const hide = () => { hideTimeout = setTimeout(() => { if (!this.isEditing) btn.classList.remove('show'); }, 400); };
        hotzone.addEventListener('mouseenter', show);
        hotzone.addEventListener('mouseleave', hide);
        btn.addEventListener('mouseenter', show);
        btn.addEventListener('mouseleave', hide);
        hotzone.addEventListener('click', () => this.toggleEditMode());
        btn.addEventListener('click', () => this.toggleEditMode());
      }

      toggleEditMode() {
        this.isEditing = !this.isEditing;
        const btn = document.getElementById('editToggle');
        if (this.isEditing) {
          btn.classList.add('active');
          btn.textContent = '完成';
          document.querySelectorAll('h1, h2, h3, p, li, blockquote').forEach(el => {
            el.setAttribute('contenteditable', 'true');
          });
        } else {
          btn.classList.remove('active');
          btn.textContent = '编辑';
          document.querySelectorAll('[contenteditable]').forEach(el => {
            el.removeAttribute('contenteditable');
          });
        }
      }
    }

    new SlidePresentation();
  </script>
</body>
</html>
```

---

## Slide Type Patterns for HTML Output

Use these HTML patterns corresponding to the 15 slide types in `slide-type-catalog.md`.

### Cover

```html
<section class="slide" style="background: linear-gradient(135deg, var(--bg-primary), var(--bg-secondary));">
  <div class="slide-content">
    <h1 class="reveal">标题</h1>
    <p class="reveal">副标题</p>
    <p class="reveal" style="font-size: var(--small-size); opacity: 0.4; margin-top: 2em;">2025 · 作者</p>
  </div>
</section>
```

### Section Divider

```html
<section class="slide" style="background: linear-gradient(135deg, var(--accent), rgba(118,75,162,1));">
  <div class="slide-content">
    <p class="reveal" style="font-size: 4em; font-weight: 900; opacity: 0.3;">01</p>
    <h2 class="reveal" style="margin-top: -0.3em;">章节标题</h2>
    <p class="reveal" style="opacity: 0.7;">一句描述</p>
  </div>
</section>
```

### Single Stat

```html
<section class="slide">
  <div class="slide-content">
    <div class="big-number reveal">71%</div>
    <p class="reveal" style="font-size: var(--h3-size);">描述文字</p>
    <p class="reveal" style="font-size: var(--small-size); opacity: 0.5;">补充细节</p>
  </div>
</section>
```

### Bullet List

```html
<section class="slide">
  <div class="slide-content left-align">
    <h2 class="reveal">列表标题</h2>
    <ul>
      <li class="reveal">要点一</li>
      <li class="reveal">要点二</li>
      <li class="reveal">要点三</li>
    </ul>
  </div>
</section>
```

### Two-Column

```html
<section class="slide">
  <div class="slide-content">
    <h2 class="reveal">对比标题</h2>
    <div class="two-cols reveal">
      <div>
        <h3 style="color: var(--accent);">左侧</h3>
        <ul><li>...</li></ul>
      </div>
      <div>
        <h3 style="color: #e74c3c;">右侧</h3>
        <ul><li>...</li></ul>
      </div>
    </div>
  </div>
</section>
```

### Quote

```html
<section class="slide">
  <div class="slide-content">
    <blockquote class="reveal">"引用内容"</blockquote>
    <p class="attribution reveal">— 出处</p>
  </div>
</section>
```

### Thesis/Statement

```html
<section class="slide">
  <div class="slide-content">
    <h2 class="reveal" style="font-size: var(--h2-size); max-width: 800px;">
      一句核心论点
    </h2>
  </div>
</section>
```

### Timeline

```html
<section class="slide">
  <div class="slide-content left-align">
    <h2 class="reveal">时间线标题</h2>
    <div style="max-width: 700px;">
      <div class="reveal" style="border-left: 3px solid var(--accent); padding-left: 1em; margin-bottom: 0.8em;">
        <strong style="color: var(--accent);">2024初</strong><br>事件描述
      </div>
      <div class="reveal" style="border-left: 3px solid var(--accent); padding-left: 1em; margin-bottom: 0.8em;">
        <strong style="color: var(--accent);">2024中</strong><br>事件描述
      </div>
    </div>
  </div>
</section>
```

---

## Content Density Limits (Hard Limits)

These are **maximum** values. Exceeding any limit requires splitting into multiple slides.

| Slide Type | Limit |
|-----------|-------|
| Cover | 1 heading + 1 subtitle |
| Content/Bullet | 1 heading + max 5 bullets |
| Feature Grid | Max 6 cards |
| Stat | 1 number + 1 context line + 1 detail |
| Quote | Max 3 lines + attribution |
| Two-Column | 2 × 3-4 items |
| Timeline | 3-5 time points |
| Code (if any) | 8-10 lines |
