# Backend Structure Document

## 1. Backend Architecture

Overall, we’ve chosen a Backend-as-a-Service (BaaS) approach centered on Supabase. This setup gives us built-in authentication, a managed PostgreSQL database, file storage, auto-generated REST endpoints, and edge functions for custom logic.

• Supabase Core
  • Authentication: Google OAuth only, managed by Supabase Auth
  • Database: PostgreSQL, with Row-Level Security (RLS) policies
  • Storage: secure buckets for images, videos, QR codes
  • Auto-Generated REST APIs (PostgREST) for CRUD operations

• Edge Functions
  • Custom serverless functions (hosted on Supabase) handle:  
    – Social metrics aggregation (LinkedIn, GitHub, Twitter)  
    – AI Design Assistant requests (OpenAI GPT-4)  
    – Stripe webhooks and subscription management  
    – Notification emails (SendGrid integration)

Why this works:
- Scalability: Serverless edge functions and a managed Postgres cluster grow automatically.  
- Maintainability: BaaS means no database upgrades, security patches, or server maintenance.  
- Performance: PostgREST endpoints are fast; edge functions run close to the user; CDN caching via Vercel.

## 2. Database Management

We use PostgreSQL via Supabase. It’s a relational database that handles structured data and enforces relationships between tables.

• Data Types and Tables
  • SQL (PostgreSQL) for core data: users, projects, subscriptions, social metrics, templates
  • Storage buckets for images, videos, QR codes  

• Data Access
  • Auto-generated REST endpoints for basic create/read/update/delete operations  
  • Row-Level Security (RLS) ensures users can only modify their own data  
  • Edge functions for complex queries (e.g., combining social metrics from multiple APIs)  

• Data Management Practices
  • Daily backups of the Postgres database (configured in Supabase)  
  • Indexes on key columns (user_id, username, project_id) for fast lookups  
  • 24-hour refresh jobs for social metrics, with caching inside edge functions

## 3. Database Schema

### Human-Readable Overview

• **users**: stores user profiles, tiers (free/paid), OAuth identifiers  
• **projects**: each record holds project metadata (title, description, status, links)  
• **project_images** & **project_videos**: file references for media stored in buckets  
• **project_tags** & **project_technologies**: many-to-many mapping tables  
• **social_metrics**: daily snapshots of LinkedIn, GitHub, Twitter counts  
• **subscriptions**: Stripe subscription records and status  
• **templates** & **layouts**: design catalog for drag-and-drop blocks  
• **ai_sessions**: records of AI design requests and outputs  
• **notifications**: logs of emails sent (welcome, renewal, etc.)

### SQL Schema (PostgreSQL)
```sql
CREATE TABLE users (
  id            uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  email         text UNIQUE NOT NULL,
  username      text UNIQUE NOT NULL,
  avatar_url    text,
  google_id     text UNIQUE NOT NULL,
  tier          text NOT NULL CHECK (tier IN ('free', 'paid')),
  created_at    timestamptz DEFAULT now()
);

CREATE TABLE projects (
  id             uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id        uuid REFERENCES users(id) ON DELETE CASCADE,
  title          text NOT NULL,
  short_summary  text,
  description    text,
  status         text CHECK (status IN ('draft','published','archived')),
  live_link      text,
  github_link    text,
  created_at     timestamptz DEFAULT now(),
  updated_at     timestamptz DEFAULT now()
);

CREATE TABLE project_images (
  id         uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  project_id uuid REFERENCES projects(id) ON DELETE CASCADE,
  url        text NOT NULL,
  position   int DEFAULT 0
);

CREATE TABLE project_videos (
  id         uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  project_id uuid REFERENCES projects(id) ON DELETE CASCADE,
  url        text NOT NULL,
  position   int DEFAULT 0
);

CREATE TABLE project_tags (
  project_id uuid REFERENCES projects(id) ON DELETE CASCADE,
  tag        text,
  PRIMARY KEY (project_id, tag)
);

CREATE TABLE project_technologies (
  project_id    uuid REFERENCES projects(id) ON DELETE CASCADE,
  technology    text,
  PRIMARY KEY (project_id, technology)
);

CREATE TABLE social_metrics (
  id             uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  project_id     uuid REFERENCES projects(id) ON DELETE CASCADE,
  platform       text CHECK (platform IN ('linkedin','github','twitter')),
  likes          int DEFAULT 0,
  comments       int DEFAULT 0,
  stars          int DEFAULT 0,
  forks          int DEFAULT 0,
  watchers       int DEFAULT 0,
  retweets       int DEFAULT 0,
  replies        int DEFAULT 0,
  recorded_at    date DEFAULT current_date
);

CREATE TABLE subscriptions (
  id              uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id         uuid REFERENCES users(id) ON DELETE CASCADE,
  stripe_id       text UNIQUE NOT NULL,
  status          text NOT NULL,
  plan            text NOT NULL,
  current_period_end timestamptz,
  created_at      timestamptz DEFAULT now()
);

CREATE TABLE templates (
  id          uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  name        text NOT NULL,
  blocks_json jsonb NOT NULL,
  created_at  timestamptz DEFAULT now()
);

CREATE TABLE ai_sessions (
  id           uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id      uuid REFERENCES users(id) ON DELETE CASCADE,
  input_json   jsonb,
  output_json  jsonb,
  created_at   timestamptz DEFAULT now()
);

CREATE TABLE notifications (
  id           uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id      uuid REFERENCES users(id) ON DELETE CASCADE,
  type         text,
  sent_at      timestamptz DEFAULT now(),
  status       text
);
```

