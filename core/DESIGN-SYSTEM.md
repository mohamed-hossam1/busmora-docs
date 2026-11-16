# BusMora Design System 3.0 — Cosmic Director

> **Version:** 3.0  
> **Status:** Official visual system; landing-page and component-foundation baseline implemented  
> **Date:** 2026-11-16  
> **Applies to:** `busmora-web` (Next.js 16 App Router, React 19, TypeScript, Tailwind CSS v4, shadcn/ui owned in `src/shared/ui`, Radix UI primitives)  
> **Product authority:** [PRD v2.0](./prd.md), [Frontend Technical Design](./FRONTEND-TECHNICAL-DESIGN.md), and the [domain glossary](../domain/GLOSSARY.md)

Design System 3.0 establishes BusMora's Electric Purple and Cosmic Dark identity. It adopts the visual language demonstrated by `/home/mohamed/Templates/test`: a deep cosmic stage, a nested pale-lavender work surface, floating adaptive navigation, product-led bento storytelling, layered dashboard scenes, restrained conic light effects, sticky narrative cards, and perspective presentation.

The reference is a visual and interaction source—not a product or content source. BusMora uses original domain-correct UI scenes and never copies ADYOUNEED artwork, trademarks, screenshots, claims, pricing, or advertising workflows.

---

## 1. Design thesis

### 1.1 The stage and the workbench

BusMora has two coordinated surface modes:

1. **Cosmic stage** — `#080911` and `#171926`. It frames the brand, navigation, Marketing Director Canvas, hero scenes, and presentation moments.
2. **Operational workbench** — `#edf0fe` and white. It holds forms, evidence, Roadmap steps, Brain records, Deliverables, Scheduling, tables, and governed decisions.

Marketing pages may literally nest the light workbench inside the dark stage. Authenticated product screens interpret the same system more conservatively: dark structural chrome around a predominantly light workspace.

### 1.2 Product character

BusMora should feel strategic rather than magical, premium rather than ornamental, alive while work is happening, calm at rest, Arabic-first, and explicit about evidence, uncertainty, versions, authority, and recovery.

Visual spectacle must never hide domain truth. `Review`, publishing `Approval`, Roadmap activation, budget extension, and owner attestation are distinct decisions and must remain visually and verbally distinct.

### 1.3 Product as proof

Marketing visuals reconstruct real BusMora experiences using semantic HTML and owned components:

- Marketing Director Conversation and live Canvas;
- Onboarding Draft confirmation;
- Adaptive Marketing Roadmap;
- Business Brain provenance and Pending Brain Updates;
- Deliverables with citations and limitations;
- Scheduling and exact-version Batch Review.

Every non-live scene is labelled **Illustrative product demonstration**. It must not claim fabricated activity, agent work, performance, publication, or business impact.

---

## 2. Colour system

### 2.1 Exact primitive palette

These values are immutable brand primitives.

| Token | Value | Role |
|---|---:|---|
| `purple-electric` | `#6437fc` | primary action, brand light, active state |
| `purple-accent` | `#6742d1` | secondary purple, CTA gradient stop |
| `purple-hover` | `#5229db` | darker purple hover state |
| `purple-glow` | `rgba(100, 55, 252, 0.35)` | primary ambient glow |
| `purple-focus` | `#8f70ff` | accessible focus on Cosmic Deep |
| `cosmic-deep` | `#080911` | deepest page and stage background |
| `cosmic-canvas` | `#171926` | outer brand canvas |
| `cosmic-card` | `#121520` | dark card and dashboard surface |
| `cosmic-raised` | `#1b1d30` | elevated dark surface |
| `cosmic-active` | `#212936` | active/selected dark surface |
| `lavender-canvas` | `#edf0fe` | nested light workbench surface |
| `lavender-alt` | `#edf0fd` | light illustration, feature card and tint surface |
| `lavender-tint` | `#eaeeff` | subtle dividers, step pills, and gradient transitions |
| `cyan-electric` | `#00b1ff` | process, live work, information |
| `pink-electric` | `#ff3067` | destructive/error on dark surfaces |
| `gold-electric` | `#e5b869` | warning, awaiting decision |
| `slate-structure` | `#42516b` | light-surface text, control borders, and structure |
| `stage-muted` | `#bdc5e6` | secondary copy on dark surfaces |
| `stage-subtle` | `#a6afd5` | tertiary captions, input hints, subtle borders |

The multi-tier dashboard gradient is exact:

```css
linear-gradient(115deg, #2b2648 0%, #24213d 36%, #1a1e29 72%)
```

### 2.2 Semantic shadcn contracts

Shared components consume semantic variables, never primitive hex values.

| Contract | Light workbench | Cosmic stage |
|---|---|---|
| `--background` | `#edf0fe` | `#080911` |
| `--foreground` | `#171926` | `#ffffff` |
| `--card` | `#ffffff` | `#121520` |
| `--card-foreground` | `#171926` | `#ffffff` |
| `--popover` | `#ffffff` | `#1b1d30` |
| `--popover-foreground` | `#171926` | `#ffffff` |
| `--primary` | `#6437fc` | `#6437fc` |
| `--primary-foreground` | `#ffffff` | `#ffffff` |
| `--secondary` | `#e3e7fb` | `#212936` |
| `--secondary-foreground` | `#25283a` | `#ffffff` |
| `--muted` | `#e6e9f8` | `#1b1d30` |
| `--muted-foreground` | `#42516b` | `#bdc5e6` |
| `--accent` | `#dce6ff` | `#212936` |
| `--accent-foreground` | `#263450` | `#ffffff` |
| `--destructive` | `#c81e4b` | `#ff3067` |
| `--destructive-foreground` | `#ffffff` | `#080911` |
| `--border` | `#7d88a5` | `rgb(255 255 255 / 16%)` |
| `--input` | `#6e7b9a` | `rgb(255 255 255 / 24%)` |
| `--ring` | `#6437fc` | `#8f70ff` |
| `--sidebar` | `#121520` | `#080911` |
| `--sidebar-foreground` | `#ffffff` | `#ffffff` |
| `--sidebar-primary` | `#6437fc` | `#6437fc` |
| `--sidebar-primary-foreground` | `#ffffff` | `#ffffff` |
| `--sidebar-accent` | `#212936` | `#212936` |
| `--sidebar-accent-foreground` | `#ffffff` | `#ffffff` |
| `--sidebar-border` | `rgb(255 255 255 / 14%)` | `rgb(255 255 255 / 14%)` |
| `--sidebar-ring` | `#8f70ff` | `#8f70ff` |

