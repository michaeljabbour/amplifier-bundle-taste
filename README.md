# amplifier-bundle-taste

Anti-slop frontend design discipline for [Amplifier](https://github.com/microsoft/amplifier),
adapted from [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill).

## Why this bundle

LLMs have hardcoded statistical priors toward mediocre frontend output:
centered heroes, Inter font, purple-blue gradients, 3-column equal-width card
grids, `h-screen`, `// ...` placeholder code, "Acme"/"Nexus" placeholder names,
and "Elevate"/"Seamless" copy clichés. This bundle rewires those defaults at
the prompt level with testable, named hard bans plus three parametric dials
(DESIGN_VARIANCE / MOTION_INTENSITY / VISUAL_DENSITY) that act as global
configuration variables.

It also provides an **always-on output-discipline behavior** that is
domain-agnostic — bans `// ...` placeholders, "for brevity" hedges, and
defines a clean pause/resume protocol for token-limited outputs. **You should
mount this behavior on every Amplifier session that produces code, not just
frontend ones.**

## Install

```bash
amplifier bundle add file:///Users/$USER/dev/amplifier-bundle-taste
# or once published:
# amplifier bundle add git+https://github.com/<your-org>/amplifier-bundle-taste@main
```

## Recommended usage

| Use case | What to load |
|---|---|
| Universal output discipline | `behaviors/output-discipline.yaml` (mount on every session) |
| Generic premium frontend | `skills/design-taste.md` (auto-invokes on UI work) |
| Notion / Linear aesthetic | `design-taste.md` + `minimalist-taste.md` |
| Swiss Industrial / Terminal aesthetic | `design-taste.md` + `brutalist-taste.md` |
| Editorial luxury aesthetic | `design-taste.md` + `soft-editorial-taste.md` |
| GPT/Codex strict mode | `gpt-taste.md` (replaces base — its own structure) |
| Upgrading an existing project | `redesign-existing-projects.md` |
| Image-first design pipeline | `recipes/image-to-code.yaml` |

## Composition rule (the bricks-and-studs pattern)

The four aesthetic archetypes are **mutually exclusive** — never compose two
together (minimalist bans gradients; soft-editorial requires them). Compose
the **base + exactly one archetype**, or use one archetype standalone.

The output-discipline behavior is **always compatible** with any combination —
mount it globally.

## Source attribution

Original work © [Leonxlnx](https://github.com/Leonxlnx),
<https://github.com/Leonxlnx/taste-skill>, MIT-licensed. This bundle adapts
that work to Amplifier's bundle/skill/behavior/recipe conventions without
modifying the substantive design guidance.

See `bundle.md` for the canonical Amplifier composition manifest.
