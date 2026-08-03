# Reusable CI Workflows
This repository contains reusable GitHub Workflows that can be used across multiple repositories to standardize and streamline the CI/CD process. These workflows are designed to be modular and can be easily integrated into your existing GitHub Actions setup.

## Inputs

| Name               | Description            | Required | Default |
|--------------------|------------------------|----------|---------|
| run_sonar          | Run Sonar Analyst      | Yes      | false   |
| sonar_project_key  | SonarQube project key  | No       |         |
| sonar_project_name | SonarQube project name | No       |         |

## Secrets

| Name               | Description                       | Required |
|--------------------|-----------------------------------|----------|
| SONAR_HOST_URL     | SonarQube server URL              | No       |
| SONAR_TOKEN        | SonarQube authentication token    | No       |

## Example Usage
```yaml
name: CI
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
    ci:
        uses: Archbot/reusables-actions/.github/workflows/ci.yml@main
        with:
          run_sonar: true
          sonar_project_key: my-project-key
          sonar_project_name: My Project Name
        secrets:
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

## Steps
1. **Detect Stack**: The workflow starts by detecting the technology stack used in the repository. This is done using the `detect-stack` action, which analyzes the repository and outputs the detected stack.
2. **Setup**: The workflow sets up the environment based on the detected stack. This may include installing necessary dependencies, setting environment variables, and configuring the build environment.
3. **Build**: Based on the detected stack, the workflow proceeds to build the project using the `build` action. This action compiles the code and prepares it for testing.
4. **Test**: After a successful build, the workflow runs tests using the `test` action.
5. **SonarQube Analysis**: If the `run_sonar` input is set to true, the workflow will perform a SonarQube analysis using the provided project key and name. This step requires the `SONAR_HOST_URL` and `SONAR_TOKEN` secrets to be set in the repository.

