# Cancel

Cancel in-progress and queued workflow runs for a branch.

## Usage

```yaml
- uses: libnudget/cancel@v1
  with:
    branch: ${{ github.head_ref }}
    repo: owner/repo
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| repo | Repository | No | current repo |
| branch | Branch to cancel runs for | Yes | - |

## License

MIT