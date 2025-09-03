# Personal Project Directory – Project Requirements Document

## 1. Project Overview

The Personal Project Directory is a web and mobile platform where creators, students, job-seekers, and tech enthusiasts can build a polished portfolio under one roof. Users sign in with Google OAuth, upload and organize their projects (images, videos, links, tech stack) into a Notion-style gallery, and enrich each entry with real-time engagement metrics from LinkedIn, GitHub, and Twitter. Free users get a clean default design, while paid subscribers unlock full page customization, a catalog of templates, and an AI-powered design assistant to make their portfolios stand out without needing design expertise.

We’re building this to solve two core problems: first, the fragmented nature of online portfolios—where folks juggle multiple sites or poorly designed self-hosted pages—and second, the steep design learning curve for non-designers. Key success criteria include seamless Google sign-on, reliable social metric pull every 24 hours, a minimal default UI, an intuitive drag-and-drop editor for paid users, and a useful AI assistant that delivers JSON layout suggestions, text improvements, and styling rules. Monetization via ads for free users and Stripe-based subscriptions must generate the first revenue milestone within three months of launch.

## 2. In-Scope vs. Out-of-Scope

### In-Scope (MVP)

*   Google-only OAuth sign-up and login

*   User dashboard with Notion-inspired project gallery

*   Project CRUD: title, description, tags, cover image, extra images, optional videos, tech used, live links, GitHub repo, status, date created/updated

*   LinkedIn, GitHub, Twitter API integrations (likes, comments, stars, forks, watchers, retweets, replies), refreshed every 24 hours

*   Custom URL per user: `/username`

*   Global search bar for visitor to find users by name

*   Responsive web app (Next.js, Tailwind CSS, shadcn/ui) deployed on Vercel

*   Mobile app (React Native + Expo) mirroring web features, including QR-code profile sharing and scanning

*   Free-tier ads via Google AdSense (one sidebar + one every 4–5 project cards); no ads for paid users

*   Stripe-based web subscriptions (monthly/annual) unlocking:

    *   Ad removal
    *   Drag-and-drop editor for directory & project pages
    *   Design Catalog of templates & block packs
    *   AI Design Assistant powered by OpenAI GPT-4

*   Unit and end-to-end tests for core flows; observability via Sentry

### Out-of-Scope (Phase 1)

*   Email/password or other social OAuth providers
*   Custom domains (e.g., `janesprojects.com`)
*   Offline mobile support
*   Direct in-app purchases on iOS/Android
*   Admin dashboard for moderation or analytics within the app
*   Integrations with Behance, Dribbble or other platforms

## 3. User Flow

When a visitor lands on the homepage, they see a minimal, cold-toned gallery layout with a prominent “Sign in with Google” button. After clicking it, they complete the Google OAuth flow and arrive at their personal dashboard. The dashboard displays an empty gallery with a prompt to “Add Your First Project,” plus a top nav for Search, Settings, and Upgrade. Users click “Add Project,” fill out fields (title, description, images, tags, links, status), and save—instantly seeing their project card appear in the gallery.

In Settings, users connect LinkedIn, GitHub, and Twitter by supplying API credentials. Engagement metrics start syncing every 24 hours and display on project cards. Free users browse their default gallery; paid users click “Customize” to open the drag-and-drop editor—rearranging blocks, changing grid sizes, swapping fonts/colors. They can also open the AI Assistant, choose style preferences, and apply JSON layout suggestions with one click. Visitors searching `/username` or via global search land on public portfolios that show project galleries, social metrics, and, for paid users, a polished custom design. Mobile app users follow the same steps, with added QR-code scanning and sharing in a mobile-friendly interface.

## 4. Core Features

*   **Google OAuth Authentication**\
    Secure, passwordless sign-in/sign-up via Google only.
*   **Project CRUD & Gallery**\
    Users add, edit, delete projects with metadata: title, description, tags, images, videos, tech, links, status, dates.
*   **Custom User URLs & Search**\
    Each user gets `/username`; global search lets visitors find portfolios by name.
