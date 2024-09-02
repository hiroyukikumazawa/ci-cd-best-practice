# My Node.js Project

This is a sample Node.js project that follows best practices for managing changelogs using [Towncrier](https://github.com/twisted/towncrier) and automating release tagging with GitHub Actions.

## Table of Contents

- [Overview](#overview)
- [Setup](#setup)
- [Changelog Management](#changelog-management)
- [Automated Release Tagging](#automated-release-tagging)
- [Contributing](#contributing)
- [License](#license)

## Overview

This project demonstrates a streamlined approach to managing changelogs and release tags. It includes:

- A **Node.js** project with a simple example script.
- **Towncrier** for managing changelogs efficiently.
- A **GitHub Actions** workflow to automatically generate changelogs and tag releases when pull requests (PRs) are merged from the `staging` branch to the `main` branch.

## Setup

### 1. Install Dependencies

Make sure you have **Node.js** and **Python** installed on your machine.

Install the necessary dependencies:

```bash
npm install
pip install towncrier
```

### 2. Configure Towncrier

Towncrier is configured using the `pyproject.toml` file located in the root of the repository.

Open `pyproject.toml` and ensure it looks like this:

```toml
[tool.towncrier]
package = "my-nodejs-project"
directory = "changelog.d"
filename = "CHANGELOG.md"
version = "0.0.1"

[[tool.towncrier.section]]
name = "New Features"
path = "features"
showcontent = true

[[tool.towncrier.section]]
name = "Bug Fixes"
path = "bugfixes"
showcontent = true

[[tool.towncrier.section]]
name = "Documentation"
path = "docs"
showcontent = true
```

### 3. Create Changelog Entries

Before merging a PR, create a changelog entry in the `changelog.d` directory:

```bash
towncrier create --section "New Features" --dir changelog.d --content "Added initial project setup." 1234.feature
```

Replace `1234.feature` with a unique identifier or related issue number.

### 4. Set Up GitHub Actions Workflow

The `.github/workflows/tag-release.yml` file defines a workflow that runs automatically when a PR is merged from the `staging` branch to the `main` branch.

This workflow:

- Generates a changelog using Towncrier.
- Commits the updated changelog to the repository.
- Creates a new release tag in the format `y-m-d-count`.

### Example Workflow Configuration

```yaml
name: Tag Release and Generate Changelog

on:
  pull_request:
    types: [closed]

jobs:
  release:
    if: github.event.pull_request.merged == true && github.event.pull_request.base.ref == 'main' && github.event.pull_request.head.ref == 'staging'
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3
        with:
          token: ${{ secrets.ACTIONS_PAT }}

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.12'

      - name: Install Towncrier
        run: pip install towncrier

      - name: Generate Changelog
        run: towncrier build --version $(date +"%Y-%m-%d") --yes

      - name: Commit and Push Changelog
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add CHANGELOG.md
          git commit -m "Update changelog for $(date +"%Y-%m-%d")"
          git push

      - name: Create Tag
        run: |
          DATE_TAG=$(date +"%Y-%m-%d")
          COUNTER=1
          TAG_NAME="${DATE_TAG}-${COUNTER}"

          # Increment counter until a unique tag name is found
          while git rev-parse "$TAG_NAME" >/dev/null 2>&1; do
            COUNTER=$((COUNTER + 1))
            TAG_NAME="${DATE_TAG}-${COUNTER}"
          done

          # Create and push the new tag
          git tag "$TAG_NAME"
          git push origin "$TAG_NAME"
```

## Changelog Management

Changelogs are managed using Towncrier, which allows for:

- Easy creation of changelog fragments for each PR.
- Automatic generation of a consolidated changelog file (`CHANGELOG.md`).

To create a new changelog entry, use:

```bash
towncrier create --section "Section Name" --dir changelog.d --content "Description of the change." unique-id.feature
```

Replace `"Section Name"` with one of the predefined sections in `pyproject.toml` and provide an appropriate description and unique ID.

## Automated Release Tagging

The GitHub Actions workflow automatically tags the project whenever a PR is merged from the `staging` branch to the `main` branch. The tag format follows `y-m-d-count`.

## Contributing

To contribute to this project:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/my-feature`).
3. Make your changes.
4. Create a changelog entry using Towncrier.
5. Push to your fork and submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
