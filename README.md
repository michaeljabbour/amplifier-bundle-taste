# amplifier-bundle-taste

> An [Amplifier](https://github.com/microsoft/amplifier) bundle that ports the
> anti-slop frontend design discipline from
> **[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)** into
> Amplifier-native skills, behaviors, context files, and a recipe.

[![Amplifier Bundle](https://img.shields.io/badge/Amplifier-Bundle-blue?style=flat-square)](https://github.com/microsoft/amplifier)
[![Adapted from](https://img.shields.io/badge/adapted_from-Leonxlnx%2Ftaste--skill-yellow?style=flat-square)](https://github.com/Leonxlnx/taste-skill)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)

## Credit

**This bundle is an adaptation of [taste-skill](https://github.com/Leonxlnx/taste-skill) by [Leonxlnx](https://github.com/Leonxlnx).**

All design rules, parametric dials, named bans, archetype palettes, the Bento 2.0 paradigm, the Doppelrand pattern, the image-to-code workflow, and the research-backed output discipline are Leonxlnx's original work. This bundle reformats those skills into Amplifier's bundle/skill/behavior/recipe conventions without modifying the substantive design guidance. Original work is MIT-licensed; please support the author at <https://tasteskill.dev> and via [GitHub Sponsors](https://github.com/sponsors/Leonxlnx).

If you use this bundle, **star and credit the original repo first**.

## What it does

LLMs have hardcoded statistical priors toward mediocre frontend output:
centered heroes, Inter font, purple-blue gradients, 3-column equal-width card
grids, `h-screen`, `// ...` placeholder code, "Acme"/"Nexus" placeholder names,
and "Elevate"/"Seamless" copy clichés. This bundle rewires those defaults at
the prompt level with testable, named hard bans plus three parametric dials
(`DESIGN_VARIANCE` / `MOTION_INTENSITY` / `VISUAL_DENSITY`) that act as global
configuration variables.

It also provides an **always-on output-discipline behavior** that is
domain-agnostic — bans `// ...` placeholders, "for brevity" hedges, and
defines a clean pause/resume protocol for token-limited outputs. **You should
mount this behavior on every Amplifier session that produces code, not just
frontend ones.**

## Add to Amplifier (from GitHub)

This is an Amplifier bundle. Install it directly from GitHub:

```bash
# Add the bundle (tracks main branch — auto-updates on `amplifier bundle refresh`)
amplifier bundle add git+https://github.com/michaeljabbour/amplifier-bundle-taste@main

# Or pin to a specific tag for stability
amplifier bundle add git+https://github.com/michaeljabbour/amplifier-bundle-taste@v0.1.0

# Verify it loaded
amplifier bundle list | grep taste-skill
```

### Use it as an active app bundle

Once added, you can either compose it into your own bundle, or run Amplifier directly with it as the active bundle:

```bash
# Run an interactive session with this bundle mounted
amplifier --bundle taste-skill

# Or one-off prompts
amplifier --bundle taste-skill run "Build a minimalist landing page hero in Next.js"
```

### Compose into your own bundle

In your project's `bundle.md`:

```yaml
includes:
  - bundle: taste-skill:behaviors/output-discipline   # always-on completeness
  # Skills auto-discover from skills/ — pick an archetype at invocation time
```

### Install for development (editable, from local clone)

```bash
git clone https://github.com/michaeljabbour/amplifier-bundle-taste.git ~/dev/amplifier-bundle-taste
amplifier bundle add file://$HOME/dev/amplifier-bundle-taste
```

## What's inside

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
| `context/dials.md` | The 3 parametric dials and their threshold logic |
| `context/ai-tells-design.md` | Merged hard-ban list across all design archetypes |
| `context/bento-2.md` | Bento 2.0 reference — 5 named card archetypes + Framer Motion physics |
| `context/design-variations.md` | Combinatorial variation menus |
| `context/output-discipline-research.md` | Distilled research backing for output-discipline |
| `recipes/image-to-code.yaml` | 3-stage image-first workflow: generate references → analyze → implement |

## Recommended usage

| Use case | What to load |
|---|---|
| **Universal output discipline** | `behaviors/output-discipline.yaml` — mount on every session |
| **Generic premium frontend** | `skills/design-taste.md` (auto-invokes on UI work) |
| **Notion / Linear aesthetic** | `design-taste.md` + `minimalist-taste.md` |
| **Swiss Industrial / Terminal aesthetic** | `design-taste.md` + `brutalist-taste.md` |
| **Editorial luxury aesthetic** | `design-taste.md` + `soft-editorial-taste.md` |
| **GPT/Codex strict mode** | `gpt-taste.md` (replaces base — its own structure) |
| **Upgrading an existing project** | `redesign-existing-projects.md` |
| **Image-first design pipeline** | `recipes/image-to-code.yaml` |

## Composition rule (the bricks-and-studs pattern)

The four aesthetic archetypes (`minimalist`, `brutalist`, `soft-editorial`,
`gpt`) are **mutually exclusive** — never compose two together (minimalist
bans gradients; soft-editorial requires them). Compose the **base + exactly
one archetype**, or use one archetype standalone.

The output-discipline behavior is **always compatible** with any combination —
mount it globally.

## What changed from the source

This adaptation makes minimal substantive changes — the design rules are
intact. What was reshaped:

1. **Frontmatter** rewritten to follow the [Agent Skills specification](https://agentskills.io/) and Amplifier extensions (`description` is now a "Use when…" trigger statement, not a workflow summary; added `model_role` for routing).
2. **Output discipline split into two layers** — a skill (`skills/output-discipline.md`) plus a behavior wrapper (`behaviors/output-discipline.yaml`) so it auto-loads on session start.
3. **Image-to-code skill converted to a recipe** — the 3-phase pipeline (generate references → analyze → implement) is inherently sequential with phase-gates, so a recipe (declarative YAML workflow) is the right form, not a skill.
4. **Stitch DESIGN.md skill replaced** with a format-neutral `design-context-generator` — the original generated Stitch's proprietary semantic-design-language format; this version generates a generic `DESIGN.md` consumable by any AI session.
5. **Variation engine simplified** — replaced the Python `random.choice()` theater (the model doesn't actually execute Python) with the explicit rule "choose a combination not used in your immediately prior output."
6. **Reference material extracted** — the Bento 2.0 specs, 3-dial logic, merged ban lists, and variation menus moved into `context/` files that skills @-mention rather than embed inline. Keeps each skill scannable.

The taste-skill base, all 4 archetypes, the redesign skill, the output skill,
and the entire research corpus from `research/laziness/` are otherwise preserved.

## License & attribution

MIT, matching the upstream license. See [LICENSE](LICENSE).

Original work © [Leonxlnx](https://github.com/Leonxlnx) at
<https://github.com/Leonxlnx/taste-skill>. **Star the original repo** —
that's where the substantive work lives.

This adaptation © 2026 contributors to <https://github.com/michaeljabbour/amplifier-bundle-taste>.

## Issues, bugs, contributions

- **Adaptation issues** (frontmatter, bundle structure, recipe wiring, Amplifier integration): file here at <https://github.com/michaeljabbour/amplifier-bundle-taste/issues>
- **Substantive design guidance** (a ban is wrong, a dial logic is off, an archetype rule needs revision): file at the [upstream repo](https://github.com/Leonxlnx/taste-skill/issues) so the original author can fix it once and we'll re-port.
