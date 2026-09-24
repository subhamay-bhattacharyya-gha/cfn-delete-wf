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

## Workflow Name

### Workflow Description

This reusable GitHub Actions workflow automates CloudFormation stack deletion. It verifies stack existence using AWS OIDC authentication and deletes CloudFormation stacks across multiple environments (ci, devl, test, prod). The workflow integrates with environment-scoped variables for AWS credentials and region configuration, providing a secure and scalable approach to stack management.

---

## Inputs

| Name               | Description                                                                           | Required | Default              |
|--------------------|-------------------------------------------------------------------------------------- |----------|----------------------|
| `environment`      | GitHub environment name for accessing environment-scoped variables (ci, devl, test, prod) | No       | `ci`                 |
| `concurrency-group`| Concurrency group name for workflow runs to prevent simultaneous executions            | No       | `cfn-deploy-{env}-{ref}` |
| `stack-name`       | CloudFormation stack name to delete. Defaults to repository name if not provided       | No       | Repository name      |

---

## Example Usage

```yaml
name: Delete CloudFormation Stack

on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      environment:
        description: 'AWS Environment'
        required: false
        default: 'ci'
      stack-name:
        description: 'CloudFormation stack name'
        required: false

jobs:
  delete-stack:
    uses: subhamay-bhattacharyya-gha/cfn-delete-wf/.github/workflows/cfn-delete.yaml@v1
    with:
      environment: ${{ github.event.inputs.environment || 'ci' }}
      stack-name: ${{ github.event.inputs.stack-name || github.event.repository.name }}
      concurrency-group: cfn-delete-${{ github.repository }}-${{ github.ref }}
    secrets: inherit
```

### Environment Variables Required

Configure the following environment variables in your GitHub environment settings:

| Variable                   | Description                           |
|----------------------------|---------------------------------------|
| `AWS_REGION`               | AWS region for CloudFormation deployment |
| `AWS_ACCOUNT_ID`           | AWS account ID                        |
| `AWS_OIDC_ROLE_NAME`       | IAM role name for OIDC authentication |
| `CFN_TEMPLATES_S3_BUCKET`  | S3 bucket for CloudFormation templates |

## License

MIT
