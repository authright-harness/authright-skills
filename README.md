# AuthRight agent skills

Standalone coding-agent skills for reading IAM in a repository. They do **not** require `@authright/cli` or `authright init`. Inspired by `@authright/cli`.

| Skill | Question | Default report |
| --- | --- | --- |
| [authright-scan](authright-scan/) | How does IAM work here today? | `~/.authright/reports/<repo>/AUTHRIGHT-SCAN.md` (+ `.html`) |
| [authright-doctor](authright-doctor/) | What is wrong, and what should change? | `~/.authright/reports/<repo>/AUTHRIGHT-DOCTOR.md` (+ `.html`) |

- **authright-scan** — rebuilds the current identity / auth / access-control map from source (green current-state report).
- **authright-doctor** — diagnoses IAM problems across design, practice, and code-level access, with a suggested fix per finding (blue diagnosis). Optionally offers a library version check after the report. Prefer **authright-scan** when you only need a map.

## Install

Copy each skill folder into the skills / prompts / plugins path your host uses (Cursor, Codex, Claude Code, OpenCode, and similar). Reload or restart the host if it only discovers skills at session start.

```bash
# from this repository
cp -R authright-scan   <host-skills-dir>/
cp -R authright-doctor <host-skills-dir>/
```

Examples of `<host-skills-dir>`:

| Host | Typical path |
| --- | --- |
| Cursor | `.cursor/skills/` or `.agents/skills/` (project); `~/.cursor/skills/` or `~/.agents/skills/` (user) |
| Claude Code | `.claude/skills/` or `~/.claude/skills/` |
| Codex | `.agents/skills/` or `~/.agents/skills/` |
| OpenCode | `.agents/skills/` or `.opencode/skills/` (project); `~/.config/opencode/skills/` (user) |
| GitHub Copilot | `.agents/skills/`, `.github/skills/`, or `.claude/skills/` (project); `~/.copilot/skills/` or `~/.agents/skills/` (user) |
| Gemini CLI | `.agents/skills/` or `.gemini/skills/` (project); `~/.gemini/skills/` or `~/.agents/skills/` (user) |
| Cline | `.agents/skills/` or `.cline/skills/` (project); `~/.cline/skills/` (user) |
| Windsurf | `.windsurf/skills/` or `~/.codeium/windsurf/skills/` |
| Hermes | `.hermes/skills/` or `~/.hermes/skills/` |
| OpenClaw | `skills/` (project) or `~/.openclaw/skills/` (user) |
| Many others | `.agents/skills/` (project) or `~/.agents/skills/` (user) — shared cross-agent home when the host supports it |

Invoke by name: **authright-scan** or **authright-doctor** (or attach the folder if the host loads skills by path). The host only needs ordinary read tools: list directories, search, and open files.

Details and file layout for each skill: [authright-scan/README.md](authright-scan/README.md) · [authright-doctor/README.md](authright-doctor/README.md).
