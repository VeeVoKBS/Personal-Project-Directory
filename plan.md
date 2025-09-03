Phase 1: Authentication & Accounts

 Set up Google OAuth

   Configure Supabase for Google authentication
  
   Implement account creation with Google (sign-up & login)
  
   Store user profile (name, email, Google ID) in Supabase DB

   Add logout flow

 Extend OAuth providers

   Add GitHub OAuth login/signup
  
   Add Twitter (X) OAuth login/signup

 Profile Editing

   Add profile fields: bio, avatar, personal links (GitHub, LinkedIn, portfolio, etc.)
  
   Allow editing during signup and later from a settings page

Phase 2: Project Upload & Directory

 Project CRUD Features

   Design DB schema for projects (title, description, tags, images, links, owner_id, visibility)
  
   Implement “Add New Project” form with validation
  
   Upload project images/files (store in Supabase Storage)
  
   Display projects in gallery layout (Notion-style cards)
  
   Add filtering/search by tags or keywords
  
   Allow users to set project visibility (public/private)

 Versioning

   Track updates to projects (title/description/content changes)
  
   Add “Last updated” indicator on project cards

Phase 3: Social Integrations

 LinkedIn Integration

   Connect LinkedIn API (OAuth flow for user)
  
   Store single post ID per project (no multiple posts)
  
   Fetch likes & comments for linked posts
  
   Display engagement metrics on project cards

 GitHub Integration

   Connect GitHub API for repository links (single repo per project)
  
   Pull repo stars, forks, watchers count
  
   Display GitHub metrics alongside project

 Twitter (X) Integration

   Connect Twitter API (OAuth)
  
   Allow linking a single tweet per project
  
   Track likes, retweets, and replies
  
   Display metrics under project card

Phase 4: Monetization

 Ads Integration

   Add Google AdSense banners to free-tier accounts
  
   Hide ads completely for premium users

 Subscription Model

   Set up Stripe (or Paddle) for payment handling
  
   Create subscription tiers (Free, Premium, Pro)
  
   Premium features:

    Directory & project page customization
    
    AI design assistant access
    
    Analytics dashboard
    
    Priority project placement

 Analytics Dashboard (Premium)

   Profile views tracking
  
   Project impressions tracking
  
   Engagement trends tracking

Phase 5: Design Catalog & Customization

 Design Catalog Setup

   Define JSON-based template DSL for layouts and blocks
  
   Seed initial catalog (6 directory templates, 6 project templates, 3 block packs)
  
   Create API endpoints for fetching catalog, previews, and applying templates

 Customization Editor

   Implement block editor (drag-drop for paid users, limited edits for free users)
  
   Allow title/description editing inline
  
   Save chosen templates and user edits in Supabase (directory_pages, project_pages)
  
   Allow undo while editing, but history is cleared once user clicks “Save Changes”

 AI Design Assistant

   Build API endpoint for AI recommendations (/design/recommend)
  
   Implement AI-driven template ranking (based on tags, content, preferences)
  
   Implement real-time AI chat assistant to help users customize layouts & rewrite text
  
   Add guardrails (tone, profanity filter, length limits)

Phase 6: Mobile App

 React Native / Expo Setup

   Reuse Supabase backend for mobile clients
  
   Replicate login & project CRUD features in mobile app
  
   Support template selection (simplified compared to web)
  
   View-only engagement metrics

 Testing

   Run e2e tests on mobile flows (login, project upload, template selection, view projects)

Phase 7: Infrastructure, Testing & Deployment

 Version Control & CI/CD

   Set up GitHub repo with pnpm
  
   Use descriptive commits (Conventional Commits)
  
   Configure GitHub Actions for CI (tests + linting)

 Testing

   Unit tests for DB logic and API functions
  
   e2e tests for sign-up → upload project → apply design → view directory flow

 Deployment

   Deploy web app to Vercel

 Set up monitoring and error tracking in Sentry

 Continuous deployment pipeline for mobile (Expo EAS, TestFlight, Play Store)
