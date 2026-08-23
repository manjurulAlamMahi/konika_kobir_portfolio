# Konika Portfolio Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page, premium, feminine-elegant portfolio site for Kamrun Nahar Konika as static `index.html` / `style.css` / `script.js`, using only content from her CV.

**Architecture:** One HTML document with a `<main>` containing content sections in scroll order, plus sibling floating chrome (nav, social stack, WhatsApp button) positioned via `position: fixed`. One CSS file organized as: variables/reset → background/chrome → sections → responsive media queries. One JS file with independent, small functions: typing effect, scrollspy + smooth scroll, IntersectionObserver fade-ins, mobile nav toggle — wired up from a single `DOMContentLoaded` entry point.

**Tech Stack:** Plain HTML5, CSS3 (custom properties, flexbox/grid, keyframe animations), vanilla ES6 JS (no build step, no frameworks, no libraries). Google Fonts via `<link>` (Playfair Display + Poppins).

## Global Constraints

- Content restricted exclusively to `Resume Of KAMRUN NAHAR KONIKA.pdf`; no invented projects/skills/experience/achievements/contact info.
- Facebook and Instagram links are `#` placeholders with an HTML comment flagging them for replacement; GitHub (`https://github.com/konikakobir28`) and LinkedIn (`https://linkedin.com/in/konika-kobir-b3a982363`) use real CV values.
- WhatsApp button links to `https://wa.me/8801735490177`.
- Palette: Rose Gold & Blush — blush pink + rose-gold gradient accents, deep plum/charcoal text, cream/ivory background.
- No external JS/CSS libraries or CDNs other than Google Fonts.
- All animations must respect `prefers-reduced-motion: reduce`.
- No horizontal overflow at any breakpoint from 320px up.
- Profile image is a swappable placeholder: `<img src="assets/profile.jpg" alt="Kamrun Nahar Konika" />` inside a styled empty-state frame — no external placeholder image service.

---

## File Structure

```
index.html   - all markup, sections in scroll order + floating chrome
style.css    - variables, reset, background, chrome, sections, responsive
script.js    - typing effect, scrollspy/smooth-scroll, fade-in observer, mobile nav toggle
```

No `assets/` files are created (no image exists yet); the `img` tag path is written as documented above and will 404 gracefully behind the styled frame until the user adds the file.

---

### Task 1: Project scaffold + design system foundation

**Files:**
- Create: `index.html`
- Create: `style.css`
- Create: `script.js`

**Interfaces:**
- Produces: CSS custom properties on `:root` that every later task's CSS must reuse (no new ad-hoc colors): `--font-display`, `--font-body`, `--color-bg`, `--color-bg-alt`, `--color-text`, `--color-text-soft`, `--color-blush`, `--color-blush-soft`, `--color-rose-gold`, `--color-rose-gold-deep`, `--color-plum`, `--color-glass`, `--shadow-soft`, `--radius-lg`, `--radius-md`, `--radius-sm`, `--container-width`.
- Produces: `index.html` skeleton with `<main>` (empty, later tasks append `<section>` children) and a `.bg-decor` wrapper (empty, holds blob divs) as the first child of `<body>`.
- Produces: `script.js` with an empty `document.addEventListener('DOMContentLoaded', () => { });` entry point that later tasks fill in.

- [ ] **Step 1: Create `index.html` skeleton**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kamrun Nahar Konika | Portfolio</title>
  <meta name="description" content="Kamrun Nahar Konika — B.Sc in Computer Science and Engineering, University of Asia Pacific. Portfolio, projects and experience." />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@500;600;700&family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="bg-decor" aria-hidden="true">
    <!-- decorative blobs added in Task 2 -->
  </div>

  <!-- floating nav added in Task 3 -->
  <!-- floating social stack + whatsapp button added in Task 3 -->

  <main>
    <!-- sections appended by Tasks 4-6 -->
  </main>

  <script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: Create `style.css` with reset + design tokens**

```css
/* ---------- Design tokens ---------- */
:root {
  --font-display: 'Playfair Display', Georgia, 'Times New Roman', serif;
  --font-body: 'Poppins', 'Segoe UI', Tahoma, sans-serif;

  --color-bg: #FBF3EF;
  --color-bg-alt: #FFF9F6;
  --color-text: #3A2A33;
  --color-text-soft: #6B5560;

  --color-blush: #F4C6D0;
  --color-blush-soft: #FBE4E9;
  --color-rose-gold: #C98A9C;
  --color-rose-gold-deep: #B76E79;
  --color-plum: #5C3A4E;

  --color-glass: rgba(255, 255, 255, 0.55);
  --color-glass-border: rgba(255, 255, 255, 0.65);

  --shadow-soft: 0 10px 30px rgba(183, 110, 121, 0.18);
  --shadow-lift: 0 16px 40px rgba(183, 110, 121, 0.26);

  --radius-lg: 28px;
  --radius-md: 18px;
  --radius-sm: 10px;

  --container-width: 1100px;
}

/* ---------- Reset ---------- */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font-body);
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.6;
  overflow-x: hidden;
  min-height: 100vh;
}
img { max-width: 100%; display: block; }
a { color: inherit; text-decoration: none; }
ul { list-style: none; }
h1, h2, h3 { font-family: var(--font-display); color: var(--color-plum); line-height: 1.2; }
section { position: relative; padding: 6rem 1.5rem; max-width: var(--container-width); margin: 0 auto; }
.bg-decor { position: fixed; inset: 0; z-index: -1; overflow: hidden; pointer-events: none; }

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.001ms !important; animation-iteration-count: 1 !important; transition-duration: 0.001ms !important; scroll-behavior: auto !important; }
}
```

- [ ] **Step 3: Create `script.js` entry point**

```js
document.addEventListener('DOMContentLoaded', () => {
  // Initializers wired up by later tasks (typing effect, scrollspy, fade-in observer, mobile nav toggle)
});
```

- [ ] **Step 4: Verify files parse cleanly**

Run: `node --check script.js`
Expected: no output (exit code 0)

Run (from project root): `grep -c "color-rose-gold" style.css`
Expected: a number ≥ 1 (confirms the token was written)

