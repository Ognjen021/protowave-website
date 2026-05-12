# ProtoWave Website — project context

Static marketing site for **ProtoWave**, a Serbian engineering studio offering CAD, 3D printing, 3D scanning, and automation. Originally a Claude Design HTML/CSS/JS prototype handoff; being implemented as a real site.

**Repo:** https://github.com/Ognjen021/protowave-website
**Local path:** `/Users/a1234/Downloads/protowave-website`
**Deploy:** Vercel (auto-deploys from `main`)
**Language:** Serbian (`lang="sr"`)

---

## Stack

- Pure static HTML + CSS + vanilla JS. **No build system, no framework, no bundler.**
- Shared design tokens in `colors_and_type.css` (CSS custom properties).
- Page-specific styles live in an inline `<style>` block at the top of each HTML file.
- Fonts: Inter (sans) + JetBrains Mono (mono captions, IDs, labels) — loaded from Google Fonts via `colors_and_type.css`.
- Viewport: `width=1280` — **desktop-only**, no responsive/mobile yet.

## Repo layout (flat — all at root)

```
index.html              ← home page
kontakt.html            ← contact page (form + quote calc + FAQ)
colors_and_type.css     ← design tokens + base typography
logo-mark.svg
tweaks-panel.jsx        ← design-tool artifact on home (React+Babel CDN); not production-needed
uploads/                ← design-tool asset PNGs (probably can be deleted)
README.md               ← original Claude Design handoff README — DO NOT modify
CLAUDE.md               ← this file
.gitignore
```

There used to be a `project/` subfolder; it was flattened so Vercel serves `index.html` at `/`.

---

## Design system (top-level)

Dark, minimal, technical — Linear-inspired with a 3D-print accent.

**Tokens** (`colors_and_type.css`, all `--pw-*` prefix):
- Backgrounds: `--pw-bg` `#08090a`, `--pw-bg-2` `#0e0f11`, surfaces `#131418` → `#1f2127`
- Foregrounds: `--pw-fg` `#f7f8f8` → `--pw-fg-5` `#3a3d44` (5-step ramp)
- Borders: `rgba(255,255,255,0.06)` → `0.22`
- Brand violet: `--pw-brand` `#6E5BF1`
- Accent mint: `--pw-accent` `#4DDFC2`
- Wave gradient: `linear-gradient(120deg, #6E5BF1, #5C8BFF, #4DDFC2)` — use sparingly (hero accent text, key marks, slider thumbs)
- Mono color tokens: success/warning/danger/info + print-job-specific (printing/queued/paused/failed/done)
- Spacing: 4pt grid via `--pw-s-1`…`--pw-s-12`
- Radii: 6px controls, 8/10/12px cards, 14px hero cards
- Motion: `--pw-ease: cubic-bezier(0.32, 0.72, 0, 1)`, durations 120/180/320ms

**Layout patterns:**
- `.wrap` = `max-width: 1120px; margin: 0 auto; padding: 0 32px`
- `.section` = 96px vertical padding, hairline border-bottom
- Section heading pattern: `.section-eyebrow` (caps, mono-ish, 0.08em tracking) → `.section-title` (40px, 600, -0.02em) → `.section-sub` (17px, fg-2)
- Sticky nav with `backdrop-filter: blur(12px)` + `rgba(8,9,10,0.85)` bg
- Cards: `--pw-surface` bg + `--pw-border` hairline + 12px radius + hover transitions to surface-2 / border-2
- Mono captions everywhere: IDs (`M-01`, `01`), file paths, ETAs, units

**Buttons** (`.btn` + modifier):
- `.btn-primary` = brand violet, white text, inset highlight
- `.btn-secondary` = surface-2 bg, border-2
- `.btn-ghost` = transparent, fg-2 → fg on hover
- `.btn-lg` = 44px height (default is 36px)

---

## Pages built so far

### `index.html` — Home (originally `Home Screen.html` from the handoff)
Single long-scroll: nav · hero (with 3D print viz SVG + KPIs) · client logos · services grid (6 cards) · process (4 steps) · materials (split with material chip grid) · contact teaser · footer.

The hero has an interactive `tweaks-panel.jsx` (React via CDN + Babel standalone). It's a design-tool leftover for swapping hero viz variants / accent palettes. **Can be removed for production** without affecting layout.