Exact reference colours remain available as primitives. Semantic values may use an accessible palette member or opaque derivative when a literal pairing would fail WCAG 2.2 AA.

### 2.3 Accent meanings

- **Purple:** primary action, selection, authored BusMora intelligence.
- **Cyan:** active process, live connection, informational evidence.
- **Gold:** awaiting owner decision, warning, hold, partial state.
- **Pink:** destructive action, terminal error, rejection.
- **Slate:** structure and secondary information on light surfaces.

Colour is never the only status signal. Pair it with an icon and translated label.

### 2.4 Verified contrast

| Pair | Ratio | Use |
|---|---:|---|
| white on Electric Purple | 6.01:1 | primary controls |
| Cosmic Canvas on Lavender Canvas | 15.37:1 | light-workbench primary text |
| Slate Structure on Lavender Canvas | 7.06:1 | secondary text |
| Stage Muted on Cosmic Canvas | 10.21:1 | dark-stage secondary text |
| Purple Focus on Cosmic Deep | 5.61:1 | dark focus indicator |
| `#7d88a5` on Lavender Canvas | 3.12:1 | control boundaries |
| `#6e7b9a` on Lavender Canvas | 3.73:1 | input boundaries |
| cyan / gold / pink on Cosmic Card | 7.57 / 9.87 / 5.09:1 | status text/icons |

Low-opacity borders may be decorative only. Inputs, focus indicators, selected controls, and essential boundaries must meet 3:1.

### 2.5 Strict shadcn Token System & Prohibition of Arbitrary Tag Styles

To ensure standard, maintainable UI architecture across `busmora-web`, **no hardcoded hex values, arbitrary bracketed colors (`[#hex]`), inline color styles (`style={{ color: ... }}`), or arbitrary font styles may appear directly on JSX tags**. All styling must consume standard shadcn tokens and design-system theme tokens registered in `src/app/globals.css`.

#### Token-to-Class Mapping Matrix

| Brand Primitive / Token | CSS Variable | shadcn / Tailwind Utility Class | Semantic Role |
|---|---|---|---|
| `cosmic-deep` | `--cosmic-deep` | `bg-cosmic-deep`, `text-cosmic-deep` | Deepest page canvas (`#080911`) |
| `cosmic-canvas` | `--cosmic-canvas` | `bg-cosmic-canvas`, `text-cosmic-canvas` | Outer brand canvas (`#171926`) |
| `cosmic-card` | `--cosmic-card` | `bg-cosmic-card`, `border-cosmic-card` | Dark cards and dashboard frames (`#121520`) |
| `cosmic-raised` | `--cosmic-raised` | `bg-cosmic-raised` | Elevated dark cards and dropdowns (`#1b1d30`) |
| `cosmic-active` | `--cosmic-active` | `bg-cosmic-active` | Active/selected elements and pills (`#212936`) |
| `lavender-canvas` | `--lavender-canvas` | `bg-lavender-canvas` | Nested light workbench surface (`#edf0fe`) |
| `lavender-alt` | `--lavender-alt` | `bg-lavender-alt` | Capabilities grid card surface (`#edf0fd`) |
| `lavender-tint` | `--lavender-tint` | `bg-lavender-tint`, `border-lavender-tint` | Subtle dividers and step pills (`#eaeeff`) |
| `purple-electric` | `--purple-electric` / `--primary` | `text-primary`, `bg-primary`, `text-purple-electric` | Primary brand action and AI intelligence (`#6437fc`) |
| `purple-accent` | `--purple-accent` | `bg-purple-accent`, `hover:bg-purple-hover` | Button gradient endpoints (`#6742d1` / `#5229db`) |
| `purple-focus` | `--purple-focus` / `--ring` | `text-purple-focus`, `text-ring` | High-contrast accessible focus ring and icon accent (`#8f70ff`) |
| `purple-glow` | `--purple-glow` | `bg-purple-glow`, `shadow-primary/40` | Primary ambient glow (`rgba(100, 55, 252, 0.35)`) |
| `cyan-electric` | `--cyan-electric` | `text-cyan-electric`, `stroke-cyan-electric` | Active processes and connections (`#00b1ff`) |
| `pink-electric` | `--pink-electric` | `text-pink-electric`, `bg-pink-electric` | Errors, rejections, destructive states (`#ff3067`) |
| `gold-electric` | `--gold-electric` | `text-gold-electric`, `border-gold-electric` | Review, pending approval, warnings (`#e5b869`) |
| `slate-structure` | `--slate-structure` / `--muted-foreground` | `text-slate-structure`, `border-slate-structure` | Secondary typography & borders on light surfaces (`#42516b`) |
| `stage-muted` | `--stage-muted` | `text-stage-muted` | Secondary copy on dark stages (`#bdc5e6`) |
| `stage-subtle` | `--stage-subtle` | `text-stage-subtle` | Tertiary captions, input hints (`#a6afd5`) |

