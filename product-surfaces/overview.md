# Standard Product Surfaces

Every digital product should include these core surfaces. They represent the minimum viable product structure.

## Required Surfaces

### 1. Marketing Page (Landing Page)
The public face of the product.

**Purpose**:
- Communicate value proposition
- Drive user acquisition
- Provide clear call-to-action

**Key Elements**:
- Hero section with clear headline
- Feature highlights
- Social proof (testimonials, logos, metrics)
- CTA buttons (Sign up, Learn more)
- Footer with links

**Success Metric**: Conversion to signup

---

### 2. Authentication Flow
User identity and access management.

**Components**:
- Sign up page
- Login page
- Password reset flow
- Email verification (if needed)
- OAuth integration (optional)

**Key Elements**:
- Clear form validation
- Error messaging
- Loading states
- Success redirects
- Security best practices

**Success Metric**: Successful login rate

---

### 3. Dashboard
The primary authenticated experience.

**Purpose**:
- User's home base after login
- Quick access to key features
- Display personalized data

**Key Elements**:
- User profile dropdown (top right)
  - User name/email
  - Settings link
  - Logout button
- Navigation menu
- Main content area
- Quick actions

**Success Metric**: User engagement and return rate

---

### 4. Blog
Content marketing and communication.

**Purpose**:
- Share updates and announcements
- Educate users
- Improve SEO
- Build community

**Key Elements**:
- Post listing page
- Individual post pages
- Categories/tags
- RSS feed
- Social sharing

**Success Metric**: Traffic and engagement

---

### 5. Documentation Site
User education and reference.

**Purpose**:
- Help users get started
- API/feature reference
- Troubleshooting guides
- Reduce support burden

**Key Elements**:
- Getting started guide
- Feature documentation
- API reference (if applicable)
- Search functionality
- Navigation sidebar

**Success Metric**: Self-service success rate

## Core User Flow

The fundamental path that must always work:

```
Landing Page
    ↓
  Login
    ↓
Dashboard
    ↓
 Logout
```

**Testing Priority**: This flow must be tested end-to-end on every change.

All additional features are built on top of this working foundation.

## Surface Dependencies

```
Landing Page (public)
    ↓
   Auth
    ↓
Dashboard (authenticated)
    ↓
Other Features (authenticated)

Blog (public/semi-public)
Docs (public)
```

## Implementation Notes

- Marketing page and blog can share styling/components
- Dashboard and authenticated features share layout/navigation
- Docs can be separate or integrated into marketing site
- All surfaces should be mobile-responsive
- All surfaces should follow brand guidelines
