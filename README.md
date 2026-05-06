# pangeachat/workflows

Reusable GitHub Actions workflows shared across pangeachat repos.

This repo exists because **public repos cannot call private reusable workflows**, and we have a mix of public and private repos that share CI logic. The actual cross-repo design / instruction docs live in [pangeachat/.github](https://github.com/pangeachat/.github) (private); only the public-safe reusable workflows live here.

## Workflows

- `reusable-issue-to-test-check.yaml` — TO TEST checklist enforcement on issues. Posts a reminder when an issue closes without a checkbox-style TO TEST list, and applies repo-specific needs-testing labels on close. See the calling pattern in any of the [pangeachat repos that use it](https://github.com/search?q=org%3Apangeachat+%22pangeachat%2Fworkflows%2F.github%2Fworkflows%2Freusable-issue-to-test-check.yaml%22&type=code).