Opacity modifiers are fully supported by Tailwind v4 via native alpha syntax (e.g., `border-slate-structure/40`, `bg-cosmic-canvas/95`, `bg-primary/20`).

#### Registered Surface Utility Classes

Complex multi-stop or radial gradients are standardized in `globals.css` rather than declared on tags:

| Utility Class | Implementation | Usage |
|---|---|---|
| `.interactive-tabs-bg` | `linear-gradient(115deg, #2b2648 0%, #24213d 36%, #1a1e29 72%)` | Multi-tier interactive tabs dark frame |
| `.testimonial-card-bg` | `radial-gradient(circle at 10% 10%, rgba(66,81,107,0.4), rgba(47,58,77,0.4))` | Customer review masonry card surface |
| `.bottom-cta-panel-bg` | `radial-gradient(circle at 50% 20%, rgba(103,56,252,0.62), transparent 34%), linear-gradient(145deg, #171b36 4%, #30256f 50%, #182d55 100%)` | Perspective bottom conversion container |
| `.card-radial-lavender` | `radial-gradient(circle at 100% 100%, var(--lavender-canvas), rgba(255,255,255,0))` | Light workbench feature cards |
| `.card-radial-lavender-subtle` | `radial-gradient(circle at 50% 100%, rgba(237,240,254,0.6), rgba(237,240,254,0.28))` | Wide workbench banner cards |
| `.process-flow-stage-bg` | `radial-gradient(circle at 50% 0, rgba(216,223,255,0.8), rgba(255,255,255,0.4))` | Step-by-step interactive workflow stage |
| `.process-flow-card-bg` | `linear-gradient(to top, var(--lavender-tint), #ffffff)` | Step card presentation shell |
| `.glass-pill` | `background: rgba(14, 17, 27, 0.75); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.09);` | Floating pills, status capsules, and subtle dark badges |
| `.glass-card` | `background: var(--cosmic-canvas); border: 1px solid rgba(255, 255, 255, 0.08);` | Translucent dark cards on cosmic background |
| `.light-card` | `background: #ffffff; border: 1px solid #e9edf5; box-shadow: 0 4px 20px -2px rgba(15, 23, 42, 0.05);` | Standard product card on Lavender workbench |

---

## 3. Typography

### 3.1 Families

| Script | Family | Loading |
|---|---|---|
| Latin | **Inter** | variable font through `next/font/google` |
| Arabic | **IBM Plex Sans Arabic** | 400, 500, 600, 700 through `next/font/google` |

Both are self-hosted by Next.js and exposed as `--font-inter` and `--font-plex-arabic`.

```css
:root { --font-active: var(--font-plex-arabic), var(--font-inter), system-ui, sans-serif; }
html[dir="ltr"] { --font-active: var(--font-inter), var(--font-plex-arabic), system-ui, sans-serif; }
```

Rules:

- Arabic never receives uppercase transformation, italics, or negative tracking.
- Use `rtl:tracking-normal` on editorial headings.
- Arabic body copy uses 1.6–1.75 line height; Latin uses 1.45–1.6.
- Generated/user content receives `dir="auto"`.
- Mixed Arabic, English product names, URLs, currencies, dates, and times remain in one semantic string when possible.

### 3.2 Editorial marketing scale

| Role | Size | Weight | Line height |
|---|---|---:|---:|
| Hero display | `clamp(3.1rem, 7vw, 7.25rem)` | 600 | `.98` Latin; `1.02` Arabic |
| Section display | `clamp(2.5rem, 5.4vw, 5.25rem)` | 600 | `1.02` |
| Scene title | `clamp(2.4rem, 4.8vw, 4.8rem)` | 600 | `1.02` |
| Feature title | `1.5–2.25rem` | 600 | `1.15` |
| Presentation body | `1–1.3rem` | 400 | `1.6–1.8` |

Latin editorial headings may use tracking down to `-0.055em`. Arabic tracking remains normal.

### 3.3 Product scale

- Page title: 24px mobile / 32px desktop.
- Section title: 20–24px.
- Body: 16px minimum.
- Dense body and controls: 14px minimum.
- Caption/meta: 12px minimum.
- Illustrative UI must not use text below 12px.

---

## 4. Geometry, depth, and surfaces

### 4.1 Radius hierarchy

The radius scale is anchored to `--radius: 0.875rem` (14px) and registered in `@theme inline`:

| Theme Token | Tailwind Utility | Computed Value | Component Usage |
|---|---|---:|---|
| `--radius-sm` | `rounded-sm` | ~7.7px (`calc(var(--radius) * 0.55)`) | Tiny indicators, miniature chips |
| `--radius-md` | `rounded-md` | ~11.2px (`calc(var(--radius) * 0.8)`) | Compact controls, inner badges |
| `--radius-lg` | `rounded-lg` | 14px (`var(--radius)`) | Standard inputs, compact buttons, dropdown items |
| `--radius-xl` | `rounded-xl` | ~19.6px (`calc(var(--radius) * 1.4)`) | Standard buttons (`h-11`), tooltips, control groups |
| `--radius-2xl` | `rounded-2xl` | ~25.2px (`calc(var(--radius) * 1.8)`) | Standard cards, bento cells, floating nav capsule |
| `--radius-3xl` | `rounded-3xl` | ~30.8px (`calc(var(--radius) * 2.2)`) | Dialogs, modal sheets, drawer surfaces |
| `--radius-4xl` | `rounded-4xl` | ~39.2px (`calc(var(--radius) * 2.8)`) | Large pricing frames, perspective panels |
| `--radius-canvas` | `rounded-canvas` / `rounded-[47.7px]` | **47.7px** | Signature nested feature canvas |
| — | `rounded-full` | 9999px | Pills, status chips, circular icon buttons |

