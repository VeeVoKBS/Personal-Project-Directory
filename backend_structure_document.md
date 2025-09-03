# Backend Structure Document

# Backend Structure – Personal Project Directory

---

## 1. Authentication
- **OAuth Providers**: Google, GitHub, Twitter.  
- Supabase Auth handles secure login and session tokens.  
- User profiles stored in `users` table with fields:
  - `id` (UUID, PK)
  - `name`
  - `email`
  - `avatar_url`
  - `bio`
  - `links` (JSONB: github, linkedin, website, etc.)
  - `created_at`

---

## 2. Projects
- Table: `projects`
- Fields:
  - `id` (UUID, PK)
  - `owner_id` (UUID, FK → users.id)
  - `title`
  - `description`
  - `tags` (Text[])
  - `image_url`
  - `link`
  - `visibility` (Text: `public` or `private`)
  - `version` (Int, increments on updates)
  - `created_at`
  - `updated_at`
- Features:
  - Public projects visible in global search and directories.
  - Private projects visible only to owner.

---

## 3. Engagements
- Table: `engagements`
- Fields:
  - `id`
  - `project_id` (FK → projects.id)
  - `platform` (linkedin/github/twitter)
  - `likes`
  - `comments`
  - `stars`
  - `retweets`
  - `last_synced`
- Engagements refreshed daily via API jobs.

---

## 4. Subscriptions
- Table: `subscriptions`
- Fields:
  - `id`
  - `user_id` (FK → users.id)
  - `plan` (free, premium, pro)
  - `status` (active, canceled, past_due)
  - `started_at`
  - `ends_at`
- Integration: Stripe Webhooks.
  - On cancel → downgrade user to **free plan**.
  - Effects:
    - Ads re-enabled.
    - Customization & AI disabled.
    - Analytics dashboard hidden.
    - Projects kept but shown in default design.

---

## 5. Design & Customization
- Table: `design_templates`
- Fields: `id`, `name`, `level`, `preview_url`, `tags`, `config_json`, `is_premium`
- Tables: `directory_pages`, `project_pages`
  - store chosen template & `config_json`
- Customization only available for paid users.  
- On cancellation → stored layouts preserved, but default template shown until re-upgrade.

---

## 6. Analytics
- Tables:
  - `profile_views` → logs profile page visits.
  - `project_impressions` → logs project card impressions.
  - `engagement_history` → historical engagement snapshots.
- Only visible to Premium/Pro users.
- Downgrade hides dashboard, but data is preserved.

---

## 7. Global Search
- Endpoint: `/api/search?query=...`
- Searches `users.name` + `projects.title/description`.  
- Returns only **public projects**.  
- Accessible by visitors and logged-in users.

---

## 8. Notifications
- Integration: SendGrid.  
- Emails:
  - Welcome
  - Subscription upgrade/downgrade/cancellation
  - Project/social updates

1. Backend Architecture

We’ve chosen a Backend-as-a-Service (BaaS) approach centered on Supabase, which provides built-in authentication, a managed PostgreSQL database, file storage, auto-generated REST endpoints, and edge functions for custom logic.

Supabase Core

Authentication: Google, GitHub, and Twitter OAuth via Supabase Auth.

Database: PostgreSQL with Row-Level Security (RLS) policies.

Storage: secure buckets for images, videos, QR codes, and AI-generated assets.

Auto-Generated REST APIs (PostgREST) for basic CRUD operations.

Edge Functions

Custom serverless functions (hosted on Supabase) handle:

Social metrics aggregation (LinkedIn, GitHub, Twitter).

AI Design Assistant requests (OpenAI or other AI providers).

Stripe webhooks for subscription upgrades, downgrades, and cancellations.

Notification emails (SendGrid integration).

Why this works:

Scalability: serverless edge functions and a managed Postgres cluster grow automatically.

Maintainability: BaaS reduces overhead on patches, upgrades, and server management.

Performance: PostgREST endpoints are fast; edge functions run close to the user; CDN caching via Vercel accelerates global delivery.

