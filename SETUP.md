# Setup Instructions

## GitHub Pages Configuration

1. Go to repository Settings → Pages
2. Set Source to "GitHub Actions"
3. The site will be available at `https://thewrightmatt.github.io/markdown-page/`

## Workflow Triggers

### Manual Trigger
Run the workflow manually from the Actions tab:
1. Go to Actions → Fetch Calculator Releases
2. Click "Run workflow"
3. Select branch and click "Run workflow"

### Automatic Trigger (from Calculator Repository)

To automatically trigger this workflow when a release is created in the calculator repository, add the following workflow to `thewrightmatt/calculator/.github/workflows/`:

```yaml
name: Notify Release

on:
  release:
    types: [published]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger markdown-page workflow
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github+json" \
            -H "Authorization: Bearer ${{ secrets.PERSONAL_ACCESS_TOKEN }}" \
            https://api.github.com/repos/thewrightmatt/markdown-page/dispatches \
            -d '{"event_type":"calculator-release"}'
```

**Note:** You'll need to create a Personal Access Token with `repo` scope and add it as a secret named `PERSONAL_ACCESS_TOKEN` in the calculator repository.

### Creating a Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name like "Calculator Release Notifier"
4. Select the `repo` scope
5. Generate and copy the token
6. In the calculator repository, go to Settings → Secrets and variables → Actions
7. Create a new repository secret named `PERSONAL_ACCESS_TOKEN` with the token value

## Testing

After setup, create a new release in the calculator repository, and it should automatically appear on the markdown-page website within a few minutes.
