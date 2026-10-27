# BusMora — Design System

> **Version:** 1.0 · **Status:** Ready for implementation · **Date:** 2026-10-26
> **Applies to:** `busmora-frontend` (Next.js 14+ App Router, TypeScript, Tailwind CSS, shadcn/ui owned in `shared/ui`, React Flow).
> **Builds on:** [FRONTEND-TECHNICAL-DESIGN.md](./FRONTEND-TECHNICAL-DESIGN.md) (structure, RTL rules, `next/font`, graphs in LTR containers), [SYSTEM-ARCHITECTURE.md](./SYSTEM-ARCHITECTURE.md) §5.3 (screens), [prd.md](./prd.md) §5 (screen requirements), [domain/GLOSSARY.md](../domain/GLOSSARY.md) (names and statuses).
> **Resolves:** FTD gap **FG-8** (Arabic + Latin font choice) and the design part of **FG-11** (accessibility target and supported browsers).
> **Visual references (measured from their production CSS on 2026-10-26):**
> - **talentsarena.net**: colour palette, typography, shape, layout rhythm, buttons, gradient-stroke cards, dark "stage" sections.
> - **firecrawl.dev**: motion vocabulary (orbiting rings, counter-rotation, live ping, shimmer, typing cursor, reveals).
>
> Where a reference value failed WCAG 2.2 AA, it was adjusted. Every adjustment is listed in §15.

---