2. Database Management

Core Tables: users, projects, subscriptions, engagements, design_templates, directory_pages, project_pages, ai_sessions, notifications.

Analytics Tables: profile_views, project_impressions, engagement_history.

Storage Buckets: media (images, videos), QR codes, AI assets.

Data Access

Supabase REST endpoints for CRUD.

RLS ensures users can only update their own data.

Edge functions for advanced queries (e.g., metrics aggregation).

Practices

Daily backups configured in Supabase.

Indexes on key columns (user_id, project_id) for fast lookups.

24-hour refresh jobs for social metrics with in-function caching.

3. Database Schema (High-Level)

users: id, name, email, avatar_url, bio, links (JSONB), created_at.

projects: id, owner_id, title, description, tags, media, link, visibility (public/private), version (int), created_at, updated_at.

engagements: platform metrics (likes, stars, retweets, comments, forks).

subscriptions: plan (free, premium, pro), status (active, canceled, past_due), stripe_id, started_at, ends_at.

design_templates: reusable layout JSON for directory/project customization.

directory_pages / project_pages: applied templates + saved configs.

ai_sessions: inputs/outputs of AI design requests.

notifications: logs of system and subscription emails.

profile_views: logs each profile visit.

project_impressions: logs when project cards appear.

engagement_history: stores daily engagement metrics snapshots.

4. API Design and Endpoints
Supabase REST (PostgREST)

GET /users, GET /projects, POST /projects, PATCH /projects, DELETE /projects

Queries support filters (e.g., visibility=public).

Custom Edge Functions

POST /functions/v1/getSocialMetrics – fetches LinkedIn/GitHub/Twitter metrics.

POST /functions/v1/aiDesignAssistant – processes customization + AI design requests (real-time).

POST /functions/v1/webhooks/stripe – handles upgrades, cancellations, downgrades.

POST /functions/v1/sendNotification – sends subscription and activity emails.

GET /functions/v1/search – global search endpoint, returns only public projects for visitors.

5. Hosting Solutions

Supabase: managed Postgres, Auth, Storage, Functions.

Vercel: hosts frontend and SSR/edge routes; CDN for static assets.

Expo EAS: for mobile app builds and distribution.

6. Infrastructure Components

Load Balancers & API Gateway: managed by Supabase/Vercel.

Caching:

Edge caching on Vercel for static/SSR pages.

Engagement metrics cached in edge functions (24h TTL).

CDN: Vercel + Supabase Storage CDN.

Background Jobs: cron jobs for social metric refresh + analytics sync.

7. Security Measures

Authentication & Authorization: OAuth via Supabase; JWT tokens for sessions.

Row-Level Security: ensures project/user isolation.

Data Encryption: TLS in transit, AES-256 at rest.

Compliance: GDPR-friendly data handling; PCI DSS via Stripe.

Rate Limits: exponential backoff for external APIs.

8. Monitoring and Maintenance

Error Tracking: Sentry across frontend + backend.

Metrics: Supabase dashboards for queries, Vercel analytics for function invocations.

CI/CD: GitHub Actions (lint, test, e2e, deploy).

Backups: daily automated Postgres backups.

Maintenance: quarterly security reviews, automated dependency updates.

9. Subscription Cancellation Flow

Trigger: user cancels Premium/Pro in Stripe billing.

Stripe webhook updates subscriptions.plan → free.

Immediate effects:

Ads re-enabled.

Customization editor + AI Assistant disabled.

Analytics tab hidden (data preserved).

Projects remain published but displayed with default template.

Send cancellation confirmation email.

10. Conclusion

The backend is a serverless, scalable BaaS stack:

Multi-OAuth authentication (Google, GitHub, Twitter).

Rich Postgres schema for projects, social metrics, AI sessions, subscriptions, analytics.

Edge functions for external APIs (LinkedIn, GitHub, Twitter), AI assistant, Stripe, and notifications.

Strong security (RLS, encryption), automated scaling, and daily backups.

