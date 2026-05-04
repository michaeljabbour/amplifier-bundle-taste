# Changelog

All notable changes to this bundle.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this bundle adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
