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
amplifier bundle list | grep taste
```

### Use it as an active app bundle

Once added, you can either compose it into your own bundle, or run Amplifier directly with it as the active bundle:

```bash
# Run an interactive session with this bundle mounted
amplifier --bundle taste

# Or one-off prompts
amplifier --bundle taste run "Build a minimalist landing page hero in Next.js"
```

### Compose into your own bundle

In your project's `bundle.md`:

```yaml
includes:
  - bundle: taste:behaviors/output-discipline   # always-on completeness
```

Skills register with `tool-skills` automatically and are loadable on demand
via `load_skill`. The model can invoke them based on description matching —
pick an archetype at invocation time rather than hard-wiring it into the
bundle.

### Install for development (editable, from local clone)

```bash
git clone https://github.com/michaeljabbour/amplifier-bundle-taste.git ~/dev/amplifier-bundle-taste
amplifier bundle add file://$HOME/dev/amplifier-bundle-taste
```

## Quickstart — your first session

After installing, mount the bundle and try one of these:

```bash
# Option 1: Generate a new UI in a specific aesthetic
amplifier --bundle taste run "Build a Notion-style dashboard hero section in Next.js with Tailwind"
# → loads design-taste + minimalist-taste, generates faithful code

# Option 2: Audit an existing UI codebase
amplifier --bundle taste run "Audit src/components/ for design taste violations"
# → delegates to taste:design-critic agent, produces structured report

# Option 3: Bootstrap a design system for a new project
amplifier --bundle taste run "I'm starting a new B2B SaaS app — help me lock in an aesthetic"
# → runs taste:recipes/design-system-bootstrap interview
```

If you're unsure which archetype to use, just describe your project. The
bundle's instructions tell the model how to disambiguate based on keywords
("Notion-style" → minimalist, "Swiss design" → brutalist, "luxury" →
soft-editorial, "GPT-strict" → gpt-taste).

## Before / after — what the bundle actually changes

A concrete example. Same prompt, same model, with and without the bundle.

**Prompt:** *"Build a Next.js hero section with a headline, subheadline, and CTA button."*

### Without taste bundle (typical LLM defaults)

```tsx
// hero.tsx
export function Hero() {
  return (
    <section className="h-screen flex items-center justify-center bg-gradient-to-br from-purple-600 to-blue-600">
      <div className="max-w-4xl mx-auto text-center px-4">
        <h1 className="text-5xl font-bold text-white mb-4 font-[Inter]">
          Elevate Your Workflow
        </h1>
        <p className="text-xl text-white/80 mb-8">
          Seamlessly integrate next-gen AI into your business with our cutting-edge platform.
        </p>
        <button className="bg-white text-purple-600 px-8 py-3 rounded-full font-semibold hover:bg-gray-100">
          Get Started — It's Free
        </button>
      </div>
    </section>
  )
}
```

**Counts of LLM tells:** centered hero (1), `h-screen` iOS-bug (1), purple→blue gradient (1), Inter font (1), "Elevate" cliché (1), "Seamlessly" cliché (1), "next-gen" cliché (1), `rounded-full` on CTA (1) = **8 banned patterns in one component**.

### With `design-taste` + `minimalist-taste`

```tsx
// hero.tsx
export function Hero() {
  return (
    <section className="min-h-[100dvh] grid grid-cols-12 gap-x-6 px-8 py-24 bg-[#FBFBFA]">
      <div className="col-span-12 md:col-span-7 md:col-start-2 self-end">
        <h1 className="font-[Cabinet_Grotesk] text-[clamp(3rem,7vw,6rem)] leading-[0.95] tracking-[-0.03em] text-[#111111] mb-6">
          Notes that think with you.
        </h1>
        <p className="font-[Newsreader] text-xl leading-[1.4] text-[#2F3437] max-w-prose mb-10">
          A document editor that reads alongside your work — pulling references, surfacing precedents, drafting passages on request. Ships next quarter.
        </p>
        <button className="px-5 py-2.5 bg-[#111111] text-[#FBFBFA] text-sm font-medium rounded-md hover:bg-[#2F3437] transition-colors">
          Join the private beta
        </button>
      </div>
    </section>
  )
}
```

**What changed:**
- `min-h-[100dvh]` not `h-screen` (iOS Safari viewport bug)
- Asymmetric 12-column grid, not centered
- Cabinet Grotesk + Newsreader (editorial pairing) instead of Inter
- Off-black `#111111` instead of `#000000`, off-white `#FBFBFA` background
- Concrete copy ("Notes that think with you", "ships next quarter") instead of "Elevate Your Workflow"
- `rounded-md` not `rounded-full` on the CTA
- Single-color treatment, no gradient