Subscription lifecycle (upgrade → premium features; cancel → revert to free) handled seamlessly.

1) RPC helpers (SQL)

All functions are SECURITY INVOKER (default) so they respect RLS. Safe to deploy.

-- =========================================================
-- RPC: Full-text-ish search over public projects (+ own)
-- Usage: select * from public.search_projects('portfolio');
-- Respects RLS: anon sees only public; owners see their private too (per policies).
-- =========================================================
create or replace function public.search_projects(q text)
returns table (
  project_id uuid,
  title text,
  description text,
  tags text[],
  created_at timestamptz,
  owner_id uuid,
  owner_name text,
  owner_avatar text
)
language sql
stable
as $$
  select
    p.id as project_id,
    p.title,
    p.description,
    p.tags,
    p.created_at,
    u.id as owner_id,
    u.name as owner_name,
    u.avatar_url as owner_avatar
  from public.projects p
  join public.users u on u.id = p.owner_id
  where
    (q is null or
      p.title ilike '%'||q||'%' or
      coalesce(p.description, '') ilike '%'||q||'%' or
      (p.tags is not null and exists (
        select 1 from unnest(p.tags) t where t ilike '%'||q||'%'
      ))
    )
  order by p.created_at desc;
$$;

-- =========================================================
-- RPC: Safe upsert for subscriptions (Stripe webhooks)
-- Sets plan/status atomically for a user.
-- =========================================================
create or replace function public.upsert_subscription(
  p_user_id uuid,
  p_plan plan_enum,
  p_status sub_status_enum,
  p_stripe_customer_id text default null,
  p_stripe_subscription_id text default null,
  p_ends_at timestamptz default null
) returns public.subscriptions
language plpgsql
security definer  -- webhooks run with service role; needs to bypass RLS on subscriptions
as $fn$
declare
  rec public.subscriptions;
begin
  insert into public.subscriptions as s(
    user_id, plan, status, stripe_customer_id, stripe_subscription_id, ends_at
  )
  values (p_user_id, p_plan, p_status, p_stripe_customer_id, p_stripe_subscription_id, p_ends_at)
  on conflict (user_id) do update
  set plan = excluded.plan,
      status = excluded.status,
      stripe_customer_id = coalesce(excluded.stripe_customer_id, s.stripe_customer_id),
      stripe_subscription_id = coalesce(excluded.stripe_subscription_id, s.stripe_subscription_id),
      ends_at = excluded.ends_at
  returning * into rec;

  return rec;
end;
$fn$;

-- Lock this down so only service role (your API) can execute:
revoke all on function public.upsert_subscription(uuid, plan_enum, sub_status_enum, text, text, timestamptz) from public;

-- =========================================================
-- RPC: Record analytics events (single insert helpers)
-- Service role should call these; keep them invoker (RLS allows insert via service role)
-- =========================================================
create or replace function public.log_profile_view(p_user_id uuid, p_session_id uuid)
returns void language sql as $$
  insert into public.profile_views(user_id, session_id) values (p_user_id, p_session_id);
$$;

create or replace function public.log_project_impression(p_project_id uuid, p_session_id uuid)
returns void language sql as $$
  insert into public.project_impressions(project_id, session_id) values (p_project_id, p_session_id);
$$;

Seed templates for the Design Catalog

These give you something nice to show immediately. Feel free to tweak the JSON later.

-- Minimal seed set: 2 directory, 2 project, 1 block pack
insert into public.design_templates (name, level, preview_url, tags, config_json, is_premium)
values
('Clean Grid', 'directory', '/previews/clean-grid.png', array['minimal','cold','gallery'],
 '{
   "type":"directory",
   "layout":"grid",
   "props":{"gap":"lg","columns":{"md":2,"lg":3},"cardStyle":"subtle"},
   "blocks":[{"type":"projectCard","props":{"showMetrics":true}}]
 }'::jsonb, false),

