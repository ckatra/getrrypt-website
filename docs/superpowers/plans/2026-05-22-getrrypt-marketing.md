# GetRypt Marketing Site — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a single-page static marketing site for GetRypt (getrypt.com), Billy Katranis's personal training brand.

**Architecture:** Plain HTML5 + CSS3, no build step, no npm. One `index.html` and one `style.css` drive the whole site. Billy's real training photos serve as hero and about imagery. All CTAs link to Instagram (`@Billyk.sw`).

**Tech Stack:** HTML5, CSS3, Google Fonts (Bebas Neue + Inter), Python http.server for local dev, Vercel CLI for deployment.

---

### Task 1: Project scaffold

**Files:**
- Create: `index.html`
- Create: `style.css`
- Create: `favicon.svg`
- Create: `.vercelignore`

- [ ] **Step 1: Create `index.html` shell**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>GetRypt — Personal Training by Billy Katranis</title>
  <meta name="description" content="Calisthenics, strength, and nutrition coaching by Billy Katranis. Personalized training in Montreal and online. Book via Instagram @Billyk.sw." />
  <link rel="canonical" href="https://getrypt.com" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://getrypt.com" />
  <meta property="og:title" content="GetRypt — Personal Training by Billy Katranis" />
  <meta property="og:description" content="Calisthenics, strength, and nutrition coaching. Personalized to you." />
  <meta property="og:image" content="https://getrypt.com/images/hero.jpg" />

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600&display=swap" rel="stylesheet" />

  <!-- Preload hero image -->
  <link rel="preload" as="image" href="images/hero.jpg" />

  <!-- Favicon -->
  <link rel="icon" href="favicon.svg" type="image/svg+xml" />

  <link rel="stylesheet" href="style.css" />
</head>
<body>

  <header id="site-header">
    <div class="container header-inner">
      <a href="#" class="wordmark" aria-label="GetRypt home">GETRYPT</a>
      <a href="https://www.instagram.com/Billyk.sw" target="_blank" rel="noopener noreferrer" class="btn btn-primary" aria-label="Train with Billy on Instagram">Train with Billy</a>
    </div>
  </header>

  <main>
    <!-- sections added in subsequent tasks -->
  </main>

  <footer>
    <div class="container footer-inner">
      <p class="footer-copy">© <span class="year"></span> GetRypt · Billy Katranis</p>
    </div>
  </footer>

  <script>
    (function () {
      document.querySelectorAll('.year').forEach(function (el) {
        el.textContent = new Date().getFullYear();
      });
    })();
  </script>

</body>
</html>
```

- [ ] **Step 2: Create `style.css` with design tokens and base reset**

```css
:root {
  --bg: #0D0F14;
  --accent: #00D4FF;
  --text: #FFFFFF;
  --text-muted: rgba(255, 255, 255, 0.55);
  --card-bg: #161A22;
  --font-head: 'Bebas Neue', system-ui, sans-serif;
  --font-body: 'Inter', system-ui, sans-serif;
  --max-w: 960px;
  --radius: 10px;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  font-size: 1rem;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

img { display: block; max-width: 100%; height: auto; }

a { color: inherit; text-decoration: none; }

.container {
  width: 100%;
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 0 1.5rem;
}

.btn {
  display: inline-block;
  padding: 0.75rem 1.75rem;
  border-radius: var(--radius);
  font-family: var(--font-body);
  font-weight: 600;
  font-size: 0.95rem;
  letter-spacing: 0.03em;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.15s;
}
.btn:hover { opacity: 0.85; transform: translateY(-1px); }
.btn:active { transform: translateY(0); }

.btn-primary {
  background: var(--accent);
  color: #000;
}

.btn-outline {
  background: transparent;
  color: var(--accent);
  border: 2px solid var(--accent);
}
```

- [ ] **Step 3: Create `favicon.svg`**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="6" fill="#0D0F14"/>
  <text x="16" y="23" font-family="system-ui, sans-serif" font-size="18" font-weight="900" fill="#00D4FF" text-anchor="middle">GR</text>
</svg>
```

- [ ] **Step 4: Create `.vercelignore`**

```
docs/
.git/
*.md
```

- [ ] **Step 5: Start local dev server and verify**

In PowerShell from the GetRypt folder:
```powershell
python -m http.server 8080
```
Open http://localhost:8080 — dark blank page with no console errors. Fonts may not load from localhost (expected).

- [ ] **Step 6: Commit**

```bash
git add index.html style.css favicon.svg .vercelignore
git commit -m "feat: project scaffold — HTML shell, CSS tokens, favicon"
```

---

### Task 2: Sticky header

**Files:**
- Modify: `style.css` — add header styles

- [ ] **Step 1: Add header CSS to `style.css`** (append after `.btn-outline`)

```css
/* ─── Header ─────────────────────────────────── */
#site-header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(13, 15, 20, 0.92);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
}

.wordmark {
  font-family: var(--font-head);
  font-size: 1.6rem;
  color: var(--accent);
  letter-spacing: 0.04em;
}
```

- [ ] **Step 2: Verify in browser**

Reload http://localhost:8080. Dark translucent header with "GETRYPT" in cyan left, cyan "Train with Billy" button right. Scroll past the fold — header stays fixed.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: sticky header with wordmark and CTA"
```

---

### Task 3: Hero section

**Files:**
- Modify: `index.html` — add hero section inside `<main>`
- Modify: `style.css` — add hero styles

- [ ] **Step 1: Replace `<!-- sections added in subsequent tasks -->` in `index.html` with**

```html
    <section id="hero">
      <div class="hero-bg" aria-hidden="true">
        <img src="images/hero.jpg" alt="" />
      </div>
      <div class="hero-overlay" aria-hidden="true"></div>
      <div class="container hero-content">
        <h1 class="hero-headline">Train different.</h1>
        <p class="hero-sub">Calisthenics · Strength · Nutrition — personalized to you</p>
        <a href="https://www.instagram.com/Billyk.sw" target="_blank" rel="noopener noreferrer" class="btn btn-primary hero-cta">Start training →</a>
      </div>
    </section>

    <!-- about, programs, testimonials, contact added below -->
```

- [ ] **Step 2: Add hero CSS to `style.css`** (append after header styles)

```css
/* ─── Hero ───────────────────────────────────── */
#hero {
  position: relative;
  height: 100svh;
  min-height: 540px;
  display: flex;
  align-items: center;
}

