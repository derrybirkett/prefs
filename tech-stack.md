# Technology Stack

## Repository Structure

### NX Monorepo
- Default to NX workspace for projects with multiple related applications
- Organize code into apps and libraries
- Leverage NX caching and task orchestration
- Share common code across applications

**When to deviate**: Single-purpose applications, very small projects, or when the team has strong preference for a different build system.

**Why NX**: Provides excellent tooling for managing multiple related applications, enforces architectural boundaries, and scales well as products grow.

## Frontend

### UI Components
- **shadcn/ui**: Primary component library
- Copy components into the project (not installed as dependency)
- Customize components to match brand and requirements
- Build on Radix UI primitives for accessibility

### Framework
- Use the framework that best fits the product needs
- Ensure it works well with NX and Playwright
- Prefer frameworks with strong TypeScript support

## Testing

### Playwright
- End-to-end testing for all user flows
- Test the complete stack (frontend + backend + auth)
- Run tests as part of CI/CD pipeline
- Maintain test coverage for critical paths

**Critical Path Tests**:
1. Landing page loads correctly
2. User can sign up / log in
3. Dashboard loads with user data
4. User can log out
5. Unauthenticated users are redirected

## Version Control

### Git
- Source of truth for all code and system state
- Use conventional commit messages
- Branch-based workflow (no direct commits to main)

### GitHub
- Issues for tracking all work (features, bugs, tasks)
- Pull requests for all code changes
- Code review required before merge
- GitHub Actions for CI/CD automation

## Build Order

Default to: **CLI → API → UI**

This approach:
- Validates core logic and data models early
- Creates a reusable API for all interfaces
- UI becomes a consumer of the API rather than duplicating business logic

**When to deviate**: Consumer apps, dashboards, or UI-first products.

## Development Tools

### Required Automation
- Pre-commit hooks for linting and formatting
- Automated testing in CI
- Automated changelog generation
- Automated deployment on release

### Documentation
- In-repo documentation using Markdown
- Auto-generated API docs from code
- Keep docs close to the code they describe
