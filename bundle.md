---
bundle:
  name: taste
  version: 0.1.0
  description: Anti-slop frontend design taste — ported from Leonxlnx/taste-skill. Provides a base design-taste skill, four mutually-exclusive aesthetic archetype overlays (minimalist, brutalist, soft-editorial, gpt-strict), an audit-then-fix redesign skill, a generic design-context-generator, an always-on output-discipline behavior preventing placeholder truncation, and an image-to-code recipe for image-first design-to-code workflows.
---

# taste bundle

Anti-slop frontend design discipline for Amplifier. Adapted from
[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) — a curated
SKILL.md collection of Awwwards-tier engineering rules that replace LLM
statistical defaults (centered heroes, Inter font, purple gradients,
placeholder code) with testable, opinionated bans and parametric controls.

## What's in the box

| Path | What it provides |
|---|---|
| `skills/design-taste.md` | **Base** premium frontend skill — 3-dial parametric config + Bento 2.0 paradigm + Creative Arsenal |
| `skills/minimalist-taste.md` | Notion / Linear editorial overlay |
| `skills/brutalist-taste.md` | Swiss Industrial / CRT Tactical overlay |
| `skills/soft-editorial-taste.md` | Ethereal Glass / Editorial Luxury overlay (with Doppelrand) |
| `skills/gpt-taste.md` | GPT/Codex-optimized variant — XML design plan, GSAP-heavy |
| `skills/redesign-existing-projects.md` | Audit-then-fix for upgrading existing projects |
| `skills/design-context-generator.md` | Generates a generic `DESIGN.md` reference doc (concept extracted from stitch-skill, format-neutralized) |
| `skills/output-discipline.md` | Auto-loaded — bans `// ...` placeholders, "for brevity" hedges, defines pause/resume protocol |
| `behaviors/output-discipline.yaml` | Behavior wrapper that auto-loads the output-discipline skill on session start |
| `context/dials.md` | The 3 parametric dials (DESIGN_VARIANCE / MOTION_INTENSITY / VISUAL_DENSITY) and their threshold logic |
| `context/ai-tells-design.md` | Merged hard-ban list across all design archetypes |
| `context/bento-2.md` | Bento 2.0 reference — 5 named card archetypes + Framer Motion physics |
| `context/design-variations.md` | Combinatorial variation menus (theme × bg × typography × hero × sections × components × motion) |
| `context/output-discipline-research.md` | Distilled research backing for the output-discipline behavior |
| `recipes/image-to-code.yaml` | 3-stage image-first workflow: generate references → analyze → implement |

## Composition pattern

```yaml
# Project bundle that wants minimalist frontend design + universal output discipline
includes:
  - bundle: taste:behaviors/output-discipline   # always-on
  # Skills auto-discovered from skills/ — pick one archetype at invocation time
```

## Source attribution

Original SKILL.md collection by [Leonxlnx](https://github.com/Leonxlnx) at
<https://github.com/Leonxlnx/taste-skill> (MIT-licensed, Vercel Labs
`agent-skills` compatible). This bundle adapts those skills to Amplifier-native
format (frontmatter extensions, behavior wrapping, recipe extraction) without
changing the substantive guidance.
