# Fork customizations — SanityHarness

- Fork: https://github.com/RichelynScott/SanityHarness.git; upstream: https://github.com/lemon07r/SanityHarness.git; branch checked: main @ 34ba88c; upstream ref: upstream/main @ 34ba88c; merge-base: 34ba88c; fork-only commits: 0; drafted by ollama-cloud/deepseek-v4.1-flash on 2026-10-24 (drafted by DeepSeek V4.1 Flash; reviewed by suni_3_n 2026-10-24)
- Expert skill: /home/richie/.hermes/skills/fork-management/sanity-harness-docs-config-expert/SKILL.md (present, 19891 bytes; not installed under ~/.agents, ~/.claude, ~/.codex, or ~/.omp skills trees); .fork-sync.yml: absent

**Result: zero verified customizations.** This fork carries no fork-only commits and no file-level divergence from upstream. `git rev-list --left-right --count HEAD...upstream/main` → `0 0`; merge-base equals HEAD; `git diff --stat 34ba88c HEAD` is empty; `git diff --name-status $(git merge-base HEAD upstream/main) HEAD` is empty. The private `origin` remote is identity evidence, not feature evidence. The rows below therefore record *absence* with its evidence, not hidden customizations.

| ID | Customization | Owner | Files / extension point | Mechanism | Supported upstream range | How to reapply after an upstream update | How to remove | Regression check | Evidence |
|---|---|---|---|---|---|---|---|---|---|
| C0 | None. Fork is a byte-identical mirror of upstream at the checked ref. | n/a (no fork-authored commit exists) | none | none (plain fast-forward mirror) | merge-base 34ba88c, upstream commit dated 2026-04-17 | Nothing to reapply; `git merge --ff-only upstream/main` suffices | Nothing to remove | `git rev-list --left-right --count HEAD...upstream/main` must print `0\t0` | 34ba88c (HEAD = origin/main = upstream/main) |
| C1 | *(not a customization — recorded to prevent misclassification)* Two unmerged bot branches exist on origin: `dependabot/go_modules/go.opentelemetry.io/otel-1.41.0` and `.../otlptrace/otlptracehttp-1.43.0`. Both are single-commit go.mod/go.sum bumps authored by `dependabot[bot] <49699333+dependabot[bot]@users.noreply.github.com>`, branched from 34ba88c, unpublished to upstream, never merged into main. | dependabot[bot] (automated) | go.mod, go.sum (indirect otel + golang.org/x/sys versions) | dependency-bump branch, not applied to main | branched from 34ba88c (2026-04-17) | Delete or merge; upstream update does not touch them | `git push origin --delete <branch>` | `git diff --stat upstream/main 78f1e0c` → 2 files, 9 +/9 -; `git diff --stat upstream/main f9ecb25` → 2 files, 39 +/37 - | 78f1e0c, f9ecb25 |

## Upstream-file patches at risk on the next update

None. There is no fork-modified upstream file to overwrite, because the fork holds no delta against `upstream/main`:

- `git diff --name-status $(git merge-base HEAD upstream/main) HEAD` → empty output.
- `git diff --stat 34ba88c HEAD` → empty output.
- All 49 local tags are merged into `upstream/main` (`git tag --no-merged upstream/main` → empty), so no fork releases diverge either.
- Working tree clean, no stash entries, no untracked/ignored files (`git status --ignored --short` → empty), single worktree at /data/Proj/SanityHarness.

Consequence: the next upstream update is a guaranteed fast-forward (`git merge --ff-only upstream/main`) with no conflict resolution and no reapply work. The only paths a future *operator* change could put at risk are the project's own contract surfaces named in the expert skill — `internal/config/config.go`, `internal/task/task.go`, `internal/cli/eval.go`, `internal/cli/verify.go`, `internal/runner/{runner,docker}.go`, `tasks/`, `docs/`, `Makefile`, `.github/workflows/{release,docker}.yml` — but none of those is locally modified today.

Facts confirmed at the checked ref (useful when re-baselining):

- `git describe --tags --always HEAD` → `v1.8.12-1-g34ba88c`; HEAD is one commit past upstream tag v1.8.12.
- `git remote -v`: origin = RichelynScott/SanityHarness (fetch+push), upstream = lemon07r/SanityHarness (fetch+push).
- `origin/HEAD -> origin/main`; `refs/remotes/upstream/HEAD` symref is **absent** locally, so the upstream default branch was taken from the presence of `upstream/main` rather than a verified symref (no network used).
- `go.mod` declares `module github.com/lemon07r/sanityharness`, `go 1.25.3` — the fork does not even rename the Go module path, further evidence of an unmodified mirror.
- No `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, `.fork-sync.yml`, `FORK*.md`, or `CUSTOMIZATIONS*.md` exists in the checkout (`git ls-files | grep -Ei 'AGENTS.md|CLAUDE.md|CODEX.md|fork-sync'` → empty; direct `ls` of the fork-record paths → No such file).
- No dependabot config file is tracked (`.github/` contains only `workflows/`), so the two bot branches come from GitHub's account-level defaults, not from a fork-owned config file.

## Unknowns

1. **Staleness of `upstream/main` — the central unknown.** All zero-delta conclusions are relative to the locally cached `upstream/main` at 34ba88c (2026-04-17). This sweep was offline by rule, so no `git fetch` / `git ls-remote` was run. If upstream has published commits since then, the fork is behind by an unmeasured amount, and the "no reapply work" conclusion still holds (a behind-only fork fast-forwards), but the *count* of pending upstream commits is UNKNOWN. Recheck with network: `git fetch upstream && git rev-list --count HEAD..upstream/main`.
2. **Upstream default branch identity** is UNKNOWN locally: `refs/remotes/upstream/HEAD` is not set and no remote query was permitted. `upstream/main` was selected because it is the only upstream ref present (`git branch -r | grep -i upstream` → `upstream/main`).
3. **Whether any fork divergence ever existed and was discarded.** Local history shows no fork-authored commit and no fork tag (`git log --all --author='RichelynScott'` → empty), but a divergence could have been pushed, then force-overwritten and garbage-collected, leaving no local trace. Cannot be verified offline — UNKNOWN, and no evidence suggests it.
4. **Two dependabot branches were inspected by diff only**, not checked out (read-only rule). Their CI/test status is UNKNOWN; `git diff --stat` against `upstream/main` is the only evidence cited for them.
5. **Expert skill path drift.** The skill names `/home/riche/Proj/SanityHarness` and `/home/riche/.hermes/...` (SKILL.md lines 19, 52, 129), while the actual checkout and install are under `/home/richie/...`. Its commands would fail as written; the facts it asserts (HEAD/origin/main/upstream/main all 34ba88c, zero fork delta, no policy files) were independently reproduced here and still match. The stale paths themselves are a skill maintenance gap, outside this ledger's scope.
