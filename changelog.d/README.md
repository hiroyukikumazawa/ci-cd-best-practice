# Changelog Fragments Directory

This directory contains the changelog fragments used by [Towncrier](https://github.com/twisted/towncrier) to generate the project's `CHANGELOG.md`.

## Creating Changelog Fragments

To create a changelog fragment for a pull request (PR), follow these steps:

1. **Choose the Correct Section**

   Decide the type of change you're making:
   - **New Features**: For adding new features.
   - **Bug Fixes**: For fixing bugs.
   - **Documentation**: For improvements or updates to the documentation.

2. **Create a Changelog Fragment**

   Use the `towncrier create` command to create a changelog fragment. Run this command from the project's root directory:

   ```bash
   towncrier create --section "<Section Name>" --dir changelog.d --content "<Description of the change>" <issue-or-pr-number>.<type>
   ```

   Replace `<Section Name>` with one of the predefined sections (`New Features`, `Bug Fixes`, `Documentation`), `<Description of the change>` with a brief summary, and `<issue-or-pr-number>` with the corresponding issue or PR number.

   For example, if you are adding a new feature:

   ```bash
   towncrier create --section "New Features" --dir changelog.d --content "Added support for automated tagging in GitHub Actions." 123.feature
   ```

3. **Available Types of Changelog Fragments**

   The following types are supported:
   - `.feature` - A new feature.
   - `.bugfix` - A bug fix.
   - `.doc` - A documentation improvement.
   - `.removal` - A deprecation or removal of a public API.
   - `.misc` - An issue has been closed, but it is not of interest to users.

4. **File Naming Convention**

   The fragment file should follow this naming pattern:

   ```
   <issue-or-pr-number>.<type>
   ```

   - `<issue-or-pr-number>`: The GitHub issue or PR number associated with the change.
   - `<type>`: The type of change (`feature`, `bugfix`, `doc`, `removal`, `misc`).

   Examples:
   - `123.feature`
   - `456.bugfix`
   - `789.doc`

## Committing Changelog Fragments

After creating the changelog fragment, commit it to the repository along with your code changes:

```bash
git add changelog.d/<issue-or-pr-number>.<type>
git commit -m "Add changelog fragment for <issue-or-pr-number>"
```

## Generating the Changelog

The changelog will be automatically generated when a pull request is merged from the `staging` branch to the `main` branch, using the GitHub Actions workflow.

## Important Notes

- **Do Not Remove Fragments Manually**: Changelog fragments will be automatically removed by Towncrier after they are incorporated into the main `CHANGELOG.md` file.
- **Keep Fragments Descriptive and Concise**: Write clear and concise descriptions of changes that will be meaningful to end users.
