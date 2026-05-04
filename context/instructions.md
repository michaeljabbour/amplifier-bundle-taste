# taste bundle — agent instructions

You are operating in an Amplifier session that has the `taste` bundle mounted.
This bundle provides anti-slop frontend design discipline: parametric dials,
named hard bans, mutually-exclusive aesthetic archetypes, and always-on output
completeness enforcement.

## Capabilities at your disposal

### Skills (load on demand via `load_skill`)

| Skill | When to invoke |
|---|---|
| `design-taste` | Generating new frontend UI, web pages, dashboards. Auto-invokes if user mentions UI/frontend work. |
| `minimalist-taste` | User mentions Notion, Linear, Anthropic, OpenAI, editorial, or document-editor aesthetics |
| `brutalist-taste` | User mentions Swiss design, terminal/CRT, industrial, vercel-monospace, or hacker aesthetics |
| `soft-editorial-taste` | User mentions luxury, agency-tier, glass, ethereal, premium, agency portfolio aesthetics |
| `gpt-taste` | User explicitly working with GPT/Codex and wants strict deterministic structure (replaces base, not overlays) |
| `redesign-existing-projects` | User wants to upgrade existing project's design quality without migration |
| `design-context-generator` | User wants to author a `DESIGN.md` design system reference for the project |
| `output-discipline` | Already auto-loaded — applies to every session |

### Agents (delegate via `task` tool)

| Agent | When to delegate |
|---|---|
| `taste:design-critic` | Auditing existing UI code against the ban list. Returns structured violation report. |
| `taste:component-designer` | Implementing a UI from a structured design spec or analysis. Default implementer for the image-to-code recipe. |

### Recipes (run via `recipes` tool)

| Recipe | When to run |
|---|---|
| `taste:recipes/image-to-code` | User has reference images (or wants them generated) and needs faithful code implementation |
| `taste:recipes/redesign-audit` | User wants the full audit-then-fix workflow on an existing project |
| `taste:recipes/design-system-bootstrap` | User is starting a new project and wants to lock in a design system from day one |

## Critical rules

### 1. Archetype mutual exclusion

The 4 aesthetic archetypes (`minimalist-taste`, `brutalist-taste`,
`soft-editorial-taste`, `gpt-taste`) are MUTUALLY EXCLUSIVE. Never load two
simultaneously. Their rules contradict (minimalist bans gradients; soft-editorial
requires them; brutalist bans border-radius; minimalist requires it).

If the user request is ambiguous, ASK which aesthetic they want before
generating code. Do not silently pick.

### 2. Composition model

Two valid patterns:
- **Base + one overlay:** Load `design-taste` (base) + ONE of {minimalist, brutalist, soft-editorial}. The overlay's specific rules take precedence in context.
- **Standalone:** Load `gpt-taste` ALONE. It replaces the base; do not combine.

### 3. Disambiguation by user keywords

When the user uses common product/aesthetic keywords, route to the right archetype:

| User said... | Route to |
|---|---|
| "Notion-style", "Linear-style", "Anthropic", "OpenAI ChatGPT", "editorial", "document editor", "minimalist" | `minimalist-taste` |
| "Swiss", "industrial", "terminal", "CRT", "monospace heavy", "newspaper", "Vercel docs", "brutalist" | `brutalist-taste` |
| "luxury", "agency", "premium", "ethereal glass", "high-end", "boutique", "fashion-tech" | `soft-editorial-taste` |
| "GPT-strict", "Codex", "AIDA landing page", "GSAP-heavy" | `gpt-taste` (standalone) |

If none match, default to `design-taste` base alone and ask the user which
archetype they want before producing finished work.

### 4. Output discipline (always on)

Every code artifact you produce must be COMPLETE. No `// ...` placeholders,
no `// rest of code`, no "for brevity" hedges. If the artifact is genuinely
too long for one turn, use the pause/resume protocol:

`[PAUSED — X of Y complete. Send "continue" to resume from: <next section name>]`

See `@taste:skills/output-discipline.md` for the full ban list and protocol.

### 5. Audit-before-fix for existing projects

If the user asks to "improve", "fix", "upgrade", or "modernize" existing UI,
do NOT start editing immediately. Run the audit first via the
`taste:design-critic` agent or the `taste:recipes/redesign-audit` recipe so
both you and the user have a clear list of violations before fixes begin.

## Cross-references

- `@taste:context/ai-tells-design.md` — full ban list (universal + archetype-specific)
- `@taste:context/dials.md` — DESIGN_VARIANCE / MOTION_INTENSITY / VISUAL_DENSITY threshold logic
- `@taste:context/bento-2.md` — Bento 2.0 paradigm and 5 card archetypes
- `@taste:context/design-variations.md` — combinatorial selection menus
- `@taste:context/output-discipline-research.md` — empirical motivation for output discipline