.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.hero-bg img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  filter: brightness(0.75);
}

.hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    rgba(13, 15, 20, 0.25) 0%,
    rgba(13, 15, 20, 0.5) 60%,
    rgba(13, 15, 20, 1) 100%
  );
}

.hero-content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.hero-headline {
  font-family: var(--font-head);
  font-size: clamp(4rem, 12vw, 9rem);
  line-height: 1;
  color: var(--text);
  letter-spacing: 0.01em;
}

.hero-sub {
  font-size: clamp(0.9rem, 2vw, 1.1rem);
  color: var(--text-muted);
  letter-spacing: 0.04em;
}

.hero-cta {
  align-self: flex-start;
  margin-top: 0.5rem;
}
```

- [ ] **Step 3: Verify in browser**

Reload. Full-viewport dark hero. No image yet (no `images/hero.jpg`) — expected. "Train different." renders large in white. Sub-line is muted. Cyan "Start training →" button bottom-left.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: hero section — full-viewport layout, headline, CTA"
```

---

### Task 4: About Billy section

**Files:**
- Modify: `index.html` — add about section
- Modify: `style.css` — add about styles

- [ ] **Step 1: Add about HTML in `index.html`** after the hero `</section>` (before the comment)

```html
    <section id="about" class="section-pad reveal">
      <div class="container about-inner">
        <div class="about-photo">
          <img src="images/about.jpg" alt="Billy Katranis — personal trainer" loading="lazy" />
        </div>
        <div class="about-text">
          <h2 class="section-heading">Billy Katranis</h2>
          <p class="about-body">Billy has spent years mastering the art of bodyweight training. Rooted in calisthenics and sharpened by strength work, his approach strips fitness back to what matters — movement, discipline, and results.</p>
          <p class="about-body">Whether you're starting from zero or pushing past a plateau, Billy builds programs around you. No templates. No guesswork.</p>
          <p class="about-location">Based in Montreal. Training everywhere.</p>
          <a href="https://www.instagram.com/Billyk.sw" target="_blank" rel="noopener noreferrer" class="btn btn-outline">Follow @Billyk.sw</a>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Add about CSS to `style.css`** (append after hero styles)

```css
/* ─── Shared section spacing ─────────────────── */
.section-pad { padding: 6rem 0; }

.section-heading {
  font-family: var(--font-head);
  font-size: clamp(2.2rem, 6vw, 3.5rem);
  color: var(--text);
  letter-spacing: 0.02em;
  margin-bottom: 1.5rem;
}

/* ─── About ──────────────────────────────────── */
.about-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}

.about-photo img {
  width: 100%;
  height: 520px;
  object-fit: cover;
  object-position: center top;
  border-radius: var(--radius);
  filter: brightness(0.85);
}

