# The Three Parametric Dials

The base `design-taste` skill (and most archetype overlays) is governed by three numerical dials that the skill body branches on. They are global variables — every other rule reads them.

## Default values (active baseline)

```
DESIGN_VARIANCE   = 8     (1 = Perfect Symmetry,    10 = Artsy Chaos)
MOTION_INTENSITY  = 6     (1 = Static / no motion,  10 = Cinematic / magic physics)
VISUAL_DENSITY    = 4     (1 = Art Gallery / airy,  10 = Pilot Cockpit / packed data)
```

These defaults are deliberately set high on variance, medium on motion, and low on density to push generations away from the symmetric, static, card-spam baseline that LLMs default to.

## How to use

These are **GLOBAL VARIABLES** that drive branching logic throughout the skill body. The skill body and archetype overlays test thresholds like `DESIGN_VARIANCE > 4` and `VISUAL_DENSITY > 7` to decide whether to enable or ban specific patterns. They are not vibes — they are switches.

**ALWAYS listen to the user.** Adapt the values dynamically based on what they explicitly request:

- "clean", "calm", "minimal" → lower variance, lower motion, lower density
- "crazy", "creative", "art-directed", "editorial" → raise variance and motion
- "dashboard", "cockpit", "data-heavy" → raise density
- "premium SaaS", "enterprise" → keep clarity high, art direction controlled
- Otherwise: use the defaults above.

Do not ask the user to edit a config file. Read their intent from chat and adjust.

---

## DESIGN_VARIANCE (1–10)

Controls layout symmetry and asymmetry.

- **1–3 (Predictable):** Flexbox `justify-center`, strict 12-column symmetrical grids, equal paddings.
- **4–7 (Offset):** `margin-top: -2rem` overlapping, varied image aspect ratios (4:3 next to 16:9), left-aligned headers over center-aligned data.
- **8–10 (Asymmetric):** Masonry layouts, CSS Grid with fractional units (`grid-template-columns: 2fr 1fr 1fr`), massive empty zones (`padding-left: 20vw`).

### Threshold rules

- **`DESIGN_VARIANCE > 4` → Centered Hero/H1 sections are BANNED.** Force "Split Screen" (50/50), "Left content / Right asset", or "Asymmetric White-space" structures.
- **`DESIGN_VARIANCE 4–10` → Mobile override is mandatory.** Any asymmetric layout above `md:` MUST aggressively fall back to a strict, single-column layout (`w-full`, `px-4`, `py-8`) on viewports `< 768px` to prevent horizontal scrolling and layout breakage.

---

## MOTION_INTENSITY (1–10)

Controls animation richness and physics.

- **1–3 (Static):** No automatic animations. CSS `:hover` and `:active` states only.
- **4–7 (Fluid CSS):** `transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1)`. Use `animation-delay` cascades for load-ins. Stick to `transform` and `opacity`. Use `will-change: transform` sparingly.
- **8–10 (Advanced Choreography):** Complex scroll-triggered reveals, parallax, Framer Motion hooks. NEVER use `window.addEventListener('scroll')` — use IntersectionObserver or Framer Motion's `useScroll` / `whileInView`.

### Threshold rules

- **`MOTION_INTENSITY > 5` → Magnetic micro-physics enabled.** Buttons pull slightly toward the mouse cursor. NEVER use React `useState` for magnetic hover or continuous animation — use EXCLUSIVELY Framer Motion's `useMotionValue` and `useTransform` outside the React render cycle, or mobile performance collapses.
- **`MOTION_INTENSITY > 5` → Perpetual micro-interactions required.** Embed continuous infinite micro-animations (Pulse, Typewriter, Float, Shimmer, Carousel) in standard components. Apply spring physics (`type: "spring", stiffness: 100, damping: 20`) to all interactive elements. No linear easing.
- **Any motion level → Interactivity isolation.** If MOTION_INTENSITY > 3, the specific interactive UI component MUST be extracted as an isolated leaf Client Component (`'use client'` at the very top). Server Components must exclusively render static layouts.

---

## VISUAL_DENSITY (1–10)

Controls content packing and spacing.

- **1–3 (Art Gallery):** Lots of white space. Huge section gaps. Everything feels expensive and clean.
- **4–7 (Daily App):** Normal spacing for standard web apps.
- **8–10 (Cockpit):** Tiny paddings. No card boxes — just 1px lines (`border-t`, `divide-y`) to separate data. Everything is packed. **Mandatory:** use `font-mono` for all numbers.

### Threshold rules

- **`VISUAL_DENSITY > 7` → Card containers are BANNED.** Use logic-grouping via `border-t`, `divide-y`, or pure negative space. Data metrics breathe without being boxed in unless elevation (z-index) is functionally required.
- **`VISUAL_DENSITY ≤ 3` → Macro-whitespace required.** Use `py-24` to `py-40` between major sections. Allow the design to breathe heavily.

---

## Reading the dials in skill bodies

When writing or extending an archetype skill, branch on these dials directly:

```
IF DESIGN_VARIANCE > 4: forbid centered hero
IF MOTION_INTENSITY > 5: require Framer Motion useMotionValue (not useState)
IF VISUAL_DENSITY > 7: forbid card containers, require font-mono for numbers
```

Do not soften the bans into suggestions. The thresholds are switches.
