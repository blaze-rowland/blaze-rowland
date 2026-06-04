# GitHub Profile README Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the plain profile README with an animated SVG hero banner (Gradient Pulse), lightly edited content, and two zero-maintenance hosted stat cards.

**Architecture:** Two hand-crafted animated SVGs (dark/light) live in `assets/` and are selected with `<picture>` + `prefers-color-scheme`. The README is plain GitHub-flavored markdown plus the limited HTML GitHub allows. Stats come from hosted services (github-readme-stats, streak-stats) — no Actions, no tokens.

**Tech Stack:** SVG with SMIL + embedded CSS animations, GitHub-flavored markdown, hosted stat card services.

**Spec:** `docs/superpowers/specs/2026-06-04-github-profile-design.md`

**Note on testing:** This repo is content + static assets — there is no test framework. Each task replaces TDD with explicit visual verification steps (local browser render, then on-GitHub verification in Task 4). Do not skip verification steps.

---

### Task 1: Dark-mode hero banner

**Files:**
- Create: `assets/hero-dark.svg`

- [ ] **Step 1: Create the SVG**

Create `assets/hero-dark.svg` with exactly this content:

```xml
<svg width="1200" height="300" viewBox="0 0 1200 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Blaze Rowland — distributed systems, event-driven architecture, TypeScript">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#0d1117">
        <animate attributeName="stop-color" values="#0d1117;#1a1040;#0a2540;#0d1117" dur="10s" repeatCount="indefinite"/>
      </stop>
      <stop offset="50%" stop-color="#1a1040">
        <animate attributeName="stop-color" values="#1a1040;#0a2540;#0d1117;#1a1040" dur="10s" repeatCount="indefinite"/>
      </stop>
      <stop offset="100%" stop-color="#0a2540">
        <animate attributeName="stop-color" values="#0a2540;#0d1117;#1a1040;#0a2540" dur="10s" repeatCount="indefinite"/>
      </stop>
    </linearGradient>
    <style>
      .name { font: 800 64px 'Segoe UI', Ubuntu, Helvetica, Arial, sans-serif; fill: #ffffff; letter-spacing: -1px; }
      .tag  { font: 500 24px 'Segoe UI', Ubuntu, Helvetica, Arial, sans-serif; fill: #a78bfa; }
      .fade-up { opacity: 0; animation: fadeUp 1.2s ease 0.5s forwards; }
      @keyframes fadeUp {
        from { opacity: 0; transform: translateY(10px); }
        to   { opacity: 1; transform: translateY(0); }
      }
    </style>
  </defs>
  <rect width="1200" height="300" rx="12" fill="url(#bg)"/>
  <text x="80" y="155" class="name">Blaze Rowland</text>
  <g class="fade-up">
    <text x="80" y="205" class="tag">distributed systems · event-driven architecture · TypeScript</text>
  </g>
</svg>
```

