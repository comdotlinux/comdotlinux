<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-09-13 | Updated: 2026-09-13 -->

# workflows

## Purpose
One workflow, `metrics.yaml`, regenerates every SVG in the repo root using `lowlighter/metrics` and commits the result to `main`.

## Key Files

| File | Description |
|------|-------------|
| `metrics.yaml` | Workflow `Metrics`: 8 independent jobs, one per SVG. Each runs `lowlighter/metrics` once with its own `filename` and plugin set. |

## Job → output map

| Job | Output SVG | Config highlights |
|-----|------------|-------------------|
| `achievements` | `achievements.svg` | `base: achievements`, `plugin_achievements` threshold B, compact, secrets on |
| `github-metrics` | `header.svg` | `base: header`, classic template |
| `lines` | `lines-of-code.svg` | `base: repositories`, `plugin_lines`. No `user:` set → defaults to repo owner |
| `acti_comm` | `acti_comm.svg` | `base: activity, community` |
| `iso_calender` | `iso_calender.svg` | `base: ""`, `plugin_isocalendar` half-year |
| `habits` | `github-habits.svg` | `base: ""`, `plugin_habits` 200 events / 14 days, charts, trim |
| `issue_pr_lang` | `issue_pr_lang.svg` | `base: ""`, `plugin_followup` + `plugin_languages` |
| `repositories` | `repositories.svg` | `base: repositories` |

## Triggers
- `schedule`: `59 23 */10 * *` (UTC) → 1st/11th/21st/31st of each month, approx. every 10 days.
- `workflow_dispatch`: manual.
- `push` to `main`, `paths-ignore: .github/workflows/**`.

## For AI Agents

### Working In This Directory
- `uses:` is SHA-pinned with a `# vX.Y` comment. Keep that format; dependabot relies on the comment to bump it. Do not revert to `@latest` (mutable branch).
- Each job has `permissions: { contents: write }` only. The action commits via `committer_token: GITHUB_TOKEN`; nothing else is needed. Do not widen.
- `token: METRICS_SECRET` is a PAT used only for *reading* profile data. `config_timezone: USER_TIMEZONE` is also a secret.
- All 8 jobs commit to `main` concurrently. The action retries on push conflict (default `retries: 3`, `retries_delay: 300`). Expect 8 separate bot commits per run.
- New card: copy an existing job, change `filename` + plugins, then embed it in `README.md` (see root `AGENTS.md`).
- Valid inputs are defined in `lowlighter/metrics` `action.yml` at the pinned ref. Verify a plugin input exists there before adding it.

### Testing Requirements
- `actionlint .github/workflows/metrics.yaml` locally.
- Run via `workflow_dispatch` on GitHub and check the resulting bot commits render.

## Dependencies

### External
- `lowlighter/metrics` — composite action, single bash step pulling a docker image. Pinned to v3.34 (latest release; identical to the upstream `latest` branch as of 2026-09).

<!-- MANUAL: -->
