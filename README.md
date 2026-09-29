# pod-check-keepalive

The `check-keepalive` candy of the OpenCharly candy library, as a standalone repo
(the candy de-submodule cutover, kind-prefixed naming). It is a minimal
supervisord keepalive that holds a **service-less pod** at steady-state.

## What it provides

A single `keepalive` supervisord program running `/usr/bin/sleep infinity`, so a
pod whose layer-under-test is a build-time install (with no service of its own)
still reaches steady-state for `charly check live` / `charly check run`. It also
gives supervisord a valid `/etc/supervisord.conf`, which its baked
`supervisorctl pid` check needs — an assembled config only exists when some layer
contributes a service.

| Property | Value |
|---|---|
| Service | `keepalive` (`/usr/bin/sleep infinity`, `restart: always`) |
| Composes | `layer-supervisord` |
| Packages | none (`sleep` is in coreutils on every base) |
| Distros | distro-agnostic — arch, fedora, debian |

The candy installs nothing of its own; its observable effects are the composed
supervisord binary and the assembled config carrying the keepalive program.

## How to use it

Compose the candy into a test image that backs a `kind: check` pod bed:

```yaml
vscode-test:
  candy:
    base: cachyos.cachyos
    candy:
      - '@github.com/opencharly/pod-check-keepalive:<tag>'
```

## Layout

- `charly.yml` — the `check-keepalive` candy entity (description, `candy`
  composition, `service`, plan).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning procedure: `/charly-image:layer` — candy authoring reference
  (`charly.yml` schema, service declarations).
- `/charly-check:check` — the disposable-deploy R10 bed surface this candy backs.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
