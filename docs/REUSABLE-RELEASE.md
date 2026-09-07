# Reusable Release Workflow
This workflow is designed to automate the release process for your project. It handles versioning, changelog generation, and publishing of releases. The workflow is modular and can be easily integrated into your existing GitHub Actions setup.

## Inputs

| Name             | Description                                                                     | Required | Default                                        |
|------------------|---------------------------------------------------------------------------------|----------|------------------------------------------------|
| `git_user_name`  | The name of the Git user for the release commit                                 | false    | 'github-actions[bot]'                          |
| `git_user_email` | The email of the Git user for the release commit                                | false    | 'github-actions[bot]@users.noreply.github.com' |

## Usage

```yaml
name: Release
on:
  workflow_dispatch:
  push:
    branches:
      - develop

jobs:
  release:
    uses: Archbot/reusables-actions/.github/workflows/release.yml@main
    with:
      git_user_name: 'Your Name'
      git_user_email: 'your.email@example.com'
```

## Steps
1. **Checkout**: The workflow starts by checking out the repository using the `actions/checkout` action.
2. **Setup Git**: It sets up Git configuration using the provided username and email for commits.
3. **Detect Stack**: The workflow detects the technology stack used in the repository to ensure compatibility with the release process.
4. **Setup Environment**: It sets up the necessary environment based on the detected stack, including installing dependencies and configuring the build environment.
5. **Determine Version**: The workflow determines the current version of the project and calculates the next version based on the specified bump type (major, minor, or patch).
6. **Update Version**: It updates the version in the relevant files (e.g., `package.json`, `pom.xml`, etc.) to reflect the new release version.
7. **Generate Changelog**: The workflow generates a changelog based on the commits since the last release, summarizing the changes made in the new version.
8. **Commit Changes**: The workflow commits the updated version and changelog files to the repository.
9. **Create Release**: It creates a new release on GitHub with the updated version and changelog.
10. **Create tag**: The workflow creates a new git tag corresponding to the release version.
11. **Push Next Development Version**: Finally, the workflow bumps the version for the next development cycle and pushes