- [ ] **Step 5: Commit**

```bash
git add index.html style.css script.js
git commit -m "Scaffold portfolio site with design token system"
```

---

### Task 2: Background decor + hero section

**Files:**
- Modify: `index.html` (fill `.bg-decor`, add `<section id="home" class="hero">` inside `<main>`)
- Modify: `style.css` (blob animations, `.hero` styles)
- Modify: `script.js` (typing effect)

**Interfaces:**
- Consumes: design tokens from Task 1 (`--color-blush`, `--color-rose-gold`, `--radius-lg`, `--shadow-soft`, etc.).
- Produces: `#home` section id (nav in Task 3 links to it). Produces `.scroll-indicator` element with id `scrollDown` (Task 3/6 may reuse). Produces JS function `initTypingEffect()` called from the `DOMContentLoaded` handler.

- [ ] **Step 1: Add background blobs to `index.html`**

```html
<div class="bg-decor" aria-hidden="true">
  <span class="blob blob-1"></span>
  <span class="blob blob-2"></span>
  <span class="blob blob-3"></span>
</div>
```

- [ ] **Step 2: Add hero markup inside `<main>`**

```html
<section id="home" class="hero">
  <div class="hero-photo-frame">
    <img src="assets/profile.jpg" alt="Kamrun Nahar Konika" />
  </div>
  <h1 class="hero-name">Kamrun Nahar Konika</h1>
  <p class="hero-title">B.Sc in Computer Science and Engineering</p>
  <p class="hero-typing"><span id="typingText"></span><span class="typing-cursor" aria-hidden="true">|</span></p>
  <p class="hero-intro">
    Computer Science and Engineering graduate with foundational knowledge of programming,
    web technologies, and software development concepts. Skilled in communication,
    teamwork, presentation, and public interaction through university activities and
    customer-facing roles.
  </p>
  <a href="#about" class="scroll-indicator" id="scrollDown" aria-label="Scroll to About section">
    <span class="scroll-mouse"></span>
  </a>
</section>
```

- [ ] **Step 3: Style blobs in `style.css`**

```css
.blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.55;
  animation: float 16s ease-in-out infinite;
}
.blob-1 { width: 420px; height: 420px; top: -120px; left: -100px; background: var(--color-blush); }
.blob-2 { width: 340px; height: 340px; top: 40%; right: -140px; background: var(--color-rose-gold); animation-delay: -5s; }
.blob-3 { width: 300px; height: 300px; bottom: -140px; left: 30%; background: var(--color-blush-soft); animation-delay: -10s; }

@keyframes float {
  0%, 100% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(30px, -40px) scale(1.08); }
}
```

- [ ] **Step 4: Style hero in `style.css`**

```css
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: 1.1rem;
  padding-top: 4rem;
}
.hero-photo-frame {
  width: clamp(180px, 28vw, 260px);
  height: clamp(180px, 28vw, 260px);
  border-radius: 50%;
  background: linear-gradient(145deg, var(--color-blush), var(--color-rose-gold));
  padding: 6px;
  box-shadow: var(--shadow-lift);
  margin-bottom: 0.5rem;
}
.hero-photo-frame img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  background: var(--color-bg-alt);
}
.hero-name { font-size: clamp(2.2rem, 5vw, 3.4rem); }
.hero-title { font-size: clamp(1rem, 2vw, 1.25rem); color: var(--color-text-soft); font-weight: 500; }
.hero-typing {
  font-family: var(--font-display);
  font-size: clamp(1.2rem, 2.5vw, 1.6rem);
  color: var(--color-rose-gold-deep);
  min-height: 2.2rem;
}
.typing-cursor { animation: blink 1s step-start infinite; color: var(--color-rose-gold-deep); }
@keyframes blink { 50% { opacity: 0; } }
.hero-intro { max-width: 620px; color: var(--color-text-soft); font-size: 1rem; }

.scroll-indicator { margin-top: 1.5rem; }
.scroll-mouse {
  display: block;
  width: 26px;
  height: 42px;
  border: 2px solid var(--color-rose-gold-deep);
  border-radius: 14px;
  position: relative;
}
.scroll-mouse::before {
  content: '';
  position: absolute;
  top: 6px;
  left: 50%;
  width: 4px;
  height: 8px;
  background: var(--color-rose-gold-deep);
  border-radius: 2px;
  transform: translateX(-50%);
  animation: scrollDot 1.6s ease-in-out infinite;
}
@keyframes scrollDot { 0% { opacity: 1; top: 6px; } 100% { opacity: 0; top: 22px; } }
```

- [ ] **Step 5: Implement typing effect in `script.js`**

```js
function initTypingEffect() {
  const el = document.getElementById('typingText');
  if (!el) return;
  const words = ['CSE Student', 'Python Developer', 'Django Developer', 'Web Developer', 'Full-Stack Enthusiast'];
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (reduceMotion) { el.textContent = words[0]; return; }

  let wordIndex = 0, charIndex = 0, deleting = false;

  function tick() {
    const current = words[wordIndex];
    if (!deleting) {
      charIndex++;
      el.textContent = current.slice(0, charIndex);
      if (charIndex === current.length) {
        deleting = true;
        setTimeout(tick, 1400);
        return;
      }
    } else {
      charIndex--;
      el.textContent = current.slice(0, charIndex);
      if (charIndex === 0) {
        deleting = false;
        wordIndex = (wordIndex + 1) % words.length;
      }
    }
    setTimeout(tick, deleting ? 45 : 90);
  }
  tick();
}

document.addEventListener('DOMContentLoaded', () => {
  initTypingEffect();
});
```

- [ ] **Step 6: Remove the placeholder empty `DOMContentLoaded` listener from Task 1**

Replace the empty listener block from Task 1 Step 3 so there is exactly one `DOMContentLoaded` listener in `script.js` (the one added in Step 5 above).

- [ ] **Step 7: Verify**

Run: `node --check script.js`
Expected: no output (exit 0)

Run: `grep -o "Full-Stack Enthusiast" script.js`
Expected: prints `Full-Stack Enthusiast` (confirms word list present)