('Hero + Gallery', 'directory', '/previews/hero-gallery.png', array['hero','gallery','cold'],
 '{
   "type":"directory",
   "layout":"stack",
   "blocks":[
     {"type":"hero","props":{"title":"My Projects","subtitle":"Selected work"}},
     {"type":"gallery","props":{"gap":"md","columns":{"md":2,"lg":3}}}
   ]
 }'::jsonb, true),

('Case Study Minimal', 'project', '/previews/case-minimal.png', array['case','minimal','cold'],
 '{
   "type":"project",
   "layout":"stack",
   "blocks":[
     {"type":"title","props":{"align":"left"}},
     {"type":"media","props":{"variant":"wide"}},
     {"type":"richText","props":{"sections":["Overview","Challenges","Solution","Impact"]}},
     {"type":"metrics","props":{"showEngagement":true}}
   ]
 }'::jsonb, false),

('Showcase Split', 'project', '/previews/showcase-split.png', array['showcase','split','cold'],
 '{
   "type":"project",
   "layout":"split",
   "props":{"ratio":"2:1"},
   "blocks":[
     {"type":"media","props":{"variant":"tall"}},
     {"type":"sidebar","props":{"blocks":[
       {"type":"summary"},
       {"type":"links"},
       {"type":"techTags"}
     ]}}
   ]
 }'::jsonb, true),

('Stats + Testimonials Pack', 'block_pack', '/previews/blockpack-stats.png', array['pack','stats','social-proof'],
 '{
   "type":"blockPack",
   "blocks":[
     {"type":"stats","props":{"items":["Stars","Forks","Likes"]}},
     {"type":"testimonials","props":{"style":"compact"}}
   ]
 }'::jsonb, true);

RLS policy tests (quick verification scripts)

These help you prove anonymous visitors see only public projects, and owners see their own private ones.
In the Supabase SQL editor you can simulate auth with request.jwt.claims.

-- Setup: create two users and two projects (one public, one private)
-- (In a real project, auth.users rows come from Supabase Auth; for testing, we fake IDs.)
select gen_random_uuid() as u1 \gset
select gen_random_uuid() as u2 \gset

insert into auth.users(id, email) values (:'u1', 'u1@example.com') on conflict do nothing;
insert into auth.users(id, email) values (:'u2', 'u2@example.com') on conflict do nothing;

insert into public.users(id, name, email) values (:'u1','Alice','u1@example.com') on conflict do nothing;
insert into public.users(id, name, email) values (:'u2','Bob','u2@example.com') on conflict do nothing;

-- Projects: one public by Alice, one private by Bob
select gen_random_uuid() as p_public \gset
select gen_random_uuid() as p_private \gset

insert into public.projects(id, owner_id, title, visibility) values (:'p_public',  :'u1', 'Alice Public',  'public');
insert into public.projects(id, owner_id, title, visibility) values (:'p_private', :'u2', 'Bob Private',   'private');

-- ===== Test A: Anonymous (no JWT) should only see public
reset all;  -- clear any jwt claims
select title, visibility from public.projects order by title;

-- EXPECT: "Alice Public" only.

-- ===== Test B: Auth as Alice should see her own (public or private) + any public
set local role postgres;  -- required to set claims in SQL editor
set local "request.jwt.claims" = json_build_object('sub', :'u1')::text;

select auth.uid();  -- should be u1
select title, visibility from public.projects order by title;

-- EXPECT: "Alice Public" (public) AND NOT "Bob Private" (belongs to Bob)

-- ===== Test C: Auth as Bob should see his private + any public
set local "request.jwt.claims" = json_build_object('sub', :'u2')::text;

select auth.uid();  -- should be u2
select title, visibility from public.projects order by title;

-- EXPECT: "Bob Private" and "Alice Public"

-- Cleanup (optional)
-- delete from public.projects where id in (:'p_public', :'p_private');
-- delete from public.users where id in (:'u1', :'u2');
-- delete from auth.users where id in (:'u1', :'u2');


This setup meets the project’s goals of rapid development, minimal maintenance, strong security, and a high-performance user experience.
