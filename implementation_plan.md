# Implementation Plan – Personal Project Directory

---

## Phase 1: Authentication & Profiles
- Set up Supabase Auth with Google, GitHub, Twitter OAuth.
- Store user profiles (name, email, avatar, bio, links).
- Enable profile editing in Settings.
- Add sign out functionality.

---

## Phase 2: Project Management
- DB schema for projects (title, desc, tags, media, visibility, version).
- CRUD operations with Supabase.
- Implement versioning (auto increment + last updated).
- Project gallery grid (Notion style).

---

## Phase 3: Directory & Global Search
- Public-facing directory page with project gallery.
- Implement **global search**:
  - Visitors → search public users/projects.
  - Logged-in users → search with filters.
- Return only public projects to visitors.

---

## Phase 4: Social Integrations
- LinkedIn OAuth + metrics fetch (likes/comments).
- GitHub OAuth + repo metrics (stars/forks/watchers).
- Twitter OAuth + tweet metrics (likes/retweets/replies).
- Limit: one integration per project.
- Background job to sync metrics daily.

---

## Phase 5: Monetization
- Integrate Stripe checkout for subscription plans.
- Free Tier:
  - Ads displayed.
  - Default template only.
- Premium/Pro:
  - Ads removed.
  - Customization unlocked.
  - AI assistant enabled.
  - Analytics dashboard accessible.
- **Cancellation Flow**:
  - On cancel, Stripe webhook updates plan → `free`.
  - Ads re-enabled.
  - Customization/AI disabled.
  - Analytics hidden (data preserved).
  - Send downgrade email.

---

## Phase 6: Customization & AI
- Build Design Catalog API (templates, block packs).
- Customization editor for paid users.
- Real-time AI Design Assistant for layout + text suggestions.
- Undo available while editing; history cleared on save.
- On downgrade → layouts preserved, but locked until upgrade.

---

## Phase 7: Analytics
- Track:
  - Profile views
  - Project impressions
  - Engagement history
- Build analytics dashboard (charts: line, bar).
- Restrict visibility to Premium/Pro users.
- Hide dashboard for downgraded users, preserve data.

---

## Phase 8: Mobile App
- React Native/Expo client.
- Features:
  - OAuth login.
  - Create/view projects.
  - Apply templates (no full customization editor).
  - View engagement metrics.
- Simplified navigation vs. web.

---

## Phase 9: Testing
- Unit tests: auth, CRUD, social API sync, subscription logic.
- e2e tests: sign-in → create project → upgrade → customize → cancel → downgrade.
- Edge case tests: OAuth fail, API rate limit, AI error, payment failure.

---

## Phase 10: Deployment
- Deploy web app via Vercel.
- Deploy mobile via Expo EAS (TestFlight, Play Store).
- Configure monitoring (Sentry).
- Set up CI/CD with GitHub Actions:
  - Lint, test, e2e, deploy.
