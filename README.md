# Cancel

Cancel in-progress and queued workflow runs for a branch.

## What it does

- Cancels all in-progress runs for a branch
- Cancels all queued runs for a branch

## Usage

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

## Trigger Command

```bash
# In a PR comment
/cancel-runs
/cancel-runs help
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| repo | Repository | No | current repo |
| branch | Branch to cancel runs for | Yes | - |

## Example

**Comment:** `/cancel-runs`
**Bot:** Cancels all runs on the PR branch

## License

MIT