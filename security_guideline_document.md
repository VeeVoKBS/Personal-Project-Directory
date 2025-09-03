# Implementation Plan for Personal Project Directory

This implementation plan outlines the architecture, components, data models, workflows, security measures, testing strategy, and deployment roadmap for the “Personal Project Directory” platform (Web + Mobile MVP).

---

## 1. High‐Level Architecture

```plaintext
+------------------+        +----------------+        +-----------------
|  Next.js Web     | <–API–> |  Supabase      | <–>   |  Postgres DB    |
|  (Tailwind, UI)  |        |  Auth, Storage |       | (Projects,      |
|                  |        |                |       |  Users, Metrics)|
+------------------+        +----------------+        +-----------------
        |  ▲    |                                             ▲
        |  |    |                                             |
        v  |    v                                             |
+------------------+   ↔    +---------------+  ↔  +----------------+
| Expo Mobile App  |        |  Stripe       |     |  Social APIs   |
| (React Native)   |        | (Payments)    |     |  (GitHub,      |
+------------------+        +---------------+     |   LinkedIn,    |
                                                 |   Twitter)     |
                                                 +----------------+
         
+--------------------+     +----------------+    +----------------+
|  OpenAI GPT‐4      |     |  Google AdSense|    |  Sentry        |
|  (AI Design Assist)|     |  (Ads)         |    |  (Error Mon)   |
+--------------------+     +----------------+    +----------------+
```  

### 1.1 Key Integration Points
- **Authentication**: Google OAuth (Supabase Auth)
- **Data Storage**: Supabase Postgres, Supabase Storage
- **Social Metrics**: Server-side scheduled jobs (e.g., Supabase Edge Functions / CRON)
- **Payments**: Stripe Webhooks & Checkout
- **Ads**: Google AdSense script on free‐user pages
- **AI Assistant**: Next.js API routes → OpenAI GPT-4
- **Monitoring**: Sentry DSN via env vars

---

## 2. Functional Components

1. **Authentication & Authorization**
   - Google OAuth only (passwordless)
   - RBAC: Free vs. Paid roles
2. **Project Management**
   - CRUD on projects (title, description, tags, media, links, status)
   - Engagement metrics view
3. **Social Integration**
   - Periodic fetch (24 h) of GitHub, LinkedIn, Twitter metrics
4. **Customization & AI Design**
   - Drag-and-drop editor (shadcn/ui)
   - AI layout/text suggestions (OpenAI GPT-4)
5. **Monetization**
   - Free: ads (AdSense)
   - Paid: Stripe subscriptions, customizable themes
6. **Mobile App**
   - Mirror key features (View & manage projects)
   - Offline caching (optional)
7. **Notifications & Emails**
   - Welcome, subscription events (SendGrid)
---

## 3. Data Model (Supabase / Postgres)

Tables:

- **users**
  • id (UUID, PK)
  • email (unique)
  • role (enum: free, paid)
  • created_at, updated_at

- **projects**
  • id (UUID, PK)
  • user_id (FK→ users.id)
  • title, description, summary
  • tags (array)
  • status (enum)
  • live_url, repo_url
  • created_at, updated_at

- **media**
  • id, project_id, type (image, video), url

- **social_metrics**
  • id, project_id, source (GitHub/LinkedIn/Twitter)
  • likes, comments, stars, forks, retweets, replies
  • fetched_at

- **subscriptions**
  • id, user_id, stripe_sub_id, status, current_period_end

- **theme_customizations** (paid users)
  • id, user_id, json_config

---

## 4. Authentication Flow

1. User clicks “Login with Google.”
2. Next.js calls Supabase Auth OAuth endpoint.
3. On success, Supabase issues a JWT (short-lived, `exp` enforced).
4. Frontend stores session cookie (`HttpOnly`, `Secure`, `SameSite=Strict`).
5. Role lookup on each protected API call.
6. Logout revokes session.

**Security Controls:**
- Enforce HTTPS & HSTS on all domains.
- CSRF protection via SameSite cookies + anti-CSRF tokens on forms.
- JWT signature & expiry validation server-side.
- Least privilege on Supabase service key (use anon key for client calls, service key only in serverless functions).

---

