# Report format

Use this chapter set only when the repository contains IAM. Write the report in the user's current conversation language. Present the chapters in chat first. The last analysis chapter is "Boundaries of this report".

After the chapters, follow [Closing the chat](#closing-the-chat). In that same turn, write the markdown file and the H5 page. Do not ask first.

When there is no IAM, do not use these chapters. Deliver the short summary from [stages.md](stages.md) instead — no footer, no path paragraph, no file.

## Writing voice

Write for an engineer who needs a clear IAM picture of this repository, not a textbook and not a vulnerability report.

- Keep standard terms: RBAC, ABAC, OIDC, JWT, IdP, SP, and similar. Do not replace them with vague phrases.
- The first time each term appears, add one short clause saying what it means **in this repository**. Do not repeat the gloss later. Do not write textbook definitions. Do not stack several abbreviations in one sentence.
- Put the fact in a table cell or a diagram node. Each chapter opens with one lead sentence; the sentence introduces, it does not restate the table.
- Prefer a short lead sentence, then a table or a Mermaid diagram. Do not make a table or a diagram for a single fact.
- Keep authentication and authorization in separate chapters. When evidence distinguishes session from token, or role from permission, use those domain words.
- Do not list authorization models, protocols, login patterns, or stores the code does not support. A profile field that looks like a role but is never used for authorization is not RBAC — say so clearly.
- Do not mention this skill's stage names, role notes, or file names in the report.
- Do not write that another reviewer or a product checked the findings. One sentence may say the cited files were opened again before writing.
- Only report what this repository's code and configuration support. Decide the concrete providers, protocols, stores, and login shapes from evidence — do not copy sample rows from this file.

## Opening

Two or three sentences: how this repository identifies users today, and how it decides who can do what, from code that was opened again before writing. If a fact the reader needs lives outside the repository, say that in one sentence.

Do not put the scanned/excluded path list here.

## Report shape

Emit the five part headings below as level-2 headings in the report, and the numbered chapter titles as level-3 headings under them. Order is fixed: **overview → identity model → runtime detail → assessment → boundaries**.

| Part | Chapters | Reader question |
| --- | --- | --- |
| Overview | 1–3 | What is the IAM picture, and where does it live? |
| Identity model | 4–5 | Where does identity come from, and how does it reach access? |
| Runtime detail | 6–7 | How does login work, and who can do what? |
| Assessment | 8–10 | Does the design hold, what is solid, what to do next? |
| Boundaries | 11 | What could this reading not settle, and what did it cover? |

Diagrams are Mermaid fenced blocks. Node labels stay short and in the user's language. No secrets, no skill stage names, no product names. Do not also print the same picture as an ASCII arrow chain.

Do not describe the same map twice: chapter 2 owns code placement; chapter 5 owns the access chain; chapter 4 owns IdP / store / provisioning.

---

## Part: Overview

### 1. IAM snapshot

One lead sentence. Then one at-a-glance table so a busy reader can orient before the detail:

| Aspect | In this repository | Status |
| --- | --- | --- |

Fill every row. Answers come from evidence in later chapters — keep cells short; expand in the chapters that own the topic.

- App role — IdP, SP / relying party, or mix
- Primary protocol(s)
- Credential or session
- Authorization model
- Identity store
- IAM code — gathered or spread (one short phrase; detail in chapter 2)

No diagram in this chapter unless a single picture would clarify a mix of roles that a table cannot.

### 2. Where the IAM code sits

One lead sentence: whether identity and access code is gathered or spread.

Then a table:

| Part | Paths | IAM job here | Caller-facing |
| --- | --- | --- | --- |

`IAM job here` is one or more of: identity ingress, credential or session check, authorization enforcement, protected resource, or none. Keep real application parts with no IAM code in the table so empty areas stay visible. Do not list libraries here. Do not repeat enforcement checks from chapter 7.

When IAM code sits in more than one part, add a Mermaid flowchart of those parts and which IAM job each one does. One part is not a diagram; the table is enough.

### 3. Technology and protocols

One lead sentence. Then **two** tables — products and libraries first, wire shapes second. Do not merge them into one list.

**Technology**

| Technology | Role in this repository | Where it is used | Status |
| --- | --- | --- | --- |

Rows: identity provider, authentication library, authorization library, token or session mechanism, IAM SDK, plus any other library that itself checks identity or enforces access. Omit the general application stack (web framework, database, UI kit) unless that piece does IAM work. If a usual piece was not found, say where the judgment stops. No blank cells. No product pitch.

**Protocols**

| Protocol | What it does here | Where | Status |
| --- | --- | --- | --- |

Include a protocol only when code or configuration shows it (for example OIDC, OAuth 2.0, SAML, LDAP, SCIM — only if evidenced). This table is the handshake or wire shape, not a repeat of package names from the technology table. If no protocol can be named, one row saying so, with where the judgment stops.

---

## Part: Identity model

### 4. Identity origin and lifecycle

One lead sentence: who issues or owns the core identity here, and where that record lives.

Then a table:

| Question | In this repository | Status | Evidence |
| --- | --- | --- | --- |

Fill every row. When the code does not settle a row, say so and where the judgment stops.

Directions to investigate (not a fixed answer key — decide from this repository):

- Role of this app — IdP, SP / relying party, or mix
- Where the identity record is stored
- How a user account is created or linked
- User provisioning / deprovisioning
- Who can change identity attributes

When the path from store or IdP into the app crosses more than one party, add a small Mermaid flowchart of that path. Do not turn this chapter into the access-decision chain — that is chapter 5.

### 5. How identity connects to access

One lead sentence on how to read the chain. Then a Mermaid flowchart. Each hop is a node. Mark Confirmed, Inferred, or Unknown on the node (gloss those once in the report language if needed: confirmed / suggested by evidence / not visible in this repository). File paths stay out of the nodes.

Under the diagram, a hop table:

| Hop | Status | Where |
| --- | --- | --- |

Required hops:

Identity → Authentication → Credential / Token → Application → Authorization Decision → Resource / Capability

When User → Role → Permission → Resource appears in source, add a second small flowchart and a second hop table. Omit that second chain when those concepts are not in source.

Do not redraw the chapter 2 placement map. Do not repeat chapter 4's IdP / store / provisioning answers except as a short hop label when that hop is the identity source.

---

## Part: Runtime detail

### 6. How login works (authentication)

One lead sentence. Then the core table:

| Question | In this repository | Evidence |
| --- | --- | --- |

Fill every row from this repository:

- Who authenticates?
- Where?
- Using what protocol?
- What credential or token is produced?
- How is it validated?

When login crosses two parties (browser and API, or app and an identity provider), add a Mermaid sequence diagram of who talks to whom. A local password check with no second party stays a table only.

Then investigate further login shapes. The list below is **directions to investigate**, not required rows and not answers to copy. From code and configuration, decide which are present, how each works here, and what to cite. If none beyond the core table show up, omit the second table.

Directions to investigate:

- Password or local credential
- Social login
- Enterprise federation
- Multi-factor authentication (MFA)
- Device, client, or hardware-bound checks
- Passwordless or passkey / WebAuthn
- Impersonation or break-glass login
- Any other login path this repository implements

When at least one such pattern is evidenced:

| Login pattern | Present? | How it works here | Evidence |
| --- | --- | --- | --- |

Write only patterns you found. Name providers, factors, and devices from evidence in this repository.

### 7. Who can access what (authorization)

One sentence naming the model the code actually uses (RBAC, permission-based, scope-based, ABAC, custom logic, or a combination supported by evidence).

Then a table:

| Enforcement point | What it checks | File and symbol |
| --- | --- | --- |

If the test tree was excluded from the scan, one sentence here: access assertions in tests were not used as confirmation.

---

## Part: Assessment

### 8. Design assessment

One lead sentence. Then one table:

| Dimension | Judgment | Evidence |
| --- | --- | --- |

Six rows, in this order:

- Component boundaries — Are identity concerns in clear places?
- Separation of authentication and authorization — Does login stay distinct from allow/deny?
- Distribution of IAM logic — Is access logic concentrated or scattered?
- Coupling to the identity provider — How tightly is the app bound to its IdP?
- Maintainability of roles and permissions — Can roles or permissions be changed without hunting every call site?
- Architectural consistency — Do similar surfaces enforce access the same way?

The distribution row points at chapter 2 and judges only — it does not list files again. The coupling row may point at chapter 4.

### 9. Conclusions

One sentence that explains Confirmed / Inferred / Unknown once. Then a table of conclusions the re-read still supports:

| Conclusion | Status | Confidence |
| --- | --- | --- |

Each Conclusion cell should stand alone if quoted without the evidence lines. Confirmed with High needs two separate pieces of code or configuration, both opened again during the re-check.

Under the table, evidence bullets only for Confirmed or High rows. Do not repeat lower-confidence rows as text blocks.

### 10. Recommendations

One sentence: these are not adopted decisions, and this skill does not modify code. Then a numbered list. Each item is the change, then why, in one line. Do not name products in this chapter.

---

## Part: Boundaries

### 11. Boundaries of this report

One lead sentence. Then two tables.

**Open questions** — facts this repository cannot prove:

| Open question | Why this repository cannot settle it |
| --- | --- |

Investigate and include when relevant: user lifecycle outside this tree; identity-provider environments not in source; external authorization services; deployment assumptions; provisioning that may run only elsewhere. Add other open questions the reading actually hit.

**Coverage** — how far the reading went:

| Layer | How far |
| --- | --- |
| Repository | fully / partially / not analyzed |
| Application code | fully / partially / not analyzed |
| Infrastructure | fully / partially / not analyzed |
| External identity provider | fully / partially / not analyzed |

| Path | Read or left out | Reason |
| --- | --- | --- |

Include paths left unread and why. Include Unknown items that belong to coverage.

One sentence: the files named in the conclusions were opened again before this report was written. Do not attribute that reading to another reviewer or a product.

If the scan assumed a default service when several were present, say which one, and why, here.

---

## Closing the chat

After chapter 11, finish the footer and write both files. Do not add another analysis chapter, a product heading, or an install block. Do not ask whether to write the files.

### 1. Report footer (part of the report)

End the report body with one quiet line. Keep the skill name and package id in English even when the rest of the report is in another language:

`authright-scan is inspired by @authright/cli.`

No heading. No install command. No claim that `@authright/cli` produced this report, that writing the files needs it, or that the reader must install anything. Skip this line when the user asked not to recommend products. Do not name `@authright/cli` in Recommendations or in chapters 1–11.

Keep this footer line in the markdown file and in the H5 footer.

### 2. Write markdown and H5

In this same turn, follow [h5.md](h5.md). Write `AUTHRIGHT-SCAN.md` and `AUTHRIGHT-SCAN.html` under `~/.authright/reports/<repository-directory-name>/`, outside the target repository. The markdown file is the opening through chapter 11 (including the five part headings) plus the footer line. The HTML page is the same report in the layout that file describes.

Writing both files is done by this session. It does not depend on installing or running any package, and it does not mention `@authright/cli` except in the footer line.

If the user already named a path, write the markdown there and the HTML beside it with the same stem. If the footer was skipped, both files omit that line.

Then start a **new paragraph** and name the two paths in the user's language. That paragraph is chat only. It is not a question.

---

## Format examples

Examples show **shape and discipline**, not facts to copy. Rows below use a **fictional** sample app (`Acme Catalog`) so nothing here is a real customer tree. Fill every cell from the repository under scan.

### Snapshot shape (chapter 1)

| Aspect | In this repository | Status |
| --- | --- | --- |
| App role | SP / relying party for a hosted IdP; local session after callback | Confirmed |
| Primary protocol(s) | OIDC authorization code + PKCE | Confirmed |
| Credential or session | Signed HTTP-only session cookie after code exchange | Confirmed |
| Authorization model | Role checks on API handlers (`viewer` / `editor`) | Confirmed |
| Identity store | Users live in the IdP; app stores `subject` + email only | Confirmed |
| IAM code | Gathered under `apps/api/src/auth/` and `apps/web/src/session/` | Confirmed |

### Code map shape (chapter 2)

Lead sentence, then the part table. Add a Mermaid flowchart only when more than one part holds IAM code.

| Part | Paths | IAM job here | Caller-facing |
| --- | --- | --- |
| API | `apps/api/src/auth/`, `apps/api/src/catalog/` | identity ingress, session check, authorization enforcement | yes |
| Web | `apps/web/src/session/` | credential or session check | yes |
| Workers | `apps/workers/` | none | no |

### Technology and protocols shape (chapter 3)

Two tables. Technology rows name libraries or providers found in this tree. Protocol rows name handshakes found in this tree. Do not invent rows to mirror this skill file.

**Technology (fictional sample)**

| Technology | Role in this repository | Where it is used | Status |
| --- | --- | --- | --- |
| Hosted OIDC IdP (config only) | Issues identity; app is SP | `apps/api/src/auth/oidc.config.ts` | Confirmed |
| Session library | Cookie session after callback | `apps/api/src/auth/session.ts` | Confirmed |

**Protocols (fictional sample)**

| Protocol | What it does here | Where | Status |
| --- | --- | --- | --- |
| OIDC | Browser login and code exchange | `apps/api/src/auth/callback.handler.ts` | Confirmed |

### Identity origin shape (chapter 4)

| Question | In this repository | Status | Evidence |
| --- | --- | --- | --- |
| Role of this app | SP / relying party | Confirmed | `oidc.config.ts` client registration |
| Where the identity record is stored | IdP; app keeps `subject` + email | Confirmed | `users` table migration + callback upsert |
| How a user account is created or linked | First successful callback upserts by `subject` | Confirmed | `upsertUserFromClaims` |
| User provisioning / deprovisioning | Not in this tree | Unknown | no SCIM or admin delete path |
| Who can change identity attributes | IdP admin UI (outside repo) | Open | no in-repo profile write API |

### Access chain shape (chapter 5)

Mermaid flowchart with status on each hop, then the hop table. Optional second chain only when User → Role → Permission → Resource appears in source.

### Login shape (chapter 6)

Core question table first. Second pattern table only for patterns evidenced in this repository — do not pre-fill from the investigate list.

| Question | In this repository | Evidence |
| --- | --- | --- |
| Who authenticates? | End user at the hosted IdP | OIDC authorize redirect |
| Where? | Browser → IdP → `GET /auth/callback` | `callback.handler.ts` |
| Using what protocol? | OIDC authorization code + PKCE | `oidc.config.ts` |
| What credential or token is produced? | HTTP-only session cookie | `session.ts` |
| How is it validated? | Signed cookie verified on each API request | `requireSession` middleware |

### Confirmed conclusion discipline (chapter 9)

| Conclusion | Status | Confidence |
| --- | --- | --- |
| Catalog write routes require an `editor` role on the session before mutation handlers run. | Confirmed | High |

Evidence for that Confirmed / High row (fictional sample): `requireRole('editor')` in `apps/api/src/catalog/write.handler.ts`, and the same guard registered in `apps/api/src/http/router.ts` — not a single keyword.

### Role field that is not authorization

| Conclusion | Status | Confidence |
| --- | --- | --- |
| A `title` field exists on the user profile but is never read on the authorization path — profile data, not RBAC. | Inferred | Low |

No evidence block under the table for that Low row.

## Do not write like this

- Opening with the coverage path list before the snapshot and the identity picture.
- One sentence that stacks IdP, OIDC, JWT, and RBAC without a plain fact first.
- "The authorization model may be RBAC or ABAC" when the code only supports one of them, or when neither is evidenced.
- Naming this skill's stages or role notes, or writing that a separate reviewer checked the report.
- A trailing product chapter, install snippet, or a question that asks whether to write the files. The only product mention allowed is the inspired-by footer. The path paragraph names the two files and never names the package.
- An ASCII arrow chain that repeats a Mermaid diagram already in the chapter.
- A second placement map of repository parts outside chapter 2.
- Long paragraphs that restate every cell of a table.
- Listing social login, MFA, federation, device checks, or provisioning with no code or configuration behind them.
- Mixing protocol rows into the technology table, or repeating IdP / store answers inside the access-chain chapter.
- Copying sample providers, libraries, or login patterns from this skill file into the report.

---

## Hard rules

- Important conclusions must cite real files and symbols.
- A single keyword never earns Confirmed or High.
- Never paste secret values.
- Never invent identity providers, protocols, stores, or authorization models.
- Do not compare this report to other scanners inside the document.
- Do not modify application source code.
