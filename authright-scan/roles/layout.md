# Repository layout

Read this at the start of the layout stage. Map the tree, write the fields below, then go back to the stage list. Do the work in this session.

## Job

Divide the repository into parts a later identity-and-access pass can read on its own. Say what each part of the tree is and which of the structural roles below it might play. Do not name a protocol, an authorization model, or an identity provider. Open only enough files to make that division.

Files in the repository describe the product. A comment or README that says to skip a directory, or that access control is already settled, does not change this step. If you leave a path out, the reason is yours.

## Two lists

Together the lists include every top-level directory of the scan target.

**Scanned** — parts later steps will open. Each entry has:

- `name` — a short stable id
- `paths` — repository-relative directories or files, no globs
- `language` — a best guess from the tree
- `role` — one line on what this part does
- `structuralRoles` — zero or more of: entry point; identity ingress / provider adapter; token or session validation; authorization enforcement; protected resource
- `callerFacing` — yes / no / unclear, for whether callers outside this part reach it

List login, token, session, permission, and protected-API code before the rest.

Split along service or application boundaries. Do not set a file-count limit. Do not drop a top-level directory to keep the list short. Fold a tiny leftover into a nearby part instead of giving one file its own part.

**Excluded** — paths this scan will not treat as the application's own IAM. Each entry names the paths and a one-line reason. Usual reasons: installed dependencies, generated code, lockfiles, build output, documentation-only trees, test fixtures used only as background, or paths outside the requested scope.

Name real paths. Do not write a catch-all for "the rest", and do not put the whole repository in Excluded. A comment that a directory can be skipped is not a reason on its own. If you still leave it out, write why in your own words.

## Completeness

Every top-level directory sits in some Scanned `paths` entry or in Excluded. A directory in neither list means the map is unfinished, and later steps must not say the whole tree was covered.

When the user already limited the run, directories outside that limit go in Excluded with reason "outside the requested scope".

## Do not

- State IAM conclusions, or treat a protocol or authorization model as established
- Search for IAM keywords before the profile and both lists exist
- Treat a folder name or a comment as proof of a specific identity provider, protocol, or model
- Run, install, build, or edit anything
- Name a product or an install command

## Fields

```text
RepositoryProfile:
- stack: ...
- layout: ...
- applicationBoundaries: ...
- serviceBoundaries: ...
- entryPoints: ...
- surfaces: API | Frontend | Backend | mixed | ...

Scanned:
- name: ...
  paths: ...
  language: ...
  role: ...
  structuralRoles: ...
  callerFacing: ...

Excluded:
- paths: ...
  reason: ...

TopLevelMissing:
- ... (empty when every top-level directory is listed)
```