The fractional `47.7px` radius is a marketing signature. In authenticated product screens, standard containers use `rounded-2xl` (25px) or `rounded-3xl` (31px).

### 4.2 Surface stack

1. Cosmic Deep page.
2. Cosmic Canvas stage with subtle grid/radiating-line artwork.
3. Cosmic Card/Raised UI frames.
4. Lavender Canvas nested feature surface.
5. White or translucent-white content cards.
6. Purple/cyan/gold/pink state accents.

Depth comes primarily from nesting, contrast, clipping, borders, and ambient light—not heavy shadows.

### 4.3 Glass

Glass is allowed on floating navigation, transient overlays, and selected decorative stage elements. Do not use glass for forms, tables, evidence, approval details, schedules, Brain records, or destructive confirmations.

### 4.4 Dashboard presentation

Marketing dashboard mockups use:

```css
.perspective-stage { perspective: 1300px; }
.perspective-dashboard {
  transform: rotateX(10deg);
  transform-origin: center top;
  box-shadow: 0 42px 120px rgb(0 0 0 / 60%),
              0 0 70px rgb(100 55 252 / 22%);
}
```

Perspective is removed below 768px and under reduced motion. Authenticated product screens never tilt actionable UI.

### 4.5 Animated conic border

Use a rotating pseudo-element behind a stable card surface. Never rotate the card or its content. The effect is decorative and becomes static under reduced motion.

---

## 5. Layout and responsive system

### 5.1 Breakpoints and containers

Use Tailwind v4 defaults: base (320px minimum), `sm` 640, `md` 768, `lg` 1024, `xl` 1280, `2xl` 1536.

- Marketing narrative: max 1180–1240px.
- Product lists: max 1200px.
- Reading/deliverable surface: max 800px.
- Settings/forms: max 720px.
- Marketing stage outer gutter: `3.125vw` from `sm`; edge-to-edge on phones.
- Base inline padding: 16px; 32px where space permits.

### 5.2 Bento grammar

Bento layouts are semantic, not masonry:

- four desktop columns;
- a lead capability may span two columns;
- collapse to two columns on tablets and one on phones;
- every card contains one product promise and one domain-correct UI fragment;
- do not impose heights that clip translated text.

### 5.3 Product shell

- Desktop: dark structural navigation frame and inset light workspace.
- Tablet: compact rail plus light workspace.
- Phone: compact dark header and full-bleed work surface.
- Canvas may remain Cosmic Dark; evidence-heavy adjacent content remains light.
- A whole-application dark-mode toggle is not part of 3.0.

---

## 6. Motion

### 6.1 Tiers and tokens

Expressive marketing motion may use scroll-linked translation, low-amplitude scale, perspective, ambient line travel, conic borders, and one-shot reveals. Product motion is limited to state feedback: opening surfaces, focus/hover, active Task indicators, publishing progress, and Canvas edges.

| Token | Value | Use |
|---|---:|---|
| fast | 100ms | press feedback |
| base | 200ms | colour, focus, small hover |
| medium | 300ms | cards and navigation |
| reveal | 750ms | marketing entrance |
| `ease-expressive` | `cubic-bezier(0.16, 1, 0.3, 1)` | entrances/scroll scenes |

### 6.2 Rules

- Animate transform and opacity; use filter only for short one-shot marketing reveals.
- Scroll-linked effects use small owned requestAnimationFrame hooks.
- IntersectionObserver pauses work outside the viewport.
- Do not add Framer Motion or another animation runtime for this system.
- Mobile receives static composition when choreography would obscure content.
- Continuous decorative animations lasting more than five seconds require a pause mechanism.

### 6.3 Reduced motion

`prefers-reduced-motion: reduce` disables smooth scrolling, collapses animation and transition durations, removes perspective/large translations, and shows every sticky-process card as ordinary static content. No information may disappear.

---

## 7. shadcn/ui and Radix ownership

### 7.1 Foundation

BusMora uses shadcn/ui as source-owned component code, not as an external runtime component library. Interactive primitives use the unified `radix-ui` package.

Owned components live only in `src/shared/ui/`.

Initial inventory:

- Button, Card, Badge;
- Input, Textarea, Label, Select;
- Tabs;
- Dialog, Sheet, Dropdown Menu, Tooltip;
- Table, Skeleton, Separator, Scroll Area.

`components.json` aliases `ui` and `components` to `@/shared/ui` and enables RTL generation. Base UI is not part of the stack.

### 7.2 Architecture boundary

```text
src/
  app/                    # route/layout composition and global tokens
  features/
    marketing/ui/         # landing-page composition
  shared/
    lib/                  # technical helpers
    routes/               # single ROUTES registry
    ui/                   # shadcn/Radix primitives
      motion/             # owned motion helpers
```

- `app/` contains no business rules or heavy composition.
- Feature-specific UI belongs to its feature.
- Cross-feature imports use feature `index.ts` exports.
- `shared/` never imports from a feature.
- All navigation uses `ROUTES`; hardcoded application URLs are forbidden.

### 7.3 Component contract

Every interactive component supports default, hover, focus-visible, active, disabled, loading where asynchronous, and invalid where a form control. Components require keyboard operation, translated accessible names, and 44×44px essential touch targets.

Buttons use 44px default and 52px large height. Purple buttons always use white text. Destructive actions use the semantic destructive contract, not Electric Purple.

### 7.4 Surface scope

