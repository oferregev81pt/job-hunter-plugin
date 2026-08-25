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
| **notion-connector** | Sets up a job-search tracker (applications, target companies, networking contacts) — in Notion or as local files, your choice | "Set up my job tracker" |
| **target-company-finder** | Researches and maintains a ranked target-company list in your tracker | "Find me 20 target companies" |

## How to use

Paste your CV and the job description when asked. All skills work by chat — no files, no forms.

Skills that need company information will run a quick web search automatically. You don't need to research the company first.

## Tracker storage: Notion or local

`notion-connector` and `target-company-finder` ask up front whether to track things in Notion or as local CSV files on your machine (default: `./job-search-tracker/`) — Notion is entirely optional. Choose Notion only if you already have a Notion MCP connector available in your Claude Code session (connect it via your connector settings, or `/mcp` in an interactive session); otherwise local files work with no setup at all.
