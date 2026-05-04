# Security policy

This bundle is an adaptation of [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill), originally MIT-licensed. It contains content (Markdown skills, YAML behavior, YAML recipes) — no executable code in this v0.2.0 release.

## Reporting vulnerabilities

### Issues with this adaptation

For security issues specific to this bundle (incorrect ban patterns that could leak credentials, prompts that exfiltrate data, malformed YAML that crashes Amplifier, etc.):

- File a [GitHub security advisory](https://github.com/michaeljabbour/amplifier-bundle-taste/security/advisories/new)
- Or open a regular issue if non-sensitive: <https://github.com/michaeljabbour/amplifier-bundle-taste/issues>

### Issues with the upstream skill content

For substantive issues with the design rules themselves (a ban that's wrong, a dial threshold that has a security implication, etc.):

- Report at the upstream repo: <https://github.com/Leonxlnx/taste-skill/issues>
- We will re-port from upstream after the original author acknowledges/fixes

### Issues with Amplifier itself

For Amplifier kernel/foundation security issues:
- See the [Amplifier security policy](https://github.com/microsoft/amplifier/security)

## Supported versions

Only the latest tagged version is supported. We do not backport security fixes to older minor versions.

| Version | Supported |
|---|---|
| 0.2.x | ✅ |
| 0.1.x | ❌ (use 0.2.0+) |

## Security considerations for users

- This bundle's `output-discipline` behavior auto-loads on every session that mounts the bundle. The injected content is plain Markdown — no executable code.
- The `image-to-code` recipe delegates to external services (imagegen MCP). Review your imagegen provider's data-handling policy.
- The `design-context-generator` skill produces a `DESIGN.md` file. Do not include credentials, API keys, or sensitive paths in the project description fed to this skill — they may end up persisted in version control.
