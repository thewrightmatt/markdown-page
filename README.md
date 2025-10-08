# markdown-page

A GitHub Pages website that displays releases from the [calculator repository](https://github.com/thewrightmatt/calculator).

## Features

- Simple, clean interface with sans-serif fonts
- Displays markdown-formatted release notes
- Automatically fetches new releases via GitHub Actions workflow
- Can be manually triggered or automatically triggered by calculator releases
- Creates pull requests for new releases instead of directly committing to main
- Branch names match release versions for easy tracking

## Structure

- `index.html` - Main page with markdown rendering
- `style.css` - Styling with sans-serif fonts
- `releases/` - Directory containing markdown files for each release
- `releases.json` - Index of available releases
- `.github/workflows/fetch-releases.yml` - Workflow to fetch releases from calculator repo

## Usage

The website is automatically updated when:
1. A new release is published in the calculator repository (via repository_dispatch or workflow_call)
2. The workflow is manually triggered from the Actions tab
3. The workflow creates a branch named after the release version and opens a pull request
4. Once the PR is merged, the release appears on the website

Each release is saved as a markdown file named after its tag and displayed on the website.