*   **Social Metrics Integration**\
    LinkedIn (likes/comments), GitHub (stars/forks/watchers), Twitter (likes/retweets/replies). Data refreshes every 24 h.
*   **Free vs. Paid Tiers & Ads**\
    Free: default gallery + AdSense ads (sidebar + per 4–5 cards). Paid: no ads + advanced features.
*   **Drag-and-Drop Editor**\
    Paid users rearrange layout blocks, adjust grid, edit inline text, swap fonts/colors, live preview.
*   **Design Catalog**\
    Pre-built templates and modular block packs for quick page styling.
*   **AI Design Assistant**\
    Powered by GPT-4; inputs: project content & style prefs; outputs: layout JSON, rewritten text, styling rules.
*   **Stripe Subscriptions**\
    Web-only monthly/annual plans; unlock premium features and remove ads.
*   **Mobile App (React Native/Expo)**\
    Full feature parity, QR code profile sharing and scanning.
*   **Testing & Monitoring**\
    Unit + e2e tests, GitHub Actions CI/CD, Sentry for errors and performance.

## 5. Tech Stack & Tools

*   Frontend Web: Next.js, React, Tailwind CSS v4, shadcn/ui
*   Backend & Auth: Supabase (Auth, Database, Storage)
*   Mobile: React Native + Expo
*   Hosting & CI/CD: Vercel, GitHub Actions
*   Payments: Stripe (web)
*   Ads: Google AdSense
*   AI Assistant: OpenAI GPT-4 (via REST API)
*   Emails: Supabase built-in for password resets; SendGrid for branded notifications
*   Monitoring: Sentry
*   IDE/Editor: Cursor (AI-powered coding suggestions)

## 6. Non-Functional Requirements

*   **Performance**:

    *   Page load time <1 second on 3G slow-3G (First Contentful Paint)
    *   API calls (metrics fetch) limited to once per 24 h

*   **Security & Compliance**:

    *   OAuth tokens stored securely (Supabase)
    *   GDPR-compliant data handling
    *   AdSense policy adherence

*   **Scalability**:

    *   Supabase must handle up to 10 K users and 50 K project cards in Phase 1

*   **Usability & Accessibility**:

    *   WCAG 2.1 AA compliance for color contrast and screen readers
    *   Responsive design across desktop, tablet, and mobile

*   **Reliability**:

    *   99.9% uptime SLA for core features (auth, project CRUD)
    *   Error rate <0.1%; alerts via Sentry

## 7. Constraints & Assumptions

*   **Constraints**:

    *   Only Google OAuth; no email/password or other social logins.
    *   Supabase as single backend for web & mobile.
    *   Stripe on web only; no in-app billing initially.
    *   Ads limited by AdSense policy and placement rules.

*   **Assumptions**:

    *   Users prefer Google sign-on and will not demand manual signup.
    *   API credentials for social platforms will be procured in time.
    *   Free-tier users tolerate non-intrusive ads without churn.
    *   GPT-4 latency and cost are acceptable for design assistant calls.

## 8. Known Issues & Potential Pitfalls

*   **API Rate Limits & Changes**\
    Social platform policies can shift; mitigate by caching metrics for 24 h and adding exponential backoff on failures.
*   **AI Output Quality**\
    GPT-4 may suggest layouts that break the minimal aesthetic; enforce guardrails in JSON schema and offer a “revert to default” option.
*   **AdSense Impact on UX**\
    Ads might slow page loads or annoy users; limit to two spots and lazy-load ad slots.
*   **Supabase Scaling**\
    Supabase quotas may throttle heavy read/write; monitor usage, plan for partitioning or read replicas.
*   **Mobile-Web Data Sync**\
    Conflicts can occur if users edit simultaneously on web and mobile; implement optimistic UI updates and simple conflict resolution (last-write wins).
*   **Stripe Web-Only Subscriptions**\
    Users may expect in-app purchases; clearly document web-only billing in mobile app onboarding to avoid confusion.

This document lays out every feature, flow, and constraint in everyday English. It serves as the definitive reference for all subsequent technical plans, ensuring the AI model and engineering team have zero ambiguity about what to build.