## Table of contents
1. [Principles](#1-principles)
2. [Colour](#2-colour)
3. [Typography](#3-typography)
4. [Layout, spacing and breakpoints](#4-layout-spacing-and-breakpoints)
5. [Shape, depth and texture](#5-shape-depth-and-texture)
6. [Iconography](#6-iconography)
7. [Motion](#7-motion)
8. [Components](#8-components)
9. [Status and category systems](#9-status-and-category-systems)
10. [Screens and responsive behaviour](#10-screens-and-responsive-behaviour)
11. [Bilingual and RTL rules](#11-bilingual-and-rtl-rules)
12. [Accessibility (WCAG 2.2 AA)](#12-accessibility-wcag-22-aa)
13. [Browser support and performance](#13-browser-support-and-performance)
14. [Implementation](#14-implementation)
15. [Deviations from the references](#15-deviations-from-the-references)
16. [Open items](#16-open-items)

---

## 1. Principles

1. **A dark stage and a light workspace.** Graphite surfaces (`#0d0d0d`) frame the product: the marketing hero, the top bar, the app sidebar and the live Canvas. Work happens on airy light surfaces (`#f7f9fc` and white). Mint (`#1cdcab`) is the light in the room, used for action, progress and focus.
2. **Calm by default, alive when working.** Marketing pages may move continuously and slowly. Inside the app, motion appears **only while something is happening** (a Task runs, a Post publishes, a crawl progresses) and stops when it ends.
3. **Arabic-first, never Arabic-only.** Every screen is designed in RTL first and verified in LTR. No layout may depend on reading direction except where §11 allows it.
4. **Accessible by construction.** Every token pair used for text or controls is pre-checked against WCAG 2.2 AA (§2.4). Colour is never the only carrier of meaning (§9).
5. **Owned, consistent components.** Everything is built from the tokens (§14.2) and the components in §8. A page never invents its own colours, radii, shadows or animations.

---

## 2. Colour

### 2.1 Primitive palettes

All values are exact. "★" marks a value adjusted from the reference for accessibility (see §15).

**Mint (brand)**
| Token | Hex | Primary use |
|---|---|---|
| mint-50 | `#e9fcf6` | faintest tint, info panels |
| mint-100 | `#d2f8ee` | selected rows, chips, tertiary-button hover, text on mint-700 |
| mint-200 | `#a4f1dd` | accent card borders |
| mint-300 | `#77eacd` | decorative on dark |
| mint-400 | `#49e3bc` | primary-button hover fill on dark stage |
| **mint-500** | **`#1cdcab`** | **primary fill**, active indicators on dark, focus ring on dark, glows |
| mint-600 | `#16b089` | decorative fills only (fails as text) |
| **mint-700 ★** | **`#0f7c61`** | **accent text and links on light**, focus ring on light, primary-button hover fill, status icons on light |
| mint-800 | `#0b5844` | small accent text on tints, pressed states |
| mint-900 | `#083d30` | deep accent surfaces |
| mint-950 | `#0b1512` | near-black mint (dark cards) |

**Graphite (dark neutrals)**
| Token | Hex | Primary use |
|---|---|---|
| graphite-100 | `#d3d4d5` | body text on stage |
| graphite-200 | `#a7a9ac` | secondary text on stage, idle sidebar items |
| graphite-250 ★ | `#8a8f94` | **control borders on light** (inputs, checkboxes, radios, switches) |
| graphite-300 | `#7b7f82` | captions on stage only |
| graphite-350 ★ | `#686d72` | **meta text on light** (timestamps, help text) |
| graphite-400 | `#4f5459` | secondary text on light |
| graphite-500 | `#0d0d0d` | **stage background; primary text on light** |
| graphite-600 | `#0a0a0a` | darker stage sections |
| graphite-700 | `#080808` | stage chips/tags |
| graphite-800 | `#141516` ★ | elevated surface on stage (cards, Canvas nodes) |
| graphite-900 | `#030303` | dark inputs |

**Mist (light neutrals)**
| Token | Hex | Primary use |
|---|---|---|
| mist-100 | `#f7f9fc` | **page background (light)** |
| mist-200 | `#f0f4f8` | subtle fills, table headers, hover rows, section tags |
| mist-300 | `#e8eef5` | dividers |
| mist-400 | `#e1e9f1` | **card borders** (decorative) |
| mist-500 | `#d9e3ee` | stronger decorative borders, skeletons |

**Supporting accents** (category chips, charts, illustrations; never primary actions)
| Family | 100 | 500 | 600 | 700 | 800 |
|---|---|---|---|---|---|
| sky | `#d8f1ff` | `#3dbbff` | `#3196cc` | `#257099` | `#184b66` |
| orange | `#ffe1d2` | `#ff6a1f` | `#cc5519` | `#994013` | `#662a0c` |
| purple | `#f4dffe` | `#c75df9` | `#9f4ac7` | `#773895` | `#502564` |

**Danger** (not in the references; required for errors and destructive actions)
| Token | Hex | Use |
|---|---|---|
| danger-50 | `#fef3f2` | error panel background |
| danger-200 | `#fecdca` | error panel border |
| danger-600 | `#d92d20` | destructive button fill (white text) |
| danger-700 | `#b42318` | error text and icons |

Warnings use the **orange** family (100 background, 800 text, 600–700 icon). Info uses **sky**.

### 2.2 Semantic tokens

Components use only these semantic names (§14.2 maps them to CSS variables and shadcn names).

| Semantic token | Light workspace | Dark stage |
|---|---|---|
| `background` | mist-100 | graphite-500 |
| `surface` (cards, popovers) | white | graphite-800 |
| `surface-subtle` | mist-200 | `white/5` |
| `foreground` (primary text) | graphite-500 | white |
| `foreground-secondary` | graphite-400 | graphite-200 |
| `foreground-meta` | graphite-350 | graphite-300 |
| `border` (decorative) | mist-400 | `white/10` |
| `border-control` (inputs) | graphite-250 | `white/40` |
| `accent-text` / links | mint-700 | mint-500 |
| `primary` / `primary-foreground` | mint-500 / graphite-500 | mint-500 / graphite-500 |
| `primary-hover` / `primary-hover-foreground` | mint-700 / mint-100 | mint-400 / graphite-500 |
| `ring` (focus) | mint-700 | mint-500 |
| `danger` / `danger-foreground` | danger-700 text; danger-600 fill with white | same |

### 2.3 Usage rules
- **Mint fills always carry dark text** (`graphite-500`). White text on mint-500 or mint-600 is forbidden (1.8:1).
- **One primary (mint) button per view region.** Secondary actions use the secondary or tertiary variants.
- Accent families never replace status colours (§9.1).
- Gradients are allowed only for the gradient stroke, the hero ring and glows (§5). No gradient text, except the shimmer animation M3.

### 2.4 Verified contrast (WCAG 2.2 AA)

Computed with the WCAG relative-luminance formula. Text needs ≥ 4.5:1 (≥ 3:1 at 24px+, or 18.66px+ bold). UI boundaries and graphics need ≥ 3:1.

| Pair | Ratio | Status |
|---|---|---|
| graphite-500 text on mist-100 | 18.4 | ✅ |
| graphite-400 text on mist-100 | 7.3 | ✅ |
| graphite-350 text on mist-100 / white / mist-200 | 5.0 / 5.2 / 4.7 | ✅ |
| mint-700 text on mist-100 / white | 4.9 / 5.2 | ✅ |
| mint-700 text on mint-100 | 4.5 | ✅ (minimum; prefer mint-800 = 7.4) |
| graphite-500 text on mint-500 (primary button) | 11.0 | ✅ |
| mint-100 text on mint-700 (primary hover) | 4.5 | ✅ |
| white text on mint-700 | 5.2 | ✅ |
| white / graphite-100 / graphite-200 text on graphite-500 | 19.4 / 13.1 / 8.3 | ✅ |
| graphite-300 text on graphite-500 | 4.8 | ✅ (captions only) |
| mint-500 text on graphite-500 / graphite-800 | 11.0 / 10.3 | ✅ |
| sky-800 on sky-100 · orange-800 on orange-100 · purple-800 on purple-100 | 8.0 · 8.9 · 9.4 | ✅ |
| danger-700 on white / danger-50 / mist-100 | 6.6 / 6.1 / 6.2 | ✅ |
| white on danger-600 | 4.8 | ✅ |
| **Focus ring** mint-700 on mist-100 · mint-500 on graphite-500 | 4.9 · 11.0 | ✅ |
| **Control border** graphite-250 on white / mist-100 | 3.3 / 3.1 | ✅ |
| Dark control border `white/40` on graphite-500 | 3.8 | ✅ |
| Status icon mint-700 on white | 5.2 | ✅ |
| ❌ white on mint-500 | 1.8 | forbidden |
| ❌ mint-500 ring / dot on light | 1.7 | forbidden; use mint-700 |
| ❌ mist-500 or graphite-200 as input border | 1.3 / 2.4 | forbidden; use graphite-250 |
| ❌ graphite-300 text on light | 3.8–4.0 | forbidden; use graphite-350 |
| ❌ `white/40` text on stage | 3.8 | forbidden; use graphite-300 or `white/60` (7.3) |

---

## 3. Typography

### 3.1 Families (resolves FG-8)
| Script | Family | Weights loaded | `next/font/google` export | CSS variable |
|---|---|---|---|---|
| Latin | **Ubuntu** | 400, 500, 700 | `Ubuntu` (subsets `latin`, `latin-ext`) | `--font-ubuntu` |
| Arabic | **IBM Plex Sans Arabic** | 400, 500, 600, 700 | `IBM_Plex_Sans_Arabic` (subset `arabic`) | `--font-plex-arabic` |

- Both are loaded once in `app/layout.tsx` through `next/font` (self-hosted, `display: 'swap'`), and both variables go on `<html>`.
- **Stacks by direction:**
  - `html[dir="rtl"]` → `var(--font-plex-arabic), var(--font-ubuntu), system-ui, sans-serif`
  - `html[dir="ltr"]` → `var(--font-ubuntu), var(--font-plex-arabic), system-ui, sans-serif`
- **Mixed text works per glyph:** Latin product names inside Arabic render in Plex's Latin glyphs; Arabic inside English falls back to Plex Arabic.
- **Why this pair:** the reference loads Ubuntu only, so its Arabic falls back to unpredictable system fonts. Plex Sans Arabic is a humanist sans with clear counters at small UI sizes and a full weight range, and it sits well next to Ubuntu's rounded humanist forms.

### 3.2 Scale

**Marketing pages (fluid, 360 → 1280 px viewport).** Sizes follow the reference's 56/48/40 · 48/40/32 steps.
| Style | Size | Line height | Weight (Latin / Arabic) | Use |
|---|---|---|---|---|
| Display | `clamp(2.5rem, 2.109rem + 1.739vw, 3.5rem)` (40→56) | 1.2 | 500 / 600 | hero headline |
| Title-1 | `clamp(2rem, 1.609rem + 1.739vw, 3rem)` (32→48) | 1.2 | 500 / 600 | section titles |
| Title-2 | `clamp(1.5rem, 1.109rem + 1.739vw, 2.5rem)` (24→40) | 1.2 | 500 / 600 | sub-section titles |
| Title-3 | `clamp(1.25rem, 1.152rem + 0.435vw, 1.5rem)` (20→24) | 1.25 | 500 / 600 | card titles |
| Body-L | `clamp(1rem, 0.951rem + 0.217vw, 1.125rem)` (16→18) | 1.5 (Arabic 1.7) | 400 | lead paragraphs |

**Application (fixed sizes; dense and predictable).**
| Style | Size | Line height | Weight (Latin / Arabic) | Use |
|---|---|---|---|---|
| Page title | 24px (mobile 20px) | 1.25 | 500 / 600 | one per page |
| Section title | 18px | 1.3 | 500 / 600 | card headers, panels |
| Body | 16px | 1.5 (Arabic 1.7) | 400 | default text, chat messages |
| Body-S | 14px | 1.45 (Arabic 1.6) | 400 / 500 | tables, lists, buttons, inputs |
| Caption | 12px | 1.4 | 500 | badges, meta, helper text |

**Rules:**
- Headings use **weight 500 in Latin** (as the reference) and **600 in Arabic** (Plex Arabic 500 reads lighter). Implement as `:lang(ar)` or `html[dir=rtl]` overrides.
- Never use weights above 700. Never use italic for Arabic.
- No uppercase or letter-spacing on Arabic. Overlines use `tracking-wide uppercase` in LTR only.
- **Body text never goes below 14px**, and inputs never below 16px on mobile (this prevents iOS zoom on focus).
- Readable line length: prose containers max **72ch** (deliverable viewer, landing paragraphs).

### 3.3 Generated content (Deliverables, chat replies)
- **Markdown typography:**
  - h1 24px, h2 20px, h3 18px (weight 500 Latin / 600 Arabic)
  - paragraphs 16px/1.7
  - lists with `padding-inline-start: 1.5rem`
  - tables with mist-400 borders and a mist-200 header
  - blockquote with a 3px mint-500 `border-inline-start`
  - inline code on mist-200
- Every generated block gets `dir="auto"` (FTD §9.6).

---

## 4. Layout, spacing and breakpoints

### 4.1 Spacing scale (4px base; the reference uses the same steps)
`4, 8, 12, 16, 24, 32, 48, 64, 80, 96, 128` px. These are Tailwind's default spacing units (1 = 4px). Component padding uses 8/12/16/24; section padding uses 24–128.

### 4.2 Breakpoints
| Name | Min width | Reference equivalent | Typical device |
|---|---|---|---|
| (base) | 0 | ≤ 479 | phones (designs must work from **320px**) |
| `xs` | 480px (30rem) | 480–767 | large phones |
| `sm` | 640px (40rem) | — | small tablets, landscape phones |
| `md` | 768px (48rem) | 768–991 | tablets |
| `lg` | 1024px (64rem) | ≥ 992 | small laptops |
| `xl` | 1280px (80rem) | desktop container | laptops/desktops |
| `2xl` | 1536px (96rem) | — | large desktops |

Only `xs` is custom. The rest are Tailwind defaults.

### 4.3 Containers and section rhythm
| Context | Max width | Inline padding | Block padding |
|---|---|---|---|
| Marketing section | 1280px (80rem), centred | 32px ≥ md · 24px sm · 16px base | 96px ≥ lg · 64px md · 48px base |
| Marketing text block | 624px (section header), prose 72ch | — | — |
| App content (list pages) | 1200px | 32px ≥ lg · 24px md · 16px base | 32px ≥ md · 24px base |
| App reading pages (deliverable, settings) | 800px (settings 720px) | same | same |
| App full-bleed (chat + Canvas, Brain graph) | none (fills viewport) | 0 | 0 |

### 4.4 Grids
- **Marketing:** 12 columns, gap 24px ≥ lg; 6 columns at md; 2 columns below md (as the reference).
- **App card grids:** `repeat(auto-fill, minmax(min(100%, 320px), 1fr))`, gap 16px (24px ≥ xl). This gives 1 column on phones, 2 on tablets, 3 or more on desktop.

### 4.5 Layering (z-index scale)
| Layer | z | Examples |
|---|---|---|
| base | 0 | content |
| raised | 10 | sticky table headers, bulk-action bar |
| sticky chrome | 20 | top bar, mobile bottom actions |
| sidebar / drawer | 30 | app sidebar, mobile nav drawer |
| overlay | 40 | sheet/dialog backdrop |
| modal | 50 | dialog, sheet, command menu |
| popover | 60 | dropdowns, tooltips, date pickers |
| toast | 70 | notifications |

---

## 5. Shape, depth and texture

### 5.1 Radius
| Token | Value | Use |
|---|---|---|
| `radius-sm` | 4px | tiny elements (tooltip arrows, progress bars) |
| `radius-md` | 8px | badges-as-tags, section tags, small icon buttons, menu items |
| `radius-control` | **16px** | buttons, inputs, selects, segmented controls |
| `radius-card` | **24px** | cards, dialogs, sheets, chat bubbles |
| `radius-panel` | **32px** | feature cards, hero panels, gradient-stroke cards |
| `radius-full` | 9999px | pills, avatars, status dots |

Tailwind mapping: `rounded-2xl` = 16px, `rounded-3xl` = 24px, `rounded-4xl` = 32px.

### 5.2 Borders
- Decorative (cards, dividers): 1px `border` token.
- Controls: 1px `border-control` token, which becomes 2px `ring` on focus (§12.2).
- Active vertical-nav indicator: 2px mint on the **inline-start** edge.

### 5.3 Shadows (soft and rare; borders do most of the separation)
| Token | Value | Use |
|---|---|---|
| `shadow-card` | `0 8px 50px rgba(0,0,0,0.05)` | resting cards on light |
| `shadow-pop` | `0 12px 32px rgba(13,13,13,0.12), 0 2px 6px rgba(13,13,13,0.06)` | popovers, menus, dialogs |
| `shadow-primary` | `0 4px 12px rgba(28,220,171,0.30)` | primary-button hover |
| `shadow-dark` | `0 4px 8px rgba(0,60,44,0.64)` | buttons on dark stage, hover |
| `glow-mint` | `0 0 24px rgba(28,220,171,0.35)` | active Canvas node, orbit core |

### 5.4 Gradient stroke (signature)
A card sits inside a 1px wrapper whose background is a 135° gradient. The inner surface radius is the wrapper radius minus 1px.
- **Rest:** `linear-gradient(135deg, mist-100, mist-400 50%, mist-100)`
- **Hover / selected:** `linear-gradient(135deg, mist-100, mint-500 50%, mist-100)`
- **Info variant (hover):** use sky-500 instead of mint-500.
- **Transition:** 300ms with the `ease-brand` curve (§7.2). Add `translateY(-2px)` on hover for pointer devices only (`@media (hover: hover)`).
- Use it on feature/stat cards, Deliverable cards, Batch Review post cards and onboarding steps. **Not** on dense lists or tables.

### 5.5 Glass
- Light: `background: rgba(255,255,255,0.8)` + `backdrop-filter: blur(16px)` (sticky top bars over content).
- Dark: `rgba(255,255,255,0.05)` + `blur(16px)` + `white/10` border (cards on stage).
- Always include the `-webkit-backdrop-filter` prefix (Safari).
- If `backdrop-filter` is unsupported, fall back to an opaque surface.

### 5.6 Stage effects (marketing and empty states only)
- **Hero ring:** a large mint ring glow anchored at the bottom centre of the hero, 1280px wide (1180px ≤ md, 900px ≤ sm), half visible. Built in CSS (no image):
  - `radial-gradient(closest-side, transparent 72%, rgba(28,220,171,0.55) 84%, transparent 100%)`
  - `filter: blur(24px)`
  - `opacity: 0.6`
- **Ambient glows:** up to 3 blurred circles (mint-500 at 20% and sky-500 at 12% opacity, 300–480px, `blur(80px)`), drifting slowly (M8). **Hidden below `md`**, where a static radial gradient replaces them for performance.

---

## 6. Iconography

- **Library:** lucide (shadcn/ui's default icon set, owned through `shared/ui`). Names below were checked against `lucide-react` 1.61; pin the version, because lucide renames icons between majors. Stroke width 1.75, round caps.
- **Sizes:** 16px inside buttons, inputs and badges; 20px in navigation and list rows; 24px for page-header tiles and empty states.
- **Icon tile:** `size-11` (44px), `radius-control`, mint-100 background, mint-700 icon. On stage: `white/5` background, mint-500 icon.
- **Directional icons flip in RTL:** arrows, chevrons, "send", "back", "next", undo/redo, and progress arrows.
- **Never flip:** logos, checkmarks, media play/pause, clocks, charts, the Canvas and Brain graphs, and brand marks of Facebook/Instagram/X.
- **Platform icons** keep their brand colour: Facebook `#1877F2`, Instagram (official gradient or `#E4405F`), X `#000000` (white on dark).

---

## 7. Motion

### 7.1 Principles
1. Animate **only `transform` and `opacity`** (plus `background-position` for M3). Never animate layout properties.
2. **Marketing:** continuous, slow, ambient motion is allowed.
3. **App:** at most **two continuous animations visible at once**, and only while a process is active. Everything is static at rest.
4. **Pause what isn't visible:** continuous animations get `animation-play-state: paused` when off-screen (IntersectionObserver) or when the tab is hidden (`visibilitychange`).
5. **Every continuous animation is pausable** (§7.5), as WCAG 2.2.2 requires.

### 7.2 Duration and easing tokens
| Token | Value | Use |
|---|---|---|
| `duration-fast` | 100ms | press feedback (`active:scale-[0.98]`) |
| `duration-base` | 200ms | colour/opacity changes, hovers of small elements |
| `duration-medium` | 300ms | buttons, cards, gradient stroke (reference value) |
| `duration-slow` | 600ms | reveals, sheet/dialog enter |
| `ease-brand` | `cubic-bezier(.77, 0, .175, 1)` | UI transitions (reference value) |
| `ease-out-quart` | `cubic-bezier(.165, .84, .44, 1)` | entrances, reveals |
| `ease-ping` | `cubic-bezier(0, 0, .2, 1)` | ping, pulse |
| linear | `linear` | continuous rotation, shimmer |

### 7.3 Keyframes
Defined once in the global stylesheet (§14.2).

```css
@keyframes bm-spin { to { transform: rotate(360deg); } }
@keyframes bm-spin-reverse { from { transform: rotate(360deg); } to { transform: rotate(0deg); } }
@keyframes bm-ping { 75%, 100% { transform: scale(2); opacity: 0; } }
@keyframes bm-shimmer { from { background-position: 100% 0; } to { background-position: -100% 0; } }
@keyframes bm-cursor { 0%, 100% { opacity: 0; } 50% { opacity: 1; } }
@keyframes bm-fade-up { from { opacity: 0; transform: translateY(12px); } to { opacity: 1; transform: none; } }
@keyframes bm-grow-y { from { transform: scaleY(0); } to { transform: scaleY(1); } }
@keyframes bm-drift {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(-12px, -8px) scale(1.04); }
  50% { transform: translate(10px, -6px) scale(1.07); }
  75% { transform: translate(12px, 6px) scale(1.03); }
}
@keyframes bm-scan {
  0%, 100% { transform: translateY(0); opacity: 1; }
  45% { transform: translateY(var(--scan-distance, 120px)); opacity: .6; }
}
@keyframes bm-glow { 0%, 100% { opacity: .45; } 50% { opacity: .9; } }
```

### 7.4 Motion catalogue
| ID | Name | Specification | Where | Trigger / stop |
|---|---|---|---|---|
| **M1** | Orbit | Ring: `bm-spin 30s linear infinite`. Items on the ring: `bm-spin-reverse 30s linear infinite` (same duration keeps them upright). Outer ring: `bm-spin-reverse 48s linear infinite`, dashed stroke `white/15`. | Landing hero visual | Always, while in view; pausable |
| **M1-fast** | Spinner | `bm-spin 0.9s linear infinite`, 16px, 2px stroke, mint-700 on light / mint-500 on dark | Loading buttons, inline loaders | While loading |
| **M2** | Live ping | 8px solid dot + an identical absolute twin running `bm-ping 1s ease-ping infinite`. Mint-700 on light, mint-500 on dark. **Always paired with a text label** ("Live", "Publishing…"). | Canvas "Live" header, active Canvas node, "publishing" pill, crawl in progress | While the process runs |
| **M3** | Thinking shimmer | Text filled with `linear-gradient(90deg, currentColor 40%, mint-500 50%, currentColor 60%)` at `200% 100%`, `background-clip: text`, `bm-shimmer 2.2s linear infinite`; reversed in RTL | Running Task status line, "Analysing your website…" | While running; finished items become static |
| **M4** | Typing cursor | 2px × 0.9em mint-500 bar after the text, `bm-cursor 1s steps(1) 5` (**blinks 5 times, then stays solid**) | Landing headline | On load only |
| **M5** | Reveal | Starts at `opacity-0 translate-y-3`. When 15% visible (IntersectionObserver, once): `bm-fade-up 600ms ease-out-quart both`. Children stagger 60ms, at most 8 staggered items. | Marketing sections, card grids on first paint | Once |
| **M6** | Count-up and grow | Numbers count from 0 to the value over 900ms (requestAnimationFrame, `ease-out-quart`), formatted with `Intl.NumberFormat` in the UI locale. Bars use `bm-grow-y 800ms ease-out both` with `transform-origin: bottom`. | Landing stats; any future charts | Once, when visible |
| **M7** | Scan highlight | A mint-500 bordered, `radius-md` box over mock text lines. `bm-scan 3.2s ease-in-out infinite` (set `--scan-distance` to the line block height) plus `bm-glow 2.4s ease-in-out infinite` on the border. | Landing "reads your website"; onboarding crawl card | Landing: in view. Onboarding: while crawling. |
| **M8** | Ambient drift | Glow blobs: `bm-drift 14s ease-in-out infinite`, each with a different negative delay | Dark stage backgrounds ≥ md | In view; pausable |
| **M9** | Stroke hover | Gradient-stroke swap (§5.4), 300ms ease-brand, with lift on hover-capable devices | Feature, deliverable and post cards | Hover/focus-within |
| **M10** | Button feedback | Hover: 300ms ease-brand colour + `scale(1.03)` (pointer devices only). Press: `scale(0.98)` 100ms. | All buttons | Interaction |
| **M11** | Surface enter | Dialogs: fade + `scale(0.98→1)` 200ms ease-out-quart. Sheets: slide from the inline-end (bottom sheet from bottom) 300ms. Toasts: fade + `translateY(8px→0)` 200ms. | Overlays | Open/close |
| **M12** | Canvas edge flow | Active edge: dashed stroke (`6 6`) animating `stroke-dashoffset` via React Flow's `animated` edge, mint-500 on dark | Canvas | While the edge is active |

### 7.5 Reduced motion and pausing (mandatory)
1. **System setting:** under `@media (prefers-reduced-motion: reduce)`:
   - all `animation-duration` become `0.001ms` with `animation-iteration-count: 1`
   - transitions become `0.001ms`
   - M5 elements render visible immediately (no hidden start state)
   - M1/M8 render static
   - M2 shows the solid dot without the ping
   - M3 shows plain text with a static spinner
2. **In-app setting:** Settings → "Reduce animations" (stored per user, default follows the system). It sets `data-motion="reduced"` on `<html>`, which applies the same rules.
3. **Landing pause control:** a small "Pause animations" toggle (icon button, `aria-pressed`) in the hero corner. It pauses M1, M8 and M7 with `animation-play-state: paused` on a `data-motion="paused"` scope.
4. **No flashing:** nothing flashes more than 3 times per second.

---

## 8. Components

> All components are owned shadcn/ui components in `shared/ui`, restyled with the tokens above (FTD-ADR-12). Every interactive component has these states: **default, hover, focus-visible, active, disabled**, plus **loading** where async and **invalid** where it is a form control.

### 8.1 Button
| Variant | Default | Hover (pointer) | Active | Notes |
|---|---|---|---|---|
| **primary** | mint-500 fill, graphite-500 text, 1px mint-500 border | mint-700 fill, mint-100 text, `shadow-primary`, `scale(1.03)` | `scale(0.98)` | main action of a region |
| **secondary** | transparent, mint-700 text, 1px mint-500 border | same as primary hover | `scale(0.98)` | on stage: mint-500 text |
| **tertiary** | transparent, graphite-400 text, no border | mint-100 fill, 1px mint-500 border, mint-800 text | — | low emphasis, toolbars |
| **ghost-on-dark** | transparent, graphite-200 text | `white/5` fill, white text | — | sidebar/footer actions |
| **danger** | transparent, danger-700 text, 1px danger-200 border | danger-50 fill | — | reject, delete, disconnect |
| **danger-solid** | danger-600 fill, white text | darker (`#b42318`) | — | confirmations only |

- **Sizes:**
  - sm: height 36px, padding-inline 16px, 14px text
  - md: height 44px, padding-inline 24px, 14px text (**default**; meets the 44px target)
  - lg: height 52px, padding-inline 32px, 16px text (marketing CTAs use lg with 48px inline padding, as the reference)
- **Shape and type:** `radius-control`; weight 500 Latin / 600 Arabic.
- **Icons:** 16px; gap 8px; leading icon at the inline-start; directional icons flip.
- **Loading:** M1-fast spinner replaces the leading icon, the label stays, `aria-busy="true"`, the button is disabled, and its width doesn't change.
- **Disabled:** 50% opacity, no hover effects, `cursor: not-allowed`.
- **Mobile (< sm):** primary/secondary CTAs inside forms, dialogs and onboarding are **full width**, stacked with the primary on top.
- **Icon button:** square 44px (36px compact in dense toolbars, with a 44px hit area kept by padding), `radius-control`. **Requires `aria-label`.**

### 8.2 Form controls
- **Input / Textarea / Select:**
  - height 48px (textarea min 96px), white fill, 1px graphite-250 border, `radius-control`, padding 12px/16px
  - text 16px, placeholder graphite-350
  - **Focus:** border mint-700 + 3px ring `rgba(15,124,97,0.25)`
  - **Invalid:** border danger-700 + ring `rgba(180,35,24,0.2)` + message
  - **Disabled:** mist-200 fill, graphite-350 text
- **On stage (dark):** graphite-900 fill, `white/40` border, white text, graphite-300 placeholder, mint-500 focus border.
- **Label:** 14px, weight 500/600, graphite-500, 8px above the control. Required fields show "(required)" text, not just an asterisk.
- **Help text:** 12–14px, graphite-350, 6px below. **Error text:** 14px, danger-700, with an alert icon, `id` linked through `aria-describedby`.
- **Checkbox / Radio:** 20px box (target padded to 44px), 1.5px graphite-250 border; checked = mint-700 fill with a white mark; focus ring as §12.2.
- **Switch:** track 44×24, off = graphite-250, on = mint-700; thumb white.
- **Segmented control** (language toggle, list/graph toggle): `radius-control` track mist-200, selected segment white + `shadow-card` + graphite-500 text. On stage: track `white/5`, selected mint-500 with graphite-500 text.
- **"Search yourself"** (structured forms, PRD FR-032): a tertiary button with a search icon next to each field. When chosen, the field is disabled and shows a mint-100 note: "The employee will research this".

### 8.3 Badge, status pill and section tag
- **Badge / status pill:** `radius-full`, height 24px, padding-inline 10px, 12px weight 500. Colours per §9 (100 background, 800 text, 700 icon). **Always text + icon**, never colour alone.
- **Count badge** (notifications, pending candidates): min 20px circle, mint-700 fill, white 12px text; on stage mint-500 with graphite-500 text. Caps at "99+".
- **Section tag** (marketing overline, from the reference): `radius-md`, mist-200 fill, graphite-400 text, padding 8/16. Hover (if it's a link): mint-100 fill, mint-700 border and text.

### 8.4 Cards
| Type | Spec |
|---|---|
| **Standard** | white, 1px mist-400 border, `radius-card`, `shadow-card`, padding 24px (16px < sm) |
| **Stroke card** | gradient-stroke wrapper (§5.4) + white inner, `radius-panel`, padding 24px |
| **Tint card** | mint-50 fill, 1px mint-200 border, `radius-card`; used for "Built from N Brain facts" and info panels |
| **Dark card** | graphite-800 (or glass dark §5.5), 1px `white/10`, `radius-card`, white text |
| **Interactive card** | if the whole card is clickable, it's a single link/button element with a visible focus ring; nested actions are not allowed inside a fully clickable card |

### 8.5 Navigation
- **App sidebar (dark stage):** width 264px.
  - **Top:** logo (40px tile + wordmark).
  - **Middle:** nav items, each 44px high, `radius-control`, padding 12/16, 20px icon + label 14–16px.
  - **Bottom:** UI language segmented control, Business Time Zone hint, account menu.
  - **Item states:** idle = graphite-200 text; hover = `white/5` + white; **active = mint-500 text + `mint-500/10` fill + 2px mint-500 bar on the inline-start edge**; focus ring mint-500.
  - **Badges:** Brain shows the pending-candidate count; the notification bell sits in the top bar.
- **Top bar (app):** 64px, glass light on content pages, containing the page context and actions on the inline-end. On mobile it's the dark stage bar with a menu button.
- **Marketing top bar:** 80px (5rem, as the reference), dark stage. Links are graphite-200, hover white with a 2px mint-500 bottom border. Language toggle, "Log in" (secondary-on-dark), "Get started" (primary). Below `lg` it collapses to a menu button and a full-height dark drawer.
- **Tabs (horizontal):** 44px; underline 2px mint-700 on the active tab (mint-500 on dark); text graphite-400 → graphite-500 active.
- **Vertical tabs** (settings sections): the reference pattern, with an inline-start border 1px graphite-250 and the active one 2px mint-700 + mint-700 text.
- **Breadcrumbs** (deep pages only): 14px graphite-350, separators flip in RTL.

### 8.6 Overlays
- **Dialog:** max width 560px (720px for forms), `radius-card`, `shadow-pop`, padding 24px, M11 enter. Below sm it becomes a **bottom sheet** (full width, `radius-card` top corners, max height 90dvh, drag handle).
- **Sheet (side panel):** enters from the **inline-end**, width 420px (480px for the Brain item panel); below md it's full width.
- **Popover / dropdown:** white, 1px mist-400, `radius-control`, `shadow-pop`, items 40px high (44px on touch), `radius-md`, hover mist-200, active mint-100.
- **Tooltip:** graphite-500 fill, white 12px text, `radius-md`, 6px/10px padding, delay 300ms. Never the only way to reveal essential information.
- **Toast:** bottom inline-end ≥ md, bottom centre on mobile, `radius-control`, white with a 4px status-colour bar on the inline-start, auto-dismiss 5s (errors persist until dismissed), `role="status"` (`alert` for errors).
- **Focus:** overlays trap focus, return it to the trigger, and close on Esc.

### 8.7 Data display
- **Table (≥ md):**
  - header mist-200, 12–14px weight 500, graphite-400
  - rows 52px with 1px mist-300 dividers, hover mist-100
  - numbers right-aligned in LTR and left in RTL (use `text-end`)
- **Below md, tables become stacked cards:** label/value pairs, with the primary field as the card title.
- **List row:** 56–64px, a leading icon tile or avatar, title + meta (graphite-350), trailing actions in a ⋯ menu below sm.
- **Pagination:** cursor based (FTD §7.2). A "Load more" secondary button, or infinite scroll with a sentinel and a visible "Loading…" row.
- **Skeleton:** mist-500 at 60% with a `radius` matching the content; static under reduced motion. Show after 300ms to avoid flicker.
- **Empty state:** centred, 24px icon tile, title (section-title style), one sentence (graphite-400), one primary action. Max width 420px.
- **Error state:** danger-50 panel, danger-200 border, danger-700 icon and title, plain-language message (from `messageKey`), the correlation id as small copyable meta text, and a secondary "Try again".

### 8.8 Progress and status
- **Linear progress:** 6px track mist-300, fill mint-700 (mint-500 on stage), `radius-full`, with a percentage or "x of y" label. Indeterminate = a 30% segment sliding (static under reduced motion).
- **Step indicator** (onboarding): numbered 32px circles. Done = mint-700 with a check; current = mint-500 ring with graphite-500 text; upcoming = graphite-250 outline. Labels below (stacked on mobile).

---

## 9. Status and category systems

Every status has a **colour family + icon + translated label**. Names follow [domain/GLOSSARY.md](../domain/GLOSSARY.md); i18n keys are `status.<entity>.<value>`.

### 9.1 Post (Governed Publishing)
| Status | Family | Icon (lucide) | Motion |
|---|---|---|---|
| draft | mist (graphite-400 text) | `FilePen` | — |
| pending_approval | orange | `Clock` | — |
| approved | mint | `CircleCheck` | — |
| rejected | danger | `CircleX` | — |
| scheduled | sky | `CalendarClock` | — |
| publishing | sky | `Send` | **M2 ping** |
| published | mint (solid: mint-700 fill, white text) | `BadgeCheck` | — |
| failed | danger | `TriangleAlert` | — |
| cancelled | mist | `Ban` | — |
| expired | orange | `CalendarX` | — |

### 9.2 Task
| Status | Family | Icon | Motion |
|---|---|---|---|
| requested | mist | `Hourglass` | — |
| running | mint | `Loader` | M1-fast + **M3** label |
| awaiting_clarification | orange | `MessageCircleQuestionMark` | — |
| completed | mint | `CircleCheck` | — |
| partially_completed | orange | `CircleDashed` | — |
| failed | danger | `TriangleAlert` | — |
| cancelled | mist | `Ban` | — |

### 9.3 Knowledge Item, Connected Account, Crawl
| Entity | Value → family / icon |
|---|---|
| Knowledge Item | candidate → orange / `Sparkles` · approved → mint / `CircleCheck` · rejected → danger / `CircleX` · retired → mist / `Archive` |
| Connected Account | connected → mint / `Link` · reconnect_needed → orange / `RefreshCw` (+ "Reconnect" action) · disconnected → mist / `Unlink` |
| Crawl outcome | complete → mint · partial → orange · failed → danger · in progress → mint with **M2** + **M7** |

### 9.4 Knowledge categories (Brain graph nodes and chips)
Colours meet 3:1 against white as graphics. Chips use the family's 100 background and 800 text.

| Category | Colour (icon/border) | Chip | Icon |
|---|---|---|---|
| company_profile | mint-700 | mint | `Building` |
| offering | sky-700 | sky | `Package` |
| audience | purple-600 | purple | `Users` |
| brand_rule | orange-600 | orange | `Palette` |
| positioning | mint-800 | mint | `Crosshair` |
| strategic_goal | sky-800 | sky | `Flag` |
| channel | purple-700 | purple | `Megaphone` |
| competitor | orange-700 | orange | `Swords` |
| company_fact | graphite-400 | mist | `BadgeInfo` |

Families repeat, so the **icon and label are mandatory** on every node and chip.

### 9.5 Candidate Review sections (PRD FR-013)
Company Info · Products & Services · Brand & Messaging · Audience & Market · Contact & Locations · Policies & FAQ · Other. Shown as a section list with counts (the mapping from categories to sections is OD-18).

---

## 10. Screens and responsive behaviour

Breakpoint names follow §4.2. "Stage" means the dark graphite style.

### 10.1 App shell
| Range | Behaviour |
|---|---|
| ≥ lg | Fixed dark sidebar 264px at the inline-start; content scrolls independently; the top bar sits inside the content column |
| md – lg | Sidebar collapses to a **72px icon rail** (tooltips on labels, active bar kept); content gains the space |
| < md | Dark top bar 56px (menu button, logo, notification bell); the sidebar becomes a full-height **drawer** from the inline-start over a `graphite-500/60` overlay; a skip link is the first focusable element |

- Viewport units use `dvh`.
- Safe-area insets (`env(safe-area-inset-*)`) apply to fixed bars on iOS.

### 10.2 Auth (register, verify email, login, reset)
- **Split layout ≥ lg:**
  - Form panel (max 440px) on the inline-start, on mist-100.
  - Stage panel on the inline-end with the hero ring, a short value statement and an M1 mini-orbit.
- **< lg:** a single column; the stage panel becomes a 160px stage header.
- Inputs are 48px; the primary CTA is full width; password visibility toggle; errors inline (§8.2).

### 10.3 Onboarding
- A 3-step indicator: Website → Business Time Zone → Review.
- **Crawl card** (stroke card): URL, M7 scan visual over mock lines, M3 "Reading your website…", a linear progress bar ("Pages crawled 4 of 10", "About 2 min left").
- The flow is **non-blocking**: a "Continue to your workspace" secondary button stays available (PRD FR-014).
- On completion, a mint tint card with the candidate count and "Review candidates" (primary).
- Mobile: single column, sticky bottom action bar.

### 10.4 Marketing Employee (chat + Canvas)
| Range | Layout (with the app sidebar from §10.1) |
|---|---|
| ≥ 2xl (1536) | Three columns: conversation list 280px (inline-start) · chat (flexible, ~570px+, max 760px reading width) · **Canvas 420px** (inline-end, stage style) |
| xl – 2xl (1280–1535) | Chat + Canvas 400px (chat ≈ 616px at 1280); the conversation list opens as a sheet from a header button |
| md – xl (768–1279) | Chat full width; a "Show workflow" toggle opens the Canvas as a sheet from the inline-end (480px, full width < sm) |
| < md | Chat full screen. The conversation list is a sheet; the Canvas opens as a **bottom sheet** (90dvh) from a floating "Live workflow" pill that shows the M2 ping while a Task runs (PRD FR-030: "on mobile, canvas is hidden or toggled") |

Width check: the chat column never drops below **560px** when the Canvas is docked (1280 − 264 sidebar − 400 Canvas = 616; 1536 − 264 − 280 − 420 = 572).

**Chat:**
- **Message column:** max 760px, messages gap 12px; group gap 24px; a date separator chip.
- **User bubble:** graphite-500 fill, white text, `radius-card`, the corner at the inline-end bottom is 6px. Max width 80% (90% < sm).
- **Assistant bubble:** white, 1px mist-400, graphite-500 text, the inline-start bottom corner is 6px.
- **Task status line:** centred chip (mist-200, 14px) with an icon. The **running** item uses M1-fast + M3; finished items get a mint check. Wrapped in `aria-live="polite"`.
- **Structured form** (FR-032): a standard card with a title, fields per §8.2, "Search yourself" per field, and Submit (primary) + "Search all yourself" (tertiary). One column always.
- **Deliverable card:** stroke card with a type icon tile, title, type chip and time; actions "View" (secondary) and "Send to Batch Review" (primary, content types only). Actions stack < sm.
- **Knowledge notification:** tint card "3 new knowledge items for review" + link to the Brain review.
- **Error message:** error state (§8.7) inline in the thread.
- **Composer:** sticky at the bottom; white, `radius-card`, 1px mist-400; autosizing textarea (max 6 lines, 16px); send = primary icon button 44px (flips in RTL); `Enter` sends, `Shift+Enter` adds a new line; disabled while sending. On mobile it respects the safe area and the keyboard (`dvh`).

**Canvas (stage):**
- **Background:** graphite-500 with a dotted grid of `white/6` 1px dots every 18px. **The graph container is `dir="ltr"`** (FTD §9.6); labels translated.
- **Header:** "Live workflow" + M2 "Live" while running; idle text graphite-300 otherwise; Replay (tertiary on dark) when a finished Task is shown.
- **Nodes** (Orchestrator larger: 200×64; sub-agents 168×56): graphite-800 card, 1px `white/10`, `radius-control`, 20px icon, label 14px white, state subtitle 12px.

  | State | Style |
  |---|---|
  | idle | 60% opacity, graphite-300 subtitle "Waiting" |
  | active | 1px mint-500 border + `glow-mint` + M2 dot; subtitle = **abstracted tool label** (FR-044) with M3 |
  | completed | mint-500 border at 50%, mint check badge |
  | error | danger-600 border + alert badge (only if the error is user-visible) |

- **Edges:** `white/18` 1.5px; active = mint-500 2px with M12; "delegated / returned" labels as 11px chips on hover/focus.
- **Layout:** Orchestrator at the top centre and sub-agents below. Below md, a vertical stack, with the graph fitted on resize.
- **Accessibility:** the Canvas has `role="img"` with an `aria-label` summary, plus a visually hidden live list of agent states ("Market Research: working — Searching Reddit").

### 10.5 Brain
- **Header:** title, the pending candidates badge, "Review candidates" (primary), "Add item" (secondary), a list/graph segmented control.
- **Filters bar:** search input (semantic search), category multi-select chips (§9.4), relationship-type select, status select. Below md the filters collapse into a "Filters" sheet with an applied-count badge.
- **Graph view** (light workspace, not stage):
  - mist-100 background with mist-400 dots
  - nodes are white cards with a 2px border in the category colour, icon + title (truncated at 2 lines) and a status chip
  - candidate nodes use a **dashed** border; retired nodes are hidden unless filtered
  - edges graphite-250 1.5px with a relation label chip
  - container `dir="ltr"`
  - zoom/fit controls at the inline-end bottom
  - **keyboard:** Tab through nodes, Enter opens the panel; "Add relationship" is also available as a form (no drag-only actions)
- **Item side panel:** sheet from the inline-end 480px with title, category chip, status, summary, properties (key/value list, editable), provenance (source, Snapshot link, captured at) and actions. Below md it's a full-screen sheet.
- **List view:** cards grouped by category with counts; default on mobile (the graph is still available).
- **Candidate Review:**
  - 7 sections (§9.5) as a vertical tab list ≥ md, or a select/accordion below md
  - each candidate is a card with Approve (primary), Edit (secondary, inline edit then "Approve edited"), Reject (danger)
  - **individual only, with no bulk controls** (D-24)
  - the next candidate gets focus after an action
- **Conflicts:** orange tint cards listing the conflicting items side by side (stacked < md) with a "Keep A / Keep B / Edit" decision.

### 10.6 Deliverables
- **List:** type filter chips + date range; a card grid (§4.4) of stroke cards with the type icon tile, title (2 lines), type chip and relative date. "Load more" is cursor based.
- **Viewer:**
  - reading width 800px; header with type chip, date and title
  - toolbar: Copy, Download `.md`, Print/PDF (stretch goal), Send to Batch Review
  - toolbar items collapse into a ⋯ menu < sm
  - "Built from N Brain facts" tint card (expandable chips)
  - body uses the Markdown typography (§3.3)
  - tables scroll horizontally inside their own container, never the page
  - sources/citations list at the end with external-link icons

### 10.7 Batch Review
- **Header:** title, session hint, view toggle (grid/list), and the bulk bar "Approve all remaining (n)" (primary) / "Reject all remaining" (danger).
- **The bulk bar is sticky:** at the top on desktop; on mobile it's a bottom action bar with safe-area padding.
- **Post card** (stroke card):
  - platform icon + name, Scheduled Time in the **Business Time Zone with the zone label**, status pill (§9.1)
  - a **platform preview** that keeps the platform's own look (Facebook/Instagram/X), so it is *not* re-themed
  - the Brand Visual image (Instagram 4:5, Facebook 1.91:1, X 16:9) with alt text
  - actions: Approve (primary), Edit (secondary, opens the editor with a live preview), Reject (danger), and a ⋯ menu for Regenerate / Reschedule / Cancel / Retry, shown only when the status allows (§9.1)
- **Failure reason:** danger tint row under the preview, with Retry.
- **Lazy connect prompt:** orange tint card "Connect Instagram to publish" + Connect (primary).
- **Grid:** 1 column < md, 2 columns md–xl, 3 columns ≥ xl. List view: dense rows (preview thumbnail 64px).

### 10.8 Settings
- **Layout:** vertical tabs (Account · Connected Accounts · Business Time Zone · Language & accessibility) ≥ md; a select/accordion below md; content max 720px.
- **Connected Accounts:** one card per platform with brand icon, account name, status (§9.3) and Connect / Reconnect / Disconnect (danger, with a confirmation dialog).
- **Business Time Zone:** a searchable select of IANA zones showing the current local time preview, and a note that approved/scheduled Posts don't move.
- **Language & accessibility:** a UI language segmented control (switches `lang`/`dir` without a reload) and the **"Reduce animations"** switch (§7.5).

### 10.9 Notifications
- A bell in the top bar with a count badge, opening a popover ≥ md or a full sheet < md.
- Items: 56px rows, icon by kind, title, relative time, unread dot (mint-700) + bold title; "Mark all read".
- Each links to its subject (the Brain review, a deliverable, Batch Review).

### 10.10 Landing page (marketing)
| # | Section | Surface | Content and motion | Responsive |
|---|---|---|---|---|
| 1 | Top bar | stage | §8.5 marketing top bar | drawer < lg |
| 2 | **Hero** | stage + hero ring + M8 glows | Section tag ("AI Marketing Employee · Arabic-first"), Display headline with the key phrase in mint-500 and **M4** cursor, Body-L in graphite-100, CTAs: "Get started" (primary lg) + "Try the demo" (secondary lg). Visual: **M1 orbit** with the BusMora core (mint glow, logo) and 5 agent chips on the inner ring, translated labels. Pause-animations toggle. | ≥ lg: two columns (text inline-start, orbit inline-end). < lg: centred text, orbit below scaled to the container (SVG viewBox, max 420px). < sm: CTAs full width stacked; orbit max 300px, outer ring hidden |
| 3 | Proof strip | light | 3 stroke stat cards with **M6** count-up: "9 knowledge categories", "5 AI agents", "2 languages" | 3 columns ≥ md, 1 column < md |
| 4 | How it works | light | Section tag + Title-1 + 3 numbered stroke cards (Learn → Work → Publish) with **M5** stagger | 3 columns ≥ lg, 1 column below |
| 5 | Reads your website | stage | Text + a mock browser card with **M7** scan over text lines → extracted fact chips appear (M5) | two columns ≥ lg, stacked below |
| 6 | Watch it work | light | A static Canvas illustration (stage panel) with one active node (M2) and a mock chat | two columns ≥ lg |
| 7 | Governed publishing | light | Facebook-style post preview card + "Approve → Publish → Verified" steps with mint checks | stacked < lg |
| 8 | Final CTA | stage + hero ring | Title-1 + primary lg button | — |
| 9 | Footer | graphite-600 | Links (graphite-200), language toggle, copyright (graphite-300) | columns collapse to accordion-free stacked lists |

**Hero performance:** the headline is plain text, which is the LCP element. The orbit is inline SVG/CSS (no image, no canvas), and the ring/glows are CSS gradients.

### 10.11 System pages
- **404 / 500:** stage mini-hero with the hero ring, a short message, primary "Go to workspace" and secondary "Contact support".
- **Offline / network error:** inline banner (orange tint) "You're offline — we'll retry automatically" (FTD §9.4).

---

## 11. Bilingual and RTL rules

1. `<html lang dir>` comes from the UI language (FTD §9.6). Switching it changes the font stack automatically (§3.1).
2. **Logical utilities only:** `ms/me/ps/pe/start/end/text-start/text-end/border-s/border-e/rounded-s/rounded-e/inset-inline`. Physical left/right utilities are forbidden (enforced by lint, FTD FG-6).
3. **What mirrors:** layout order, sidebars and sheets (inline-start/inline-end), directional icons (§6), the shimmer direction (M3), bubble corners, and breadcrumb separators.
4. **What never mirrors:** graphs (Canvas, Brain) inside `dir="ltr"` containers, the M1 orbit geometry, platform previews' internal layout (they follow the post's own text direction), media controls, charts, logos.
5. **Generated or user text** uses `dir="auto"` on its block. Mixed-direction strings (Arabic with English product names) are never split.
6. **Numbers and dates:**
   - Format with `Intl` in the UI locale (`ar-EG` / `en`), so Arabic UI shows Arabic-Indic digits for dates and counts.
   - **Inputs accept both** digit sets.
   - Prices and units always come from the source text unchanged.
   - Times are always shown in the **Business Time Zone with the zone label** (FTD §9.6).
7. **Text expansion:** English strings are often 20–30% longer than Arabic ones. Layouts never rely on fixed text widths; buttons grow and labels truncate with an ellipsis + tooltip only in dense lists.

---

## 12. Accessibility (WCAG 2.2 AA)

**Target:** WCAG 2.2 Level AA across all screens, in both languages.

### 12.1 Perception
- **Colour contrast:** only the pairs in §2.4.
- **Colour is never the only signal** (§9): every status and category has an icon + label.
- **Text resizing to 200%** and **reflow at 320 CSS px** without horizontal page scrolling. Tables, code blocks and graphs scroll inside their own containers.
- **Images:** Brand Visuals have alt text (the post's first line plus "Brand visual"); decorative visuals (orbit, glows, ring, scan mock) use `aria-hidden="true"`.

### 12.2 Focus and keyboard
- **Focus ring:** `outline: 2px solid` ring token (mint-700 on light, mint-500 on stage) with `outline-offset: 2px`, shown on `:focus-visible` only, **never removed**. Focused elements are never hidden under sticky bars (`scroll-margin` = sticky height + 8px).
- **Full keyboard operation:** a skip link to main content, logical tab order following the reading direction, Esc closes overlays, arrow keys in menus/tabs/segmented controls, and Enter/Space on all controls.
- **Graphs:** everything doable by dragging has a non-drag alternative (WCAG 2.5.7): node editing via the panel, relationship creation via a form, zoom via buttons.

### 12.3 Targets and input
- **Interactive targets ≥ 44×44 px** (above the 2.5.8 minimum of 24×24). Dense icon buttons keep a 44px hit area.
- **Forms:** visible labels (placeholders are not labels), errors described in text and linked with `aria-describedby`, the first invalid field gets focus on submit, and inputs have `autocomplete` and appropriate `inputmode`.
- **No time limits** in the UI. The Clarification 7-day expiry is shown as information, not as a countdown.

### 12.4 Dynamic content
- **Chat thread:** `role="log"`; Task status updates are `aria-live="polite"`; errors are `role="alert"`.
- **Canvas:** a hidden live list of agent states (§10.4).
- **Toasts:** `role="status"` (errors `role="alert"`).
- **Loading regions:** `aria-busy="true"` with visible text ("Loading deliverables…").
- **Motion:** §7.5 (system preference, in-app setting, landing pause control; the cursor blink stops after 5 blinks).

### 12.5 Structure
- **Landmarks:** `header`, `nav` (labelled "Main"), `main`, `aside` (Canvas labelled "Live workflow"), `footer`.
- **Headings:** one `h1` per page, with no skipped levels.
- **Page titles:** `<title>` = "Page – BusMora" in the UI language.

### 12.6 Verification (definition of done for every screen)
- Automated: axe-core scan with zero serious/critical issues (the tool is part of FTD gap FG-5).
- Manual keyboard pass: every action reachable, focus always visible.
- Screen-reader smoke test: NVDA + Firefox (Windows) and VoiceOver + Safari (iOS) in Arabic and English.
- Zoom 200% and a 320px width check.
- Reduced-motion check.

---

## 13. Browser support and performance

### 13.1 Supported browsers (resolves the browser part of FG-11)
| Browser | Versions |
|---|---|
| Chrome, Edge | last 2 major versions |
| Firefox | last 2 major versions |
| Safari (macOS and iOS) | 16.4 and newer |
| Samsung Internet | last 2 major versions |

The design relies on: CSS logical properties, `dvh` units, `:focus-visible`, `backdrop-filter` (with `-webkit-` prefix and an opaque fallback), `background-clip: text` (with `-webkit-` prefix), CSS custom properties, and IntersectionObserver. All are available in the versions above. **No reliance on** `:has()` for core layout, container queries, or view transitions.

### 13.2 Performance budgets
| Metric | Budget |
|---|---|
| LCP (landing, 4G mid-range phone) | ≤ 2.5s |
| CLS | ≤ 0.1 (fonts via `next/font` with size-adjusted fallbacks; images with explicit dimensions) |
| INP | ≤ 200ms |
| Fonts | 2 families, ≤ 7 weight files total, subset per script, preloaded by `next/font` |
| Continuous animation groups visible | landing ≤ 3 groups (orbit, ambient glows, scan); app ≤ 2 (§7.1) |
| Blur | at most 3 blurred glow elements per viewport; none below `md` |

**Animations:**
- use `will-change: transform` only on M1 rings while running
- off-screen ones are paused (§7.1)
- no JavaScript animation loops except M6 count-up (which stops when done)

---

## 14. Implementation

### 14.1 Where things live (follows FTD §4)
| Concern | Location |
|---|---|
| Fonts (`next/font`), `<html lang dir data-motion>` | `src/app/layout.tsx` |
| Tokens, keyframes, base styles, reduced-motion rules | `src/app/globals.css` (single source) |
| shadcn component config | `components.json` → `shared/ui` |
| Owned components (§8) | `src/shared/ui/*` |
| Motion helpers (Reveal, LiveDot, Shimmer, CountUp, usePausedWhenHidden) | `src/shared/ui/motion/*` |
| Graph styling (Canvas/Brain node + edge components) | `src/shared/graph/*` |
| Markdown typography | `src/shared/markdown/*` |
| Landing visuals (Orbit, ScanCard, HeroRing) | `src/app/(public)/_components/*` or a `marketing` feature folder |

### 14.2 Token stylesheet
For Tailwind CSS v4 (CSS-first) and shadcn/ui variable names. If the project pins Tailwind v3, the same values go into `tailwind.config` `theme.extend`, and the `:root` variables stay identical.

```css
@import "tailwindcss";

/* ---------- Primitive + semantic tokens ---------- */
@theme {
  /* Mint */
  --color-mint-50: #e9fcf6;  --color-mint-100: #d2f8ee; --color-mint-200: #a4f1dd;
  --color-mint-300: #77eacd; --color-mint-400: #49e3bc; --color-mint-500: #1cdcab;
  --color-mint-600: #16b089; --color-mint-700: #0f7c61; --color-mint-800: #0b5844;
  --color-mint-900: #083d30; --color-mint-950: #0b1512;
  /* Graphite */
  --color-graphite-100: #d3d4d5; --color-graphite-200: #a7a9ac; --color-graphite-250: #8a8f94;
  --color-graphite-300: #7b7f82; --color-graphite-350: #686d72; --color-graphite-400: #4f5459;
  --color-graphite-500: #0d0d0d; --color-graphite-600: #0a0a0a; --color-graphite-700: #080808;
  --color-graphite-800: #141516; --color-graphite-900: #030303;
  /* Mist */
  --color-mist-100: #f7f9fc; --color-mist-200: #f0f4f8; --color-mist-300: #e8eef5;
  --color-mist-400: #e1e9f1; --color-mist-500: #d9e3ee;
  /* Accents */
  --color-sky-100: #d8f1ff; --color-sky-500: #3dbbff; --color-sky-600: #3196cc; --color-sky-700: #257099; --color-sky-800: #184b66;
  --color-orange-100: #ffe1d2; --color-orange-500: #ff6a1f; --color-orange-600: #cc5519; --color-orange-700: #994013; --color-orange-800: #662a0c;
  --color-purple-100: #f4dffe; --color-purple-500: #c75df9; --color-purple-600: #9f4ac7; --color-purple-700: #773895; --color-purple-800: #502564;
  /* Danger */
  --color-danger-50: #fef3f2; --color-danger-200: #fecdca; --color-danger-600: #d92d20; --color-danger-700: #b42318;

  /* Type */
  --font-sans: var(--font-active), system-ui, sans-serif;
  /* Breakpoint added to Tailwind defaults */
  --breakpoint-xs: 30rem;
  /* Radius */
  --radius-control: 1rem; --radius-card: 1.5rem; --radius-panel: 2rem;
  /* Shadows */
  --shadow-card: 0 8px 50px rgba(0, 0, 0, 0.05);
  --shadow-pop: 0 12px 32px rgba(13, 13, 13, 0.12), 0 2px 6px rgba(13, 13, 13, 0.06);
  --shadow-primary: 0 4px 12px rgba(28, 220, 171, 0.30);
  --shadow-dark: 0 4px 8px rgba(0, 60, 44, 0.64);
  --shadow-glow-mint: 0 0 24px rgba(28, 220, 171, 0.35);
  /* Easing */
  --ease-brand: cubic-bezier(.77, 0, .175, 1);
  --ease-out-quart: cubic-bezier(.165, .84, .44, 1);
  --ease-ping: cubic-bezier(0, 0, .2, 1);
  /* Animations (keyframes in §7.3, declared below) */
  --animate-orbit: bm-spin 30s linear infinite;
  --animate-orbit-counter: bm-spin-reverse 30s linear infinite;
  --animate-orbit-outer: bm-spin-reverse 48s linear infinite;
  --animate-spinner: bm-spin .9s linear infinite;
  --animate-live-ping: bm-ping 1s var(--ease-ping) infinite;
  --animate-shimmer: bm-shimmer 2.2s linear infinite;
  --animate-cursor: bm-cursor 1s steps(1) 5;
  --animate-reveal: bm-fade-up .6s var(--ease-out-quart) both;
  --animate-grow-y: bm-grow-y .8s ease-out both;
  --animate-drift: bm-drift 14s ease-in-out infinite;
  --animate-scan: bm-scan 3.2s ease-in-out infinite;
  --animate-glow: bm-glow 2.4s ease-in-out infinite;
}

/* ---------- shadcn/ui semantic variables (light workspace) ---------- */
:root {
  --font-active: var(--font-plex-arabic), var(--font-ubuntu);
  --background: #f7f9fc;      --foreground: #0d0d0d;
  --card: #ffffff;            --card-foreground: #0d0d0d;
  --popover: #ffffff;         --popover-foreground: #0d0d0d;
  --primary: #1cdcab;         --primary-foreground: #0d0d0d;
  --secondary: #f0f4f8;       --secondary-foreground: #0d0d0d;
  --muted: #f0f4f8;           --muted-foreground: #686d72;
  --accent: #d2f8ee;          --accent-foreground: #0b5844;
  --destructive: #d92d20;     --destructive-foreground: #ffffff;
  --border: #e1e9f1;          --input: #8a8f94;   --ring: #0f7c61;
  --radius: 1rem;
  --sidebar: #0d0d0d;         --sidebar-foreground: #a7a9ac;
  --sidebar-primary: #1cdcab; --sidebar-primary-foreground: #0d0d0d;
  --sidebar-accent: rgba(255, 255, 255, 0.05); --sidebar-accent-foreground: #ffffff;
  --sidebar-border: rgba(255, 255, 255, 0.10); --sidebar-ring: #1cdcab;
  --chart-1: #0f7c61; --chart-2: #257099; --chart-3: #9f4ac7; --chart-4: #cc5519; --chart-5: #4f5459;
}
html[dir="ltr"] { --font-active: var(--font-ubuntu), var(--font-plex-arabic); }

/* Dark stage scope: hero, top bar, sidebar, Canvas, footer */
.stage {
  --background: #0d0d0d;  --foreground: #ffffff;
  --card: #141516;        --card-foreground: #ffffff;
  --muted: rgba(255, 255, 255, 0.05); --muted-foreground: #a7a9ac;
  --border: rgba(255, 255, 255, 0.10); --input: rgba(255, 255, 255, 0.40); --ring: #1cdcab;
  background-color: var(--background); color: var(--foreground);
}

@theme inline {
  --color-background: var(--background);  --color-foreground: var(--foreground);
  --color-card: var(--card);              --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);        --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);        --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);    --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);            --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);          --color-accent-foreground: var(--accent-foreground);
  --color-destructive: var(--destructive);
  --color-border: var(--border);          --color-input: var(--input);  --color-ring: var(--ring);
  --color-sidebar: var(--sidebar);        --color-sidebar-foreground: var(--sidebar-foreground);
  --color-sidebar-primary: var(--sidebar-primary);
  --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
  --color-sidebar-accent: var(--sidebar-accent);
  --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
  --color-sidebar-border: var(--sidebar-border); --color-sidebar-ring: var(--sidebar-ring);
}

/* ---------- Base ---------- */
html { -webkit-text-size-adjust: 100%; }
body {
  background: var(--background); color: var(--foreground);
  font-family: var(--font-sans); line-height: 1.5;
  -webkit-font-smoothing: antialiased; text-rendering: optimizeLegibility;
}
html[dir="rtl"] body { line-height: 1.7; }
h1, h2, h3, h4 { line-height: 1.2; font-weight: 500; }
html[dir="rtl"] :is(h1, h2, h3, h4) { font-weight: 600; }
:focus-visible { outline: 2px solid var(--ring); outline-offset: 2px; }
::selection { background: rgba(28, 220, 171, 0.30); }

/* ---------- Keyframes (§7.3) ---------- */
/* paste the §7.3 block here */

/* ---------- Signature helpers ---------- */
.stroke-card {
  padding: 1px; border-radius: var(--radius-panel);
  background-image: linear-gradient(135deg, #f7f9fc, #e1e9f1 50%, #f7f9fc);
  transition: background-image .3s var(--ease-brand), transform .3s var(--ease-brand);
}
.stroke-card > * { border-radius: calc(var(--radius-panel) - 1px); background: var(--card); }
.stroke-card:is(:focus-within, [data-selected="true"]) { background-image: linear-gradient(135deg, #f7f9fc, #1cdcab 50%, #f7f9fc); }
@media (hover: hover) {
  .stroke-card:hover { background-image: linear-gradient(135deg, #f7f9fc, #1cdcab 50%, #f7f9fc); transform: translateY(-2px); }
}
.shimmer-text {
  background: linear-gradient(90deg, currentColor 40%, #1cdcab 50%, currentColor 60%) 0 0 / 200% 100%;
  -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent;
  animation: var(--animate-shimmer);
}
html[dir="rtl"] .shimmer-text { animation-direction: reverse; }

/* ---------- Motion control (§7.5) ---------- */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: .001ms !important; animation-iteration-count: 1 !important;
    transition-duration: .001ms !important; scroll-behavior: auto !important;
  }
  .shimmer-text { -webkit-text-fill-color: currentColor; background: none; }
}
html[data-motion="reduced"] *, html[data-motion="reduced"] *::before, html[data-motion="reduced"] *::after {
  animation-duration: .001ms !important; animation-iteration-count: 1 !important; transition-duration: .001ms !important;
}
html[data-motion="reduced"] .shimmer-text { -webkit-text-fill-color: currentColor; background: none; }
[data-motion="paused"] *, [data-motion="paused"] *::before, [data-motion="paused"] *::after { animation-play-state: paused !important; }
```

### 14.3 Fonts (`app/layout.tsx`)
- `Ubuntu({ weight: ['400','500','700'], subsets: ['latin','latin-ext'], variable: '--font-ubuntu', display: 'swap' })`
- `IBM_Plex_Sans_Arabic({ weight: ['400','500','600','700'], subsets: ['arabic'], variable: '--font-plex-arabic', display: 'swap' })`
- Apply both `.variable` class names to `<html>`, together with `lang`, `dir` and `data-motion`.

### 14.4 Build order
| Step | Scope | Done when |
|---|---|---|
| 1 | Tokens (§14.2), fonts (§14.3), base styles, keyframes, motion control | Tokens render; contrast spot-check matches §2.4; reduced motion works |
| 2 | Core components §8.1–§8.4 and §8.6–§8.8 in `shared/ui`, with RTL/LTR stories | All variants and states exist; keyboard and focus verified |
| 3 | App shell §10.1 + navigation §8.5 | 320px → 1536px verified in both languages |
| 4 | Status/category systems §9 as shared components (`StatusPill`, `CategoryChip`, `CategoryIcon`) | Every enum value renders with icon + label |
| 5 | Screens §10.2–§10.9 (with their features) | Each screen passes §12.6 |
| 6 | Graph styling (§10.4 Canvas, §10.5 Brain) in `shared/graph` | Graph accessible list + keyboard alternatives work |
| 7 | Landing §10.10 + motion helpers | LCP/CLS budgets (§13.2) met; pause control works |

### 14.5 QA matrix (every screen)
- **Viewports:** 320 · 375 · 768 · 1024 · 1280 · 1536 px.
- **Languages:** Arabic (RTL) · English (LTR), including mixed-direction content.
- **Settings:** default · reduced motion · 200% zoom.
- **Input:** mouse · keyboard only · touch.
- **Browsers:** §13.1 (at minimum Chrome, Safari iOS, Firefox).

---

## 15. Deviations from the references

| Reference value | Where | Changed to | Why |
|---|---|---|---|
| mint-700 `#118467` as text/hover | Talents Arena | `#0f7c61` | 4.40:1 on the page background → 4.88:1 (AA) |
| Hover text mint-300 on mint-700 | Talents Arena buttons | mint-100 | 3.5:1 → 4.5:1 |
| Mint-500 focus/selection on light | — | mint-700 ring | 1.7:1 fails the 3:1 non-text rule |
| Input borders in light greys | Talents Arena / common practice | graphite-250 `#8a8f94` (new step) | 1.3:1 → 3.3:1 (WCAG 1.4.11) |
| graphite-300 meta text on light | — | graphite-350 `#686d72` (new step) | 3.8:1 → 5.0:1 |
| graphite-800 `#050505` | Talents Arena | `#141516` | Elevated dark cards need visible separation from the `#0d0d0d` stage |
| Ubuntu for Arabic text | Talents Arena (Ubuntu only) | IBM Plex Sans Arabic for Arabic | Ubuntu has no Arabic glyphs; the reference falls back to system fonts |
| Headings weight 500 | Talents Arena | 500 Latin / **600 Arabic** | Plex Arabic 500 reads lighter than Ubuntu 500 |
| Continuous decorative motion | Firecrawl | Pausable, reduced-motion aware, landing only; cursor stops after 5 blinks | WCAG 2.2.2 and 2.3.3, and calm product screens |
| Image-based hero ring | Talents Arena (webp) | CSS radial gradient | No image request; scales to any width |
| Error/destructive colours | Not defined in the references | danger family (§2.1) | Needed for failed/rejected states and destructive actions |

---

## 16. Open items

| # | Item | Needed for |
|---|---|---|
| DS-1 | Logo and wordmark (current placeholder: icon tile + "BusMora" in Ubuntu 700) | top bars, favicon, Brand Visual defaults |
| DS-2 | Illustration/screenshot set for landing sections 5–7 (the specs above use built-in mock UI) | landing polish |
| DS-3 | Dark theme for the whole app (tokens are structured to allow it; not in scope) | future |
| DS-4 | Email templates (verification, reset) in this visual language | auth emails (provider still open, OD-10) |
| DS-5 | Mapping of the 9 categories to the 7 Review sections | §9.5 (OD-18) |
| DS-6 | Accessibility test tooling choice (axe-core in CI) | §12.6 (FTD FG-5) |
