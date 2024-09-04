# My CI-CD Test Project

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
- A **GitHub Actions** workflow to automatically generate changelogs and tag releases when pull requests (PRs) are merged from the `staging` branch to the `main` branch, or for production deployments.

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

### 4. Set Up GitHub Actions Workflows

The `.github/workflows` directory includes workflows for tagging releases, managing changelogs, and deploying to production or staging environments.

- **`prod-deploy.yml`**: This workflow handles production deployments and changelog management when a PR is merged from `staging` to `main`.
- **`staging.yml`**: This workflow handles tagging and changelog management for staging deployments when a PR is merged into the `staging` branch.
- **`common-tagging.yml`**: This reusable workflow contains common tagging tasks shared between production and staging workflows.

### Example Workflow Configuration

**Production Deployment Workflow (`prod-deploy.yml`):**

This workflow:

- Deploys code to production.
- Runs tagging tasks using the `common-tagging.yml` workflow.

**Staging Deployment Workflow (`staging-deploy.yml`):**

This workflow:

- Deploys code to the staging environment.
- Runs tagging tasks using the `common-tagging.yml` workflow.

**Common Tagging Workflow (`common-tagging.yml`):**

This workflow:

- Generates a changelog using Towncrier.
- Commits the updated changelog to the repository.
- Creates a new release tag in the format `v<year>.<month>.<day>-<count>`.

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

The GitHub Actions workflows automatically tag the project whenever a PR is merged from the `staging` branch to the `main` branch or when a deployment is triggered. The tag format follows `prefix-v<year>.<month>.<day>-<count>`.

## Contributing

To contribute to this project:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/my-feature`).
3. Make your changes.
4. Create a changelog entry using Towncrier.
5. Push to your fork and submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