## 5. API & Integration Workflows

### 5.1 Social Metrics Sync (Cron)
- Supabase Edge Function / Serverless Job executes every 24 h.
- Fetch metrics via OAuth or API Tokens (stored in Vault).
- Update `social_metrics` table.
- Rate-limit requests; exponential backoff on failures.
- Log errors to Sentry (no PII).

### 5.2 Stripe Subscription Webhooks
- Endpoint verifies `stripe-signature` header.
- Updates `subscriptions` table & user role.
- Send confirmation emails (SendGrid).

### 5.3 AI Design API
- POST `/api/ai/design` with project content & style prefs.
- Validate input schema (zod or Joi).
- Call OpenAI GPT-4 with temperature limits.
- Sanitize & validate OpenAI response JSON.

---

## 6. UI / UX Considerations

- **Web (Next.js + Tailwind + shadcn/ui):**
  • Responsive grid for projects.
  • Sidebar (ads for free users).
  • Drag-and-drop editor for paid users (react-dnd).
  • Input validation errors shown inline (no raw errors).
  • Accessibility (a11y) and keyboard navigation.

- **Mobile (Expo):**
  • List + detail screens.
  • Image/video uploads via native pickers.
  • Offline caching with `async-storage` for recent projects.

**Security / Privacy:**
- Sanitize all user input before display.
- CSP header: `default-src 'self'; script-src 'self' cdn.jsdelivr.net;` etc.
- SRI for third-party scripts (AdSense).
- No sensitive data in localStorage; rely on SecureStore or cookies.

---

## 7. Infrastructure & CI/CD

- **Code Repo**: GitHub (pnpm, lockfile).
- **CI**: GitHub Actions workflow
  • Lint, TypeScript checks, unit tests → on PR.
  • E2E tests (Playwright) → nightly.
  • SCA scan (e.g., Dependabot, GitHub security alerts).
- **Deploy**: Vercel for Web (preview & prod). Expo Application Services (EAS) for mobile.
- **Secrets**: Store in GitHub Secrets / Vercel Env / Expo Secrets. Use Vault or AWS Secrets Manager for critical tokens.

**Security Hardening:**
- Disable debug in production builds.
- Restrict CORS to `*.vercel.app`, custom domains, `localhost` for dev.
- Rate limit public API endpoints via Vercel Edge config.

---

## 8. Testing Strategy

1. **Unit Tests**: Jest for React components, API route handlers.
2. **Integration Tests**: Supabase Edge Functions (local emulator).
3. **E2E Tests**: Playwright/Cypress, covering login, project CRUD, payment flow.
4. **Security Tests**:
   - Static code analysis (ESLint, Snyk).
   - Dependency vulnerability scans.
   - OWASP ZAP scan on staging.

---

## 9. Monitoring & Logging

- **Sentry**: Capture exceptions, performance traces. Scrub PII.
- **Analytics**: Vercel analytics + custom events (project created, subscription).
- **Logs**: Structured JSON logs for serverless functions. Rotate & archive logs.

---

## 10. Deployment Roadmap & Milestones

| Phase          | Deliverables                         | Timeline  |
|----------------|--------------------------------------|-----------|
| Phase 1 (MVP)  | Auth, Project CRUD, Social Sync, Web | 4 weeks   |
| Phase 2        | Mobile App, Stripe Integration, Ads  | 3 weeks   |
| Phase 3        | Customization Editor, AI Assistant   | 4 weeks   |
| Phase 4        | Polish, Security Audit, Beta Launch  | 2 weeks   |

**Total Duration:** ~13 weeks

---

### Security Checklist by Component

- [x] HTTPS everywhere & HSTS.
- [x] Google OAuth configured securely (no insecure flows).
- [x] JWT + cookies: `HttpOnly`, `Secure`, `SameSite`.
- [x] Input validation (server & client).
- [x] Output encoding & CSP.
- [x] Rate limiting & abuse protection on APIs.
- [x] Secrets managed via ENV / Vault.
- [x] Dependency scanning & CI gating.
- [x] Error messages: no stack traces in prod.

---

**With this plan, we ensure a secure-by-design, scalable, and maintainable platform delivering core MVP features and a roadmap for advanced customization and AI assistance.**