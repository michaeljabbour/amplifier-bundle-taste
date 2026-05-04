# Contributing to amplifier-bundle-taste

Thank you for considering contributing! This bundle adapts [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) into the Amplifier ecosystem. Two repos, two different scopes for contributions.

## Where to file what

| Type of change | Where to file |
|---|---|
| Substantive design guidance (a ban is wrong, a dial threshold is off, an archetype rule needs revision) | [Upstream Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill/issues) — we re-port after the original author accepts |
| Amplifier-specific bugs (broken YAML, wrong frontmatter key, missing `includes:`) | [This repo](https://github.com/michaeljabbour/amplifier-bundle-taste/issues) |
| New agent / recipe / hook authored for Amplifier | [This repo](https://github.com/michaeljabbour/amplifier-bundle-taste/issues) — we want these |
| Cross-bundle compatibility fix (e.g., better integration with `design-intelligence`) | [This repo](https://github.com/michaeljabbour/amplifier-bundle-taste/issues) |
| Question about how Amplifier resolves bundles | [Amplifier discussions](https://github.com/microsoft/amplifier/discussions) |

## Making changes

### Fork → branch → PR

1. Fork this repo
2. Create a feature branch (`git checkout -b feat/your-change`)
3. Make changes — follow the conventions below
4. Validate with `amplifier bundle validate .` if you have Amplifier installed
5. Commit with conventional-commit format (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`)
6. Push and open a PR against `main`

### Conventions

- **Skills:** YAML frontmatter `name` + `description`. `description` MUST start with "Use when..." and describe ONLY triggering conditions, never workflow summary. Add `model_role:` for routing.
- **Agents:** `meta:` frontmatter with `name` + `description`. Detailed multi-paragraph description. Body = persona + protocol.
- **Behaviors:** `bundle:` frontmatter (name/version/description) + `context:` block with `include:` list. Do NOT invent YAML keys.
- **Recipes:** `name`/`description` at root, `context:` for variables, `steps:` array. Each step needs `id`, `description`, and either `agent:` + `prompt_template:` or `tool:` + tool-specific config.
- **Bundle composition:** This bundle is a CONTENT bundle. Do not add Python modules unless absolutely necessary. The hooks-/tools- patterns require a separate `pyproject.toml` and entry points, which complicates the install.
- **Attribution:** When adding new content, if it draws from an external source (taste-skill or others), cite the source explicitly with a URL.

### Testing your changes

```bash
# Validate bundle structure (requires Amplifier installed)
amplifier bundle validate /path/to/your/clone

# Mount and smoke-test
amplifier --bundle taste run "Build a minimalist landing page hero"
# → should NOT include `// ...` placeholders, should NOT use Inter font, should generate complete code
```

### Code review

PRs are reviewed for:
- Frontmatter validity (YAML parses, keys are canonical Amplifier keys)
- Description quality (model can decide whether to invoke based on description alone)
- Cross-references resolve (no broken `@taste:...` mentions)
- Backward compatibility (existing skills/recipes still work)

### Releasing

Maintainers tag `v0.x.y` after merging. Patch for editorial fixes, minor for additive features, major for breaking composition changes.

## Community

- Use [GitHub Discussions](https://github.com/michaeljabbour/amplifier-bundle-taste/discussions) for design questions
- Star the [upstream taste-skill](https://github.com/Leonxlnx/taste-skill) — that's where the substantive work lives
