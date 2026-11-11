# Report format

Use this chapter set only when the repository contains IAM. Write the report in the user's current conversation language. Present the chapters in chat first. The last analysis chapter is "How far this reading went" (translate the heading into the user's language).

After the chapters, follow [Closing the chat](#closing-the-chat). In that same turn, write the markdown file and the H5 page. Do not ask first.

When there is no IAM, do not use these chapters. Deliver the short summary from [stages.md](stages.md) instead — no footer, no path paragraph, no version-check offer, no file.

## Writing voice

Write for an engineer who will fix IAM in this repository: **action first, detail second**. Prefer tables. Keep standard terms (RBAC, OIDC, JWT, IdP, and similar). The first time each term appears, add one short clause for what it means **in this repository**.

- Each chapter opens with one lead sentence; the sentence introduces, it does not restate the table.
- Place each finding in **exactly one** layer chapter. Do not repeat the same issue across layer chapters. The priority table in chapter 2 is a one-line index only — it does not repeat evidence.
- Every kept finding must show a **suggested fix**. For Open items, the fix may be "confirm outside this repository".
- Do not mention this skill's stage names, role notes, or file names in the report.
- Do not write that another reviewer or a product checked the findings. One sentence may say the cited files were opened again before writing.
- Only report what this repository's code, configuration, and (for stack) opened advisories support.
- Never paste secret values. Cite locations only.
- Do not write exploit steps, request samples, or patch diffs.
- Do not name replacement products in stack or suggested-fix text.

## Opening

Two or three plain sentences: what matters most, and which layers have Shown findings. If a fact the reader needs lives outside the repository, say that in one sentence. Do not list coverage paths here.

## Report shape

Emit the three part headings below as level-2 headings in the report, and the numbered chapter titles as level-3 headings under them. Order is fixed: **glance → problems and fixes → bounds**.

| Part | Chapters | Reader question |
| --- | --- | --- |
| At a glance | 1–2 | What is wrong, and what should I fix first? |
| Problems and fixes | 3–6 | For each layer, what broke and how should it change? |
| Bounds | 7 | How far did this reading go, and what stays unsettled? |

---

## Finding card (required shape)

Every finding in chapters 3–6 is one card. Do not omit a field. Do **not** write the five fields as a wall of labeled paragraphs (`Problem. … Impact. …`).

**Layout (fixed):**

1. A level-4 heading: short id + broken concern in plain words  
   Example: `#### C1 — Binding: invoice detail uses caller-supplied account id`
2. One two-column table (Field | Detail). Keep each Detail cell to one idea; put lists inside the cell when needed.
3. A horizontal rule `---` between cards when there is more than one.

| Field | What goes in Detail |
| --- | --- |
| Problem | One sentence that can stand alone if quoted |
| Impact | What a caller can do — one sentence, no reproduction steps |
| Suggested fix | What to change; what to leave alone. Prefer 2–4 short bullets in the cell when there are several symbols |
| Where | Paths and symbols as a short bullet list (one path per line), not a comma-glued paragraph |
| Status | Shown / Partial / Open |

**Shape example** (fictional sample only — names and paths are invented; fill every cell from the repository under diagnosis):

```markdown
#### C1 — Binding: invoice detail uses caller-supplied account id

| Field | Detail |
| --- | --- |
| Problem | `InvoiceDetailHandler` returns another account’s invoice lines and payment status from a caller-supplied `accountId`. |
| Impact | A caller who can reach the billing API can read invoice rows for an account they name, including when `SoftAuthMiddleware` continues with no session. |
| Suggested fix | - Set `accountId` from the session principal in `getInvoiceByAccountAndPeriod`, `listPaymentEvents`, and `getOutstandingBalance`.<br>- Keep the back-office handler on `urn:ops:billing:*` for cross-account reads.<br>- Leave `listOwnInvoices` (already sets `query.accountId = req.principal.accountId`). |
| Where | - `services/billing/src/api/invoice-detail.handler.ts` — `InvoiceDetailHandler`<br>- `…/InvoiceQueryService.getInvoiceByAccountAndPeriod`<br>- `…/PaymentLedgerService.listPaymentEvents`<br>- Mounted from `apps/customer-web/.../billing/[[...params]].page.ts` and `apps/ops-console/.../billing/[[...params]].page.ts` |
| Status | Shown |
```

In chat, bullets inside a table cell may use `<br>` or a real multiline cell — pick one style and keep it for the whole report. Prefer scannable lists over one long sentence.

The first time Status appears in the report, gloss once in the user's language: Shown = verified from re-opened sources; Partial = only partly supported; Open = not settleable in this repository (or advisory not opened). Do not repeat the gloss later.

When a layer has no kept findings, one sentence pointing back to the summary — no empty table.

---

## Part: At a glance

### 1. Diagnosis summary

Reader question: which layers need attention?

One lead sentence. Then a four-row table — every layer gets a row, even when clean:

| Layer | Judgment | Status |
| --- | --- | --- |
| Overall solution | … | … |
| Technology stack | … | … |
| Implementation practice | … | … |
| Code-level access | … | … |

When a layer has no Shown finding, the Judgment cell says so in one short phrase. Do not leave cells blank.

On the main report, the Technology stack row says the version check was not run: versions may have been read from lockfiles, and advisories were not opened. Do not write that the stack is fine, current, or in no need of upgrade.

### 2. Fix first

Reader question: what should I change first?

One lead sentence. Then one priority table of **every** kept finding from the main reading. One line per finding — no evidence blocks here. Detail stays in chapters 3–6. Do not add stack rows for a version check that was not run.

| Priority | Problem | Layer | Suggested fix | Status |
| --- | --- | --- | --- | --- |

Sort rows by this order:

1. Missing decision
2. Bad binding
3. Wrong question (login as authorization, role as object)
4. Inconsistent paths
5. Revocation
6. Partial
7. Open

Within the same priority band, order layers: solution → stack → practice → code.

When there are no kept findings, one sentence: nothing to prioritize; point at chapter 7 for coverage. Do not invent rows.

---

## Part: Problems and fixes

### 3. Overall solution

Reader question: can this IAM design express and enforce the policy?

One lead sentence. Then one [finding card](#finding-card-required-shape) per solution finding: missing relationship, where the decision should live, how call sites should consume that decision after the change.

If the solution holds, one short sentence is enough — point at later chapters for real fixes. Do not expand into architecture prose.

### 4. Technology stack

Reader question: which identity or auth libraries need upgrade or replacement?

On the main report, do not answer that question. One sentence: this pass did not judge whether locked versions are affected or should be upgraded. No finding cards. Do not summarize libraries as clean.

When the repository has no IAM libraries, one sentence saying so, and do not offer the version check later.

Use the cards below only in the separate note after the user agrees to the version check (or the opening request already asked for it):

One lead sentence. Then one finding card per library conclusion that is upgrade, replace, or Open (version read, advisory not checked).

- Upgrade: Suggested fix names upgrading into a fixed range; Where includes advisory id and affected range.
- Replace: Suggested fix says which API is gone or which required check the library cannot do — without naming a replacement product.
- Conclusion "none" for a library does not get a card; say so in the lead sentence of that separate note.

That note stays outside chapters 1–7. It is not part of the markdown or H5 report unless the user asks to save it too.

### 5. Implementation practice

Reader question: which adopted mechanisms are incomplete?

One lead sentence. Then one finding card per practice deviation that stayed in this layer.

When there are no practice findings, one sentence pointing back to the summary.

### 6. Code-level access

Reader question: which caller-reachable effects break an access invariant?

One lead sentence that sets context (what these routes share), then **only** [finding cards](#finding-card-required-shape) — no prose blocks that restate Problem / Impact / Suggested fix outside the table.

Order cards as in chapter 2: missing decision → bad binding → wrong question → inconsistent paths → revocation → Partial → Open.

Number cards `C1`, `C2`, … Put the broken invariant in the heading (`subject` / `binding` / `action` / `object` / `placement`), not only buried in the Problem cell.

When there are no code findings, one sentence pointing back to the summary.

---

## Part: Bounds

### 7. How far this reading went

Reader question: what was checked, and what could this reading not settle?

One lead sentence. Then tables as needed:

**Coverage**

| Item | How far |
| --- | --- |
| Effect surfaces checked | … |
| Manifests / lockfiles read | … |
| Advisories opened | none on the main report; list them only after the optional version check |
| Paths left unread | … |

| Path | Read or left out | Reason |
| --- | --- | --- |

Include Open items from the main reading. On that reading, libraries whose versions were recorded but not checked belong in this coverage table, not as Open findings in chapter 4. Test trees left unread belong here too, with the reason that this diagnosis reads production source. Do not turn that into a finding about missing tests or uncovered vulnerabilities. If scope assumed a default service when several were present, say which one and why.

When there are zero Shown findings, use this chapter to state the scope that was read. Do not write that the application is safe.

One sentence: the files named in the findings were opened again before this report was written.

---

## Closing the chat

After chapter 7, finish the footer, write both files, then offer the version check when this note says to. Do not add another analysis chapter, a product heading, or an install block. Do not ask whether to write the files.

### 1. Report footer (part of the report)

End the report body with one quiet line. Keep the skill name and package id in English even when the rest of the report is in another language:

`authright-doctor is inspired by @authright/cli.`

No heading. No install command. No claim that `@authright/cli` produced this report, that writing the files needs it, or that the reader must install anything. Skip this line when the user asked not to recommend products. Do not name `@authright/cli` in chapters 1–7.

Keep this footer line in the markdown file and in the H5 footer.

### 2. Write markdown and H5

In this same turn, follow [h5.md](h5.md). Write `AUTHRIGHT-DOCTOR.md` and `AUTHRIGHT-DOCTOR.html` under `~/.authright/reports/<repository-directory-name>/`, outside the target repository. The markdown file is chapters 1–7 (including the three part headings) plus the footer line. The HTML page is the same report in the layout that file describes.

Writing both files is done by this session. It does not depend on installing or running any package, and it does not mention `@authright/cli` except in the footer line.

If the user already named a path, write the markdown there and the HTML beside it with the same stem. If the footer was skipped, both files omit that line. Neither file includes the version-check offer or a stack note from a later turn.

Then start a **new paragraph** and name the two paths in the user's language. That paragraph is chat only. It is not a question.

### 3. Version-check offer (chat only — not part of the report file)

Then, when the surface notes recorded at least one IAM library and the user has not already asked for a version or upgrade check, start another paragraph. Ask once, in the user's language, whether to check those locked versions against public advisories for upgrade or replacement. Say that this report did not do that check. Name no package. Wait. Do not open an advisory in this turn.

If the opening request already asked whether a library version is a problem or should be upgraded or replaced, skip this question. After the two files are written, run the optional stack pass from [stages.md](stages.md) and present that separate note.

If they later say yes, run only that pass and present the separate note. If they say no, or do not answer, stop.

---

## Hard rules

- Important findings must cite real files and symbols (and advisories for stack upgrades).
- A single keyword never earns Shown or High.
- Every kept finding has a suggested fix (including Open → confirm elsewhere).
- Never paste secret values.
- Never invent identity providers, protocols, models, or advisories.
- Never invent CVE numbers from memory.
- Do not recommend a replacement product because a newer library exists.
- Do not compare this report to other scanners inside the document.
- Do not modify application source code.
- Do not report that tests are incomplete or that they fail to cover a vulnerability.
- One finding, one layer.
