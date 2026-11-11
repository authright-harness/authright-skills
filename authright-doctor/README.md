# authright-doctor

Agent skill that **diagnoses IAM problems** in a code repository from source, across four layers: overall design, identity/auth stack versions or obsolescence, implementation practice, and code-level access-control failures. Each kept finding includes a suggested fix.

It reads production source with ordinary list/search/open tools. It does not analyze test code, and it does not judge whether tests are complete or cover a vulnerability. The main diagnosis does not look up whether library versions need an upgrade. After the report, it asks once whether to do that check; only then may it open public security advisories. It does not apply patches, and does not rebuild a full current-state map (use **authright-scan** for that).

Inspired by `@authright/cli`.

## Install

Copy this folder onto any coding agent that can load a skill directory — Cursor, Codex, OpenCode, Claude Code, Hermes, OpenClaw, and similar. The agent follows the notes in order.

1. Copy the entire `authright-doctor/` folder into the skills, prompts, or plugins path that host uses.
2. Reload or restart the host if it only discovers skills at session start.
3. Invoke it by name: **authright-doctor** (or attach this folder if the host loads skills by path).

The host needs ordinary read tools: list directories, search, and open files. Opening public advisories happens only if you accept the version check offered after the report.

## Use

Ask the agent to diagnose IAM, check whether an identity or auth library needs upgrade or replacement, review authentication or authorization practice, or find access-control gaps. It then:

1. Settles scope when the tree has several services
2. Lists caller-reachable effect surfaces and IAM libraries
3. Stops with a short summary if there is no IAM (no markdown or H5 file)
4. Checks code invariants, path forks, and revocation when IAM is present
5. Judges overall solution and practice, each finding in one layer. It does not judge library versions in this pass
6. Attaches a suggested fix and opens cited sources again before reporting

If the repository has no IAM implementation, you get a short summary only. There is no chaptered report, no version-check offer, and no markdown or H5 file.

If IAM is present, the agent presents the report in three parts the reader can scan in order:

1. **At a glance** — diagnosis summary, then a fix-first priority list (problem + suggested fix)
2. **Problems and fixes** — overall solution, then a technology-stack chapter that says the version check was not run, then implementation practice and code-level access
3. **Bounds** — how far the reading went

Then the line `authright-doctor is inspired by @authright/cli.` It then writes the report without asking. The default paths are `~/.authright/reports/<repository-directory-name>/AUTHRIGHT-DOCTOR.md` and `AUTHRIGHT-DOCTOR.html`, outside the repository. The HTML page is the blue diagnosis: blue sidebar and hero, page title starting with `authright-doctor`, and one card per finding. After that, when the tree has an IAM library, it asks once whether to check those locked versions for upgrade or replacement. That check is a separate note. It does not start unless you say yes, or you already asked for it.

## Versus authright-scan

| | authright-scan | authright-doctor |
| --- | --- | --- |
| Question | How does IAM work here today? | What is wrong, and what should change? |
| Output | Current-state map | Four-layer diagnosis + suggested fixes |
| Default files | `~/.authright/reports/<name>/AUTHRIGHT-SCAN.md`, `.html` | `~/.authright/reports/<name>/AUTHRIGHT-DOCTOR.md`, `.html` |

An existing scan report may guide where to look first. Every doctor finding still re-opens source.

## Files

| File | Role |
| --- | --- |
| `SKILL.md` | Entry point and delivery rules |
| `stages.md` | Order of work and how findings are labeled |
| `report.md` | Report parts, finding cards, and footer |
| `h5.md` | Markdown and H5 files written when the report is delivered |
| `roles/surface.md` | Effect surfaces and IAM libraries |
| `roles/invariant.md` | Five code invariants per effect |
| `roles/solution.md` | Overall design judgment |
| `roles/stack.md` | Optional version check after the report: upgrade or replace for IAM libraries |
| `roles/practice.md` | Practices required by adopted mechanisms |
| `roles/fix.md` | One suggested fix per finding |
| `roles/recheck.md` | Open cited sources again and keep, narrow, or drop |