Open `index.html` directly in a browser and confirm: circular frame renders (broken-image icon behind the frame is expected until a photo is added), name/title/intro show, typed text cycles through the 5 words, blobs float slowly in the background.

- [ ] **Step 8: Commit**

```bash
git add index.html style.css script.js
git commit -m "Add hero section with typing effect and floating background blobs"
```

---

### Task 3: Floating nav, social stack, WhatsApp button

**Files:**
- Modify: `index.html` (add `<nav class="floating-nav">`, `<div class="social-stack">`, `<a class="whatsapp-btn">` as siblings before `<main>`)
- Modify: `style.css` (glass nav, social stack, WhatsApp button, mobile bottom-bar transform)
- Modify: `script.js` (scrollspy + smooth scroll)

**Interfaces:**
- Consumes: section ids `#home` (Task 2) and `#about`, `#skills`, `#experience`, `#education`, `#projects`, `#achievements`, `#languages`, `#contact` (Tasks 4-5 must use exactly these ids).
- Produces: function `initNavScrollSpy()` called from the single `DOMContentLoaded` handler; adds/removes `.active` class on `.floating-nav a` matching the section in view.

- [ ] **Step 1: Add floating nav markup to `index.html`** (before `<main>`)

```html
<nav class="floating-nav" aria-label="Section navigation">
  <ul>
    <li><a href="#home" class="nav-link active" data-section="home"><span class="dot"></span><span class="label">Home</span></a></li>
    <li><a href="#about" class="nav-link" data-section="about"><span class="dot"></span><span class="label">About</span></a></li>
    <li><a href="#skills" class="nav-link" data-section="skills"><span class="dot"></span><span class="label">Skills</span></a></li>
    <li><a href="#experience" class="nav-link" data-section="experience"><span class="dot"></span><span class="label">Experience</span></a></li>
    <li><a href="#education" class="nav-link" data-section="education"><span class="dot"></span><span class="label">Education</span></a></li>
    <li><a href="#projects" class="nav-link" data-section="projects"><span class="dot"></span><span class="label">Projects</span></a></li>
    <li><a href="#achievements" class="nav-link" data-section="achievements"><span class="dot"></span><span class="label">Achievements</span></a></li>
    <li><a href="#languages" class="nav-link" data-section="languages"><span class="dot"></span><span class="label">Languages</span></a></li>
    <li><a href="#contact" class="nav-link" data-section="contact"><span class="dot"></span><span class="label">Contact</span></a></li>
  </ul>
</nav>

<div class="social-stack">
  <a href="https://github.com/konikakobir28" target="_blank" rel="noopener" aria-label="GitHub" class="social-icon">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 .5C5.65.5.5 5.65.5 12c0 5.09 3.29 9.4 7.86 10.93.57.1.79-.25.79-.55 0-.27-.01-1.15-.02-2.09-3.2.7-3.88-1.36-3.88-1.36-.52-1.34-1.28-1.7-1.28-1.7-1.04-.72.08-.7.08-.7 1.15.08 1.76 1.19 1.76 1.19 1.03 1.76 2.69 1.25 3.34.96.1-.75.4-1.25.73-1.54-2.55-.29-5.24-1.28-5.24-5.68 0-1.26.45-2.28 1.19-3.09-.12-.29-.52-1.46.11-3.04 0 0 .97-.31 3.18 1.18a11 11 0 0 1 5.78 0c2.2-1.49 3.17-1.18 3.17-1.18.64 1.58.24 2.75.12 3.04.74.81 1.18 1.83 1.18 3.09 0 4.41-2.69 5.39-5.25 5.67.41.36.78 1.06.78 2.14 0 1.55-.01 2.79-.01 3.17 0 .3.21.66.8.55A10.51 10.51 0 0 0 23.5 12C23.5 5.65 18.35.5 12 .5Z"/></svg>
  </a>
  <a href="https://linkedin.com/in/konika-kobir-b3a982363" target="_blank" rel="noopener" aria-label="LinkedIn" class="social-icon">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M20.45 20.45h-3.56v-5.58c0-1.33-.02-3.04-1.85-3.04-1.86 0-2.14 1.45-2.14 2.94v5.68H9.34V9h3.42v1.56h.05c.48-.9 1.64-1.85 3.37-1.85 3.6 0 4.27 2.37 4.27 5.45v6.29ZM5.34 7.43a2.07 2.07 0 1 1 0-4.14 2.07 2.07 0 0 1 0 4.14ZM7.12 20.45H3.56V9h3.56v11.45Z"/></svg>
  </a>
  <!-- Placeholder: replace with real Facebook profile URL -->
  <a href="#" aria-label="Facebook" class="social-icon social-icon--placeholder">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M22 12a10 10 0 1 0-11.56 9.88v-6.99H7.9V12h2.54V9.8c0-2.5 1.49-3.89 3.77-3.89 1.09 0 2.23.2 2.23.2v2.45h-1.26c-1.24 0-1.63.77-1.63 1.56V12h2.77l-.44 2.89h-2.33v6.99A10 10 0 0 0 22 12Z"/></svg>
  </a>
  <!-- Placeholder: replace with real Instagram profile URL -->
  <a href="#" aria-label="Instagram" class="social-icon social-icon--placeholder">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 2.16c3.2 0 3.58.01 4.85.07 1.17.05 1.8.25 2.23.41.56.22.96.48 1.38.9.42.42.68.82.9 1.38.16.42.36 1.06.41 2.23.06 1.27.07 1.65.07 4.85s-.01 3.58-.07 4.85c-.05 1.17-.25 1.8-.41 2.23-.22.56-.48.96-.9 1.38-.42.42-.82.68-1.38.9-.42.16-1.06.36-2.23.41-1.27.06-1.65.07-4.85.07s-3.58-.01-4.85-.07c-1.17-.05-1.8-.25-2.23-.41a3.7 3.7 0 0 1-1.38-.9 3.7 3.7 0 0 1-.9-1.38c-.16-.42-.36-1.06-.41-2.23-.06-1.27-.07-1.65-.07-4.85s.01-3.58.07-4.85c.05-1.17.25-1.8.41-2.23.22-.56.48-.96.9-1.38.42-.42.82-.68 1.38-.9.42-.16 1.06-.36 2.23-.41 1.27-.06 1.65-.07 4.85-.07Zm0 1.8c-3.14 0-3.5.01-4.74.07-.96.04-1.48.2-1.83.34-.46.18-.79.39-1.13.74-.35.34-.56.67-.74 1.13-.14.35-.3.87-.34 1.83-.06 1.24-.07 1.6-.07 4.74s.01 3.5.07 4.74c.04.96.2 1.48.34 1.83.18.46.39.79.74 1.13.34.35.67.56 1.13.74.35.14.87.3 1.83.34 1.24.06 1.6.07 4.74.07s3.5-.01 4.74-.07c.96-.04 1.48-.2 1.83-.34.46-.18.79-.39 1.13-.74.35-.34.56-.67.74-1.13.14-.35.3-.87.34-1.83.06-1.24.07-1.6.07-4.74s-.01-3.5-.07-4.74c-.04-.96-.2-1.48-.34-1.83a2.96 2.96 0 0 0-.74-1.13 2.96 2.96 0 0 0-1.13-.74c-.35-.14-.87-.3-1.83-.34-1.24-.06-1.6-.07-4.74-.07Zm0 3.06a4.98 4.98 0 1 1 0 9.96 4.98 4.98 0 0 1 0-9.96Zm0 1.8a3.18 3.18 0 1 0 0 6.36 3.18 3.18 0 0 0 0-6.36Zm5.18-1.98a1.16 1.16 0 1 1-2.32 0 1.16 1.16 0 0 1 2.32 0Z"/></svg>
  </a>
</div>

<a href="https://wa.me/8801735490177" target="_blank" rel="noopener" class="whatsapp-btn" aria-label="Chat on WhatsApp">
  <svg viewBox="0 0 24 24" width="26" height="26" fill="currentColor"><path d="M17.47 14.38c-.29-.15-1.7-.84-1.96-.93-.26-.1-.46-.15-.65.14-.19.29-.75.93-.92 1.12-.17.19-.34.22-.63.07-.29-.15-1.22-.45-2.32-1.43-.86-.76-1.44-1.71-1.6-2-.17-.29-.02-.45.13-.6.13-.13.29-.34.43-.51.14-.17.19-.29.29-.48.1-.19.05-.36-.02-.51-.07-.15-.65-1.57-.9-2.15-.24-.57-.48-.5-.65-.5-.17-.01-.36-.01-.55-.01s-.51.07-.78.36c-.26.29-1.02 1-1.02 2.43s1.05 2.83 1.19 3.02c.14.19 2.06 3.15 5 4.42.7.3 1.24.48 1.67.62.7.22 1.34.19 1.84.12.56-.08 1.7-.7 1.94-1.37.24-.67.24-1.24.17-1.37-.07-.12-.26-.19-.55-.34Z"/><path d="M12 2.5A9.5 9.5 0 0 0 3.6 17l-1.1 4.5 4.6-1.2A9.5 9.5 0 1 0 12 2.5Zm0 1.8a7.7 7.7 0 0 1 6.44 11.93l-.2.31.65 2.65-2.7-.71-.3.18A7.7 7.7 0 1 1 12 4.3Z"/></svg>
</a>
```