Implementation notes (why it's built this way — do not "improve" these):
- The gradient drift uses **SMIL `<animate>` on stop-color**, not CSS, because CSS cannot animate SVG gradient stops. SMIL plays when the SVG is rendered via `<img>`, which is how GitHub displays it.
- The tagline fade uses **CSS keyframes in an embedded `<style>`** — also works in `<img>` context.
- Fonts are a system stack; external font loading is blocked inside `<img>`-rendered SVGs, so do not add `@import` or webfont references.

- [ ] **Step 2: Verify locally in a browser**

Run: `open assets/hero-dark.svg`
Expected: Banner renders 1200×300 with rounded corners; background gradient slowly shifts between navy/indigo/blue on a ~10s loop; tagline fades up half a second after load. Name is white, tagline is violet (#a78bfa).

- [ ] **Step 3: Commit**

```bash
git add assets/hero-dark.svg
git commit -m "Add animated dark-mode hero banner"
```

---

### Task 2: Light-mode hero banner

**Files:**
- Create: `assets/hero-light.svg`

- [ ] **Step 1: Create the SVG**

Create `assets/hero-light.svg` with exactly this content (same structure as dark, light palette):

```xml
<svg width="1200" height="300" viewBox="0 0 1200 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Blaze Rowland — distributed systems, event-driven architecture, TypeScript">
  <defs>
    <linearGradient id="bg" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#f6f8fa">
        <animate attributeName="stop-color" values="#f6f8fa;#ede9fe;#e0f2fe;#f6f8fa" dur="10s" repeatCount="indefinite"/>
      </stop>
      <stop offset="50%" stop-color="#ede9fe">
        <animate attributeName="stop-color" values="#ede9fe;#e0f2fe;#f6f8fa;#ede9fe" dur="10s" repeatCount="indefinite"/>
      </stop>
      <stop offset="100%" stop-color="#e0f2fe">
        <animate attributeName="stop-color" values="#e0f2fe;#f6f8fa;#ede9fe;#e0f2fe" dur="10s" repeatCount="indefinite"/>
      </stop>
    </linearGradient>
    <style>
      .name { font: 800 64px 'Segoe UI', Ubuntu, Helvetica, Arial, sans-serif; fill: #1f2328; letter-spacing: -1px; }
      .tag  { font: 500 24px 'Segoe UI', Ubuntu, Helvetica, Arial, sans-serif; fill: #7c3aed; }
      .fade-up { opacity: 0; animation: fadeUp 1.2s ease 0.5s forwards; }
      @keyframes fadeUp {
        from { opacity: 0; transform: translateY(10px); }
        to   { opacity: 1; transform: translateY(0); }
      }
    </style>
  </defs>
  <rect width="1200" height="300" rx="12" fill="url(#bg)"/>
  <text x="80" y="155" class="name">Blaze Rowland</text>
  <g class="fade-up">
    <text x="80" y="205" class="tag">distributed systems · event-driven architecture · TypeScript</text>
  </g>
</svg>
```

- [ ] **Step 2: Verify locally in a browser**

Run: `open assets/hero-light.svg`
Expected: Same layout and animations as the dark variant; background drifts between near-white/lavender/pale-blue; name is near-black (#1f2328), tagline is deep violet (#7c3aed). Text must be clearly readable at every point of the gradient cycle.

- [ ] **Step 3: Commit**

```bash
git add assets/hero-light.svg
git commit -m "Add animated light-mode hero banner"
```

---

### Task 3: Rewrite README.md

**Files:**
- Modify: `README.md` (full replacement)

- [ ] **Step 1: Replace README.md**

Replace the entire contents of `README.md` with:

````markdown
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/hero-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="assets/hero-light.svg">
  <img alt="Blaze Rowland — distributed systems · event-driven architecture · TypeScript" src="assets/hero-dark.svg" width="100%">
</picture>

Full-stack TypeScript engineer in Houston, TX. I lead platform architecture at **[Fishbowl](https://fishbowl.com)** — designing the systems, API contracts, and infrastructure patterns behind a restaurant engagement platform serving 5,500+ locations. Previously at MHVillage and Triskelle Software Solutions.

### Projects

| | |
|---|---|
| **[git-env / ev](https://git-env.com)** | End-to-end encrypted environment variable sync CLI — 2,000+ organic npm installs in the first week |
| **[documnt](https://documnt.app)** | Document management with client-side E2E encryption — Next.js, Supabase, BullMQ |
| **[Ten54](https://ten54.app)** | Cross-platform mobile app — React Native, Expo, Supabase |

### Stack

**Core** — TypeScript across all layers
**Frontend** — React 19, Vue 3, React Native, TanStack Router, Vite, Tailwind
**Backend** — Node.js, Hono, Fastify, Express, Symfony/PHP
**Data** — PostgreSQL, MySQL, MongoDB, Snowflake, Redis, RabbitMQ, BullMQ, SQS
**Infra** — AWS (ECS, AmazonMQ, S3, CloudWatch, WAF), Terraform, Docker, GitHub Actions
**Payments** — Stripe, Square, Checkout.com
**AI** — Claude API, OpenAI API, MCP servers, RAG pipelines

### Activity

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api?username=blaze-rowland&count_private=true&show_icons=true&hide_border=true&bg_color=00000000&title_color=a78bfa&text_color=c9d1d9&icon_color=58a6ff&hide_rank=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github-readme-stats.vercel.app/api?username=blaze-rowland&count_private=true&show_icons=true&hide_border=true&bg_color=00000000&title_color=7c3aed&text_color=1f2328&icon_color=0969da&hide_rank=true">
  <img alt="GitHub commit and contribution stats" height="170" src="https://github-readme-stats.vercel.app/api?username=blaze-rowland&count_private=true&show_icons=true&hide_border=true&bg_color=00000000&title_color=a78bfa&text_color=c9d1d9&icon_color=58a6ff&hide_rank=true">
</picture><picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://streak-stats.demolab.com?user=blaze-rowland&hide_border=true&background=00000000&ring=a78bfa&fire=58a6ff&currStreakLabel=a78bfa&currStreakNum=c9d1d9&sideNums=c9d1d9&sideLabels=8b949e&dates=8b949e&stroke=30363d">
  <source media="(prefers-color-scheme: light)" srcset="https://streak-stats.demolab.com?user=blaze-rowland&hide_border=true&background=00000000&ring=7c3aed&fire=0969da&currStreakLabel=7c3aed&currStreakNum=1f2328&sideNums=1f2328&sideLabels=57606a&dates=57606a&stroke=d0d7de">
  <img alt="GitHub contribution streak" height="170" src="https://streak-stats.demolab.com?user=blaze-rowland&hide_border=true&background=00000000&ring=a78bfa&fire=58a6ff&currStreakLabel=a78bfa&currStreakNum=c9d1d9&sideNums=c9d1d9&sideLabels=8b949e&dates=8b949e&stroke=30363d">
</picture>

---

[mail.browland@gmail.com](mailto:mail.browland@gmail.com)
````

Content decisions already approved in the spec — do not re-litigate:
- Bio trimmed to two sentences + previous-employers line; the day-to-day responsibilities list is gone deliberately.
- Angular dropped from Frontend (approved "light edits" — keeps the line tight; everything else unchanged).
- The two `<picture>` blocks for stats are adjacent with no blank line between them so the cards sit side by side (images are inline elements; a blank line would create separate paragraphs and stack them).
- `hide_rank=true` keeps the stats card compact and avoids the rank circle dominating.
- Both stat cards use transparent backgrounds (`00000000`) so they sit on GitHub's own page background in either theme.

- [ ] **Step 2: Verify markdown renders locally**

Run: `grep -c "prefers-color-scheme: dark" README.md`
Expected output: `3` (hero + two stat cards).

Then open a quick visual check of the raw structure: `open -a "Google Chrome" README.md` is NOT useful for GitHub-flavored rendering — skip local rendering; on-GitHub verification happens in Task 4. Just proofread the file once.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Redesign profile README: animated hero, themed stat cards"
```

---

### Task 4: Ship and verify on GitHub

**Files:** none (verification only)

- [ ] **Step 1: Confirm with user, then push**

Pushing makes this live on github.com/blaze-rowland immediately. Confirm the user is ready, then:

```bash
git push origin main
```

- [ ] **Step 2: Enable private contribution counts (USER ACTION — cannot be done by the agent)**

Ask the user to: GitHub → Settings → Public profile → check **"Include private contributions on my profile"** → Save.
Without this, the streak card and contribution counts show public activity only and will undercount badly (≈all work is private).

- [ ] **Step 3: Verify on github.com — dark mode**

Visit `https://github.com/blaze-rowland` with GitHub appearance set to dark (Settings → Appearance, or default).
Expected:
- Dark hero banner renders full-width with rounded corners
- Gradient drift animation is playing (GitHub proxies images through Camo — this step verifies the animation survives the proxy; it should, since Camo passes SVG through)
- Tagline fades up on page load
- Both stat cards load, use the violet/blue dark palette, and sit side by side
- Stats include private contribution counts (numbers should look plausible vs. your actual activity, not near-zero)

- [ ] **Step 4: Verify on github.com — light mode**

Switch GitHub appearance to light, reload profile.
Expected: light hero variant and light-themed stat cards render; all text readable.

- [ ] **Step 5: Verify mobile width**

In browser devtools, set viewport to 390px wide (or check on a phone).
Expected: hero scales down cleanly (it's `width="100%"`); stat cards stack vertically — acceptable and expected at narrow widths; nothing overflows horizontally.

- [ ] **Step 6: Record any tweaks**

If stat card colors clash or numbers look wrong, adjust the URL params in `README.md` (they are plain query strings — `title_color`, `ring`, `fire`, etc.), commit and push the tweak:

```bash
git add README.md
git commit -m "Tune stat card theming"
git push origin main
```

---

## Revision 1 Tasks (approved redesign — see spec Revision 1)

### Task 5: Project card SVGs (6 files)

**Files:**
- Create: `assets/card-git-env-dark.svg`, `assets/card-git-env-light.svg`, `assets/card-documnt-dark.svg`, `assets/card-documnt-light.svg`, `assets/card-ten54-dark.svg`, `assets/card-ten54-light.svg`

- [ ] Each card: viewBox `0 0 380 200`, rect rx="10" with 1px border, vertical gradient background, content per spec Revision 1 (title 24px/800, hook 14px/1.5 line-height wrapped with tspans, mono tech line 12px at bottom, "↗" 16px top-right at x=352 y=36). Dark/light palettes per spec. Validate each with xmllint. Commit: "Add project card SVGs".

Card content:
- git-env: title "git-env" + accent " / ev"; hook line 1 "E2E-encrypted env var sync CLI."; hook line 2 (success color) "2,000+ npm installs in week one."; tech "TypeScript · CLI · crypto"
- documnt: title "documnt"; hook "Document management with client-side E2E encryption."; tech "Next.js · Supabase · BullMQ"
- Ten54: title "Ten54"; hook "Cross-platform mobile app, App Store + Play."; tech "React Native · Expo · Supabase"

### Task 6: Stack panel SVGs (2 files)

**Files:**
- Create: `assets/stack-dark.svg`, `assets/stack-light.svg`

- [ ] viewBox `0 0 1180 330`, rect rx="10" bordered panel. Seven rows (y starting 48, step 40): uppercase label (12px, 700 weight, letter-spacing 1.5, accent color, x=28) + pill chips (rounded rects rx=12 height=26, mono 13px text centered, pill width = ceil(text_chars × 7.8) + 26, gap 8px, chips start x=140). Rows/chips:
  - CORE: "TypeScript — every layer"
  - FRONTEND: React 19, Vue 3, React Native, TanStack, Vite, Tailwind
  - BACKEND: Node.js, Hono, Fastify, Express, Symfony
  - DATA: PostgreSQL, MySQL, MongoDB, Snowflake, Redis, RabbitMQ, SQS
  - INFRA: AWS, Terraform, Docker, GH Actions
  - PAYMENTS: Stripe, Square, Checkout.com
  - AI: Claude API, OpenAI API, MCP servers, RAG
- [ ] Validate with xmllint; visually check chips don't overflow the 1180 width. Commit: "Add stack panel SVGs".

### Task 7: Restructure README for full-SVG layout

**Files:**
- Modify: `README.md`

- [ ] Keep: hero `<picture>` block and bio paragraph. Replace everything from `### Projects` through the end with:
  - Three project cards on one line, each `<a href="..."><picture>(dark/light sources)<img width="32%" alt="..."></picture></a>` with hrefs git-env.com / documnt.app / ten54.app
  - Stack panel `<picture>` with `<img width="100%">`
  - `---` rule + mailto link (unchanged)
  - No `### Projects` / `### Stack` / `### Activity` headings; Activity and both hosted stat cards removed entirely
- [ ] Commit: "Extend hero design language to full README; drop Activity"

### Task 8: Local visual check, push, live verify

- [ ] Screenshot rendered cards locally before pushing; fix any text overflow/clipping
- [ ] Push (user already authorized shipping this design)
- [ ] Verify live: dark + light, card click-through, mobile width

### Task 9: Expand stack panel to 9 rows (user-approved, evidence from ~/Sites scan)

**Files:** Modify `assets/stack-dark.svg`, `assets/stack-light.svg` (full regeneration, same palettes/geometry rules)

- [ ] viewBox `0 0 1180 410`, panel rect height 408. Nine rows, baseline y = 58 + N×40 (58…378). Same pill formula (round(chars×7.9)+26, gap 10, chips x=150):
  - CORE: TypeScript — every layer
  - FRONTEND: React 19, Vue 3, Next.js, Astro, Angular, TanStack, Vite, Tailwind
  - MOBILE: React Native, Expo, Expo Router, EAS Build
  - BACKEND: Node.js, Bun, Hono, Fastify, Express, Symfony/PHP, Drizzle, Prisma
  - DATA: PostgreSQL, MySQL, MongoDB, SQLite, Snowflake, Supabase, Redis, RabbitMQ, BullMQ, SQS
  - INFRA: AWS, Terraform, Docker, Caddy, GitHub Actions
  - OBSERVABILITY: Sentry, Rollbar, Prometheus, Grafana, Loki, Pino
  - PAYMENTS: Stripe, Square, RevenueCat, Checkout.com
  - AI: Claude API, OpenAI API, MCP servers, Ollama, RAG
- [ ] Widest row (DATA, 10 pills) right edge ≈1029 < 1160 ✓ (pre-computed). Update aria-label to mention the new rows. README needs no change (same filenames).
- [ ] Commit "Expand stack panel: mobile, observability, fuller rows", visual check, push.

## Self-Review (completed)

- **Spec coverage:** hero (Task 1–2), `<picture>` dark/light (Task 3), content edits (Task 3), two stat cards with `count_private` (Task 3), one-time profile setting (Task 4 Step 2), verification incl. Camo/animation, both themes, mobile (Task 4 Steps 3–5). Out-of-scope items remain untouched. ✓
- **Placeholder scan:** all code blocks complete; no TBDs. ✓
- **Consistency:** palette hexes match across both SVGs and both stat card themes (a78bfa/58a6ff/c9d1d9 dark; 7c3aed/0969da/1f2328 light). ✓
