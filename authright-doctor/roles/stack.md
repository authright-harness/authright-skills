# Technology stack

Read this only for the optional stack pass, after the main report is already delivered, and only when the user agrees to the version-check offer or the opening request already asked whether a library version is a problem or should be upgraded or replaced. Do not read it during the main diagnosis. Do not open a public advisory before that pass starts.

Judge only the IAM libraries recorded in the surface notes, write the fields below, then present a separate note. Do not rewrite the main report. Do the work in this session.

## Job

Decide, for each IAM library the application imports, whether to **upgrade**, **replace**, or leave it. Versions come from lockfiles and manifests. General frameworks count only when that package itself does IAM work.

This stage is the only one that may leave the repository to open **public security advisories**.

## Upgrade

Allowed only when all of these are true:

1. You opened a public advisory in this session
2. The locked (or, if no lock, declared) version falls in the advisory's affected range
3. The advisory matter touches identity, authentication, session, or authorization

Record the advisory identifier or page. A CVE number recalled from memory is not Shown and must not drive an upgrade recommendation. If the version is known but the advisory was not opened, status is **Open**: "version read, advisory not checked" — do not recommend upgrade.

## Replace

Allowed only when:

- The API this code uses is marked removed or deprecated in that library's current usage, and the same library has no upgrade path for that API, or
- The library cannot perform a check the application already needs, and the code is bypassing it

A newer library on the market is not a replace reason. Do not name a replacement product.

## Do not

- Invent advisories or version ranges from memory
- Recommend a vendor product
- Paste secret values
- Run, install, build, or edit application source
- Treat an unused dependency as in scope

## Fields

```text
Libraries:
- name: ...
  lockedVersion: ...
  declaredIn: ...
  conclusion: upgrade | replace | none | open
  advisoryIdOrUrl: ... (upgrade only, when opened)
  affectedRange: ... (upgrade only, when opened)
  reason: ...
  draftStatus: draft | partial | open
```