- [ ] **Step 2: Style floating nav in `style.css`**

```css
.floating-nav {
  position: fixed;
  top: 50%;
  right: 1.75rem;
  transform: translateY(-50%);
  z-index: 50;
  background: var(--color-glass);
  border: 1px solid var(--color-glass-border);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  border-radius: var(--radius-lg);
  padding: 1rem 0.6rem;
  box-shadow: var(--shadow-soft);
}
.floating-nav ul { display: flex; flex-direction: column; gap: 0.9rem; }
.nav-link { display: flex; align-items: center; gap: 0.6rem; padding: 0.3rem 0.5rem; border-radius: var(--radius-sm); transition: background 0.25s ease; }
.nav-link .dot { width: 8px; height: 8px; border-radius: 50%; background: var(--color-rose-gold); flex-shrink: 0; transition: transform 0.25s ease, background 0.25s ease; }
.nav-link .label {
  max-width: 0; overflow: hidden; white-space: nowrap; font-size: 0.85rem; color: var(--color-plum);
  transition: max-width 0.3s ease, opacity 0.3s ease; opacity: 0;
}
.nav-link:hover .label, .nav-link.active .label { max-width: 140px; opacity: 1; }
.nav-link:hover, .nav-link.active { background: var(--color-blush-soft); }
.nav-link.active .dot { background: var(--color-rose-gold-deep); transform: scale(1.3); }

.social-stack {
  position: fixed;
  top: 1.5rem;
  right: 1.75rem;
  z-index: 50;
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  background: var(--color-glass);
  border: 1px solid var(--color-glass-border);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  border-radius: var(--radius-lg);
  padding: 0.7rem 0.55rem;
  box-shadow: var(--shadow-soft);
}
.social-icon { color: var(--color-rose-gold-deep); display: flex; align-items: center; justify-content: center; width: 32px; height: 32px; border-radius: 50%; transition: transform 0.25s ease, background 0.25s ease; }
.social-icon:hover { background: var(--color-blush-soft); transform: translateY(-2px) scale(1.08); }
.social-icon--placeholder { opacity: 0.85; }

.whatsapp-btn {
  position: fixed;
  bottom: 1.75rem;
  left: 1.75rem;
  z-index: 50;
  width: 56px; height: 56px;
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  background: linear-gradient(145deg, var(--color-rose-gold), var(--color-rose-gold-deep));
  color: #fff;
  box-shadow: var(--shadow-lift);
  transition: transform 0.25s ease;
}
.whatsapp-btn:hover { transform: scale(1.1); }

@media (max-width: 820px) {
  .floating-nav {
    top: auto; bottom: 1rem; left: 50%; right: auto; transform: translateX(-50%);
    padding: 0.5rem 0.7rem; border-radius: var(--radius-lg);
  }
  .floating-nav ul { flex-direction: row; gap: 0.4rem; }
  .nav-link { padding: 0.4rem; }
  .nav-link .label { display: none; }
  .social-stack { top: auto; bottom: 5.5rem; right: 1rem; flex-direction: row; }
  .whatsapp-btn { width: 50px; height: 50px; bottom: 5.5rem; }
}
```

