# Authentication Surface

## Overview

Authentication is the gateway between public and private surfaces. It must be secure, user-friendly, and reliable.

**Note**: This project uses a 3rd-party auth provider. The specific provider (WorkOS, Clerk, Auth0, etc.) is configured in environment variables. The flow below defines the interface contract.

## Provider Interface

The auth provider must implement:

### Required Capabilities
- Email/password authentication
- Session management
- User profile data
- Logout

### Environment Configuration
```
AUTH_PROVIDER=workos  # or clerk, auth0, etc.
AUTH_API_KEY=...
AUTH_CLIENT_ID=...
AUTH_WEBHOOK_SECRET=...
```

### User Profile Schema
```typescript
interface User {
  id: string;
  email: string;
  firstName?: string;
  lastName?: string;
  avatarUrl?: string;
  createdAt: Date;
}
```

## Components

### Sign Up Flow

**Page**: `/signup` or `/register`

**Form Fields**:
- Email (required)
- Password (required, with strength indicator)
- Confirm password (required)
- Name (optional or required based on product needs)
- Terms acceptance checkbox

**Provider Integration**:
- Submit to provider's signup API
- Handle provider-specific responses
- Sync user data to local DB if needed

**Flow**:
1. User fills form
2. Client-side validation
3. Submit to auth provider
4. Provider creates account
5. Receive user profile
6. Create local session
7. Redirect to dashboard

### Login Flow

**Page**: `/login` or `/signin`

**Form Fields**:
- Email or username
- Password
- "Remember me" checkbox (optional)
- "Forgot password?" link

**Provider Integration**:
- Submit credentials to provider
- Provider validates
- Receive session/token

**Flow**:
1. User enters credentials
2. Submit to auth provider
3. Provider validates
4. Create local session
5. Redirect to dashboard (or intended destination)

**Error Handling**:
- Generic error for invalid credentials (security)
- Clear messaging for locked accounts
- Rate limiting handled by provider

### Password Reset Flow

**Pages**: `/forgot-password`, `/reset-password`

**Note**: If provider handles password reset, link to provider's reset flow. Otherwise:

**Forgot Password Page**:
- Email input
- Submit triggers provider reset

**Reset Password Page**:
- Accessed via email link with token
- New password input
- Submit updates via provider

**Flow**:
1. User requests reset
2. Provider sends reset email
3. User clicks link
4. Provider validates token
5. User enters new password
6. Provider updates password

### Logout

**Action**: Available from user profile dropdown

**Flow**:
1. User clicks logout
2. Invalidate local session
3. Redirect to provider logout (if needed)
4. Redirect to landing page

**No confirmation needed**: Logout should be quick and easy.

## Security Requirements

### Session Management
- Secure session tokens
- HttpOnly cookies
- CSRF protection
- Session timeout (configurable)

### Provider Security
- Verify provider webhook signatures
- Sanitize user data from provider
- Handle provider errors gracefully

## User Experience

### Loading States
- Show spinner during API calls
- Disable form during submission
- Clear feedback when action completes

### Error Messages
- Client-side validation before submit
- Provider errors shown clearly
- Help users fix mistakes
- Never expose sensitive information

### Success States
- Confirmation message
- Smooth redirect
- Maintain user context

## Testing Checklist

- [ ] Sign up with valid data succeeds
- [ ] Sign up with invalid email fails
- [ ] Sign up with weak password fails
- [ ] Sign up with existing email fails
- [ ] Login with correct credentials succeeds
- [ ] Login with wrong password fails
- [ ] Login with non-existent email fails
- [ ] Password reset email sends
- [ ] Password reset with valid token works
- [ ] Password reset with expired token fails
- [ ] Logout clears session
- [ ] Logout redirects to landing page
- [ ] Protected routes redirect to login when not authenticated
- [ ] After login, user redirects to originally requested page

## Integration Points

### With Dashboard
- After successful login → redirect to dashboard
- Dashboard checks auth state on load
- Invalid session → redirect to login

### With Landing Page
- Logout → redirect to landing
- CTA buttons → link to signup/login

### With Email System
- Provider sends verification email
- Provider sends password reset email
- Sync welcome email after verification
