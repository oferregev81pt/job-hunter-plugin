# job-hunter-plugin

## Versioning

Bump the plugin version in `plugins/jobhunter-ai/.claude-plugin/plugin.json` on every push to `main`, in the same commit or a preceding one.

- Patch (`0.3.0` → `0.3.1`): fixes, wording tweaks, docs-only changes to skills.
- Minor (`0.3.0` → `0.4.0`): new skill or new user-visible behavior.
- Major: breaking changes (renamed or removed skills).

Then rebuild `jobhunter-ai.plugin` so the bundle matches the manifest.
