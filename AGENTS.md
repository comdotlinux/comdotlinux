<!-- Generated: 2026-09-13 | Updated: 2026-09-14 -->

# comdotlinux

## Purpose
GitHub profile repository for user `comdotlinux` (Guruprasad Kulkarni). `README.md` renders on the profile page and embeds SVG stat cards. The SVGs are **generated artifacts**, regenerated on a schedule by `comdotlinux/metrics` (a maintained fork of `lowlighter/metrics`) via GitHub Actions and committed back by `github-actions[bot]`. There is no application code.

## Key Files

| File | Description |
|------|-------------|
| `README.md` | Profile page. Pure HTML `<a><img>` grid, two 49%-width cards per row. Embeds 7 of the 8 SVGs. |
| `header.svg` | Generated: classic header card (`base: header`). |
| `repositories.svg` | Generated: repositories card (`base: repositories`). |
| `acti_comm.svg` | Generated: activity + community card. |
| `iso_calender.svg` | Generated: half-year isometric commit calendar. Filename spelling is intentional; README references it. |
| `issue_pr_lang.svg` | Generated: followup (issues/PRs) + languages plugins. |
| `github-habits.svg` | Generated: habits plugin, last 14 days / 200 events. |
| `achievements.svg` | Generated: achievements rank B+, compact. |
| `lines-of-code.svg` | Generated: lines plugin. **Not embedded in README** — generated daily but never displayed. |
| `LICENSE` | MIT, 2024. |
| `.gitignore` | Ignores `.omc/` (local agent runtime state). |

## Subdirectories

| Directory | Purpose |
|-----------|---------|
| `.github/` | Actions workflow + dependabot config (see `.github/AGENTS.md`) |
| `.omc/` | Local oh-my-claudecode runtime state. Gitignored, do not document or commit. |

## For AI Agents

### Working In This Directory
- **Never hand-edit `*.svg`.** They are overwritten by the scheduled workflow (3 runs/month, see `.github/workflows/AGENTS.md`). To change a card, edit its job in `.github/workflows/metrics.yaml`.
- Adding a card = add a job in `metrics.yaml` (new `filename`) + add an `<a><img src="./name.svg"></a>` block in `README.md`.
- Removing a card = delete both the job and the README block; delete the stale SVG in the same commit.
- Git history is almost entirely bot commits `Update <file>.svg - [Skip GitHub Action]`. Human changes are rare; look for non-bot authors when bisecting.
- Bot commits use `GITHUB_TOKEN`, which does not trigger workflows, so no push loop.
- Every job passes the same two-line `token: |` block: `METRICS_SECRET` (personal account `comdotlinux`, the primary) then `METRICS_SECRET_WORK` (work account `gurukulkarni`, secondary). The secondary contributes **nameless data only** — contribution calendars, base counters, language bytes, followup counts, lines-of-code totals — never repository or organization names. Keep the block identical on all 8 jobs.
- No job sets `user:`; the action ignores it (with a log line) as soon as more than one token is given, and takes the primary from the first token's owner.
- Failure is fail-closed by design: a bad/expired secondary token, or a plugin that fails for the secondary account, fails all 8 jobs rather than silently emitting primary-only numbers.

### Testing Requirements
- No tests. Validate workflow edits with `actionlint .github/workflows/metrics.yaml` and trigger `workflow_dispatch` on GitHub to see the rendered SVG.
- Required repo secrets: `METRICS_SECRET` and `METRICS_SECRET_WORK` (PATs for reading user data, personal and work account), `USER_TIMEZONE`. `GITHUB_TOKEN` is automatic. If `METRICS_SECRET_WORK` is missing the block collapses to one token and the run is effectively single-account — the header then shows only the primary name and no work data is merged in.
- A `workflow_dispatch` run now takes about a minute per job (the action pulls the prebuilt image instead of building it), not the 8-10 minutes it used to.

### Common Patterns
- README layout: each card is `<a href="https://github.com/comdotlinux"><img align="center" width="49%" src="./X.svg" /></a>`; a blank line between `<a>` blocks starts a new row.

## Dependencies

### External
- `comdotlinux/metrics@bf2f1203ec9cbcc1fc40a281144bff9acb17c2ac # v3.36` (SHA-pinned, dependabot monthly) — the only dependency. Its single bash step pulls the prebuilt image `ghcr.io/comdotlinux/metrics:v3.36` (public, anonymous pull; falls back to a local docker build if the pull fails), which is why jobs take ~1 min.
- That fork is maintained locally at `/home/guru/other-repos/metrics` (branch `master`, tags `v3.35`/`v3.36`). Valid inputs come from its `action.yml` at the pinned ref, so a workflow change that needs a new or changed input has to ship in the fork first (new tag + image), then be re-pinned here.

<!-- MANUAL: Any manually added notes below this line are preserved on regeneration -->
