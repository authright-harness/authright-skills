# authright-scan

Agent skill that rebuilds the **current IAM state** of a code repository from source: identity providers, authentication flows, authorization models, enforcement points, and identity-to-resource relationships.

It writes a current-state report from the code. It does not look for vulnerabilities, assign exploit severity, or change the application.

Inspired by `@authright/cli`.

## Install

Copy this folder onto any coding agent that can load a skill directory — Cursor, Codex, OpenCode, Claude Code, Hermes, OpenClaw, and similar. The agent follows the notes in order. The scan itself does not need an AuthRight install.

1. Copy the entire `authright-scan/` folder into the skills, prompts, or plugins path that host uses.
2. Reload or restart the host if it only discovers skills at session start.
3. Invoke it by name: **authright-scan** (or attach this folder if the host loads skills by path).

The host only needs ordinary read tools: list directories, search, and open files.

## Use

Ask the agent to scan the repository for IAM, identity, authentication, authorization, or access control. It then:

1. Settles scope when the tree has several services
2. Maps the repository and records what was read or left out
3. Reads identity and access code in four passes, then one pass over hand-offs between parts
4. Rebuilds how identity connects to access
5. Opens the cited files again before reporting

If the repository has no IAM implementation, you get a short summary only. There is no chaptered report and no markdown or H5 file.

If IAM is present, the agent presents the full report in the conversation in five parts — Overview, Identity model, Runtime detail, Assessment, Boundaries — with a short lead sentence per chapter, Mermaid diagrams, and tables, then the line `authright-scan is inspired by @authright/cli.` It then writes the report without asking. The default paths are `~/.authright/reports/<repository-directory-name>/AUTHRIGHT-SCAN.md` and `AUTHRIGHT-SCAN.html`, outside the repository. The HTML page is the green current-state report: green sidebar and hero, page title starting with `authright-scan`.

## Files

| File | Role |
| --- | --- |
| `SKILL.md` | Entry point and delivery rules |
| `stages.md` | Order of work and how conclusions are labeled |
| `report.md` | Report chapters, diagrams, tables, and format examples |
| `h5.md` | Markdown and H5 files written when the report is delivered |
| `roles/layout.md` | Split the tree into what will be read and what is left out |
| `roles/evidence.md` | One identity-and-access pass at a time |
| `roles/trace.md` | One call-chain or wiring question |
| `roles/recheck.md` | Open cited files again and keep, narrow, or drop each conclusion |
