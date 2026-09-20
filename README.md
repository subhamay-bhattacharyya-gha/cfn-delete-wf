# GitHub Reusable Workflow : Delete Cloudformation Stack

<!-- Row 1: Status - Most Important -->
[![Release](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf/actions/workflows/release.yaml/badge.svg)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf)&nbsp;[![GitHub Action](https://img.shields.io/badge/GitHub-Action-blue?logo=github)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf)&nbsp;[![Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf/issues)&nbsp;[![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf/commits)

<!-- Row 2: Code Quality -->
[![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf)&nbsp;[![Commits](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf/commits)

<!-- Row 3: Tech Stack -->
[![CloudFormation](https://img.shields.io/badge/CloudFormation-IaC-orange?logo=amazonaws&logoColor=white)](https://aws.amazon.com/cloudformation/)&nbsp;[![Built with Claude Code](https://img.shields.io/badge/Built_with-Claude_Code-D97757?logo=anthropic&logoColor=white)](https://claude.ai/)

<!-- Row 4: Repository Info -->
[![Files](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf)&nbsp;[![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf)&nbsp;[![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-gha/cfn-delete-wf)](https://github.com/subhamay-bhattacharyya-gha/cfn-delete-wf/releases)

<!-- Row 5: Custom Metrics -->
[![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/7449d9a1c73320fca1729c07c9d4282d/raw/cfn-delete-wf.json?)](https://gist.github.com/bsubhamay/7449d9a1c73320fca1729c07c9d4282d)

---

## Action Name

### Action Description

This GitHub Action provides a reusable composite workflow that sets up Python and interacts with the GitHub API to post a comment on an issue, including a link to a created branch.

---

## Inputs

| Name           | Description         | Required | Default        |
|----------------|---------------------|----------|----------------|
| `input-1`      | Input description.  | No       | `default-value`|
| `input-2`      | Input description.  | No       | `default-value`|
| `input-3`      | Input description.  | No       | `default-value`|
| `github-token` | GitHub token. Used for API authentication. | Yes | — |

---

## Example Usage

```yaml
name: Example Workflow

on:
  issues:
    types: [opened]

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Custom Action
        uses: your-org/your-action-repo@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          input-1: your-value
          input-2: another-value
          input-3: something-else
```

## License

MIT
