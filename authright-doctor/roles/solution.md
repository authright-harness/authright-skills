# Overall solution

Read this at the start of the solution stage. Judge the design shape from code already opened, write the fields below, then go back to the stage list. Do the work in this session. Do not name products.

## Job

Ask whether this IAM design can express and enforce the policy — not whether one call site forgot an existing guard.

## Counts as a solution problem

- Authentication and authorization are collapsed into one check with no separate allow/deny for the action
- An effect needs object-level policy, and the existing roles or permissions cannot express that relationship
- Decisions are scattered so a policy change cannot land in one place
- There is no shared admit point on the caller boundary; only some clients enforce
- Identity-provider details leak into every business decision

## Does not count here

A single path that skips an existing guard stays in the **code** layer. Do not condemn the whole design for one miss.

## Standard for a draft finding

At least **two independent** pieces of code or configuration must support that this is the shape of the design, not a local slip. Comments and README claims do not count.

When the solution holds, write one short note that it holds, and point later layers at the real fixes. Do not invent architecture advice.

## Do not

- Raise status to Shown or High here (re-check does that)
- Write exploit steps or patch diffs
- Name products
- Paste secret values

## Fields

```text
SolutionHolds: yes | no | unclear

Findings:
- claim: ...
  evidence:
  - path: ...
    symbolOrKey: ...
  missingRelationOrLayer: ...
  draftStatus: draft | partial

NoteWhenHolds: ...
```