The bundle didn't add new code paths or libraries — it shifted the model's defaults. Same prompt, same complexity, completely different output quality.

## Migrating from upstream `taste-skill`

If you're already using [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) directly via `npx skills add` and want to switch to this Amplifier-native version, the mappings are:

- **Skill names** — upstream `taste-skill` → bundle `design-taste`; `gpt-tasteskill` → `gpt-taste`; `minimalist-skill` → `minimalist-taste`; `brutalist-skill` → `brutalist-taste`; `soft-skill` → `soft-editorial-taste`; `redesign-skill` → `redesign-existing-projects`; `output-skill` → `output-discipline` (now mounted as a behavior, auto-loads on session start).
- **`image-to-code-skill` is now a recipe**, not a skill (`recipes/image-to-code.yaml`). The 1228-line upstream skill is split into 3 stages (generate references → analyze → implement) with an approval gate. Run via `amplifier recipe run taste:recipes/image-to-code`.
- **`stitch-skill` is replaced** with the format-neutral `design-context-generator` skill — generates a generic `DESIGN.md` consumable by any AI session, not Stitch's proprietary semantic-design-language format.
- **Parametric dials** (`DESIGN_VARIANCE`, `MOTION_INTENSITY`, `VISUAL_DENSITY`) are unchanged in semantics. They live in `context/dials.md` and are referenced by skills via @-mention.
- **The Python `random.choice()` theater** in upstream `gpt-tasteskill` is replaced by the explicit anti-default rule in `context/design-variations.md` — "Choose a combination not used in your immediately prior output. Do not default to the first option." The model doesn't actually execute Python.

For substantive design-rule changes (a ban is wrong, a dial threshold is off), file at the [upstream repo](https://github.com/Leonxlnx/taste-skill/issues) — we re-port after the original author accepts. For Amplifier-specific bugs (recipe wiring, frontmatter, behavior YAML), file [here](https://github.com/michaeljabbour/amplifier-bundle-taste/issues).

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
| `recipes/redesign-audit.yaml` | 4-stage audit-then-fix workflow with approval gate (scan → diagnose → approval → fix). Operationalizes the redesign-existing-projects skill. |
| `recipes/design-system-bootstrap.yaml` | 3-stage onboarding: interview → archetype-pick (approval gate) → DESIGN.md authoring. Use once per new project. |

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

### Prerequisites for `image-to-code` recipe

This recipe has two runtime dependencies that are not bundled:

1. **Imagegen MCP server** — required for generating reference images in stage 1.
   Install: see <https://github.com/microsoft/amplifier-mcp-imagegen> (or set
   `archetype` and provide your own reference images).

2. **(Optional) `amplifier-bundle-design-intelligence`** — if installed, the
   recipe will use its `component-designer` agent. If not, the bundle's own
   `taste:component-designer` agent (defined in `agents/`) handles
   implementation.

The recipe degrades gracefully if these are missing — but you'll get clearer
errors with them installed.

## Composition rule (the bricks-and-studs pattern)

The four aesthetic archetypes (`minimalist`, `brutalist`, `soft-editorial`,
`gpt`) are **mutually exclusive** — never compose two together (minimalist
bans gradients; soft-editorial requires them). Compose the **base + exactly
one archetype**, or use `gpt-taste` standalone (it replaces the base).

When two skills are simultaneously loaded into a session's context, the
agent resolves conflicting rules via natural-language instruction-following —
more specific archetype constraints take precedence over the base's generic
rules. This is NOT the same as Amplifier's bundle-level composition merge
(which entirely replaces parent instructions). Skills accumulate; bundles
replace.

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
