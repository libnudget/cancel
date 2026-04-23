# Cancel

Cancel in-progress and queued workflow runs for a branch.

## What it does

- Cancels all in-progress runs for a branch
- Cancels all queued runs for a branch

## Usage

### Comment-based trigger

```yaml
name: Cancel Runs Bot

on:
  issue_comment:
    types: [created]

jobs:
  cancel:
    if: github.event.issue.pull_request && contains(github.event.comment.body, '/cancel-runs')
    runs-on: ubuntu-latest
    steps:
      - name: Get branch
        id: branch
        run: |
          branch=$(gh pr view ${{ github.event.issue.number }} --json headRefName --jq '.headRefName')
          echo "branch=$branch" >> $GITHUB_OUTPUT

      - uses: libnudget/cancel@v1
        with:
          branch: ${{ steps.branch.outputs.branch }}
```

### Label-based trigger

```yaml
name: Cancel Runs Bot

on:
  pull_request:
    types: [labeled]

jobs:
  cancel:
    if: github.event.label.name == 'cancel-runs'
    runs-on: ubuntu-latest
    steps:
      - uses: libnudget/cancel@v1
        with:
          pr_number: ${{ github.event.pull_request.number }}
          token: ${{ secrets.GITHUB_TOKEN }}
```

## Trigger Commands

```bash
# Comment on PR
/cancel-runs

# Label PR
cancel-runs
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| repo | Repository | No | current repo |
| branch | Branch to cancel runs for | No* | - |
| pr_number | PR number to get branch from | No* | - |
| token | GitHub token | Yes | - |

*Either `branch` or `pr_number` is required.

## Examples

**Comment:** `/cancel-runs`
**Bot:** Cancels all runs on the PR branch

**Label:** `cancel-runs`
**Bot:** Cancels all runs on the PR branch

## License

MIT