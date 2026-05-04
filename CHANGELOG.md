# Changelog

All notable changes to this bundle.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this bundle adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0] — 2026-05-04

### Fixed
- `agents/design-critic.md` Phase 3 cited a ghost `TYPOGRAPHIC_CONTRAST` dial that doesn't exist in this bundle — replaced with a real `DESIGN_VARIANCE > 4` + centered-hero contradiction example
- `context/ai-tells-design.md` Inter font ban contradicted `brutalist-taste`'s explicit Inter Black recommendation — converted to context-sensitive ban with explicit brutalist macro-typography exception (Inter Black at clamp() sizes only)
- `context/ai-tells-design.md` intro and section headings used upstream filenames (`taste-skill`, `gpt-tasteskill`, `soft-skill`, etc.) instead of bundle-canonical names — corrected to `design-taste`, `gpt-taste`, `soft-editorial-taste`, etc.
- `recipes/redesign-audit.yaml` approval gate asked user about scope (Critical / Critical+Warning / All) but the fix step ignored the answer — added `scope` context variable, captured user reply at gate, gated fix step to honor scope
- `agents/component-designer.md` cited the pause protocol from `context/instructions.md` (the secondary mention) instead of `skills/output-discipline.md` (the authoritative source) — corrected
- `bundle.md` frontmatter description didn't note `gpt-taste`'s standalone status (warning lived in skill body, instructions.md, README, but not the canonical manifest tooling reads first) — appended one-sentence note
- `README.md` "What's inside" table was missing the two new recipes added in v0.2.0 — added rows for `redesign-audit` and `design-system-bootstrap`
- `skills/gpt-taste.md` had the standalone warning as a top-of-body blockquote, navigationally inconsistent with the formal `## Composition` section in the other 3 archetypes — promoted to a proper section

### Changed
- **Section 8 split** — `skills/design-taste.md` finished context-sink extraction. The GSAP/ThreeJS-vs-Framer-Motion boundary rule (instruction with enforcement semantics) and 8 category headers stay inline as orientation; the ~40 named premium UI patterns moved to new `context/creative-arsenal.md`. design-taste.md dropped 186 → 149 lines (in target band).

### Added
- `context/creative-arsenal.md` (NEW) — pattern reference library with 8 categories (Standard Hero Paradigm, Navigation & Menüs, Layout & Grids, Cards & Containers, Scroll-Animations, Galleries & Media, Typography & Text, Micro-Interactions & Effects). Includes archetype-conflict notes.
- `scripts/check-tells.sh` (NEW) — pure-bash audit script that greps source files for banned patterns from `ai-tells-design.md`. Critical/Warning severity tiers; exit 1 if Critical found. No Python dependency.
- `.github/workflows/validate.yaml` (NEW) — GitHub Actions workflow validating YAML parsing, skill/agent frontmatter, `@taste:` cross-reference resolution, and an informational self-check via `check-tells.sh`. Runs on PR and push to main.
- `README.md` "Before / after" section — concrete TSX example showing typical LLM defaults (centered hero, h-screen, purple gradient, Inter font, "Elevate Your Workflow" copy, 8 banned patterns total) vs. taste-bundle output (asymmetric grid, min-h-[100dvh], Cabinet Grotesk + Newsreader, off-black, concrete copy)
- `README.md` "Migrating from upstream taste-skill" section — 5 bullets mapping upstream skill names to bundle names, noting image-to-code is now a recipe, stitch-skill is replaced by design-context-generator, Python RNG theater replaced by anti-default rule

## [0.2.0] — 2026-05-04

### Fixed
- `bundle.md` now includes `foundation` and `taste:behaviors/output-discipline` so `amplifier --bundle taste` works in standalone mode
- `behaviors/output-discipline.yaml` rewrote `skills.auto_load` (invented key) → `context.include` (canonical) — the always-on behavior now actually fires
- `skills/output-discipline.md` removed unrecognized `auto-load: true` frontmatter key
- `recipes/image-to-code.yaml` resolved the broken `section_pack()` reference and replaced dangling `design-intelligence:component-designer` agent reference with bundle-internal `taste:component-designer`
- `skills/gpt-taste.md` removed the deprecated Python `random.choice()` theater; aligned with `context/design-variations.md` anti-default rule
- `skills/design-taste.md` finished context-sink extraction — Sections 6, 7, 9 now @-mention the canonical context files instead of duplicating their content (saves ~80 lines and eliminates archetype-conflict drift)
- README composition rule corrected — skills accumulate in context, they don't trigger Amplifier's bundle-level "Replace (later wins)" merge

### Added
- `agents/design-critic.md` — critique-mode agent for auditing existing UI against ban lists
- `agents/component-designer.md` — UI-implementation agent (used by image-to-code recipe)
- `recipes/redesign-audit.yaml` — 3-stage audit-then-fix workflow with approval gate
- `recipes/design-system-bootstrap.yaml` — interview → archetype-pick → DESIGN.md authoring
- `context/instructions.md` — bundle-level agent instructions (auto-loaded via output-discipline behavior)
- `CHANGELOG.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`
- README quickstart, prerequisite docs for image-to-code, archetype disambiguation guidance

### Changed
- README "skills auto-discover" framing corrected to reflect actual `tool-skills` behavior

## [0.1.0] — 2026-05-04

### Added
- Initial port of Leonxlnx/taste-skill: 8 skills, 1 behavior, 5 context files, 1 recipe.