- [ ] **Step 3: Implement scrollspy + smooth scroll in `script.js`**

```js
function initNavScrollSpy() {
  const links = document.querySelectorAll('.nav-link');
  const sections = Array.from(links)
    .map(link => document.getElementById(link.dataset.section))
    .filter(Boolean);
  if (!sections.length) return;

  const setActive = (id) => {
    links.forEach(l => l.classList.toggle('active', l.dataset.section === id));
  };

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) setActive(entry.target.id);
    });
  }, { rootMargin: '-45% 0px -45% 0px', threshold: 0 });

  sections.forEach(section => observer.observe(section));

  links.forEach(link => {
    link.addEventListener('click', (e) => {
      e.preventDefault();
      const target = document.getElementById(link.dataset.section);
      if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
    });
  });
}
```

- [ ] **Step 4: Wire into the single `DOMContentLoaded` handler**

Update the handler added in Task 2 Step 5 so it also calls the new function:

```js
document.addEventListener('DOMContentLoaded', () => {
  initTypingEffect();
  initNavScrollSpy();
});
```

- [ ] **Step 5: Verify**

Run: `node --check script.js`
Expected: no output (exit 0)

Run: `grep -c "wa.me/8801735490177" index.html`
Expected: `1`

Run: `grep -c "Placeholder: replace with real" index.html`
Expected: `2` (Facebook + Instagram)

Open in browser: confirm nav pill sits vertically centered on the right, social icons stack above it, WhatsApp button floats bottom-left; resize below ~820px and confirm nav becomes a bottom bar and social stack moves to avoid overlapping it.

- [ ] **Step 6: Commit**

```bash
git add index.html style.css script.js
git commit -m "Add floating glass nav, social icon stack, and WhatsApp button"
```

---

### Task 4: Content sections part 1 — About, Skills, Experience, Education

**Files:**
- Modify: `index.html` (append four `<section>` elements inside `<main>`, after the hero)
- Modify: `style.css` (styles for `.about`, `.skills`, `.timeline`)

**Interfaces:**
- Consumes: section id contract from Task 3 (`about`, `skills`, `experience`, `education`).
- Produces: `.fade-in` class convention (elements to be animated on scroll) applied to each section's inner card(s) — consumed by Task 6's IntersectionObserver.

- [ ] **Step 1: Add About + Skills markup**

```html
<section id="about" class="about">
  <h2 class="section-title">About Me</h2>
  <div class="glass-card fade-in">
    <p>
      Computer Science and Engineering graduate with foundational knowledge of programming,
      web technologies, and software development concepts. Skilled in communication,
      teamwork, presentation, and public interaction through university activities and
      customer-facing roles. Seeking an opportunity to gain practical experience and
      contribute positively to an organization.
    </p>
  </div>
</section>

<section id="skills" class="skills">
  <h2 class="section-title">Skills</h2>
  <div class="skills-group fade-in">
    <h3>Technical Skills</h3>
    <ul class="pill-list">
      <li class="pill">C</li><li class="pill">C++</li><li class="pill">Java</li>
      <li class="pill">Python</li><li class="pill">JavaScript</li>
      <li class="pill">HTML</li><li class="pill">CSS</li>
      <li class="pill">Microsoft Office</li><li class="pill">Basic Database Knowledge</li>
    </ul>
  </div>
  <div class="skills-group fade-in">
    <h3>Soft Skills</h3>
    <ul class="pill-list">
      <li class="pill">Communication &amp; Presentation</li>
      <li class="pill">Teamwork</li>
      <li class="pill">Event Coordination</li>
      <li class="pill">Customer Handling</li>
      <li class="pill">Time Management</li>
    </ul>
  </div>
</section>
```

- [ ] **Step 2: Add Experience + Education markup**

```html
<section id="experience" class="experience">
  <h2 class="section-title">Experience</h2>
  <div class="timeline">
    <div class="timeline-item fade-in">
      <span class="timeline-date">March 2024 &middot; 1 Month</span>
      <h3>Sales Executive</h3>
      <p class="timeline-org">Basundora Aarong</p>
      <p>Worked as a Sales Executive managing customer service, clothing arrangement, inventory handling, and store management during the Ramadan festival season.</p>
      <p class="timeline-learned"><strong>Learned:</strong> Customer handling, communication, teamwork, time management, and workplace responsibility.</p>
    </div>
    <div class="timeline-item fade-in">
      <span class="timeline-date">Mar 2023 &ndash; Aug 2023</span>
      <h3>Social Media Page Moderator</h3>
      <p class="timeline-org">Facebook Page &mdash; Make Glow</p>
      <p>Managed customer communication, responded to queries, and assisted in maintaining online page activities and engagement.</p>
      <p class="timeline-learned"><strong>Learned:</strong> Customer communication, online engagement management, problem-solving, and response handling.</p>
    </div>
  </div>
</section>

<section id="education" class="education">
  <h2 class="section-title">Education</h2>
  <div class="timeline">
    <div class="timeline-item fade-in">
      <span class="timeline-date">2023 &ndash; Present</span>
      <h3>B.Sc in Computer Science and Engineering (CSE)</h3>
      <p class="timeline-org">University of Asia Pacific</p>
      <ul class="detail-list">
        <li>Last Semester CGPA: 3.85</li>
        <li>Current Status: 4th Year 1st Semester</li>
        <li>Expected Graduation: 2027</li>
      </ul>
    </div>
    <div class="timeline-item fade-in">
      <span class="timeline-date">2019 &ndash; 2021</span>
      <h3>Higher Secondary School Certificate (HSC)</h3>
      <p class="timeline-org">Dhaka City College</p>
      <ul class="detail-list">
        <li>Subject: Science</li>
        <li>GPA: 5.00</li>
        <li>Board: Dhaka</li>
      </ul>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Style these sections in `style.css`**

```css
.section-title { font-size: clamp(1.8rem, 3.5vw, 2.4rem); text-align: center; margin-bottom: 2.5rem; position: relative; }
.section-title::after {
  content: ''; display: block; width: 64px; height: 3px; margin: 0.6rem auto 0;
  background: linear-gradient(90deg, var(--color-blush), var(--color-rose-gold-deep)); border-radius: 2px;
}

