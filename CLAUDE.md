# job-hunter-plugin

## Versioning

Bump the plugin version in `plugins/careerpilot-ai/.claude-plugin/plugin.json` on every push to `main`, in the same commit or a preceding one.

- Patch (`0.3.0` → `0.3.1`): fixes, wording tweaks, docs-only changes to skills.
- Minor (`0.3.0` → `0.4.0`): new skill or new user-visible behavior.
- Major: breaking changes (renamed or removed skills).

Then rebuild `careerpilot-ai.plugin` so the bundle matches the manifest.
