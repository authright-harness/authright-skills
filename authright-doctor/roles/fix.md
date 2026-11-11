# Suggested fixes

Read this at the start of the fix stage. Attach one suggested fix to each draft finding that still stands, write the fields below, then go back to the stage list. Do the work in this session.

## Job

For each finding, write the smallest change that restores the broken invariant or fixes the layer problem. Name the layer. Do not edit application source.

The fix must be usable in the report without rewriting: it fills the **Suggested fix** field on the finding card in [report.md](../report.md) (where / what to check / what stays the same), and the one-line Suggested fix cell in the Fix first priority table.

## What a suggested fix contains

- **Layer** — solution | stack | practice | code
- **Where** — existing symbol, middleware, or design layer where the change lands
- **Check** — what the decision or change must check or achieve
- **Unchanged** — which caller-visible behavior stays the same
- **Kind** — local check | model gap | outside repository
- **OneLiner** — one sentence for the Fix first table (problem stays on the finding; this line is only the action)

When an existing guard can express the fix, prefer a local check. When roles or permissions cannot express the object relationship, say which relationship is missing — do not pretend one more line of check is enough. When the evidence is outside this repository, the fix is to confirm there, not to invent a code change. Open findings still get a fix: confirm outside this tree.

Every kept finding must have a suggested fix. A finding with no suggested fix is unfinished.

## Do not

- Write exploit steps, request samples, or patch diffs
- Name products or recommend a vendor
- Paste secret values
- Raise status to Shown or High here
- Run, install, build, or edit application source

## Fields

```text
Finding: ...

Fix:
  layer: solution | stack | practice | code
  where: ...
  check: ...
  unchanged: ...
  kind: local check | model gap | outside repository
  oneLiner: ...
```
