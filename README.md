# Global Digital Collaboration Meeting

This repository adapts `w3c/tpac-breakouts` tooling for GDC session intake and scheduling.

## What Is In This Repo

- `.github/ISSUE_TEMPLATE/session.yml`: issue form used to collect session proposals.
- `.github/workflows/validate-session.yml`: auto-validates new/edited session issues.
- `.github/workflows/validate-grid.yml`: manual validation of all sessions.
- `.github/workflows/view-event.yml`: manual HTML export of current schedule.
- `.github/session-created.md`: auto-comment posted on new session issues.
- `package.json`: pins `tpac-breakouts` CLI dependency.
- `w3c.json`: metadata file kept for compatibility with W3C conventions.

## CLI Usage (Local)

Prerequisites:

- `gh auth login` completed.
- `EVENT`, `ROOMS`, `SLOTS` GitHub variables configured in this repo.

Use these commands from repo root:

```bash
# validate one session
REPOSITORY='user:mgaitan/gdc2026' GRAPHQL_TOKEN=$(gh auth token) GH_TOKEN=$(gh auth token) \
npx tpac-breakouts validate 1 --what everything

# export current schedule to HTML
REPOSITORY='user:mgaitan/gdc2026' GRAPHQL_TOKEN=$(gh auth token) GH_TOKEN=$(gh auth token) \
npx tpac-breakouts view --format html > /tmp/gdc-grid.html

# propose an automatic schedule and export to HTML
REPOSITORY='user:mgaitan/gdc2026' GRAPHQL_TOKEN=$(gh auth token) GH_TOKEN=$(gh auth token) \
npx tpac-breakouts schedule > /tmp/gdc-schedule.html
```

Where HTML files are generated:

- Local CLI: wherever you redirect output (`/tmp/gdc-grid.html`, `/tmp/gdc-schedule.html`, etc.).
- GitHub Actions: `view-event` uploads artifact named `schedule`.

## User Repo vs Organization Repo

Current repo is user-owned, so workflows set:

- `REPOSITORY: user:${{ github.repository }}`

When migrating to an organization repo, remove the `user:` prefix in workflows.

## Tooling Note

- `tpac-breakouts` is pinned to `7d5fba4` (last known CLI-working commit for this setup).
- Problematic upstream commit (breaks CLI import path on `main`):  
  https://github.com/w3c/tpac-breakouts/commit/56e671f19f64c1e0522f66fcd1227f98511413b4