.glass-card {
  background: var(--color-glass);
  border: 1px solid var(--color-glass-border);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: var(--radius-lg);
  padding: 2rem;
  box-shadow: var(--shadow-soft);
}
.about .glass-card { max-width: 760px; margin: 0 auto; text-align: center; color: var(--color-text-soft); }

.skills { display: flex; flex-direction: column; gap: 2rem; }
.skills-group h3 { margin-bottom: 1rem; font-size: 1.1rem; }
.pill-list { display: flex; flex-wrap: wrap; gap: 0.7rem; }
.pill {
  background: var(--color-bg-alt); border: 1px solid var(--color-blush);
  color: var(--color-plum); padding: 0.5rem 1.1rem; border-radius: 999px;
  font-size: 0.9rem; box-shadow: var(--shadow-soft); transition: transform 0.2s ease, background 0.2s ease;
}
.pill:hover { transform: translateY(-3px); background: var(--color-blush-soft); }

.timeline { display: flex; flex-direction: column; gap: 1.5rem; }
.timeline-item {
  background: var(--color-bg-alt); border-left: 3px solid var(--color-rose-gold);
  border-radius: var(--radius-md); padding: 1.5rem 1.75rem; box-shadow: var(--shadow-soft);
}
.timeline-date { font-size: 0.8rem; color: var(--color-rose-gold-deep); font-weight: 600; letter-spacing: 0.03em; text-transform: uppercase; }
.timeline-item h3 { margin-top: 0.3rem; font-size: 1.25rem; }
.timeline-org { color: var(--color-text-soft); font-style: italic; margin-bottom: 0.6rem; }
.timeline-learned { margin-top: 0.6rem; font-size: 0.92rem; color: var(--color-text-soft); }
.detail-list li { color: var(--color-text-soft); margin-top: 0.3rem; }
.detail-list li::before { content: '\2022  '; color: var(--color-rose-gold-deep); }

@media (min-width: 700px) {
  .skills { flex-direction: row; justify-content: space-between; gap: 3rem; }
}
```

- [ ] **Step 4: Verify**

Run: `grep -c "CGPA: 3.85" index.html`
Expected: `1`

Run: `grep -c "Make Glow" index.html`
Expected: `1`

Open in browser: About, Skills, Experience, Education sections render below the hero with card/timeline styling, no invented text present (spot-check against the CV).

- [ ] **Step 5: Commit**

```bash
git add index.html style.css
git commit -m "Add About, Skills, Experience, and Education sections"
```

---

### Task 5: Content sections part 2 — Projects, Achievements, Languages, Contact

**Files:**
- Modify: `index.html` (append four `<section>` elements inside `<main>`, plus a `<footer>`)
- Modify: `style.css` (styles for `.projects`, `.achievements`, `.languages`, `.contact`, `footer`)

**Interfaces:**
- Consumes: section id contract from Task 3 (`projects`, `achievements`, `languages`, `contact`); `.fade-in` convention from Task 4.

- [ ] **Step 1: Add Projects markup**

```html
<section id="projects" class="projects">
  <h2 class="section-title">Projects</h2>
  <div class="project-grid">
    <article class="project-card fade-in">
      <h3>Shonchoy</h3>
      <p class="project-sub">Digital Banking Platform</p>
      <p>Developed a full-stack cooperative banking system with secure authentication, user account management, money transactions, savings tracking, and loan management features to ensure smooth and secure banking operations.</p>
      <ul class="tag-list">
        <li>Python</li><li>Django</li><li>HTML</li><li>CSS</li><li>Bootstrap</li><li>JavaScript</li><li>SQLite/MySQL</li>
      </ul>
    </article>
    <article class="project-card fade-in">
      <h3>MindScope</h3>
      <p class="project-sub">Mental Health Screening Platform</p>
      <p>Created an interactive mental health screening and assessment platform that helps users evaluate mental health indicators through a simple, user-friendly, and responsive interface.</p>
      <ul class="tag-list">
        <li>Python</li><li>Django</li><li>HTML</li><li>CSS</li><li>Bootstrap</li><li>JavaScript</li>
      </ul>
    </article>
    <article class="project-card fade-in">
      <h3>Scrappy</h3>
      <p class="project-sub">Waste Management System</p>
      <p>Built a digital waste management platform focused on efficient waste collection, reporting, and sustainable community management to improve environmental awareness and usability.</p>
      <ul class="tag-list">
        <li>Python</li><li>Django</li><li>HTML</li><li>CSS</li><li>Bootstrap</li><li>JavaScript</li><li>DBMS</li>
      </ul>
    </article>
  </div>
</section>
```

- [ ] **Step 2: Add Achievements + Languages markup**

```html
<section id="achievements" class="achievements">
  <h2 class="section-title">Achievements &amp; Activities</h2>
  <ul class="badge-list fade-in">
    <li class="badge">Dean's Award Recipient (2025)</li>
    <li class="badge">Dean's Award Recipient (2026)</li>
    <li class="badge">Best Performer Award in University Cultural Program</li>
  </ul>
  <h3 class="sub-heading fade-in">Clubs &amp; Participation</h3>
  <ul class="detail-list fade-in">
    <li>Member of University Cultural Club</li>
    <li>Participated in cultural and academic events</li>
    <li>Assisted in event coordination and team activities</li>
    <li>Participated in presentations and group projects</li>
  </ul>
</section>

