# FORK.md — divergence from upstream (backend)

Upstream: `donetick/donetick@develop`. Last merged upstream commit: `78c4398` (2026-11-22).

This file lists every way this fork differs from upstream, one section per feature. Keep it accurate. Upstream
merges are resolved from it (see `CLAUDE.md`). Check it against `git diff upstream/develop...HEAD --stat`.

Section template:

```
## <Feature name>
- **Purpose**: why the fork needs it.
- **Flag**: `config.key` / `DT_ENV_VAR` (default: off), or "none — additive only".
- **Fork-owned files**: new files that upstream never touches.
- **Upstream files touched**: `path` — what the hook is and why it has to live there.
- **Merge notes**: what to check or re-apply when upstream changes the touched areas.
```

---

## Railway self-hosted deployment
- **Purpose**: deploy API and web UI as one image on Railway, with SQLite and uploads on a `/data` volume.
- **Flag**: none (additive only; upstream's `Dockerfile` and compose are untouched).
- **Fork-owned files**: `Dockerfile.railway`, `railway.json`, `.dockerignore`.
- **Upstream files touched**: none.
- **Merge notes**: if upstream changes the Go version, build ldflags (`config.Version/Commit/BuildDate`),
  health endpoint (`/api/v1/health`), config paths or `DT_*` env names, mirror them in `Dockerfile.railway`.
  `.dockerignore` also applies to upstream's `Dockerfile`. If upstream adds its own, merge the two lists.

## Frontend as a git submodule (`web/`)
- **Purpose**: pin the exact `lenonestrella/donetick-frontend` commit that ships with each backend commit.
  `Dockerfile.railway` builds and embeds it.
- **Flag**: none.
- **Fork-owned files**: `.gitmodules`, `web` (gitlink).
- **Upstream files touched**: none. `frontend/dist` (upstream's placeholder) is replaced only inside the Docker build.
- **Merge notes**: after syncing the frontend fork with upstream, bump `web/` to the merged frontend commit
  so backend and frontend match the same upstream point.

## Notifications documentation
- **Purpose**: developer overview (`docs/notifications.md`) and end-user guide (`docs/user-guide/notifications.md`)
  of the notification system.
- **Flag**: none (docs only).
- **Fork-owned files**: `docs/notifications.md`, `docs/user-guide/notifications.md`.
- **Upstream files touched**: none.
- **Merge notes**: the docs describe upstream code as of `abcb7a7`. If a merge changes `internal/notifier/`,
  `internal/notifier/dispatch`, the digest/notification planners or notification profiles, update the docs.

## Claude guidance files
- **Purpose**: instructions for Claude Code on working in this fork.
- **Flag**: none.
- **Fork-owned files**: `CLAUDE.md`, `FORK.md`.
- **Upstream files touched**: none.
