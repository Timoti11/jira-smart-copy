# Development Workflow

## PR Naming

We use the **JSC** prefix (Jira Smart Copy) for all PRs.

### Format

```
JSC-{number}: {Brief description}
```

### Examples

- `JSC-1: Add dark mode support`
- `JSC-2: Implement keyboard shortcuts`
- `JSC-3: Fix copy formatting in Firefox`
- `JSC-4: Add support for multiple Jira instances`
- `JSC-5: Improve toast notification styling`

## Branch Naming

### Format

```
jsc-{number}-{brief-description}
```

### Examples

- `jsc-1-dark-mode-support`
- `jsc-2-keyboard-shortcuts`
- `jsc-3-fix-firefox-formatting`
- `jsc-4-multiple-jira-instances`
- `jsc-5-improve-toast-styling`

## Development Process

> **⚠️ IMPORTANT**: Always start work by running `./scripts/start-work.sh`. Never create branches or PRs manually.

### 1. Start Working

**ALWAYS** use the script to automatically create a draft PR and branch:

```bash
./scripts/start-work.sh "Brief description of the work"
```

The script automatically:
- ✅ Updates the main branch
- ✅ Determines the next available JSC number (based on PRs)
- ✅ Creates a branch with the correct name `jsc-N-description`
- ✅ Creates an empty commit for initialization
- ✅ Creates a **draft PR** (PR number = JSC number)
- ✅ Switches to the new branch

**Example:**
```bash
./scripts/start-work.sh "Add dark mode support"
# Creates: draft PR #9, branch jsc-9-add-dark-mode-support
```

**Important:** Issues are no longer created separately - draft PRs serve their function. This ensures that JSC number = PR number.

### 2. Development

1. Make commits with clear messages
2. Use conventional commits:
   - `feat:` - new functionality
   - `fix:` - bug fix
   - `docs:` - documentation changes
   - `refactor:` - code refactoring
   - `test:` - adding tests
   - `chore:` - technical changes

**Commit examples:**
```bash
git commit -m "feat(jsc-1): add dark mode toggle in popup"
git commit -m "fix(jsc-3): correct HTML formatting in Firefox"
git commit -m "docs(jsc-1): update README with dark mode info"
```

### 3. Finishing Work

When the work is ready, mark the PR as ready for review:

```bash
gh pr ready
```

Or through the GitHub web interface.

### 4. Merge

1. After review, merge the PR into main
2. Delete the branch after merging
3. The PR will be automatically closed

## Releases

### Creating a Release

```bash
# After merging all needed features into main
git checkout main
git pull origin main

# Create and push a tag
git tag v{version}
git push origin v{version}
```

This will automatically trigger the workflow to create a release.

## Version Numbering

We use [Semantic Versioning](https://semver.org/):

- **MAJOR** (1.0.0 → 2.0.0): breaking changes
- **MINOR** (1.0.0 → 1.1.0): new functionality (backwards compatible)
- **PATCH** (1.0.0 → 1.0.1): bug fixes

### Examples

- `v1.4.1` - bug fix
- `v1.5.0` - adding dark mode
- `v2.0.0` - migration to Manifest V3 (breaking change)

## Main Branch Protection

It's recommended to set up main branch protection in GitHub:

1. Settings → Branches → Add rule
2. Branch name pattern: `main`
3. Enable:
   - ✅ Require pull request before merging
   - ✅ Require approvals (if working in a team)
   - ✅ Dismiss stale pull request approvals when new commits are pushed
   - ✅ Require linear history
   - ✅ Include administrators (optional)

## Quick Commands

```bash
# ⚠️ ALWAYS start new work with this command (creates draft PR + branch)
./scripts/start-work.sh "Description of work"

# Mark PR as ready for review
gh pr ready

# Update branch from main
git checkout main && git pull && git checkout jsc-{N}-{description} && git merge main

# Delete local branch after merge
git branch -d jsc-{N}-{description}

# Delete remote branch after merge
git push origin --delete jsc-{N}-{description}
```
