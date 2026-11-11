# Code invariants

Read this at the start of the code-invariant stage. Cover only the effect clusters from the surface notes, write the fields below, then go back to the stage list. Do the work in this session. Do not name products. Do not write remedies.

## Job

For each caller-reachable effect, decide whether these five invariants hold in code before the effect runs. A note here is still a draft. The later re-check decides Shown, Partial, or Open.

## The five invariants

1. **Subject** — Before the effect, the code has established who is acting, or the path is marked public in code.
2. **Binding** — The subject comes from a server-validated credential or session, not from a caller-supplied user id, tenant id, or role.
3. **Action** — Allow or deny targets this action. Being logged in is not enough for every action a logged-in caller can reach.
4. **Object** — When the effect names a specific resource, the decision uses that resource (ownership, tenant, scope). A global role alone is enough only when the policy in code is global.
5. **Placement** — The decision runs before the effect, and every path that can trigger the effect goes through it. A UI-only check does not count as an API check.

Hard-coded credentials and debug entries that bypass existing guards count as subject or placement failures. Cite file and symbol only — never paste secret values.

Other defect classes (cross-site scripting, injection, generic crypto) enter only when they directly defeat an identity or access decision.

## How to read

1. Group surfaces that hit the same resource into one cluster.
2. Open the entry, the handler, and any wrapping middleware or guard. If a decision lives in another file, follow that chain before judging.
3. Without an opened chain, do not claim a check is missing.
4. A keyword, directory name, or comment can start a note. It cannot finish one.
5. Decide only from production source. Do not open test trees, and do not treat a test, a spec, a fixture, or a mock as proof that a check exists or is missing. A test that calls authentication or authorization does not count. Do not judge whether tests are complete or whether they cover a vulnerability.
6. Mark a path public only when the code marks it public, or when it does not touch a subject's resource. When the tree has no such mark, a missing decision is a draft finding.

## Labels while reading

On a single step you may write only:

- **draft** — there is code here, and it has not been re-checked
- **partial** — the code supports only part of the note
- **absent** — this invariant has no implementation on this path

Do not write Shown or High. Those labels belong to the re-check.

## Do not

- Write remedies or product names
- Invent providers or models
- Paste secret values
- Run, install, build, or edit application source
- Mark anything Shown or High

## Fields

```text
Cluster: <resource or capability>

Surfaces:
- ...

Invariant:
- name: subject | binding | action | object | placement
  holds: draft | partial | absent
  where: path + symbol
  note: ...

ChainOpened:
- path: ...
  symbol: ...

LeftUnread:
- path: ...
  why: ...
```