## 4. API Design and Endpoints

We rely on a mix of Supabase’s auto-generated REST endpoints and custom edge functions:

• Auto-generated (PostgREST) endpoints
  • GET /users  
  • GET /users?id=eq.{uuid} or username=eq.{username}  
  • GET /projects?user_id=eq.{uuid}&order=created_at.desc  
  • POST /projects  
  • PATCH /projects?id=eq.{project_id}  
  • DELETE /projects?id=eq.{project_id}

• Edge Functions (RESTful)
  • POST /functions/v1/getSocialMetrics  – fetches and caches metrics from LinkedIn/GitHub/Twitter  
  • POST /functions/v1/aiDesignAssistant  – sends project content & style prefs to OpenAI GPT-4  
  • POST /functions/v1/webhooks/stripe  – handles subscription creation, updates, cancellations  
  • POST /functions/v1/sendNotification  – triggers SendGrid emails (welcome, renewal)

• GraphQL (optional future expansion)
  • Could layer Hasura on top of the same Postgres schema for flexible queries

## 5. Hosting Solutions

• Supabase (AWS under the hood)
  • Managed Postgres cluster, Auth, Storage, Edge Functions  
  • Global availability and automated scaling

• Vercel
  • Hosts any frontend edge functions and Next.js API routes  
  • CDN for static assets

Benefits:
- Reliability: SLA-backed services, built-in failover  
- Scalability: zero-configuration auto-scaling for both compute and database  
- Cost-effectiveness: pay-as-you-go, minimal DevOps overhead

## 6. Infrastructure Components

• Load Balancers & API Gateway
  • Handled by Supabase and Vercel’s serverless platforms  

• Caching
  • Edge caching on Vercel for static assets and SSR pages  
  • In-function caching of social metrics (24-hour TTL)  

• Content Delivery Network (CDN)
  • Vercel’s global CDN for frontend assets  
  • Supabase Storage CDN (optional) for serving images/videos

• Storage Buckets
  • Supabase Storage: project media, QR codes, AI-generated assets

• Background Jobs
  • Scheduled edge function or Supabase cron (via third-party) to refresh social metrics daily

## 7. Security Measures

• Authentication & Authorization
  • Google OAuth via Supabase Auth — no passwords to manage  
  • JWT tokens for session management  
  • Row-Level Security (RLS) policies to isolate user data

• Data Encryption
  • TLS encryption in transit  
  • AES-256 encryption at rest in Supabase Storage and database backups

• Compliance & Best Practices
  • GDPR-friendly data handling (users can delete accounts and data)  
  • PCI DSS compliance via Stripe for payment processing  
  • Rate-limit calls to social APIs, implement exponential backoff

## 8. Monitoring and Maintenance

• Error Tracking & Performance
  • Sentry for frontend & backend error monitoring and performance tracing
  
• Logging & Metrics
  • Supabase dashboard for database query monitoring  
  • Vercel analytics for function invocation metrics and latency  

• CI/CD
  • GitHub Actions pipeline for linting, testing (unit + e2e), and deployment to Vercel/Supabase  

• Backup & Recovery
  • Daily automated backups of the Postgres database  
  • On-demand restores via Supabase console

• Maintenance Strategy
  • Quarterly security reviews  
  • Regular dependency updates via automated pull requests (Dependabot)  
  • Periodic performance audits and index optimizations

## 9. Conclusion and Overall Backend Summary

Our backend is a modern, serverless stack built on Supabase and Vercel. By leveraging BaaS services, we minimize infrastructure overhead, ensure high availability, and scale seamlessly as user demand grows. Key highlights:

• Google-only OAuth authentication and JWT-based sessions make sign-in frictionless and secure.  
• A robust PostgreSQL schema supports rich project metadata, social metrics, AI sessions, and subscription records.  
• Edge functions power integrations with social platforms, OpenAI GPT-4, Stripe, and email notifications.  
• Global CDN, caching, and auto-scaling deliver sub-second performance and 99.9% uptime.  
• Built-in security (RLS, encryption, GDPR support) and monitoring (Sentry, Supabase/Vercel dashboards) keep the system safe and reliable.

This setup meets the project’s goals of rapid development, minimal maintenance, strong security, and a high-performance user experience.