# GitHub Profile README Redesign

**Date:** 2026-06-04
**Repo:** blaze-rowland/blaze-rowland (GitHub profile README)

## Goal

Make the profile memorable to both recruiters and fellow developers. The current README is accurate but visually plain. Chosen direction: **designed banner / editorial** (landing-page feel) with a light layer of live stats — provided the stats are accurate and require zero ongoing maintenance.

## Constraints

- Nearly all work activity is in private Fishbowl repos on this same GitHub account. Stats must reflect that honestly without exposing anything.
- No GitHub Actions, tokens, or anything that needs babysitting. A bespoke stats pipeline (scheduled Action + PAT) was explicitly considered and rejected: PAT expiry, 60-day schedule auto-disable, and stale-but-plausible failure modes.
- Content is "mostly right" — light copy edits only, no structural rewrite.

## Design

### 1. Hero banner (the centerpiece)

- Hand-crafted animated SVG, **Gradient Pulse** style: slow ambient gradient drift (deep navy → indigo → blue) behind heavy-weight "Blaze Rowland" type; tagline *distributed systems · event-driven architecture · TypeScript* fades up on load.
- Animation implemented with CSS keyframes embedded inside the SVG — plays when GitHub renders it as an `<img>`. No JS, no external requests.
- Two variants committed to the repo: `assets/hero-dark.svg` and `assets/hero-light.svg`, selected via `<picture>` + `prefers-color-scheme` so both GitHub themes get a purpose-designed banner.

### 2. Content (light edits)

- **Bio:** trimmed from four sentences to two. Keeps: leads platform architecture at Fishbowl, 5,500+ locations. Drops the day-to-day responsibilities list (redundant with the stack section).
- **Projects:** same three (git-env/ev, documnt, Ten54) as a clean aligned table; one-line hooks tightened; "2,000+ organic npm installs in the first week" stays prominent.
- **Stack:** same layers (Core/Frontend/Backend/Data/Infra/Payments/AI), condensed to one tight line each.
- **Contact:** mail.browland@gmail.com link at the bottom, unchanged.

### 3. Stats

Two hosted cards side by side, themed to match the hero palette, each wrapped in the same `<picture>` dark/light technique:

- **Streak card** — github-readme-streak-stats (hosted instance). Reflects private contributions once the profile setting is enabled.
- **Commit stats card** — github-readme-stats (hosted instance) with `count_private=true`. Includes private contribution counts without any token.

**One-time manual step (user):** GitHub Settings → Profile → enable "Include private contributions on my profile."

Explicitly excluded: top-languages card (public repos only — would misrepresent), contribution snake (requires an Action), WakaTime (requires an editor plugin + maintenance).

### 4. Failure modes

- Hosted stat card outage → broken-image alt text until the service recovers; self-healing, no action needed. Banner and all content are repo-local and cannot break.
- No tokens, schedules, or workflows exist to expire or disable.

### 5. Verification before shipping

- README renders correctly in both GitHub color modes (dark and light).
- Layout holds at mobile width.
- SVG animation plays on github.com itself (GitHub proxies images through Camo — verify animation survives, not just in local preview).
- Stat cards load and show plausible numbers with private counts included.

## Revision 1 (2026-06-04, post-ship feedback)

First shipped version: user verdict — only the hero looked good; stock markdown below it (table with empty header strip, bold-text stack lines, lone streak card) read as generic. Field findings: github-readme-stats.vercel.app is `DEPLOYMENT_PAUSED` (dead indefinitely); streak-stats works but cold-cache requests 504 through Camo before self-healing; streak numbers (current 1 / longest 12) actively unflattering.

**Approved redesign — extend the hero's design language to the whole page:**

- **Hero:** unchanged.
- **Projects:** three hand-crafted SVG cards in the hero palette (dark + light variants each), replacing the table. Each card: project name (accent-colored suffix where applicable), "↗" affordance top-right, 1–2 line hook (git-env's npm stat highlighted green), monospace tech line. Each card is wrapped in a link — the whole image is clickable. No hover states (GitHub renders SVGs as static images).
- **Stack:** one hand-crafted SVG panel (dark + light variants): seven layer rows, uppercase accent label column, technologies as pill chips (rounded rects, mono text), replacing the seven bold-text lines.
- **Activity section: removed entirely.** Both hosted cards dropped. GitHub's native contribution graph below the README covers activity (private contributions visible once the user flips the profile setting).
- **No section headings** — cards and panel flow directly, as in the approved mockup.
- **Footer:** email link, unchanged.

Palette (from hero): dark — bg #161b22→#1a1040 gradient, border #30363d, text #c9d1d9/#8b949e, accent #a78bfa, links/icons #58a6ff/#79c0ff, success #7ee787, muted #484f58. Light — bg #ffffff→#f5f3ff, border #d0d7de, text #1f2328/#57606a, accent #7c3aed, links #0969da, success #1a7f37, muted #6e7781.

All SVGs: system font stack, no external resources, `role="img"` + `aria-label`, reduced-motion-safe (these are static — no animation needed beyond the hero).

## Out of scope

- Self-hosted/bespoke stats rendering (Approach 3 — rejected, see Constraints).
- New content sections (blog feed, talks, sponsor buttons).
- Changes to pinned repositories (can be adjusted by hand later, independent of this work).