`:root` represents the Lavender operational workbench. `.dark` and `.stage` remap the same shadcn contracts to Cosmic Dark. Components must not require bespoke dark-card copies.

### 7.5 Product UI Implementation Blueprint (Rest of the Project)

To ensure the rest of the application (authenticated dashboard, Marketing Director conversation, Business Brain, Adaptive Roadmap, Deliverables, Governed Scheduling, Settings) carries the identical visual quality, contrast, and elegance established on the landing page, engineers and subagents must adhere to the following UI recipes.

#### 7.5.1 Authenticated Application Shell
- **Navigation Sidebar / Rail:**
  - Container: `bg-cosmic-card` (`#121520`) with end border `border-sidebar-border` (`rgb(255 255 255 / 14%)`).
  - Nav Item (Default): `flex h-11 items-center gap-3 rounded-xl px-4 text-sm font-medium text-stage-muted transition-colors hover:bg-cosmic-raised hover:text-white`.
  - Nav Item (Active): `bg-cosmic-active text-white font-medium shadow-sm border border-white/10`.
  - Section Label: `px-4 text-xs font-semibold uppercase tracking-wider text-stage-subtle`.
- **Top Header Bar:**
  - Container: `sticky top-0 z-30 flex h-16 w-full items-center justify-between border-b border-sidebar-border bg-cosmic-card/90 px-6 backdrop-blur-md`.
  - Actions: Search trigger, status pill, notification bell (`hover:bg-cosmic-active rounded-xl p-2`), and user avatar pill.
- **Main Workspace Containers:**
  - **Operational Workbench (Default for Forms, Records, Tables, Deliverables):**
    Surrounded by a subtle outer dark frame or nested within an inset container: `bg-lavender-canvas` (`#edf0fe`), `text-foreground` (`#171926`). Cards within the workbench use pure white `bg-card` (`#ffffff`) or `bg-lavender-alt` (`#edf0fd`) with `border border-border/40 rounded-2xl shadow-sm` or `.light-card`.
  - **Strategic Director Canvas (Live Chat, Brain Graph, Campaign Flow Preview):**
    Uses `bg-cosmic-deep` (`#080911`) or `bg-cosmic-canvas` (`#171926`), with glowing ambient purple radial light (`bg-primary/20 blur-[100px]`), floating chat capsules, and translucent panels (`bg-cosmic-card/95 border border-white/10 rounded-2xl`).

#### 7.5.2 shadcn/ui Component Recipes for Product Screens

##### 1. Buttons (`Button`)
- **Primary Action (Brand & Director Decisions):**
  `bg-primary text-white hover:bg-purple-hover active:scale-[0.98] shadow-[0_0_20px_rgba(100,55,252,0.3)] rounded-xl h-11 px-6 font-medium transition-all`
- **Secondary Action:**
  - *On Light Workbench:* `bg-secondary text-secondary-foreground hover:bg-muted rounded-xl h-11 px-5 border border-border/20 font-medium transition-colors`
  - *On Dark Stage:* `bg-cosmic-active text-white hover:bg-cosmic-raised rounded-xl h-11 px-5 border border-white/10 font-medium transition-colors`
- **Outline Action:**
  - *On Light Workbench:* `border border-border text-foreground hover:bg-lavender-alt rounded-xl h-11 px-5 font-medium transition-colors`
  - *On Dark Stage:* `border border-white/20 text-white hover:bg-white/5 rounded-xl h-11 px-5 font-medium transition-colors`
- **Destructive Action:**
  `bg-destructive text-destructive-foreground hover:bg-destructive/90 rounded-xl h-11 px-5 font-medium transition-colors`
- **Sizes:** Compact `h-9 px-3.5 text-xs rounded-lg` | Standard `h-11 px-5 text-sm rounded-xl` (44px touch target) | Large `h-13 px-8 text-base rounded-2xl` (52px).

##### 2. Cards (`Card`, `CardHeader`, `CardContent`, `CardFooter`)
- **Workbench Standard Card:**
  `rounded-2xl border border-border/40 bg-card p-6 text-card-foreground shadow-sm` or `.light-card` (`bg-white border border-[#e9edf5] shadow-[0_4px_20px_-2px_rgba(15,23,42,0.05)]`).
- **Workbench Feature / Highlight Card:**
  `card-radial-lavender relative flex flex-col overflow-hidden rounded-[24.84px] border border-white p-6`.
- **Dark Cosmic Card (Dashboard / Analytics / Stage):**
  `rounded-2xl border border-white/10 bg-cosmic-card p-6 text-white shadow-md`.
- **Active / Elevated Card:**
  `rounded-2xl border border-white/15 bg-cosmic-raised p-6 text-white shadow-lg`.

##### 3. Status Badges & Category Chips (`Badge`)
Never use arbitrary hex colors on badges. Always use the semantic status matrix:
- **Director / Authored by AI:** `inline-flex items-center gap-1.5 rounded-full border border-primary/25 bg-primary/10 px-3 py-1 text-xs font-semibold text-primary`
- **Active Process / Running / Connected:** `inline-flex items-center gap-1.5 rounded-full border border-cyan-electric/25 bg-cyan-electric/10 px-3 py-1 text-xs font-semibold text-cyan-electric`
- **Pending Review / Awaiting Owner Decision / Warning:** `inline-flex items-center gap-1.5 rounded-full border border-gold-electric/25 bg-gold-electric/10 px-3 py-1 text-xs font-semibold text-gold-electric`
- **Failed / Error / Destructive / Rejected:** `inline-flex items-center gap-1.5 rounded-full border border-pink-electric/25 bg-pink-electric/10 px-3 py-1 text-xs font-semibold text-pink-electric`
- **Draft / Neutral / Historical:** `inline-flex items-center gap-1.5 rounded-full border border-slate-structure/20 bg-slate-structure/10 px-3 py-1 text-xs font-semibold text-slate-structure`

