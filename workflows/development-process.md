# Development Process

## Work Tracking

### GitHub Issues
- All work starts with an issue
- Use issue templates for consistency
- Label issues appropriately (bug, feature, docs, etc.)
- Assign issues to track ownership
- Close issues when work is complete

### Issue Lifecycle
1. **New**: Issue created, needs triage
2. **Backlog**: Triaged, waiting to be picked up
3. **In Progress**: Actively being worked on
4. **Review**: PR open, awaiting review
5. **Done**: Merged and closed

## Development Cycle

### 1. Pick Work
- Select issue from backlog
- Ensure you understand requirements
- Ask questions if anything is unclear

### 2. Branch and Implement
- Create feature branch from main
- Write implementation code
- Write tests for new functionality
- Run tests locally

### 3. Validate End-to-End Flow
Test the complete user journey:
1. Landing page → Login → Dashboard → Logout (base flow)
2. Your new feature integrated into the flow
3. Edge cases and error states

### 4. Document Changes
- Update relevant documentation
- Add inline comments for complex logic
- Update README if behavior changes

### 5. Create Pull Request
- Push branch to GitHub
- Create PR with clear description
- Link related issue(s)
- Request review from relevant stakeholders

### 6. Address Review Feedback
- Make requested changes
- Push updates to the same branch
- Re-request review

### 7. Merge and Deploy
- Squash and merge PR
- Verify deployment (if automatic)
- Monitor for issues

## Testing Strategy

### Test Pyramid
1. **Unit Tests**: Test individual functions and components
2. **Integration Tests**: Test component interactions
3. **E2E Tests**: Test complete user flows (Playwright)

### Critical Path Testing
Always maintain passing tests for:
- User authentication (signup, login, logout)
- Dashboard loading
- Navigation between major sections
- Data persistence

### Test Before Push
- Run full test suite locally
- Fix any failures before pushing
- Don't commit broken tests

## Quality Standards

### Code Quality
- TypeScript strict mode
- ESLint with no warnings
- Prettier for consistent formatting
- No console.logs in production code

### Documentation Quality
- README with setup instructions
- API documentation for public functions
- Inline comments for non-obvious code
- Architecture decisions recorded

### User Experience Quality
- Responsive design (mobile, tablet, desktop)
- Accessible (WCAG AA compliance)
- Fast (lighthouse score > 90)
- Error messages are helpful
