<!-- Generated: 2026-09-13 | Updated: 2026-09-13 -->

# comdotlinux

## Purpose
GitHub profile repository for user `comdotlinux` (Guruprasad Kulkarni). `README.md` renders on the profile page and embeds SVG stat cards. The SVGs are **generated artifacts**, regenerated nightly by `lowlighter/metrics` via GitHub Actions and committed back by `github-actions[bot]`. There is no application code.

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
- **Never hand-edit `*.svg`.** They are overwritten by the nightly workflow. To change a card, edit its job in `.github/workflows/metrics.yaml`.
- Adding a card = add a job in `metrics.yaml` (new `filename`) + add an `<a><img src="./name.svg"></a>` block in `README.md`.
- Removing a card = delete both the job and the README block; delete the stale SVG in the same commit.
- Git history is almost entirely bot commits `Update <file>.svg - [Skip GitHub Action]`. Human changes are rare; look for non-bot authors when bisecting.
- Bot commits use `GITHUB_TOKEN`, which does not trigger workflows, so no push loop.

### Testing Requirements
- No tests. Validate workflow edits with `actionlint .github/workflows/metrics.yaml` and trigger `workflow_dispatch` on GitHub to see the rendered SVG.
- Required repo secrets: `METRICS_SECRET` (PAT for reading user data), `USER_TIMEZONE`. `GITHUB_TOKEN` is automatic.

### Common Patterns
- README layout: each card is `<a href="https://github.com/comdotlinux"><img align="center" width="49%" src="./X.svg" /></a>`; a blank line between `<a>` blocks starts a new row.

## Dependencies

### External
- `lowlighter/metrics` v3.34 (SHA-pinned, dependabot monthly) — the only dependency.

<!-- MANUAL: Any manually added notes below this line are preserved on regeneration -->
