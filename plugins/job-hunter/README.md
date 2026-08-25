# Job Hunter Plugin

AI-powered job application skills for Claude. Install once, then use any skill in your Claude chat to research companies, assess fit, tailor your CV, write cover letters, and prep for interviews.

## Skills

| Skill | What it does | Say something like |
|---|---|---|
| **company-researcher** | Deep company research with Glassdoor intel and what to reference in interviews | "Research Stripe for a PM role" |
| **fit-analyzer** | Honest fit assessment against a job description — strong / stretch / long shot | "Am I a good fit for this job?" |
| **cv-tailor** | Tailors your CV to surface JD keywords without inventing anything | "Tailor my CV for this role" |
| **cover-letter-writer** | Writes a 3-paragraph letter with a specific company hook | "Write me a cover letter" |
| **interview-prep** | Stage-specific prep doc for HR screen, professional interview, or home assignment | "Help me prep for my interview" |
| **cv-builder** | Builds a full ATS-optimized CV via conversational interview | "Help me build my CV from scratch" |
| **application-packager** | Everything above in one conversation — full application package | "Build my full application package" |
| **notion-connector** | Sets up a job-search tracker (applications, target companies, networking contacts) in your own Notion workspace | "Set up my job tracker in Notion" |
| **target-company-finder** | Researches and maintains a ranked target-company list in your Notion tracker | "Find me 20 target companies" |

## How to use

Paste your CV and the job description when asked. All skills work by chat — no files, no forms.

Skills that need company information will run a quick web search automatically. You don't need to research the company first.

## Notion integration

`notion-connector` and `target-company-finder` need a Notion MCP connector available in your Claude Code session. If you haven't connected Notion yet, connect it first (via your connector settings, or `/mcp` in an interactive session) — everything else in the plugin works without it.