##### 4. Form Controls (`Input`, `Textarea`, `Select`)
- **Workbench Inputs (Light):**
  `h-11 w-full rounded-xl border border-input bg-white px-4 text-sm text-foreground placeholder:text-muted-foreground/60 transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary focus-visible:ring-offset-2 focus-visible:ring-offset-background`
- **Dark Stage Inputs:**
  `h-11 w-full rounded-xl border border-white/20 bg-cosmic-deep/70 px-4 text-sm text-white placeholder:text-stage-subtle transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-purple-focus`

##### 5. Tables & Data Grids (`Table`)
- **Header Row (`TableHeader`, `TableHead`):**
  `border-b border-border/50 bg-muted/40 text-xs font-semibold uppercase tracking-wider text-muted-foreground py-3.5 px-4`
- **Data Rows (`TableRow`, `TableCell`):**
  `border-b border-border/30 text-sm py-4 px-4 transition-colors hover:bg-muted/20`
- **Dark Stage Tables (e.g., Pricing/Analytics Matrix):**
  Alternating rows: even rows `bg-cosmic-raised`, odd rows `bg-cosmic-card`, text `text-white`, borders `border-white/[0.06]`.

##### 6. Segmented Controls & Tabs (`Tabs`, `TabsList`, `TabsTrigger`)
- **Capsule Tab List:**
  `flex items-center gap-1.5 rounded-full border border-white/10 bg-cosmic-active/80 p-1.5 backdrop-blur-md`
- **Tab Trigger (Active):**
  `rounded-full bg-primary px-5 py-2 text-sm font-semibold text-white shadow-md transition-all`
- **Tab Trigger (Inactive):**
  `rounded-full px-5 py-2 text-sm font-medium text-stage-muted hover:text-white transition-colors`

##### 7. Overlays, Dialogs & Sheets (`Dialog`, `Sheet`, `Popover`)
- **Backdrop Overlay:**
  `fixed inset-0 z-50 bg-cosmic-deep/80 backdrop-blur-sm`
- **Dialog Surface (Light Workbench):**
  `rounded-3xl border border-border bg-card p-6 text-card-foreground shadow-2xl sm:p-8`
- **Dialog Surface (Dark Stage):**
  `rounded-3xl border border-white/15 bg-cosmic-card p-6 text-white shadow-2xl sm:p-8`

---

## 8. Navigation

### 8.1 Floating marketing navigation

Desktop navigation is a centered, fixed glass capsule:

- up to 1040px wide and 76–78px high;
- approximately 24.84px radius;
- dark translucent surface with blur and low-opacity border;
- stronger opacity/shadow after initial scroll;
- adaptive elevation over the Lavender feature surface.

Mobile uses a fixed 64px Cosmic header and a Radix Sheet. The Sheet traps focus, closes on Escape, locks background scroll, returns focus to the trigger, and carries a translated close label.

### 8.2 Authenticated navigation

Primary areas remain Marketing Director, Roadmap, Business Brain, Deliverables, Scheduling, Settings, and the Notification Bell. Normal login opens Marketing Director, not a dashboard. Incomplete setup opens Onboarding. Authorized deep links remain preserved.

---

## 9. Canonical landing-page rhythm

The official landing page sequence adapted from the visual system of `/home/mohamed/Templates/test` is:

1. **Floating adaptive glass navigation:** Centered capsule with brand logo, centralized `ROUTES`, language switch (Arabic RTL / English LTR), and primary CTA.
2. **Cosmic hero with radiating lines:** Opposing typography/dashboard parallax, authentic Arabic positioning, and truthful CTAs.
3. **Interactive Director Conversation & Canvas preview:** Simulated chat + live step-based canvas takeover, strictly labelled `عرض توضيحي للمنتج` (Illustrative product demonstration).
4. **Nested Lavender feature canvas:** Rounded surface (`rounded-[47.7px]`, `#edf0fe`) nested within the cosmic stage.
5. **Capability Bento Grid:** 4 structured cards showcasing Business Brain, Bottleneck Diagnosis, Adaptive Roadmap, and Verified Evidence with Citations.
6. **Feature Deep Dives:**
   - **Business Brain & Memory:** Permanent context, Offering & Audience rules, Epistemic status (Fact vs Hypothesis).
   - **Governed Social Publishing:** Scheduled post verification, approval boundaries, human-in-the-loop guarantee.
7. **Trust & Workspace Isolation Badge:** Clear security and privacy guarantees (single-owner isolation, encrypted context).
8. **Sticky 5-stage product loop:** Continuous sticky scroll timeline with layered rising cards:
   - Stage 1: Onboarding Draft confirmation (مسح وتأكيد سياق النشاط)
   - Stage 2: Bottleneck Diagnosis (تشخيص المعوق التسويقي)
   - Stage 3: Adaptive Roadmap generation (بناء خارطة الطريق)
   - Stage 4: Deliverables & verified copy (صياغة المخرجات الموثقة)
   - Stage 5: Governed Scheduling & human approval (الجدولة والمراجعة قبل النشر)
9. **Executive Testimonial:** Realistic Arab business owner quote highlighting clarity and control.
10. **Interactive Product Tabs:** Tabbed showcase of core views (Director Chat, Live Canvas & Roadmap, Business Brain, Governed Publishing).
11. **Transparent Pricing & FAQ:** Clear tiers (Starter, Growth, Director) with transparent limits, accompanied by an accordion FAQ addressing owner control, dialects, and safety.
12. **Customer Reviews Masonry & Perspective CTA:** Authentic SMB feedback grid, leading to a perspective 3D dashboard banner with purple ambient glow and a restrained cosmic footer.

