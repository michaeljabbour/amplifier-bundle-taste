# Design Variations — Combinatorial Selection Menus

The variation engine that drives `image-to-code` (and supports the other archetype skills) works by composing one choice from each of seven menus into a coherent visual direction. The point is to **break the LLM's tendency to pick the first option in every list** — which produces visually identical output across runs.

Source: `image-to-code-skill` Section 12, "The Combinatorial Variation Engine."

---

## Selection rule (the one that matters)

> **Choose a combination not used in your immediately prior output. Do not default to the first option in any list.**

This replaces the "simulate `random.choice()` in a Python block" theater that earlier versions of the skill leaned on. The Python block did not actually randomize anything — the model produced whatever Python output it wanted to produce, then matched the code to it. The real lever is the explicit anti-default instruction above plus a memory of the last combination used.

When uncertain, bias toward unfamiliar combinations: pair an option you rarely pick with another option you rarely pick. Coherence comes from committing to the chosen combination, not from picking "safe" defaults.

---

## 1. Theme paradigm — choose 1

| # | Option |
|---|---|
| 1 | Pristine Light Mode |
| 2 | Deep Dark Mode |
| 3 | Bold Studio Solid |
| 4 | Quiet Premium Neutral |

---

## 2. Background character — choose 1

| # | Option |
|---|---|
| 1 | Subtle technical grid / dotted field |
| 2 | Pure solid field with soft ambient gradient depth |
| 3 | Full-bleed cinematic imagery |
| 4 | Tactile textured surface feel |

---

## 3. Typography character — choose 1

| # | Option |
|---|---|
| 1 | Clean grotesk |
| 2 | Refined grotesk |
| 3 | Expressive display |
| 4 | Compressed statement typography |
| 5 | Editorial serif + sans |
| 6 | Swiss rational hierarchy |

---

## 4. Hero architecture — choose 1

| # | Option |
|---|---|
| 1 | Cinematic centered minimalist |
| 2 | Asymmetric split hero |
| 3 | Floating polaroid scatter |
| 4 | Inline typography behemoth |
| 5 | Editorial offset composition |
| 6 | Massive image-first hero with restrained text |

Note: option 1 (Cinematic centered) is banned when `DESIGN_VARIANCE > 4` per the dial threshold rules.

---

## 5. Section system — choose 1

| # | Option |
|---|---|
| 1 | Modular bento rhythm |
| 2 | Alternating editorial blocks |
| 3 | Poster-like stacked storytelling |
| 4 | Gallery-led cadence |
| 5 | Swiss grid discipline |
| 6 | Asymmetric premium marketing flow |

---

## 6. Signature component set — choose exactly 4 unique components

| # | Option |
|---|---|
| 1 | Diagonal staggered square masonry |
| 2 | 3D cascading card deck |
| 3 | Hover-accordion slice layout |
| 4 | Pristine gapless bento grid |
| 5 | Infinite brand marquee strip |
| 6 | Turning polaroid arc |
| 7 | Vertical rhythm lines |
| 8 | Off-grid editorial layout |
| 9 | Product UI panel stack |
| 10 | Split testimonial quote wall |
| 11 | Layered image crop frames |
| 12 | (reserved — extend with project-specific signature components) |

Pick 4. The components should reinforce, not contradict, your chosen Section System.

---

## 7. Motion-implied language — choose exactly 2

| # | Option |
|---|---|
| 1 | Scrubbing text reveal energy |
| 2 | Pinned narrative section energy |
| 3 | Staggered float-up energy |
| 4 | Parallax image drift energy |
| 5 | Smooth accordion expansion energy |
| 6 | Cinematic fade-through energy |

These are visual-direction cues the design should *imply*, not literal `framer-motion` props. The image generation step uses them to compose a still image whose layout suggests the chosen motion. The implementation step then translates the implied motion into actual `framer-motion` or GSAP code.

---

## How a complete selection looks

A worked example of one valid combination (use as a sample, not a default):

```
Theme:        Quiet Premium Neutral
Background:   Tactile textured surface feel
Typography:   Editorial serif + sans
Hero:         Editorial offset composition
Sections:     Alternating editorial blocks
Components:   Vertical rhythm lines, Off-grid editorial layout,
              Layered image crop frames, Split testimonial quote wall
Motion:       Scrubbing text reveal energy, Parallax image drift energy
```

That is one of approximately 4 × 4 × 6 × 6 × 6 × C(12,4) × C(6,2) ≈ 5.3M nominally valid combinations. The point of the menus is not to enumerate all of them — it is to make the model commit, deliberately, to *one* coherent direction and not silently slide back to "centered hero, clean grotesk, three card row, fade-up motion" on every generation.

---

## Anti-pattern reminders

- Do not "mash everything into chaos." Pick a coherent visual direction and execute it clearly.
- Do not pick the first option in any list as a default. If the prompt does not nudge you toward a specific choice, deliberately pick a non-first option.
- Do not switch directions between sections of the same page — Image 2 and Image 8 must belong to the same brand world (same type scale, same CTA styling, same icon mood, same image treatment).
- Do not pick combinations whose constituent options contradict each other (e.g., "Swiss rational hierarchy" + "Floating polaroid scatter" — the type system fights the layout system).
