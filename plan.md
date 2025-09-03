# Implementation Plan – Personal Project Directory

---

## Phase 1: Authentication & Profiles
- [ ] Enable Supabase Auth with Google, GitHub, Twitter OAuth.
- [ ] Store user profiles (name, email, avatar, bio, links).
- [ ] Allow profile editing in Settings.
- [ ] Implement sign-out flow.

---

## Phase 2: Project Management
- [ ] Create DB schema for projects (title, description, tags, media, visibility, version).
- [ ] Implement CRUD operations.
- [ ] Add versioning with "last updated" indicator.
- [ ] Display projects in Notion-style grid.

---

## Phase 3: Directory & Global Search
- [ ] Build public-facing directory page.
- [ ] Add **global search bar**:
  - Visitors → can search for users/projects (public only).
  - Logged-in users → same plus filters (tags, owner).
- [ ] Results show user profiles and projects with visibility = public.

---

## Phase 4: Social Integrations
- [ ] LinkedIn OAuth & metrics (likes/comments).
- [ ] GitHub OAuth & repo metrics (stars/forks/watchers).
- [ ] Twitter OAuth & tweet metrics (likes/retweets/replies).
- [ ] Limit: one integration per project.
- [ ] Background jobs: refresh metrics every 24h.

---

## Phase 5: Monetization
- [ ] Stripe checkout integration.
- [ ] Free Plan:
  - Default template.
  - Ads displayed.
- [ ] Premium/Pro Plan:
  - Ads removed.
  - Customization unlocked.
  - AI Design Assistant enabled.
  - Analytics dashboard available.
- [ ] **Cancellation Flow**:
  - Stripe webhook sets plan = free.
  - Ads re-enabled.
  - Customization/AI disabled.
  - Analytics hidden (data preserved).
  - Send downgrade confirmation email.

---

## Phase 6: Customization & AI
- [ ] Build Design Catalog (directory, project, block packs).
- [ ] Implement customization editor (paid users).
- [ ] Seed initial templates for out-of-box experience.
- [ ] Add **real-time AI Design Assistant** (chat-driven).
- [ ] Allow undo (history cleared on save).
- [ ] On downgrade: stored layouts preserved but default template shown.

---

## Phase 7: Analytics
- [ ] Track:
  - Profile views
  - Project impressions
  - Engagement trends
- [ ] Build analytics dashboard for Premium/Pro.
- [ ] Hide dashboard for downgraded users but keep data.

---

## Phase 8: Mobile App
- [ ] Build Expo/React Native client.
- [ ] Features:
  - OAuth login.
  - Project CRUD.
  - Apply templates (no customization editor).
  - View metrics.
- [ ] Simplified navigation vs web.

---

## Phase 9: Testing
- [ ] Unit tests: auth, CRUD, subscriptions, integrations.
- [ ] e2e tests: sign-in → add project → upgrade → customize → cancel → downgrade.
- [ ] Edge cases: OAuth fail, API rate limits, AI assistant fallback, payment errors.

---

## Phase 10: Deployment
- [ ] Deploy web app on Vercel.
- [ ] Deploy mobile app via Expo EAS.
- [ ] Monitor via Sentry.
- [ ] CI/CD via GitHub Actions (lint, tests, e2e, deploy).