<section id="languages" class="languages">
  <h2 class="section-title">Languages</h2>
  <div class="language-grid">
    <div class="language-card fade-in">
      <h3>Bangla</h3>
      <p class="lang-level">Native</p>
      <p class="lang-detail">Writing, Speaking &amp; Listening</p>
    </div>
    <div class="language-card fade-in">
      <h3>English</h3>
      <p class="lang-level">Professional</p>
      <p class="lang-detail">Writing, Speaking &amp; Listening</p>
    </div>
    <div class="language-card fade-in">
      <h3>Hindi</h3>
      <p class="lang-level">Basic</p>
      <p class="lang-detail">Speaking &amp; Listening</p>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Add Contact section + footer**

```html
<section id="contact" class="contact">
  <h2 class="section-title">Contact</h2>
  <div class="contact-grid fade-in">
    <a href="tel:+8801735490177" class="contact-item">
      <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M6.6 10.8c1.4 2.8 3.8 5.1 6.6 6.6l2.2-2.2c.3-.3.7-.4 1-.2 1.1.4 2.3.6 3.6.6.6 0 1 .4 1 1V20c0 .6-.4 1-1 1C10.4 21 3 13.6 3 4.6c0-.6.4-1 1-1h3.4c.6 0 1 .4 1 1 0 1.3.2 2.5.6 3.6.1.3 0 .7-.2 1L6.6 10.8Z"/></svg>
      +880 1735 490177
    </a>
    <a href="mailto:konikakobir121@gmail.com" class="contact-item">
      <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M2 5.5A1.5 1.5 0 0 1 3.5 4h17A1.5 1.5 0 0 1 22 5.5v13a1.5 1.5 0 0 1-1.5 1.5h-17A1.5 1.5 0 0 1 2 18.5v-13Zm2.2.5 7.8 6.2L19.8 6H4.2Z"/></svg>
      konikakobir121@gmail.com
    </a>
    <span class="contact-item">
      <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 2a7 7 0 0 0-7 7c0 5.25 7 13 7 13s7-7.75 7-13a7 7 0 0 0-7-7Zm0 9.5A2.5 2.5 0 1 1 12 6.5a2.5 2.5 0 0 1 0 5Z"/></svg>
      Mogbazar, Dhaka
    </span>
    <a href="https://github.com/konikakobir28" target="_blank" rel="noopener" class="contact-item">
      <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M12 .5C5.65.5.5 5.65.5 12c0 5.09 3.29 9.4 7.86 10.93.57.1.79-.25.79-.55 0-.27-.01-1.15-.02-2.09-3.2.7-3.88-1.36-3.88-1.36-.52-1.34-1.28-1.7-1.28-1.7-1.04-.72.08-.7.08-.7 1.15.08 1.76 1.19 1.76 1.19 1.03 1.76 2.69 1.25 3.34.96.1-.75.4-1.25.73-1.54-2.55-.29-5.24-1.28-5.24-5.68 0-1.26.45-2.28 1.19-3.09-.12-.29-.52-1.46.11-3.04 0 0 .97-.31 3.18 1.18a11 11 0 0 1 5.78 0c2.2-1.49 3.17-1.18 3.17-1.18.64 1.58.24 2.75.12 3.04.74.81 1.18 1.83 1.18 3.09 0 4.41-2.69 5.39-5.25 5.67.41.36.78 1.06.78 2.14 0 1.55-.01 2.79-.01 3.17 0 .3.21.66.8.55A10.51 10.51 0 0 0 23.5 12C23.5 5.65 18.35.5 12 .5Z"/></svg>
      konikakobir28
    </a>
    <a href="https://linkedin.com/in/konika-kobir-b3a982363" target="_blank" rel="noopener" class="contact-item">
      <svg viewBox="0 0 24 24" width="20" height="20" fill="currentColor"><path d="M20.45 20.45h-3.56v-5.58c0-1.33-.02-3.04-1.85-3.04-1.86 0-2.14 1.45-2.14 2.94v5.68H9.34V9h3.42v1.56h.05c.48-.9 1.64-1.85 3.37-1.85 3.6 0 4.27 2.37 4.27 5.45v6.29ZM5.34 7.43a2.07 2.07 0 1 1 0-4.14 2.07 2.07 0 0 1 0 4.14ZM7.12 20.45H3.56V9h3.56v11.45Z"/></svg>
      konika-kobir-b3a982363
    </a>
  </div>
</section>

<footer class="site-footer">
  <p>&copy; 2026 Kamrun Nahar Konika. Built with care.</p>
</footer>
```

- [ ] **Step 4: Style these sections in `style.css`**

```css
.project-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 1.75rem; }
.project-card {
  background: var(--color-bg-alt); border-radius: var(--radius-lg); padding: 1.75rem;
  box-shadow: var(--shadow-soft); transition: transform 0.25s ease, box-shadow 0.25s ease;
}
.project-card:hover { transform: translateY(-6px); box-shadow: var(--shadow-lift); }
.project-card h3 { font-size: 1.3rem; }
.project-sub { color: var(--color-rose-gold-deep); font-weight: 600; font-size: 0.85rem; margin-bottom: 0.6rem; text-transform: uppercase; letter-spacing: 0.03em; }
.tag-list { display: flex; flex-wrap: wrap; gap: 0.4rem; margin-top: 1rem; }
.tag-list li { background: var(--color-blush-soft); color: var(--color-plum); font-size: 0.78rem; padding: 0.3rem 0.7rem; border-radius: 999px; }

.badge-list { display: flex; flex-wrap: wrap; gap: 1rem; justify-content: center; margin-bottom: 2rem; }
.badge {
  background: linear-gradient(135deg, var(--color-blush), var(--color-rose-gold));
  color: #fff; padding: 0.7rem 1.3rem; border-radius: var(--radius-md); box-shadow: var(--shadow-soft); font-weight: 500; text-align: center;
}
.sub-heading { text-align: center; margin-bottom: 1rem; font-size: 1.2rem; }
.achievements .detail-list { max-width: 500px; margin: 0 auto; }

.language-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 1.5rem; }
.language-card { background: var(--color-bg-alt); border-radius: var(--radius-md); padding: 1.5rem; text-align: center; box-shadow: var(--shadow-soft); }
.lang-level { color: var(--color-rose-gold-deep); font-weight: 600; margin: 0.3rem 0; }
.lang-detail { color: var(--color-text-soft); font-size: 0.85rem; }

.contact-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1.25rem; }
.contact-item {
  display: flex; align-items: center; gap: 0.7rem; background: var(--color-bg-alt);
  border-radius: var(--radius-md); padding: 1rem 1.25rem; box-shadow: var(--shadow-soft);
  color: var(--color-plum); transition: transform 0.2s ease;
}
.contact-item svg { color: var(--color-rose-gold-deep); flex-shrink: 0; }
a.contact-item:hover { transform: translateY(-3px); }

.site-footer { text-align: center; padding: 2.5rem 1.5rem 6.5rem; color: var(--color-text-soft); font-size: 0.85rem; }
```

