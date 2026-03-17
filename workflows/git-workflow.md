# Git Workflow

## Branching Strategy

### Main Branch
- Always production-ready
- Protected (no direct commits)
- All changes via pull request
- Deployments happen from main

### Feature Branches
- Create a new branch for every unit of work
- Branch naming: `feature/description`, `fix/description`, `docs/description`
- Keep branches short-lived (merge within days, not weeks)
- Delete branches after merge

### Example Flow
```bash
# Start new work
git checkout main
git pull
git checkout -b feature/add-user-settings

# Make changes
git add .
git commit -m "feat: add user settings page"

# Push and create PR
git push origin feature/add-user-settings
# Create PR on GitHub
```

## Commit Conventions

Use conventional commit format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `test:` - Test changes
- `refactor:` - Code refactoring
- `chore:` - Build/tooling changes

### Commit Automation
**On every commit**: Update activity log with:
- Timestamp
- Branch name
- Commit message
- Files changed

## Pull Request Process

### Creating PRs
1. Ensure tests pass locally
2. Update documentation if needed
3. Create PR with clear description
4. Link related issues
5. Request review

### PR Requirements
- All tests passing
- Code review approval
- No merge conflicts
- Changelog updated

### Merging
- Squash and merge (keep main history clean)
- Delete branch after merge

## Push Automation

**On every push**: Update changelog with:
- Version (if applicable)
- Changes since last push
- Breaking changes (if any)
- Migration notes (if needed)

## Release Process

### Creating Releases
1. Ensure all PRs are merged
2. Tests pass on main
3. Create version tag
4. Generate release notes
5. Trigger deployment

### Release Automation
**On every release**: Create blog post with:
- Release highlights
- New features
- Bug fixes
- Migration guide (if needed)
- What's next

### Versioning
Use semantic versioning (semver):
- MAJOR: Breaking changes
- MINOR: New features (backwards compatible)
- PATCH: Bug fixes