All illustrative mockups and scenes must remain explicitly labelled `عرض توضيحي للمنتج`. They must never claim fabricated business results, automatic revenue generation, or unmonitored ad spending.


---

## 10. Domain-state presentation

### 10.1 Authority

The interface identifies what the Owner is deciding:

- **Review:** persistence of a Pending Brain Update;
- **Approval:** consent to publish one exact Post Version;
- **Roadmap activation/revision approval:** plan-version decision;
- **budget extension:** additional resource authorization;
- **Owner-reported Done:** attestation, never platform verification.

Governed commands do not use optimistic success. Wait for the server verdict and preserve partial failures.

### 10.2 Evidence and epistemic state

Significant generated results can expose Source Observation, Owner-Provided Claim, AI Hypothesis/Interpretation, Confirmed Decision, provenance, access date, assumptions, limitations, context version, and staleness.

Purple means BusMora intelligence or action—not truth. Approval means authorized persistence or publishing—not independent verification.

### 10.3 Status families

- Purple: selected/primary/BusMora-authored.
- Cyan: running, connected, informational.
- Gold: awaiting clarification/Approval, held, partial, uncertain.
- Pink: failed, destructive, rejected.
- Slate: draft, neutral, cancelled, historical.

Every status includes translated text and an icon. Active animations stop when work stops.

### 10.4 Canvas and Brain graphs

Graphs use stable LTR coordinates inside `dir="ltr"` containers. Labels follow the interface language. Every graph provides a visible list/detail alternative and keyboard-accessible non-drag actions.

The Canvas shows only recorded work. Quick Replies never fabricate agents. Recoverable retries remain Processing; terminal failure and Partial Result are explicit.

### 10.5 Governed Scheduling

- Post content, media, destination, Scheduled Time, timezone, and lateness allowance appear together before Approval.
- Any material edit creates a new Post Version and clears Approval.
- Submitted is not Published.
- Uncertain acceptance triggers reconciliation, never blind retry.
- No AI participates after Approval.
- Facebook Pages is the only Phase 2 Platform.

---

## 11. Arabic, RTL, and localization

1. The canonical landing page begins in Arabic RTL and provides a functional English LTR switch.
2. Root `lang` and `dir` change together.
3. Use logical utilities: `start/end`, `ms/me`, `ps/pe`, `border-s/e`, `rounded-s/e`.
4. Directional icons flip; logos, clocks, media controls, charts, and graph geometry do not.
5. Generated/user content uses `dir="auto"`.
6. UI strings, including screen-reader labels, come from localization resources or translated props.
7. Format numbers/dates with `Intl`; accept Arabic-Indic and Latin digits.
8. Always display Scheduled Time in the Business Time Zone with its zone label.
9. Language never implies country, currency, timezone, Platform, or Dialect.

---

## 12. Accessibility

Target: **WCAG 2.2 AA** in Arabic and English.

Required:

- semantic landmarks and one `h1` per page;
- skip link as the first focusable element;
- visible 2px focus outline with 4px offset;
- 44×44px essential touch targets;
- 320px reflow and 200% zoom;
- text/icon status redundancy;
- accessible names on icon buttons;
- focus trap, Escape, initial focus, and focus return in overlays;
- labels and `aria-describedby` for form errors;
- polite live announcements for Task updates;
- visible event-list alternatives for Canvas and graph alternatives for Brain;
- accessible form alternatives for drag scheduling;
- static complete content under reduced motion.

Illustrative dashboards use `role="img"` with a translated summary. Their internal controls are non-interactive. If an illustrative control resembles a real governed command, disable it and label the scene illustrative.

---

## 13. Tailwind CSS v4 implementation

Global tokens live in `src/app/globals.css`. Notice that in Tailwind CSS v4, `@import "tailwindcss";` and `@import "tw-animate-css";` are used, and `@import "shadcn/tailwind.css";` must NOT be imported as it is not an external package and breaks build/bundling.

