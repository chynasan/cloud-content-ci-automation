# CI/CD Change Detector

Detects when a PR modifies CI/CD configuration files and flags them for enhanced security review by adding a `ci-cd-change` label and posting a review checklist comment.

## Usage

```yaml
name: CI/CD Change Detection
on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  pull-requests: write

jobs:
  cicd-change-check:
    runs-on: ubuntu-latest
    steps:
      - name: Detect CI/CD changes
        uses: ansible/cloud-content-ci-automation/.github/actions/cicd_change_detector@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

> **Note:** The example above uses `@main`. For stricter supply-chain security, pin to a commit SHA (e.g., `@a1b2c3d`).

## Inputs

| Name | Required | Description |
|------|----------|-------------|
| `github-token` | Yes | GitHub token with `pull-requests: write` permission |

## Behavior

When CI/CD files are detected in a PR, the action:

1. Adds a `ci-cd-change` label to the PR (creates the label in the repo if it doesn't exist)
2. Posts a one-time security review checklist comment (skips if already posted)
