# Privacy

This policy covers the `authright-scan` and `authright-doctor` skills in this repository.

## What AuthRight collects

Nothing. The skills are instruction files that run inside your own coding agent. AuthRight Inc. does not operate a server for them, and receives no code, reports, usage data, or personal data when you install or run them.

## What the skills read and write

- **Read:** files in the repository you ask the agent to scan.
- **Write:** two report files (Markdown and HTML) on your own machine, under `~/.authright/reports/<repository-name>/`. Nothing is written into the scanned repository.
- The skills tell the agent not to copy passwords, tokens, keys, or other secret values into its reports.

## Network access

- `authright-scan` does not access the network.
- `authright-doctor` accesses the network only if you agree to its optional library version check, which it offers after the report. That check opens public security advisory pages for the identity and authentication libraries found in your repository.
- The HTML reports load no external scripts, fonts, or images.

## Your coding agent

Your coding agent and its model provider process the repository content under your own agreement with them. That processing is governed by their terms, not by this policy.

## Contact

Questions about this policy: open an issue at https://github.com/authright-harness/authright-skills/issues.
