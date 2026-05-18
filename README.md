# pangeachat/workflows

Reusable GitHub Actions workflows shared across pangeachat repos.

This repo exists because **public repos cannot call private reusable workflows, and they can't inherit default community-health files from a private `.github` repo either.** We have a mix of public and private repos that share CI logic and issue templates. The actual cross-repo design / instruction docs live in [pangeachat/.github](https://github.com/pangeachat/.github) (private); only the public-safe reusable workflows and shared templates live here.

## Workflows

- `reusable-issue-to-test-check.yaml` — TO TEST checklist enforcement on issues. Posts a reminder when an issue closes without a checkbox-style TO TEST list, and applies repo-specific needs-testing labels on close. See the calling pattern in any of the [pangeachat repos that use it](https://github.com/search?q=org%3Apangeachat+%22pangeachat%2Fworkflows%2F.github%2Fworkflows%2Freusable-issue-to-test-check.yaml%22&type=code).
- `reusable-sync-issue-templates.yaml` — Syncs the caller repo's `.github/ISSUE_TEMPLATE/{bug_report,feature_request,other}.md` from the canonical copies in this repo. Opens (or updates) a PR if drift is detected. Skips `config.yml` and any repo-specific templates.

## Issue templates

The three canonical issue templates live at `.github/ISSUE_TEMPLATE/`:

- `bug_report.md`
- `feature_request.md`
- `other.md`

**Edit these here.** Public consumer repos pick up the changes via `reusable-sync-issue-templates.yaml` on their next scheduled run (or `workflow_dispatch`). Private repos inherit the same content from [pangeachat/.github](https://github.com/pangeachat/.github)'s default community-health files; keep the two in sync when editing either side.

### Wiring up a consumer

In the consumer repo, add `.github/workflows/sync-issue-templates.yml`:

```yaml
name: Sync issue templates
on:
  schedule:
    - cron: '0 13 * * 1' # weekly Mondays 13:00 UTC
  workflow_dispatch:
jobs:
  sync:
    uses: pangeachat/workflows/.github/workflows/reusable-sync-issue-templates.yaml@<sha> # main
    permissions:
      contents: write
      pull-requests: write
```

SHA-pin per [worktree-and-pr-hygiene](https://github.com/pangeachat/.github/blob/main/.github/instructions/worktree-and-pr-hygiene.instructions.md#cross-repo-reusable-workflows).
