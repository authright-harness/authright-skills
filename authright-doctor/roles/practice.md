# Implementation practice

Read this at the start of the practice stage. Check only mechanisms the code already uses, write the fields below, then go back to the stage list. Do the work in this session. Do not name products.

## Job

Ask whether each adopted mechanism is implemented the way that mechanism needs to work. This is not a checklist of login styles the repository does not use, and it is not a demand to switch to another approach.

## Mechanisms to inspect when present

Inspect only what the code shows, for example:

- Token issuer, audience, expiry, and signature checks
- Session cookie flags (httpOnly, Secure, SameSite) and session regeneration after login
- Password storage with an adaptive hash
- Authorization-code `state` validation, and PKCE validation when PKCE is used
- Redirect allowlists
- Default deny versus default allow

## Layer routing

Every deviation must name which invariant it weakens (subject, binding, action, object, or placement).

| Situation | Layer |
| --- | --- |
| The library cannot perform the required check | **hold for the optional stack pass** — do not keep a practice finding, do not open an advisory, and do not put it in the main report |
| The design has no such decision at all | **solution** — do not keep a practice finding |
| One call site skips an otherwise correct implementation | **code** — do not keep a practice finding |
| The adopted mechanism is present but incomplete in its own setup | **practice** — keep it here |

Do not open the same issue in two layers.

## Do not

- Copy a generic best-practice list that the repository does not implement
- Demand a different login method than the code chose
- Write exploit steps or patch diffs
- Paste secret values
- Mark Shown or High here

## Fields

```text
Deviations:
- mechanism: ...
  current: ...
  requiredByMechanism: ...
  weakens: subject | binding | action | object | placement
  where: path + symbol
  staysInPractice: yes | no
  routedTo: practice | solution | stack | code
  draftStatus: draft | partial
```
