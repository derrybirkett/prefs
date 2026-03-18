# Automation Hooks

This document defines the automation that should be triggered at various stages of the development lifecycle.

## Git Hooks

### Pre-push Hook

Prevents direct pushes to protected branches (main).

**Location**: `hooks/pre-push`

**Action**: Blocks push if targeting main directly. Forces feature branch workflow.

**To install**: Copy to `.git/hooks/pre-push` and make executable:
```bash
cp hooks/pre-push .git/hooks/
chmod +x .git/hooks/pre-push
```

---

## Git Hook Triggers

### On Commit

**Trigger**: After every `git commit`

**Action**: Update activity log

**Implementation**:
- Use Git pre-commit or post-commit hook
- Append entry to `ACTIVITY.md` or `.activity-log.json`

**Log Entry Format**:
```json
{
  "timestamp": "2026-03-17T10:30:00Z",
  "branch": "feature/add-settings",
  "commit_sha": "a1b2c3d4",
  "message": "feat: add user settings page",
  "author": "User Name",
  "files_changed": [
    "src/pages/settings.tsx",
    "src/components/SettingsForm.tsx"
  ]
}
```

**Purpose**: Track all development activity for retrospectives and context.

---

### On Push

**Trigger**: After every `git push`

**Action**: None - GitHub release notes auto-generate changelog from conventional commits.

**Why**: GitHub handles this automatically:
- `feat:` → Features section
- `fix:` → Bug Fixes section  
- `docs:` → Documentation section

---

### On Release

**Trigger**: When a version tag is created (e.g., `v1.2.0`)

**Action**: Create blog post

**Implementation**:
- Use GitHub Actions on tag creation
- Generate blog post from changelog
- Auto-publish or create draft

**Blog Post Format**:
```markdown
---
title: "Release v1.2.0: Settings and Profile Updates"
date: 2026-03-17
version: 1.2.0
---

We're excited to announce version 1.2.0 with new settings features and improvements.

## What's New

- **User Settings**: Customize your experience with new settings page
- **Profile Avatars**: Upload custom avatar images

## Improvements

- Faster dashboard loading
- Fixed login redirect issues

## Migration Guide

No breaking changes in this release.

## What's Next

Stay tuned for our next release focusing on...
```

**Purpose**: Communicate updates to users and build product narrative.

---

## Automation Tools

### Recommended Tools

**Husky**: Git hooks management
```bash
npm install --save-dev husky
npx husky init
```

**Conventional Changelog**: Generate changelog from commits
```bash
npm install --save-dev conventional-changelog-cli
```

**GitHub Actions**: CI/CD automation
- Run tests on push
- Generate changelog on push
- Create release blog post on tag

### Example Configurations

**Husky post-commit hook** (`.husky/post-commit`):
```bash
#!/bin/sh
node scripts/update-activity-log.js
```

**GitHub Action** (`.github/workflows/changelog.yml`):
```yaml
name: Update Changelog
on:
  push:
    branches: [main]
jobs:
  changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm run changelog:update
      - run: git add CHANGELOG.md
      - run: git commit -m "docs: update changelog"
      - run: git push
```

**GitHub Action** (`.github/workflows/release-blog.yml`):
```yaml
name: Create Release Blog Post
on:
  release:
    types: [published]
jobs:
  blog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: node scripts/create-release-blog.js ${{ github.event.release.tag_name }}
      - run: git add blog/
      - run: git commit -m "docs: add release blog post"
      - run: git push
```

## Scripts Directory

Create a `scripts/` directory in each project with:
- `update-activity-log.js`: Appends to activity log
- `update-changelog.js`: Updates changelog from commits
- `create-release-blog.js`: Generates blog post from release

## Configuration

Each project should have an `.automation-config.json`:
```json
{
  "activityLog": {
    "enabled": true,
    "file": "ACTIVITY.md",
    "format": "json"
  },
  "changelog": {
    "enabled": true,
    "file": "CHANGELOG.md",
    "conventionalCommits": true
  },
  "releaseBlog": {
    "enabled": true,
    "directory": "blog/releases",
    "template": "release-template.md",
    "autoPublish": false
  }
}
```

## Testing Automation

Before implementing automation:
1. Test hooks locally
2. Verify they don't block normal workflow
3. Ensure they handle errors gracefully
4. Confirm output format is correct

## Disabling Automation

For one-off exceptions:
```bash
# Skip pre-commit hooks
git commit --no-verify

# Skip CI
git push -o ci.skip
```

Use sparingly and document why in commit message.
