# Dashboard Surface

## Overview

The dashboard is the authenticated user's home base. It's the first screen after login and the hub for accessing all product features.

## Layout Structure

```
┌─────────────────────────────────────────────────┐
│  [Logo]  Navigation Items        [Profile ▼]   │
├─────────────────────────────────────────────────┤
│                                                  │
│                                                  │
│              Main Content Area                   │
│                                                  │
│                                                  │
└─────────────────────────────────────────────────┘
```

## Header Components

### Logo
- Top left
- Links to dashboard (home)
- Always visible

### Navigation
- Horizontal or vertical menu
- Links to main product sections
- Highlight active section
- Responsive (hamburger on mobile)

### User Profile Dropdown
- Top right
- Shows user name or avatar
- Dropdown menu includes:
  - User name/email
  - Settings/Profile link
  - Logout button
  - Other relevant links (billing, help, etc.)

## Main Content Area

### Dashboard Homepage
The default view after login.

**Purpose**:
- Orient the user
- Show key metrics or status
- Provide quick access to common actions

**Common Widgets**:
- Welcome message
- Recent activity
- Key metrics/statistics
- Quick action buttons
- Notifications or alerts

### Content Principles
- Most important info above the fold
- Clear visual hierarchy
- Scannable layout
- Actionable items prominent

## Responsive Design

### Desktop (>1024px)
- Full navigation visible
- Side-by-side layouts
- Multiple columns

### Tablet (768-1024px)
- Condensed navigation
- Single column or adjusted layouts
- Touch-friendly targets

### Mobile (<768px)
- Hamburger menu
- Single column layout
- Simplified widgets
- Bottom navigation (optional)

## State Management

### Loading State
- Show skeleton loaders for async data
- Maintain layout structure while loading
- Don't block entire screen unless necessary

### Empty State
- Helpful message when no data
- Clear call-to-action
- Illustrations or icons

### Error State
- Clear error message
- Suggested actions to resolve
- Retry option

## Navigation Patterns

### Primary Navigation
Main sections of the product:
- Dashboard (home)
- [Product-specific sections]
- Settings

### Secondary Navigation
Within each section:
- Tabs for sub-sections
- Breadcrumbs for deep hierarchies
- Back buttons where appropriate

### User Navigation (Profile Dropdown)
- Profile/Account settings
- Billing (if applicable)
- Help/Documentation
- Logout

## Authentication Integration

### On Load
1. Check for valid session/token
2. If invalid → redirect to login
3. If valid → load user data and render dashboard

### Session Timeout
- Detect expired session
- Show modal with options:
  - Login again
  - Return to landing page
- Save draft work if possible

## Testing Checklist

- [ ] Dashboard loads after login
- [ ] Dashboard redirects to login if not authenticated
- [ ] User profile dropdown shows correct user info
- [ ] Logout button works and redirects to landing
- [ ] Navigation links work
- [ ] Responsive design works on mobile/tablet/desktop
- [ ] Loading states show appropriately
- [ ] Error states display helpful messages
- [ ] Empty states are clear and actionable

## Accessibility

- Keyboard navigation works
- Screen reader compatible
- ARIA labels on interactive elements
- Focus indicators visible
- Color contrast meets WCAG AA

## Performance

- Initial load < 2 seconds
- Smooth transitions
- Lazy load non-critical data
- Cache static assets

## Integration Points

### With Auth
- Redirect here after login
- Check auth on every load
- Handle session expiry

### With Other Features
- Hub for accessing all features
- Consistent layout for sub-sections
- Shared navigation component

### With API
- Fetch user data on load
- Poll for notifications (if applicable)
- Handle API errors gracefully
