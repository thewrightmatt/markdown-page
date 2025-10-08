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

#### Option 1: Using workflow_call (Recommended)

```yaml
name: Notify Release

on:
  release:
    types: [published]

jobs:
  notify:
    uses: thewrightmatt/markdown-page/.github/workflows/fetch-releases.yml@main
```

This option directly calls the fetch-releases workflow and requires no additional configuration.

#### Option 2: Using repository_dispatch

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

**Note:** For Option 2, you'll need to create a Personal Access Token with `repo` scope and add it as a secret named `PERSONAL_ACCESS_TOKEN` in the calculator repository.

### Creating a Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a name like "Calculator Release Notifier"
4. Select the `repo` scope
5. Generate and copy the token
6. In the calculator repository, go to Settings → Secrets and variables → Actions
7. Create a new repository secret named `PERSONAL_ACCESS_TOKEN` with the token value

## Workflow Behavior

When the fetch-releases workflow is triggered:
1. It fetches the latest release from the calculator repository
2. Creates a new branch named after the release version (e.g., `1.0.0`, `v2.3.1`)
3. Commits the release markdown file and updated releases.json to that branch
4. Creates a pull request to merge the branch into `main`
5. After the PR is merged, the GitHub Pages deployment workflow will automatically update the website

## Testing

After setup, create a new release in the calculator repository. The workflow will:
1. Automatically create a new branch with the release version
2. Create a pull request for that release
3. Once you merge the PR, the release will appear on the markdown-page website
