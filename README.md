# Job Hunter Plugin Marketplace

A plugin marketplace for Claude. Install skills that help job seekers research companies, assess fit, tailor CVs, write cover letters, and prep for interviews — all inside Claude chat.

## Install the Job Hunter plugin

In [Claude Code](https://claude.com/claude-code), run:

```
/plugin marketplace add oferregev81pt/job-hunter-plugin
/plugin install job-hunter@job-hunter-plugin
```

Then restart Claude Code (or run `/plugin` and confirm) when prompted. That's it — the 7 skills are now available in your Claude Code chat.

To update later, run `/plugin marketplace update job-hunter-plugin` followed by `/plugin update job-hunter`. To remove it, run `/plugin uninstall job-hunter@job-hunter-plugin`.

## Skills in the job-hunter plugin

| Skill | What it does | Try saying |
|---|---|---|
| **Company Researcher** | Deep company research: news, leadership, Glassdoor interview process, culture, what to reference in conversation | "Research Stripe for a PM role" |
| **Fit Analyzer** | Honest fit assessment vs a job description — with a verdict: strong / stretch / long shot | "Am I a good fit for this job?" |
| **CV Tailor** | Rewrites your CV for a specific role — surfaces JD keywords, never invents content | "Tailor my CV for this role" |
| **Cover Letter Writer** | 3-paragraph cover letter with a specific company hook, under 280 words | "Write me a cover letter" |
| **Interview Prep** | Stage-specific prep: applying now / HR screen / professional interview / home assignment | "Help me prep for my interview" |
| **CV Builder** | Conversational interview → full ATS-optimized CV from scratch | "Help me build my CV from scratch" |
| **Application Packager** | Everything in one session: research + fit + tailored CV + cover letter + prep doc | "Build my full application package" |

## Roadmap

Coming soon:
- `salary-negotiator` — research comp bands, practice negotiation responses
- `linkedin-optimizer` — rewrite your LinkedIn headline and about section for a target role

## Contributing

To add a new plugin:

1. Fork this repo
2. Create `plugins/<plugin-name>/` with this structure:
   ```
   plugins/<plugin-name>/
   ├── .claude-plugin/
   │   └── plugin.json
   ├── skills/
   │   └── <skill-name>/
   │       └── SKILL.md
   └── README.md
   ```
3. Add an entry for it in [`.claude-plugin/marketplace.json`](./.claude-plugin/marketplace.json)
4. Open a PR

See `plugins/job-hunter/` for a complete example.
