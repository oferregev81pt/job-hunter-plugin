# CareerPilot AI

AI-powered job application skills for Claude. Install once, then use any skill in your Claude chat to research companies, assess fit, tailor your CV, write cover letters, and prep for interviews.

## Skills

| Skill | What it does | Say something like |
|---|---|---|
| **company-researcher** | Deep company research (public web sources) and what to reference in interviews | "Research Stripe for a PM role" |
| **fit-analyzer** | Honest fit assessment against a job description — strong / stretch / long shot | "Am I a good fit for this job?" |
| **cv-tailor** | Tailors your CV to surface JD keywords without inventing anything | "Tailor my CV for this role" |
| **cover-letter-writer** | Writes a 3-paragraph letter with a specific company hook | "Write me a cover letter" |
| **interview-prep** | Stage-specific prep doc for HR screen, professional interview, or home assignment | "Help me prep for my interview" |
| **cv-builder** | Builds a full ATS-optimized CV via conversational interview | "Help me build my CV from scratch" |
| **application-packager** | Everything above in one conversation — full application package | "Build my full application package" |
| **notion-connector** | Sets up a job-search tracker (applications, target companies, networking contacts) — in Notion or as local files, your choice | "Set up my job tracker" |
| **target-company-finder** | Researches and maintains a ranked target-company list in your tracker | "Find me 20 target companies" |

## How to use

Paste your CV and the job description when asked. All skills work by chat — no forms to fill in.

Skills that need company information will run a quick web search automatically. You don't need to research the company first.

## Tracker storage: Notion or local

`notion-connector` and `target-company-finder` ask up front whether to track things in Notion or as local CSV files on your machine (default: `./job-search-tracker/`) — Notion is entirely optional. Choose Notion only if you have connected the Notion connector in your Claude account (via your connector settings, or `/mcp` in Claude Code); otherwise local files work with no setup at all.

## What this plugin runs and connects to

careerpilot-ai is **instructions only** (Markdown `SKILL.md` files). It ships no scripts, binaries, hooks, background processes, servers or MCP server definitions, and it makes no network requests of its own. Everything below is done by Claude, with your Claude product's normal permission prompts:

| Capability | Used by | Details |
|---|---|---|
| Web search | company-researcher, cover-letter-writer, application-packager, target-company-finder | Claude's built-in web search, for public company information only. |
| Local files | all skills that reuse your CV; notion-connector; target-company-finder | Reads and writes plain files in `./job-search-tracker/` (e.g. `cv.md`, `*.csv`) in your current directory. Shell use is limited to a read-only `find` to locate an existing tracker folder. |
| Notion connector (optional) | notion-connector, target-company-finder | Only if you choose Notion. Uses the Notion connector you have authorized in your own Claude account; the plugin does not bundle or configure it, never sees your credentials, and writes only to a page/database you confirm. Choosing local files needs no connector. |

No data is sent to the plugin author or any third party. See the [Privacy Policy](PRIVACY.md).
