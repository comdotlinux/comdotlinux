<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-09-13 | Updated: 2026-09-13 -->

# workflows

## Purpose
One workflow, `metrics.yaml`, regenerates every SVG in the repo root using `comdotlinux/metrics` (a fork of `lowlighter/metrics` that combines several GitHub accounts) and commits the result to `main`.

## Key Files

| File | Description |
|------|-------------|
| `metrics.yaml` | Workflow `Metrics`: 8 independent jobs, one per SVG. Each runs `comdotlinux/metrics` once with its own `filename` and plugin set. |

## Job → output map

| Job | Output SVG | Config highlights |
|-----|------------|-------------------|
| `achievements` | `achievements.svg` | `base: achievements`, `plugin_achievements` threshold B, compact, secrets on |
| `github-metrics` | `header.svg` | `base: header`, classic template |
| `lines` | `lines-of-code.svg` | `base: repositories`, `plugin_lines` |
| `acti_comm` | `acti_comm.svg` | `base: activity, community` |
| `iso_calender` | `iso_calender.svg` | `base: ""`, `plugin_isocalendar` half-year |
| `habits` | `github-habits.svg` | `base: ""`, `plugin_habits` 200 events / 14 days, charts, trim |
| `issue_pr_lang` | `issue_pr_lang.svg` | `base: ""`, `plugin_followup` + `plugin_languages` |
| `repositories` | `repositories.svg` | `base: repositories` |

## Tokens
Every job passes the same two-line `token: |` block: `METRICS_SECRET` (personal account `comdotlinux`, the primary: identity, avatar, repository names and every non-merged plugin come from it) followed by `METRICS_SECRET_WORK` (work account `gurukulkarni`, secondary: contributes nameless data only, i.e. contribution calendars, base counters, language bytes, followup counts and lines-of-code totals). No `user:` is set because the action ignores it when several tokens are given (the primary is the first token's owner). A failing or missing secondary token, or a failing plugin on the secondary account, fails all 8 jobs by design so that numbers are never silently un-combined.

## Triggers
- `schedule`: `59 23 1,11,21 * *` (UTC) → 3 runs/month, approx. every 10 days.
- `workflow_dispatch`: manual.
- `push` to `main`, `paths-ignore: .github/workflows/**`.

## For AI Agents

### Working In This Directory
- `uses:` is SHA-pinned with a `# vX.Y` comment. Keep that format; dependabot relies on the comment to bump it. Do not revert to `@latest` (mutable branch).
- Each job has `permissions: { contents: write }` only. The action commits via `committer_token: GITHUB_TOKEN`; nothing else is needed. Do not widen.
- `token` carries two PATs (`METRICS_SECRET`, `METRICS_SECRET_WORK`) used only for *reading* profile data; keep the block identical on all 8 jobs. `config_timezone: USER_TIMEZONE` is also a secret.
- All 8 jobs commit to `main` concurrently. The action retries on push conflict (default `retries: 3`, `retries_delay: 300`). Expect 8 separate bot commits per run.
- New card: copy an existing job, change `filename` + plugins, then embed it in `README.md` (see root `AGENTS.md`).
- Valid inputs are defined in `comdotlinux/metrics` `action.yml` at the pinned ref. Verify a plugin input exists there before adding it.

### Testing Requirements
- `actionlint .github/workflows/metrics.yaml` locally.
- Run via `workflow_dispatch` on GitHub and check the resulting bot commits render.

## Dependencies

### External
- `comdotlinux/metrics` — composite action, single bash step pulling the prebuilt image `ghcr.io/comdotlinux/metrics:v3.36` (falls back to a local docker build if the pull fails). Pinned to v3.36 (fork of `lowlighter/metrics` v3.35-beta with Node 22, current dependency majors and multi-account merging).

<!-- MANUAL: -->