- [ ] **Step 5: Verify**

Run: `grep -c "Shonchoy" index.html`
Expected: `1`

Run: `grep -c "Dean's Award Recipient (2026)" index.html`
Expected: `1`

Run: `grep -c "konikakobir121@gmail.com" index.html`
Expected: `2` (visible text + mailto href)

Open in browser: Projects grid (3 cards), Achievements badges + club list, Languages cards (3), Contact grid with working `tel:`/`mailto:`/GitHub/LinkedIn links, footer at the bottom.

- [ ] **Step 6: Commit**

```bash
git add index.html style.css
git commit -m "Add Projects, Achievements, Languages, Contact sections and footer"
```

---

### Task 6: Scroll-in animations, reduced motion, responsive QA pass

**Files:**
- Modify: `script.js` (IntersectionObserver fade-in for `.fade-in` elements)
- Modify: `style.css` (base `.fade-in` transition state, final responsive tightening pass)

**Interfaces:**
- Consumes: `.fade-in` elements produced by Tasks 4 and 5.
- Produces: `initScrollFadeIns()`, called from the same `DOMContentLoaded` handler as the final addition.

- [ ] **Step 1: Add fade-in base styles to `style.css`**

```css
.fade-in { opacity: 0; transform: translateY(24px); transition: opacity 0.7s ease, transform 0.7s ease; }
.fade-in.is-visible { opacity: 1; transform: translateY(0); }
```

- [ ] **Step 2: Implement the fade-in observer in `script.js`**

```js
function initScrollFadeIns() {
  const items = document.querySelectorAll('.fade-in');
  if (!items.length) return;
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (reduceMotion) { items.forEach(el => el.classList.add('is-visible')); return; }

  const observer = new IntersectionObserver((entries, obs) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
        obs.unobserve(entry.target);
      }
    });
  }, { threshold: 0.15 });

  items.forEach(el => observer.observe(el));
}
```

- [ ] **Step 3: Wire into the `DOMContentLoaded` handler (final version)**

```js
document.addEventListener('DOMContentLoaded', () => {
  initTypingEffect();
  initNavScrollSpy();
  initScrollFadeIns();
});
```

- [ ] **Step 4: Responsive tightening pass in `style.css`**

Add a final mobile breakpoint block to fix small-screen spacing/overflow (append to end of `style.css`):

```css
@media (max-width: 480px) {
  section { padding: 4rem 1.25rem; }
  .hero { padding-top: 3rem; gap: 0.9rem; }
  .hero-intro { font-size: 0.92rem; }
  .glass-card { padding: 1.5rem; }
  .timeline-item { padding: 1.25rem 1.35rem; }
  .badge-list { gap: 0.7rem; }
  .whatsapp-btn { left: 1rem; bottom: 1rem; }
}

* { max-width: 100%; }
html, body { max-width: 100vw; }
```

- [ ] **Step 5: Verify — automated checks**

Run: `node --check script.js`
Expected: no output (exit 0)

Run: `grep -c "fade-in" index.html`
Expected: a number ≥ 10 (confirms every content section applied the convention)

Run: `grep -c "prefers-reduced-motion" style.css script.js`
Expected: `style.css:2` and `script.js:2` (both files handle reduced motion — Task 1's global rule + Task 6's observer + Task 2's typing effect)

- [ ] **Step 6: Verify — manual browser QA**

Open `index.html` in a browser at three widths (375px, 768px, 1440px) using dev tools device toolbar and confirm for each:
- No horizontal scrollbar appears at any width.
- Floating nav is a right-side vertical pill on desktop/tablet and a bottom bar on narrow screens; active dot updates as you scroll through each section.
- Social icon stack and WhatsApp button never overlap the nav or each other.
- Sections fade/slide in the first time they enter the viewport (and stay visible after, don't re-trigger).
- Typing text in the hero cycles and loops without layout shift.
- All contact links (`tel:`, `mailto:`, GitHub, LinkedIn) are clickable and point to the correct destination; WhatsApp button opens `https://wa.me/8801735490177`.

- [ ] **Step 7: Commit**

```bash
git add index.html style.css script.js
git commit -m "Add scroll-triggered fade-ins, reduced-motion support, and mobile QA fixes"
```

---

## Self-Review Notes

- **Spec coverage:** All 9 sections (Hero, About, Skills, Experience, Education, Projects, Achievements, Languages, Contact) covered across Tasks 2, 4, 5. Floating nav/social/WhatsApp covered in Task 3. Background blobs + typing animation in Task 2. Fade-in animations + reduced motion + responsive QA in Task 6. Design tokens/reset in Task 1.
- **Placeholder scan:** No TBD/TODO; Facebook/Instagram intentionally use `#` with explanatory HTML comments per approved spec, not left ambiguous.
- **Type/id consistency:** Nav `data-section` values (`home`, `about`, `skills`, `experience`, `education`, `projects`, `achievements`, `languages`, `contact`) match the `id` attributes used on every `<section>` across Tasks 2, 4, 5 — verified by cross-reading each task's markup.
- **Function naming consistency:** `initTypingEffect`, `initNavScrollSpy`, `initScrollFadeIns` — each defined once (Tasks 2/3/6) and called exactly once from the single final `DOMContentLoaded` handler (Task 6, Step 3), which explicitly supersedes the two earlier partial versions of that handler.