.about-text {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.about-body {
  color: var(--text-muted);
  font-size: 1.05rem;
  line-height: 1.75;
}

.about-location {
  font-weight: 600;
  color: var(--accent);
  font-size: 0.95rem;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}
```

- [ ] **Step 3: Verify in browser**

Reload. Below hero: two-column section. Left = image placeholder, right = "Billy Katranis" in large Bebas Neue, two paragraphs, cyan location line, outline button.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: about Billy section — two-column layout"
```

---

### Task 5: Programs section

**Files:**
- Modify: `index.html` — add programs section
- Modify: `style.css` — add programs styles

- [ ] **Step 1: Add programs HTML in `index.html`** after the about `</section>`

```html
    <section id="programs" class="section-pad reveal">
      <div class="container">
        <h2 class="section-heading">What we offer</h2>
        <div class="programs-grid">

          <div class="program-card">
            <div class="program-icon" aria-hidden="true">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
                <circle cx="12" cy="7" r="4"/>
              </svg>
            </div>
            <h3 class="program-title">1-on-1 In-Person</h3>
            <p class="program-desc">Private sessions tailored to your goals. Train directly with Billy.</p>
          </div>

          <div class="program-card">
            <div class="program-icon" aria-hidden="true">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="3" width="20" height="14" rx="2"/>
                <path d="M8 21h8M12 17v4"/>
              </svg>
            </div>
            <h3 class="program-title">Online Coaching</h3>
            <p class="program-desc">Custom programs and weekly check-ins, wherever you are.</p>
          </div>

          <div class="program-card">
            <div class="program-icon" aria-hidden="true">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M12 2a3 3 0 0 0-3 3v1H6a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8a2 2 0 0 0-2-2h-3V5a3 3 0 0 0-3-3z"/>
                <path d="M9 12h6M9 16h4"/>
              </svg>
            </div>
            <h3 class="program-title">Nutrition Coaching</h3>
            <p class="program-desc">Practical plans built around your lifestyle. No gimmicks.</p>
          </div>

          <div class="program-card">
            <div class="program-icon" aria-hidden="true">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
                <circle cx="9" cy="7" r="4"/>
                <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
                <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
              </svg>
            </div>
            <h3 class="program-title">Group Training</h3>
            <p class="program-desc">Train with others. Same intensity, different energy.</p>
          </div>

        </div>
      </div>
    </section>
```

- [ ] **Step 2: Add programs CSS to `style.css`** (append after about styles)

```css
/* ─── Programs ───────────────────────────────── */
.programs-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  margin-top: 2.5rem;
}

.program-card {
  background: var(--card-bg);
  border-top: 3px solid var(--accent);
  border-radius: var(--radius);
  padding: 2rem 1.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.program-icon {
  color: var(--accent);
  width: 28px;
  height: 28px;
}

.program-title {
  font-family: var(--font-head);
  font-size: 1.5rem;
  color: var(--text);
  letter-spacing: 0.03em;
}

.program-desc {
  color: var(--text-muted);
  font-size: 0.95rem;
  line-height: 1.65;
}
```

- [ ] **Step 3: Verify in browser**

Reload. Below about: "What we offer" heading, 2×2 grid of dark cards each with a cyan icon, bold Bebas Neue title, and muted description. Each card has a cyan top border.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: programs section — 2x2 card grid"
```

---

### Task 6: Testimonials section

**Files:**
- Modify: `index.html` — add testimonials section
- Modify: `style.css` — add testimonials styles

- [ ] **Step 1: Add testimonials HTML in `index.html`** after programs `</section>`

> Replace these three quotes with real client feedback from Billy before deploying.

```html
    <section id="testimonials" class="section-pad reveal">
      <div class="container">
        <h2 class="section-heading">Results speak</h2>
        <div class="testimonials-row">

          <div class="testimonial-card">
            <p class="testimonial-quote">"Billy completely changed how I think about training. Three months in, I went from struggling with push-ups to doing muscle-ups."</p>
            <p class="testimonial-name">— Alex M.</p>
          </div>

          <div class="testimonial-card">
            <p class="testimonial-quote">"The nutrition coaching was worth every session on its own. Simple, practical, no BS."</p>
            <p class="testimonial-name">— Sarah T.</p>
          </div>

          <div class="testimonial-card">
            <p class="testimonial-quote">"I've worked with other coaches before, but Billy actually explains the why behind every movement."</p>
            <p class="testimonial-name">— Marc D.</p>
          </div>

        </div>
      </div>
    </section>
```

- [ ] **Step 2: Add testimonials CSS to `style.css`** (append after programs styles)

```css
/* ─── Testimonials ───────────────────────────── */
.testimonials-row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  margin-top: 2.5rem;
}