### `kontakt.html` — Contact page (built by Claude in this project)
- **Hero**: eyebrow + h1 with wave-gradient accent line + lede + 3 mono status pills.
- **Two-col main grid (1.4fr / 0.9fr)**:
  - **Form card** with `protowave.studio/kontakt/novi-upit.form` crumb bar (mirrors home's hero mock pattern) and 5 numbered fieldsets:
    1. Tip projekta (chip checkboxes)
    2. Brief (textarea + live char counter, max 800)
    3. Fajlovi (drag/drop upload zone, file input hidden)
    4. Tehnički parametri (3 selects: materijal / količina / rok)
    5. Tvoji podaci (4 inputs + NDA & terms checkboxes)
    - Submit button — currently `onsubmit` shows an alert (**demo only, needs backend wiring**)
  - **Sticky sidebar**: direct contact card, "Šta sledi" 3-step timeline with ETA chips, accepted-files chip cluster.
- **`#calc` Kalkulator ponude**: interactive. Pick material (8 options, each with M-0X mono id) → adjust mass/qty/infill sliders → live price + breakdown + estimated print time. Pricing is a rough heuristic in vanilla JS at the bottom of the file (`var setup = 4`, infill multiplier, qty discount tiers).
- **`#faq` FAQ**: 6 collapsible items with rotating `+` icon.
- Alt contact strip (email / phone / studio cards).
- Footer (identical to home).

---

## Cross-page navigation (must stay consistent)

The nav is the **same component on both pages**:
- Brand → `index.html`
- Links: `Usluge` → `index.html#usluge`, `Proces` → `#proces`, `Materijali` → `#materijali`, `Primena` → `#primena`, `Kalkulator ponude` → `kontakt.html#calc`
- Right side: empty `.btn-ghost` (placeholder for future login) + `.btn-primary` "Kontakt" → `kontakt.html`

**Home CTAs wired to Kontakt:** hero buttons, contact-section buttons, footer Resursi links (Kalkulator → `kontakt.html#calc`, FAQ → `kontakt.html#faq`). On any new page added, copy this nav block verbatim.

---

## Local dev

No build step. Serve the folder:

```bash
cd /Users/a1234/Downloads/protowave-website
python3 -m http.server 8788
```

Then `http://localhost:8788/` (home) and `http://localhost:8788/kontakt.html`.

A previous server was started on port 8787 from inside `project/` — that folder no longer exists, so that server now 404s. Kill it or start fresh.

---

## Git

- Branch: `main`
- Remote: `https://github.com/Ognjen021/protowave-website.git`
- **Git identity is NOT set globally on this machine.** Commits must use a per-command override so the config stays untouched:
  ```bash
  git -c user.name="Ognjen021" -c user.email="contact@ognjenjovkovic.com" commit -m "..."
  ```

---

## Pending / next steps

User originally asked for **2 new pages**. Only **Kontakt** is built.

The unbuilt second page was discussed but not chosen. Suggestions still on the table:
- **Primena (case studies)** — `#primena` is in the nav but currently has no destination section. Strongest candidate.
- **Usluge** (full service breakdown)
- **Materijali** (full library beyond the 8 teased on home)
- **Proces** (expanded)
- **O nama** (about/team/equipment)

Other known TODOs:
- Wire the Kontakt form to a real backend (Formspree, Vercel serverless, etc.). Currently just `alert()`.
- Replace placeholder `href="#"` links in footers: *O nama, Klijenti, Portfolio, Karijera, Uslovi, Privatnost, NDA · obrazac, Tehnički vodič*.
- Remove `tweaks-panel.jsx` + its `<script>` tags from `index.html` for production (or keep behind a query-param flag).
- Add responsive/mobile layouts — currently `viewport: width=1280`, desktop-only.
- Tighten quote calculator heuristics (current numbers are rough).
- Consider a real `/kontakt` route via a Vercel rewrite (drops the `.html`).

---

## Conventions

- Write everything in **Serbian** to match the existing copy.
- Keep page-specific CSS inline at the top of each HTML file (matches the home's pattern); shared tokens belong in `colors_and_type.css`.
- Use the existing `--pw-*` tokens — do not introduce new color literals unless extending the system intentionally.
- Use the section eyebrow → title → sub pattern for every new section.
- Mono everything that's an ID, file path, ETA, status, or unit.
- Cards: 12px radius, `--pw-surface` bg, `--pw-border` hairline, hover lifts to `--pw-surface-2` + `--pw-border-2`.
- Match the wrap (1120px) and section padding (96px) for vertical rhythm.
