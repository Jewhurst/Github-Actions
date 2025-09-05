# GitHub Actions Collection

This repository contains a collection of reusable GitHub Actions workflows for automating common repository tasks.

## Workflows

### 1. Auto Assign PR Author (`auto-assign.yml`)

**Purpose**: Automatically assigns the PR author to their own pull request if no assignee is set.

**Triggers**:
- When a pull request is opened
- When a pull request is synchronized (updated)

**Features**:
- Only runs if the PR has no existing assignee
- Uses the GitHub CLI to assign the author
- Requires minimal permissions (read contents, write pull requests)

**Usage**: 
Simply add this workflow to your repository. It will automatically run whenever a new PR is created or updated, ensuring every PR has an assignee for better tracking and accountability.

### 2. Release on Merge (`github-release.yml`)

**Purpose**: Automatically creates GitHub releases with semantic versioning when pull requests are merged to the master branch.

**Triggers**:
- When a pull request is closed (merged) to the master branch

**Features**:
- **Smart Version Calculation**: Automatically calculates the next version number based on PR labels
  - `major` label: Increments major version (e.g., v1.0.0 → v2.0.0)
  - `minor` label: Increments minor version (e.g., v1.0.0 → v1.1.0)
  - No label: Increments patch version (e.g., v1.0.0 → v1.0.1)
- **Branch-Based Versioning**: 
  - Master branch: Creates production releases (e.g., v1.0.0)
  - Other branches: Creates development releases with `-dev` suffix (e.g., v1.0.0-dev)
- **Skip Release Option**: Add `no-release` label to a PR to prevent release creation
- **Automatic Release Notes**: Generates release notes automatically from commit history
- **PR Labeling**: 
  - Creates version labels and applies them to the main PR
  - Automatically finds and labels nested PRs that were merged as part of the release
- **Full Git History**: Fetches complete git history for accurate version calculation

**Version Labels**:
- Add `major` label for breaking changes
- Add `minor` label for new features
- Add `no-release` label to skip release creation
- No label defaults to patch version bump

**Usage**:
1. Add version labels to your PRs based on the type of changes
2. Merge PRs to master branch
3. The workflow will automatically create a new release with:
   - Calculated version number
   - Auto-generated release notes
   - Version label applied to all related PRs

## Setup Instructions

1. Copy the desired workflow files to `.github/workflows/` in your repository
2. Ensure your repository has the following permissions enabled:
   - Actions: Read and write permissions
   - Pull requests: Write permissions
   - Contents: Write permissions (for releases)
   - Issues: Write permissions (for labels)

## Requirements

- GitHub CLI is used in both workflows (pre-installed on GitHub runners)
- Standard `GITHUB_TOKEN` permissions (automatically available)
- No additional setup or secrets required

## Permissions

Both workflows use minimal required permissions:

**Auto Assign**:
- `contents: read`
- `pull-requests: write`

**Release Creation**:
- `contents: write` (for creating releases)
- `issues: write` (for creating labels)
- `pull-requests: write` (for labeling PRs)