.testimonial-card {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 2rem 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.testimonial-quote {
  color: var(--text-muted);
  font-size: 0.95rem;
  line-height: 1.75;
  font-style: italic;
  flex: 1;
}

.testimonial-name {
  font-weight: 600;
  color: var(--accent);
  font-size: 0.875rem;
  letter-spacing: 0.03em;
}
```

- [ ] **Step 3: Verify in browser**

Reload. Below programs: "Results speak" heading, 3-column row of dark quote cards with italic muted quotes and cyan names.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: testimonials section — 3-card quote grid"
```

---

### Task 7: Contact / Instagram CTA section

**Files:**
- Modify: `index.html` — add contact section
- Modify: `style.css` — add contact styles

- [ ] **Step 1: Add contact HTML in `index.html`** after testimonials `</section>`

```html
    <section id="contact" class="reveal">
      <div class="contact-band">
        <div class="container contact-inner">
          <h2 class="contact-heading">Ready to start?</h2>
          <p class="contact-sub">Reach out on Instagram — Billy responds personally.</p>
          <a href="https://www.instagram.com/Billyk.sw" target="_blank" rel="noopener noreferrer" class="btn btn-primary contact-btn" aria-label="Message Billy on Instagram @Billyk.sw">
            <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true" style="display:inline;vertical-align:middle;margin-right:0.5rem;">
              <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
            </svg>
            @Billyk.sw
          </a>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Add contact CSS to `style.css`** (append after testimonials styles)

```css
/* ─── Contact CTA ────────────────────────────── */
.contact-band {
  background: var(--card-bg);
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.contact-inner {
  padding: 6rem 1.5rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
}

.contact-heading {
  font-family: var(--font-head);
  font-size: clamp(2.5rem, 7vw, 4.5rem);
  letter-spacing: 0.02em;
}

.contact-sub {
  color: var(--text-muted);
  font-size: 1.05rem;
}

.contact-btn {
  font-size: 1.05rem;
  padding: 1rem 2.25rem;
  margin-top: 0.5rem;
}
```

- [ ] **Step 3: Verify in browser**

Reload. Below testimonials: dark-card full-width band with "Ready to start?" in large Bebas Neue, muted sub-line, cyan button with Instagram icon and "@Billyk.sw".

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: contact/Instagram CTA section"
```

---

### Task 8: Footer

**Files:**
- Modify: `style.css` — add footer styles (HTML already in scaffold)

- [ ] **Step 1: Add footer CSS to `style.css`** (append after contact styles)

```css
/* ─── Footer ─────────────────────────────────── */
footer {
  padding: 2.5rem 0;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
}

.footer-inner {
  display: flex;
  justify-content: center;
}

.footer-copy {
  color: var(--text-muted);
  font-size: 0.875rem;
}
```

- [ ] **Step 2: Verify in browser**

Reload. Below contact: minimal dark footer with "© 2026 GetRypt · Billy Katranis" centered in muted text.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: footer styles"
```

---

### Task 9: Scroll animations

**Files:**
- Modify: `style.css` — add animation keyframes and reveal states
- Modify: `index.html` — add IntersectionObserver to script block

- [ ] **Step 1: Add reveal animation CSS to `style.css`** (append at the very end)

```css
/* ─── Scroll reveal ──────────────────────────── */
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(28px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  opacity: 0;
}

.reveal.visible {
  animation: fadeInUp 0.6s ease forwards;
}
```

- [ ] **Step 2: Replace the `<script>` block in `index.html`** with

```html
  <script>
    (function () {
      document.querySelectorAll('.year').forEach(function (el) {
        el.textContent = new Date().getFullYear();
      });

      var observer = new IntersectionObserver(function (entries) {
        entries.forEach(function (entry) {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            observer.unobserve(entry.target);
          }
        });
      }, { threshold: 0.12 });

      document.querySelectorAll('.reveal').forEach(function (el) {
        observer.observe(el);
      });
    })();
  </script>
```

- [ ] **Step 3: Verify in browser**

Reload. Scroll down slowly — About, Programs, Testimonials, and Contact sections should fade and slide up into view as they enter the viewport. They should NOT animate immediately on page load.

- [ ] **Step 4: Commit**

```bash
git add index.html style.css
git commit -m "feat: scroll reveal animations via IntersectionObserver"
```

---

### Task 10: Add Billy's photos

**Files:**
- Create: `images/hero.jpg`
- Create: `images/about.jpg`

- [ ] **Step 1: Create the `images/` folder**

```powershell
New-Item -ItemType Directory -Force images
```

- [ ] **Step 2: Add and compress photos**

Save the following photos into `images/` with these exact filenames:
- `images/hero.jpg` — the **outdoor pull-up bar dusk shot** (Billy on wooden outdoor bar, trees, evening light)
- `images/about.jpg` — the **gym portrait** (Billy standing in home gym, confident pose)

Compress each using [squoosh.app](https://squoosh.app): resize to max 1800px wide, JPEG quality ~82, target under 400KB each.

- [ ] **Step 3: Verify images load in browser**

Reload http://localhost:8080. Hero section shows Billy's outdoor photo full-screen. About section shows the portrait on the left.

- [ ] **Step 4: Commit**

```bash
git add images/
git commit -m "feat: add hero and about photos"
```

---

### Task 11: Responsive / mobile polish

**Files:**
- Modify: `style.css` — add media queries

- [ ] **Step 1: Add mobile media queries to `style.css`** (append after scroll reveal, before end of file)

```css
/* ─── Responsive ─────────────────────────────── */
@media (max-width: 768px) {
  .about-inner {
    grid-template-columns: 1fr;
    gap: 2.5rem;
  }

  .about-photo img {
    height: 380px;
  }

  .programs-grid {
    grid-template-columns: 1fr;
  }

  .testimonials-row {
    grid-template-columns: 1fr;
  }

  .section-pad {
    padding: 4rem 0;
  }
}

@media (max-width: 480px) {
  .hero-headline {
    font-size: clamp(3rem, 16vw, 5rem);
  }

  .hero-sub {
    font-size: 0.85rem;
  }

  .header-inner {
    height: 56px;
  }

  .wordmark {
    font-size: 1.3rem;
  }
}
```

- [ ] **Step 2: Test on mobile viewport**

In Chrome/Edge DevTools (F12 → Ctrl+Shift+M), set viewport to 390px (iPhone 14). Verify:
- Header: wordmark and CTA button not overlapping
- Hero: headline readable, not clipping
- About: photo stacks above text
- Programs: 4 cards stack single column
- Testimonials: 3 cards stack single column
- Contact: button fits within viewport

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: responsive mobile layout — single column below 768px"
```

---

### Task 12: Final QA

No code changes — verification only.

- [ ] **Step 1: Check all links**

Click every button in the browser:
- Header "Train with Billy" → opens `https://www.instagram.com/Billyk.sw` in new tab
- Hero "Start training →" → same
- About "Follow @Billyk.sw" → same
- Contact "@Billyk.sw" → same

- [ ] **Step 2: Accessibility check**

Open DevTools → Lighthouse → Accessibility. Confirm:
- Hero `<img>` has `alt=""` (correct for decorative/background images)
- `about.jpg` `<img>` has descriptive alt text
- All buttons and links have accessible labels

- [ ] **Step 3: Check page metadata**

View source (Ctrl+U). Confirm `<title>` and `<meta name="description">` are present and correct.

- [ ] **Step 4: Replace placeholder testimonials**

Get 2–3 real client quotes from Billy. Replace the names "Alex M.", "Sarah T.", "Marc D." and their quotes in `index.html`.

- [ ] **Step 5: Commit content updates**

```bash
git add index.html
git commit -m "content: update testimonials with real client quotes"
```

---

### Task 13: Deploy to Vercel + GitHub

No code changes — deployment only.

- [ ] **Step 1: Verify Vercel login**

```powershell
vercel whoami
```
If error: run `vercel login` and complete device code flow.

- [ ] **Step 2: Deploy to Vercel**

```powershell
vercel --prod --yes --name getrrypt
```
Expected output: deployment URL like `https://getrrypt.vercel.app`. Open and verify site matches localhost.

- [ ] **Step 3: Create GitHub repo and push**

```powershell
gh repo create ckatra/getrrypt-website --public --source . --remote origin --push
```
Confirms repo at `https://github.com/ckatra/getrrypt-website` with all commits.

- [ ] **Step 4: Custom domain (when ready)**

When getrypt.com DNS is available, run:
```powershell
vercel domains add getrypt.com
```
Then add to DNS registrar:
- A record: `@` → `76.76.21.21`
- CNAME record: `www` → `cname.vercel-dns.com`

- [ ] **Step 5: Final live check**

Open the Vercel URL. Confirm hero photo loads, all sections render correctly, Instagram links work.
