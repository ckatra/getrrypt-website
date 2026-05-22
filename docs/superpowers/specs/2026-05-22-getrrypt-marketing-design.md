# GetRypt Marketing Site — Design Spec
**Date:** 2026-05-22  
**Client:** Billy Katranis — getrypt.com  
**Scope:** Marketing site (Phase 1). Client management platform is a separate future sub-project.

---

## Overview

A single-page, static marketing site for GetRypt — Billy Katranis's personal training service. Focused on calisthenics, general fitness, and nutrition coaching. Dark & electric visual language using Billy's own training photos. Primary conversion action is an Instagram DM via a CTA button.

---

## Visual Language

### Palette (CSS custom properties)
```css
--bg: #0D0F14;
--accent: #00D4FF;
--text: #FFFFFF;
--text-muted: rgba(255, 255, 255, 0.55);
--card-bg: #161A22;
```
All palette values live in `:root` so the entire color scheme can be swapped by editing one block.

### Typography
- **Headlines:** Bebas Neue (Google Fonts) — tall, bold, athletic
- **Body:** Inter (Google Fonts) — clean, readable

### Photo Treatment
- Hero: full-viewport photo, dark gradient overlay fading from transparent to `--bg` at the bottom
- Section photos: `filter: brightness(0.85)` to keep dark and consistent with palette

### Animations
- Sections fade in on scroll via CSS `@keyframes fadeInUp` + a small inline `IntersectionObserver` (< 10 lines, no library)

---

## Page Structure

### 1. Header (sticky, minimal)
- Left: `GETRYPT` wordmark in Bebas Neue, `--accent` color
- Right: `"Train with Billy"` CTA button → Billy's Instagram link

### 2. Hero
- **Photo:** Outdoor pull-up bar dusk shot (most cinematic)
- **Headline:** `"Train different."` — Bebas Neue, ~80–100px, white
- **Sub-line:** `"Calisthenics · Strength · Nutrition — personalized to you"` — Inter, muted
- **CTA:** `"Start training →"` button → Instagram link
- Bottom: dark gradient fade into next section

### 3. About Billy
- **Layout:** Two-column — large portrait photo (left) + text (right). Single column on mobile.
- **Content:** 3–4 sentences on Billy's approach, calisthenics background, and coaching philosophy
- **Accent line:** Short punchy location/reach line (e.g., `"Based in Montreal. Training everywhere."`)

### 4. Programs
- **Layout:** 2×2 card grid
- **Cards:**
  1. 1-on-1 In-Person Training
  2. Online / Remote Coaching
  3. Nutrition Coaching
  4. Group Training
- Each card: icon + title + 1-line description. `--card-bg` background, `--accent` top border.

### 5. Testimonials
- 2–3 quote cards
- Horizontal scroll on mobile
- Name + quote text. No client photos required.

### 6. Contact / Instagram CTA
- Full-width dark band
- Headline: `"Ready to start?"`
- Large Instagram button with Billy's handle
- Optional: phone/email in small text below as fallback

### 7. Footer
- `© 2026 GetRypt · Billy Katranis` — minimal, centered

---

## Technical Implementation

### Stack
- Plain HTML5 + CSS3
- No build step, no npm, no frameworks
- Google Fonts via `<link>` with `font-display: swap`
- Inline SVG icons

### File Structure
```
GetRypt/
├── index.html
├── style.css
├── images/
│   ├── hero.jpg          ← outdoor pull-up bar dusk shot
│   ├── about.jpg         ← gym portrait
│   └── [additional photos as needed]
├── favicon.svg
└── .vercelignore
```

### Images
- Compressed to ~200–400KB each before deployment
- `loading="lazy"` on all images below the fold
- Hero image preloaded via `<link rel="preload">`

### CTA / Backend
- No contact form, no backend
- All CTAs link directly to Billy's Instagram
- Zero third-party scripts

### Deployment
- Vercel CLI: `vercel --prod --yes --name getrrypt`
- GitHub repo: `ckatra/getrrypt-website` (created after site is built)

---

## Out of Scope (Phase 1)
- Client login / dashboard
- Workout or nutrition plan management
- Scheduling or payments
- Multilingual support

These are captured for Phase 2 (client management platform), a separate sub-project.
