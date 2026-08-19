# AGENTS.md — OurBabyList

Guidance for AI coding agents working on [OurBabyList](./README.md), a static,
single-page baby-prep checklist for our daughter Emery Laine (due February 3,
2027), hosted on GitHub Pages.

---

## 1. Tech & Toolchain

| Item | Choice |
|---|---|
| Type | Static site — **no build step, no dependencies, no framework** |
| Language | HTML + CSS + a tiny bit of vanilla JS |
| Hosting | GitHub Pages (`main` branch, `/ (root)`) |
| Fonts | Google Fonts via `<link>` (Fraunces, DM Sans, Caveat) — works offline from `file://` |
| Styling | Single `styles.css`, theming via CSS custom properties |
| JS | One `IntersectionObserver` scroll-reveal at the bottom of `index.html`. Nothing else. |

---

## 2. File Organization

```
.
├── index.html    # All content + structure + the reveal script
├── styles.css    # All styling + both theme token sets
├── README.md     # Publish instructions
└── AGENTS.md     # This file
```

**Rules:**
- Content (names, prices, notes) lives in `index.html` inside `.item` blocks.
- Visual styling lives **entirely** in `styles.css`. Do not put inline `style` or
  new `<style>` blocks in `index.html` for page content (the only exception is a
  one-off experiment you intend to remove).
- Keep the page a single self-contained HTML file — don't split content into
  fragments or add a bundler.

---

## 3. How the Page Is Structured

```
<body>
  .orbs  (decorative background blobs — aria-hidden)
  nav.nav → .brand + .nav-links (4 anchors)
  header.hero → eyebrow, h1, .sub, .lede, .stats (4 .stat pills)
  main
    section#must-have.part.must      → 10 .category blocks
    section#nice-to-have.part.nice   →  3 .category blocks
    section#budget                    → 2×2 .panel tables (budget/comfortable, deferred/ongoing)
    section#safety                    → 5-rule list
  footer → name, sources
```

### Item markup (the unit you'll edit most)

```html
<li class="item">
  <label class="check"><input type="checkbox" aria-label="Mark purchased"><span class="box"></span></label>
  <div class="item-body">
    <div class="item-head"><span class="name">Item name</span><span class="tag tag-new">Buy new</span></div>
    <p class="note">One-line note. Keep it short.</p>
    <div class="prices">
      <span class="price"><em>Budget</em> Graco SnugRide SnugFit 35 — <b>$160</b></span>
      <span class="price"><em>Spend</em> Chicco KeyFit Max ClearTex — <b>$250</b></span>
    </div>
  </div>
</li>
```

### Tags (badges)
- `.tag tag-new` — "Buy new"
- `.tag tag-used` — "Used OK"
- `.tag tag-free` — "Free" (insurance/samples)
- `.tag tag-used` with custom text — e.g. "Used OK · new mattress" or "N/A"

### Price spans
- Two-line budget/spend: use `<em>Budget</em>` and `<em>Spend</em>`.
- Single estimate: `class="price one"` with `<em>Est.</em>` (or `<em>Used</em>`, `<em>Monthly</em>`, `<em>Optional</em>`).
- Free: `class="price free"` with `<em>Cost</em>`.

---

## 4. Design System (Blush & Petal)

All colors come from CSS variables at the top of `styles.css`. There are two
token sets — **change colors there, never inline.**

- `:root, [data-theme="a"]` — the light theme currently in use. `index.html` has
  `data-theme="a"`.
- `[data-theme="b"]` — the unused "Dark Aurora" theme. Left in place for a future
  dark-mode toggle; do not delete it unless asked.

Palette (light): warm pink-white bg `#fff5f9`, magenta accent `#d62985`,
purple `#9b3bb5`, maroon `#7e1f47`, rose `#ef8fb6`. Typography: **Fraunces**
(display serif), **DM Sans** (body), **Caveat** (handwritten accents).

Key variables: `--bg`, `--surface`, `--ink`, `--ink-soft`, `--accent`,
`--accent-2`, `--maroon`, `--must`, `--nice`, `--card-shadow`, `--line`.

---

## 5. Content & Editing Conventions

- **Static prices.** There is deliberately **no cost calculator**. Checkboxes are
  visual-only (`:checked` styling in CSS) and must not sum anything. Do not add
  JavaScript that totals prices unless the owner asks for it.
- **Keep the budget tables in sync.** `section#budget` contains two itemized
  tables ("Budget Path" ≈ $1,250, "Comfortable Path" ≈ $1,890) plus deferred and
  monthly totals, and four `.stat` pills in the hero. If you change an item's
  price, update the matching category row and the totals so the numbers reconcile.
- **Safety content is authoritative.** The items and the "Five Safety Rules"
  section encode real guidance (AAP safe sleep, CPSC recalls, vitamin D 400 IU,
  "no OTC meds under ~3 months," "buy new: car seat + mattress"). Do not soften,
  delete, or contradict these without the owner's explicit sign-off. When in
  doubt, leave the safety language alone.
- **Buy-new vs used tags are rules, not suggestions.** Car seat and mattress stay
  `.tag-new`. Everything thriftable stays `.tag-used`.
- Keep notes one line and plain-spoken; this page is read by non-technical
  family members.
- Accessible markup: decorative orbs stay `aria-hidden="true"`; every checkbox
  keeps an `aria-label`; the reveal script respects `prefers-reduced-motion`
  (already handled in CSS).

---

## 6. Git Rules & Commands

**Git policy — non-negotiable:**

- **NEVER run `git push`** (or `git push --force`, `-f`, or any other
  remote-mutating command). Only the owner pushes.
- **ONLY run `git commit` when explicitly asked.** Never commit automatically or
  as part of finishing a task unless the owner requested it.

```bash
# Preview locally (no server needed — just open it)
open index.html

# Commit — ONLY when the owner explicitly asks
git add .
git commit -m "Describe the change"
```

Publishing is the owner's job: they push, and GitHub Pages (already configured
for `main` / `(root)`) serves the site.

---

## 7. Anti-Patterns to Avoid

| Anti-pattern | Why |
|---|---|
| Adding a framework/build step | The point is a single static file that "just works." |
| Inline styles in `index.html` | All styling belongs in `styles.css`. |
| Adding a price-total script | Static prices are an explicit requirement. |
| Deleting `[data-theme="b"]` tokens | Future dark-mode option; harmless to keep. |
| Editing safety/medical wording casually | It reflects pediatric guidance; changes need owner approval. |
| Changing an item price without updating the budget tables/hero stats | The numbers must reconcile. |
| Hard-coding new colors instead of using CSS variables | Breaks theming consistency. |
| Running `git push` or `git commit` unprompted | Never push; only commit when explicitly asked. |
