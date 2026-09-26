# Central GitHub Workflows & Configurations

This repository (`Karthik22296/.github`) hosts reusable GitHub Actions workflows and shared engineering configurations across all personal and organization repositories.

---

## 🚀 Available Reusable Workflows

### 1. Central AI PR Review (`pr-review.yml`)
Automated code review powered by `@google/generative-ai` and `pr-review-agent`. It analyzes PR diffs, detects stack conventions, checks against guidelines, and posts findings directly on pull requests.

#### Usage in Client Repositories
Create `.github/workflows/code-review.yml` in your repository:

```yaml
name: Global AI PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  review:
    permissions:
      contents: read
      pull-requests: write
    uses: Karthik22296/.github/.github/workflows/pr-review.yml@main
    secrets: inherit
```

---

### 2. Central SonarCloud Analysis (`sonarcloud.yml`)
Automated code quality and security analysis scan via SonarCloud.

#### Usage in Client Repositories
Create `.github/workflows/sonarcloud.yml` in your repository:

```yaml
name: SonarCloud Analysis

on:
  push:
    branches: [main]
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: read

jobs:
  sonar:
    uses: Karthik22296/.github/.github/workflows/sonarcloud.yml@main
    with:
      project-key: "YourRepoProjectKey"
      organization: "karthik22296"
    secrets: inherit
```

---

## 🔐 Secrets Management

By using `secrets: inherit` in the caller job, the reusable workflow automatically accesses required repository or organization secrets:

- `GEMINI_API_KEY`: Required for AI PR Review.
- `SONAR_TOKEN`: Required for SonarCloud Analysis.

To configure secrets across multiple repositories at once using the GitHub CLI:

```powershell
@("Finace.Client", "Finance.ServerApi", "Finance.GoldenDb") | ForEach-Object {
    gh secret set GEMINI_API_KEY -b "YOUR_KEY" --repo "Karthik22296/$_"
}
```