```css
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *, .stage *));

@theme inline {
  --font-sans: var(--font-active);
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);
  --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-destructive: var(--destructive);
  --color-destructive-foreground: var(--destructive-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-sidebar: var(--sidebar);
  --color-sidebar-foreground: var(--sidebar-foreground);
  --color-sidebar-primary: var(--sidebar-primary);
  --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
  --color-sidebar-accent: var(--sidebar-accent);
  --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
  --color-sidebar-border: var(--sidebar-border);
  --color-sidebar-ring: var(--sidebar-ring);

  /* BusMora Design System 3.0 Brand Primitives */
  --color-cosmic-deep: var(--cosmic-deep);
  --color-cosmic-canvas: var(--cosmic-canvas);
  --color-cosmic-card: var(--cosmic-card);
  --color-cosmic-raised: var(--cosmic-raised);
  --color-cosmic-active: var(--cosmic-active);
  --color-lavender-canvas: var(--lavender-canvas);
  --color-lavender-alt: var(--lavender-alt);
  --color-lavender-tint: var(--lavender-tint);
  --color-purple-electric: var(--purple-electric);
  --color-purple-accent: var(--purple-accent);
  --color-purple-hover: var(--purple-hover);
  --color-purple-focus: var(--purple-focus);
  --color-purple-glow: var(--purple-glow);
  --color-cyan-electric: var(--cyan-electric);
  --color-pink-electric: var(--pink-electric);
  --color-gold-electric: var(--gold-electric);
  --color-slate-structure: var(--slate-structure);
  --color-stage-muted: var(--stage-muted);
  --color-stage-subtle: var(--stage-subtle);

  --radius-sm: calc(var(--radius) * 0.55);
  --radius-md: calc(var(--radius) * 0.8);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) * 1.4);
  --radius-2xl: calc(var(--radius) * 1.8);
  --radius-3xl: calc(var(--radius) * 2.2);
  --radius-4xl: calc(var(--radius) * 2.8);
  --radius-canvas: 47.7px;
  --ease-expressive: cubic-bezier(0.16, 1, 0.3, 1);
}

:root {
  --font-active: var(--font-plex-arabic), var(--font-inter), system-ui, sans-serif;
  --background: #edf0fe;
  --foreground: #171926;
  --card: #ffffff;
  --card-foreground: #171926;
  --popover: #ffffff;
  --popover-foreground: #171926;
  --primary: #6437fc;
  --primary-foreground: #ffffff;
  --secondary: #e3e7fb;
  --secondary-foreground: #25283a;
  --muted: #e6e9f8;
  --muted-foreground: #42516b;
  --accent: #dce6ff;
  --accent-foreground: #263450;
  --destructive: #c81e4b;
  --destructive-foreground: #ffffff;
  --border: #7d88a5;
  --input: #6e7b9a;
  --ring: #6437fc;
  --radius: 0.875rem;

  --sidebar: #121520;
  --sidebar-foreground: #ffffff;
  --sidebar-primary: #6437fc;
  --sidebar-primary-foreground: #ffffff;
  --sidebar-accent: #212936;
  --sidebar-accent-foreground: #ffffff;
  --sidebar-border: rgb(255 255 255 / 14%);
  --sidebar-ring: #8f70ff;

  /* Brand Primitive Values */
  --cosmic-deep: #080911;
  --cosmic-canvas: #171926;
  --cosmic-card: #121520;
  --cosmic-raised: #1b1d30;
  --cosmic-active: #212936;
  --lavender-canvas: #edf0fe;
  --lavender-alt: #edf0fd;
  --lavender-tint: #eaeeff;
  --purple-electric: #6437fc;
  --purple-accent: #6742d1;
  --purple-hover: #5229db;
  --purple-focus: #8f70ff;
  --purple-glow: rgba(100, 55, 252, 0.35);
  --cyan-electric: #00b1ff;
  --pink-electric: #ff3067;
  --gold-electric: #e5b869;
  --slate-structure: #42516b;
  --stage-muted: #bdc5e6;
  --stage-subtle: #a6afd5;
}

.dark,
.stage {
  --background: #080911;
  --foreground: #ffffff;
  --card: #121520;
  --card-foreground: #ffffff;
  --popover: #1b1d30;
  --popover-foreground: #ffffff;
  --primary: #6437fc;
  --primary-foreground: #ffffff;
  --secondary: #212936;
  --secondary-foreground: #ffffff;
  --muted: #1b1d30;
  --muted-foreground: #bdc5e6;
  --accent: #212936;
  --accent-foreground: #ffffff;
  --destructive: #ff3067;
  --destructive-foreground: #080911;
  --border: rgb(255 255 255 / 16%);
  --input: rgb(255 255 255 / 24%);
  --ring: #8f70ff;
  --sidebar: #080911;
  --sidebar-foreground: #ffffff;
  --sidebar-primary: #6437fc;
  --sidebar-primary-foreground: #ffffff;
  --sidebar-accent: #212936;
  --sidebar-accent-foreground: #ffffff;
  --sidebar-border: rgb(255 255 255 / 14%);
  --sidebar-ring: #8f70ff;
}
```

The implementation stylesheet is authoritative when this excerpt omits secondary contracts. Never copy the excerpt over the implementation wholesale.

---

## 14. Verification and definition of done

For every new screen or component:

- build, TypeScript, and lint pass;
- Arabic RTL and English LTR inspection;
- 320, 390, 768, 1024, 1280, and 1536px checks as relevant;
- realistic long, empty, loading, error, partial, held, and uncertain states;
- keyboard-only, reduced-motion, 200% zoom, and 320px reflow passes;
- axe scan with no serious or critical findings;
- screen-reader smoke test for consequential journeys.

The implemented 3.0 baseline has been checked with:

- `bun run lint`;
- `bun run build`;
- desktop Arabic RTL rendering;
- desktop English LTR rendering and functional language switch;
- 390×844 Arabic mobile hero rendering;
- visual inspection of the nested feature canvas and sticky process scene.

This validates the landing-page foundation only. It does not claim that authenticated product screens or backend capabilities are implemented.

---

## 15. Implementation order

1. Tokens, fonts, root direction, focus, reduced motion.
2. Owned shadcn/Radix primitives.
3. Canonical landing page and scene grammar.
4. Auth, Onboarding, and application shell.
5. Shared domain-state components.
6. Marketing Director Conversation and Canvas.
7. Roadmap and Roadmap Discovery.
8. Business Brain list/graph and Review.
9. Deliverables.
10. Scheduling, Batch Review, holds, and recovery.
11. Settings and notifications.

Do not create a parallel component system while migrating. New visual work must use 3.0 semantic contracts and owned primitives.

---

## 16. Open implementation items

- Final production logo/wordmark and favicon set.
- Full localization infrastructure beyond the landing dictionary.
- Authenticated application shell implementation.
- Domain-wide status mapping for every enum.
- Automated axe and visual-regression infrastructure.
- Storybook or equivalent owned component catalogue.
- Final illustration set based on verified product UI.
- Whole-application dark mode, if separately approved.

These are implementation gaps, not permission to invent unsupported product behavior.
