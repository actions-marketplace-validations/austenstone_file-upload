# Raw Upload Action

Upload files to a repo branch and get a raw GitHub URL back.

## Usage

```yaml
permissions:
  contents: write  # Required

jobs:
  upload:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5 # You must check out the repository
      - id: upload
        uses: austenstone/image-upload@v1
        with:
          path: 'images/screenshot.png'
      
      # Use the URL
      - run: echo "${{ steps.upload.outputs.url }}"
```

## Examples

**Job Summary**
```yaml
- run: echo "![Image](${{ steps.upload.outputs.url }})" >> $GITHUB_STEP_SUMMARY
```

**PR Comment**
```yaml
- uses: actions/github-script@v7
  with:
    script: |
      github.rest.issues.createComment({
        issue_number: context.issue.number,
        owner: context.repo.owner,
        repo: context.repo.repo,
        body: `![Image](${{ steps.upload.outputs.url }})`
      })
```

## Inputs/Outputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `path` | ✅ | - | File path to upload |
| `branch` | ❌ | `images` | Target branch |

| Output | Description |
|--------|-------------|
| `url` | `https://raw.githubusercontent.com/{owner}/{repo}/refs/heads/{branch}/{path}` |

## Notes

- Creates branch if it doesn't exist
- Force pushes (overwrites existing files)
- Bypasses pre-commit hooks

