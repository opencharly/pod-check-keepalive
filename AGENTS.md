# AGENTS.md — pod-check-keepalive

Standalone candy repo for the `check-keepalive` candy — a minimal supervisord
keepalive that holds a service-less pod at steady-state for `charly check live` /
`charly check run`. The entire candy lives in `charly.yml` at the repo root: the
`check-keepalive:` entity with its `candy:` composition, `service:`, and `plan:`.
There is no source tree — the candy installs nothing of its own.

Canonical files:

- `charly.yml` — the `check-keepalive` candy entity (description, `candy`,
  `service`, `plan`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-image:layer` — the owning authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, service declarations). Load before editing,
  building, or troubleshooting this candy.
- `/charly-check:check` — the disposable-deploy R10 bed surface this candy backs
  (`charly check run <bed>`).
- `/charly-internals:git-workflow` — before any git/PR action.

The candy carries no `skill:` entity of its own; the authoring reference
`/charly-image:layer` covers the surface. The gap is routed to the named
skill-authoring batch
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291);
when that lands, add the owning skill here.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The live R10 witness is a composing check bed (`charly check run <bed>`); the
  candy's own `check:` steps assert the assembled supervisord config carries the
  keepalive program and that the live pod reports it RUNNING.

## Modify this repo

- Edit the `check-keepalive:` candy entity in `charly.yml`. The `keepalive`
  service's exec and restart policy are the whole product.
- Keep the candy distro-agnostic — do not add packages; `sleep` is in coreutils
  on every supported base.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
