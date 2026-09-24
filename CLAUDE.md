# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **GitHub Action/Reusable Workflow** that provides CloudFormation stack deletion and deployment automation. It's designed to be called by other workflows via `workflow_call`. The action handles:

- CloudFormation template validation and linting
- Uploading templates to S3
- Stack deployment with parameter management
- CI/CD environment-specific logic (e.g., auto-delete in CI)

The project uses **semantic-release** with custom plugins for automated versioning and GitHub release management.

## Common Commands

### Release Management

```bash
# Install dependencies
npm ci

# Run semantic-release (only in release.yaml workflow, don't run locally)
npm run release

# Note: Semantic-release is configured to run only on `main` branch via .releaserc.json
```

### Dependency Management

```bash
# Check for dependency updates
npm outdated

# Update packages (use with caution, test thoroughly)
npm update
```

## Architecture & Key Files

### Workflow Structure (`.github/workflows/`)

**cfn-delete.yaml** (Main reusable workflow)

- **Purpose**: Orchestrates CloudFormation deployment with validation, S3 upload, and deployment steps
- **Inputs**: Environment, AWS credentials, template/parameter paths, S3 bucket, stack name
- **Jobs**:
  1. `validate`: Validates CloudFormation template and linting, outputs stack name and CI suffix
  2. `upload-to-s3`: Uploads template to S3 bucket (can be skipped with `skip-s3-upload`)
  3. `deploy`: Deploys or updates stack; deletes existing stack in CI environment
- **Key Features**:
  - OIDC-based AWS authentication (no long-lived credentials)
  - Environment-scoped variables via GitHub environments
  - Concurrency control to prevent duplicate runs
  - Parameter templating with CI suffix support
  - Stack existence checking before deployment
- **release.yaml**
  - Triggers on pushes to `main` branch
  - Runs `npm ci` and `npx semantic-release`
  - Creates GitHub releases and manages CHANGELOG.md
- **create-branch.yaml**
  - Triggers when issues are assigned
  - Uses external action `subhamay-bhattacharyya-gha/create-branch-action@main`
  - Creates feature branches with GHA prefix

### Release & Plugin Configuration

**`.releaserc.json` and `scripts/plugins/release.config.js`** (Both define the same config)

- Uses standard semantic-release plugins:
  - `@semantic-release/commit-analyzer`: Analyzes commits to determine version bump
  - `@semantic-release/release-notes-generator`: Generates release notes
  - `@semantic-release/changelog`: Updates CHANGELOG.md
  - `@semantic-release/git`: Commits changelog back to repo
  - `@semantic-release/github`: Creates GitHub releases
- **Branches**: Configured to release only from `main`

**`scripts/plugins/`** (Custom plugin stubs)

- `release.config.js`: Main export used by `package.json`
- `verify-conditions.js`, `analyze-commits.js`, etc.: Custom plugin placeholders (currently not actively used)

### Root Configuration

**`package.json`**

- Main entry point for semantic-release
- Extends config from `scripts/plugins/release.config.js`
- Dependencies: semantic-release and standard plugins

**`.releaserc.json`**

- Fallback config (same as release.config.js)

## How to Modify

### Adding Workflow Inputs

1. Edit the `workflow_call.inputs` section in `cfn-delete.yaml`
2. Add new input with `type`, `description`, `required`, and `default`
3. Reference in workflow steps with `${{ inputs.your-input-name }}`
4. Update README.md Inputs table

### Updating Semantic Release Behavior

1. Modify `scripts/plugins/release.config.js` (or `.releaserc.json`)
2. Change plugins array or plugin options
3. Test by creating a PR to main (release runs automatically on merge)
4. **Important**: Never commit directly to main; always use PRs so release only happens after proper testing

### Adding/Removing Jobs

1. Edit `cfn-delete.yaml`
2. Define job at root level with `jobs.job-name`
3. Set `needs: [other-job]` to create dependencies
4. Map inputs/outputs between jobs with `${{ needs.job-name.outputs.output-key }}`

### Custom Release Plugins

If needing to add custom release behavior (beyond standard plugins):

1. Create new file in `scripts/plugins/` (e.g., `custom-step.js`)
2. Export as CommonJS module with required lifecycle methods (`verifyConditions`, `publish`, etc.)
3. Add to `release.config.js` plugins array

## Important Design Decisions

1. **OIDC Authentication**: Uses AWS OIDC roles instead of static credentials for security
2. **Environment-Scoped Variables**: GitHub environments control AWS credentials and region per environment (ci, devl, test, prod)
3. **Reusable Workflow**: Designed as `workflow_call` for composability—caller repos trigger this from their own workflows
4. **CI Environment Logic**: In CI, stack is deleted before creation for clean slate testing
5. **Semantic Release**: Automates versioning based on conventional commits (feat:, fix:, breaking:)

## Working with This Repo

### When modifying workflows

- Test changes locally by pushing to a feature branch and manually triggering via GitHub Actions UI
- Always validate YAML syntax (GitHub will catch errors on push)
- Use `${{ secrets.GITHUB_TOKEN }}` for GitHub API calls (auto-managed by GitHub Actions)

### When updating semantic-release

- Never manually edit tags or releases—semantic-release owns versioning
- Merge to `main` automatically triggers release workflow
- If release fails, check workflow logs; common issues are git/GitHub token permissions

### Branch protection

- `main` is the release branch—all changes should come via PR
- Release workflow requires specific branch in `.releaserc.json` (currently `["main"]`)
- Feature branches use GHA prefix (auto-created by create-branch action when issues are assigned)

## Key External Dependencies

- **Semantic Release** (npm): Automated versioning and release management
- **AWS Actions**: `aws-actions/configure-aws-credentials` for OIDC authentication
- **Custom Actions**: `subhamay-bhattacharyya-gha/cfn-validate-action`, `cfn-lint-action`, `create-branch-action`
- **CloudFormation**: AWS service for IaC deployment

## Testing

No automated test suites in this repo (it's a workflow/action, not a library). Testing happens through:

- GitHub Actions workflow runs (actual AWS deployments)
- Manual validation of generated CHANGELOG and GitHub releases
- Integration tests by calling repos using this action in their workflows
