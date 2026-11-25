# CLAUDE.md — donetick backend (fork)

## Why this repo exists

This is a **fork** of [donetick/donetick](https://github.com/donetick/donetick) (Go API). It adds the owner's
custom requirements and **must keep merging cleanly with upstream**. Every change you make should keep the
next `git merge upstream/develop` as small and conflict-free as possible. When two designs both work, pick
the one that touches fewer upstream lines.

- Fork: `lenonestrella/donetick` (default branch `develop`)
- Upstream: `donetick/donetick`, branch `develop`
- Sibling fork: `lenonestrella/donetick-frontend`, embedded here as the `web/` git submodule
- Nothing in this fork is meant to go back upstream. Don't shape changes for upstream PRs.
- The fork's running changes are listed in [`FORK.md`](FORK.md). **Read it before changing anything** and
  keep it current (see below).

## Fork rules (apply to every change)

### 1. Add rather than edit
- Put new behavior in **new files** (new package, new handler file, new `_test.go` file) instead of growing
  upstream files. Upstream files should only get small hooks: one import, one registration line, one call.
- Keep each touch on an upstream file to a few contiguous lines, so a conflict is small and easy to read.
- Don't rename, move, delete or re-indent upstream code. Don't fix upstream style, typos or TODOs you happen
  to pass. Unrelated cleanups in upstream files cost you at every future merge.
- Never run a formatter or linter fixer over the whole repo. Format only the files you changed
  (`gofmt -w <files>`).
- Don't edit upstream tests unless the fork deliberately changes the behavior they assert. Add fork tests in
  new `*_fork_test.go` (or similarly named) files instead.
- Don't bump dependencies in `go.mod` unless the feature needs it. Upstream bumps them often, and `go.sum`
  conflicts are noisy.

### 2. Put custom behavior behind feature flags
- New behavior that changes what upstream code does should be **off by default**. With the flag off, the app
  behaves exactly like upstream.
- Choose the flag mechanism per feature. Usually that's a field in `config.Config` (`config/config.go`), which
  viper also reads from `DT_<SECTION>_<KEY>` env vars (prefix `DT`, `.` → `_`). Where it fits, group fork
  settings under one fork-owned config struct, so `config.go` only gains a single field.
- Purely additive features (new endpoints, docs, deploy files) that upstream code paths never reach don't
  need a flag.
- Record every flag in `FORK.md`: its name, default and what it controls.

### 3. Keep `FORK.md` current, in the same commit as the change
`FORK.md` has one section per custom feature. When you add or change a feature, add or update its section:
purpose, flag(s), the fork-owned files, and **every upstream file it touches, with why and what the hook
looks like**. Also note what to watch for in a merge (e.g. "if upstream refactors `newServer`, re-add the
route registration"). Merges are resolved from this file, so a missing entry is a bug.

### 4. Backend patterns that keep merges small
- **DI wiring (`main.go`)**: don't sprinkle `fx.Provide` lines through upstream's list. Expose one fork
  `fx.Option` (e.g. `fx.Options(...)` in a fork-owned package) and add it to `fx.New(...)` with a single line.
  Register routes the same way, with one call from where upstream registers its routes.
- **Database**: prefer new tables over new columns on upstream tables. New GORM models need registering in
  `internal/database/migration.go`. Keep that to one line per model, or one call into a fork-owned function.
  Data migrations go in `migrations/` as Go `MigrationScript`s. Give fork migrations IDs prefixed `fork_`
  (e.g. `fork_20261201_add_xyz`). They can never collide with upstream's date-prefixed IDs, and because the
  registry sorts by ID they run after upstream's. Never edit an upstream migration.
- **API shapes**: add fields or endpoints. Don't change or remove fields upstream clients (including the
  upstream mobile apps) rely on.

### 5. What's out of scope
Don't modify upstream's release/CI or mobile-related files unless the owner asks:
`.github/workflows/*`, `Dockerfile`, `Dockerfile.dev`, `docker-compose.yaml`, `CODEOWNERS`, `.github/ISSUE_TEMPLATE`.
The fork deploys through its own `Dockerfile.railway` and `railway.json`.

## Syncing with upstream

The owner merges upstream into the fork (**merge, not rebase**). Never rewrite published history. When asked
to sync:

```bash
git remote get-url upstream 2>/dev/null || git remote add upstream https://github.com/donetick/donetick.git
git fetch upstream develop
git merge --no-ff upstream/develop      # on the working branch, starting from origin/develop
```

1. Resolve conflicts yourself, using `FORK.md` to see what each fork hook must still do. Keep upstream's new
   code and re-apply the fork's hook on top, rather than keeping the fork's old copy of the file.
2. If upstream now does something a fork feature did, or moved the code a hook depends on, adapt the hook and
   update the feature's `FORK.md` section.
3. Run the full checks (below). Fix anything the merge broke.
4. Check every upstream file listed in `FORK.md` still has its hook (`git diff upstream/develop -- <file>`).
5. Report: list **every conflicted file, how you resolved it and why**. Flag anything uncertain. Put this in
   the PR description or final message so the owner can review it.
6. Sync the frontend fork too (same procedure in that repo), then bump `web/` (see below). Upstream's backend
   and frontend change together, so merge both to roughly the same upstream point.

To see the fork's divergence at any time: `git diff upstream/develop...HEAD --stat`.

## The `web/` submodule (frontend)

`web/` pins the exact `donetick-frontend` commit that ships with each backend commit. `Dockerfile.railway`
builds it (`vite build --mode selfhosted`) and embeds it into `frontend/dist`. **Whenever a task changes the
frontend fork, also bump the submodule here, in its own commit**, once the frontend commit is pushed:

```bash
git submodule update --init web
git -C web fetch origin <frontend-branch> && git -C web checkout <frontend-commit-sha>
git add web && git commit -m "Bump web submodule: <what changed>"
```

The pinned commit must exist on the remote, or Railway builds fail. Railway does not check out submodules:
`Dockerfile.railway` fetches the pinned SHA itself.

## Dev guide

### Stack
Go 1.24 · Gin (HTTP) · Uber fx (dependency injection) · GORM with SQLite (`glebarez/sqlite`, pure Go) or
Postgres · viper config · swaggo for API docs. Module path: `donetick.com/core`.

### Layout
- `main.go`: fx app wiring (`fx.Provide` for every repo, service and handler) and `newServer`.
- `config/`: `config.go` (struct and `LoadConfig`); YAML per env (`local.yaml`, `selfhosted.yaml.example`).
  `DT_ENV` (`local` | `prod` | `selfhosted`) picks the file. `DT_*` env vars override any key.
- `internal/<domain>/`: one package per domain (`chore`, `circle`, `user`, `notifier`, `reward`, `project`,
  `label`, `thing`, `webhook`, `mcp`, …). Typical shape: `handler.go` (Gin routes), `model/`, `repo/`
  (GORM queries), sometimes `api.go` (eAPI for long-lived API tokens) and `scheduler.go`.
- `internal/database/`: DB setup, GORM AutoMigrate list (`migration.go`), embedded SQL migrations.
- `migrations/`: Go data migrations (`MigrationScript`, registered with `migrations.Register`, tracked by ID).
- `external/payment/`: hosted-only billing (Stripe/RevenueCat). Leave it alone.
- `frontend/`: embeds `frontend/dist` and serves the SPA when `server.serve_frontend` is on.
- `docs/`: swagger output (`swag init -g docs/handler.go`) plus fork docs (`notifications.md`, `user-guide/`).
- `web/`: frontend submodule (fork-only).

### Commands
```bash
DT_ENV=local go run .                 # API on :2021 (config/local.yaml, SQLite)
go build ./...
go test ./...                         # what CI runs
bash scripts/check-time-utc.sh        # CI check: use time.Now().UTC(), never bare time.Now()
go mod tidy && git diff --exit-code go.mod go.sum   # CI check
golangci-lint run                     # if installed (CI uses v1.64.8, config in .golangci.yaml)
docker build -f Dockerfile.railway -t donetick .    # full image incl. frontend (needs web/ checked out)
```

### Conventions upstream enforces
- Always `time.Now().UTC()`. The CI script rejects bare `time.Now()` outside tests.
- In `handler.go` files use `ShouldBindJSON`, not `ShouldBind` (ruleguard rule in `rules.go`).
- Errors and logging: use `donetick.com/core/logging` (`logging.FromContext(ctx)`).
- Permissions are per circle. Check membership and role (admin/manager/member) the way neighboring handlers do.

### Before every commit
For changes to this repo: `go build ./...`, `go test ./...` and `bash scripts/check-time-utc.sh`. Also run
`go mod tidy` if dependencies changed. If the change also touches the frontend, run that repo's checks too.
Report failures honestly. Don't skip or weaken tests to get green.
