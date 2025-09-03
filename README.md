Project Charter

Project Name: Personal Project Directory

1. Project Summary

The Personal Project Directory is a web-based and mobile platform where users can create an account with their Google Account, upload and showcase their projects, and connect with major platforms (LinkedIn, GitHub, Twitter) to display engagement metrics.

The platform provides a clean, minimal directory inspired by Notion’s gallery design. Free users get a default layout, while paid users unlock customization features for both their directory page and individual project pages. An AI-powered design assistant and design catalog further enhance customization, making it easier for users to create visually appealing and personalized project showcases.

The platform also includes monetization features such as ads, subscriptions, and premium tiers to ensure sustainability and provide advanced user features.

2. Project Context

In today’s job market, demonstrable projects are as valuable as resumes.

Students, job seekers, and professionals need a single, modern place to create an account with Google, showcase projects, and validate their impact through LinkedIn, GitHub, and Twitter.

Monetization ensures long-term sustainability while offering users enhanced customization and analytics.

Many users struggle with design skills — the AI assistant and design catalog help bridge this gap by recommending layouts and auto-customizing content.

3. Target Users

Students & Career Changers – Showcase projects to stand out in job applications.

Job Seekers – Build portfolios with LinkedIn/GitHub/Twitter validation and optionally upgrade for premium visibility.

Tech Enthusiasts – Share hobby projects in a clean design, with opportunities to customize layouts and design.

Employers/Recruiters – Browse candidates’ portfolios and engagement metrics in one place.

4. Goals & Objectives

Allow users to create an account with their Google Account (Google OAuth).

Build a portfolio platform for project showcasing.

Allow users to connect LinkedIn, GitHub, and Twitter for engagement tracking.

Deliver a minimal, cold-toned UI inspired by Notion’s gallery design.

Deploy to production using Vercel, with Sentry for monitoring and error tracking.

Release mobile app support for iOS and Android tied to the same backend.

Implement monetization features:

Free Plan – users can add projects using the default directory design and structure.

Paid Plans – users can:

Customize their project directory page and each individual project page.

Move blocks around, customize titles/descriptions, adjust layouts.

Access a Design Catalog of pre-built templates and block packs.

Use an AI-powered design assistant to:

Recommend templates based on project content and style preferences.

Auto-customize layouts, text, and styling.

Help users design when they don’t know how.

5. Scope (Initial Phase)

In-Scope:

Account creation using Google OAuth (sign-up & login).

Project upload and management features.

LinkedIn, GitHub, and Twitter API integrations for engagement tracking.

Responsive web design using Next.js, Tailwind CSS, and shadcn/ui.

Supabase backend for project, user, and social API data storage.

GitHub-based version control with pnpm and descriptive commits.

Testing (unit + e2e) for core user journeys.

Deployment on Vercel, with observability through Sentry.

Mobile app version (cross-platform with React Native/Expo).

Monetization features:

Ads integration (e.g., Google AdSense).

Subscription model (monthly/annual premium).

Premium tier features (customization, analytics, priority placement).

Customization & AI:

Default templates for free users.

Premium users can customize directory & project pages.

AI design assistant to recommend layouts, rewrite text, and help with styling.

Design Catalog API with pre-built templates and block packs.

Out-of-Scope (for now):

Integration with additional platforms (e.g., Behance, Dribbble).

Offline support for mobile app.

6. Deliverables

MVP Website & Mobile App: account creation via Google, project directory, social integrations, and monetization.

Customization Features:

Default design available for free users.

Paid users can fully customize directory pages and individual project pages.

AI design assistant integrated to guide users with layouts, text, and styling.

Design Catalog available with templates and block packs.

Documentation: setup instructions, database schema, API usage notes.

Testing Suite: unit tests for business logic and e2e coverage for customization flows.

Monetization Modules: ads, subscription, premium-tier logic with customization unlocks.

7. Risks & Assumptions

Risks:

API rate limits or permission changes across LinkedIn, GitHub, and Twitter.

Google OAuth and multiple third-party integrations may increase complexity.

Monetization (ads + subscriptions) requires compliance with external policies.

AI customization may generate undesired or inconsistent results without guardrails.

Assumptions:

Users prefer Google-based account creation over manual signup.

Paid users will see value in customization + AI assistance.

Supabase backend will scale across web, mobile, and customization features.

Ads will not significantly harm free user experience.

8. Success Criteria

Users can create an account with Google successfully.

Projects uploaded and displayed (default design for free users).

Paid users can customize their directory and individual project pages.

AI assistant provides useful recommendations (templates, layouts, text).

Design Catalog API allows users to preview and choose templates.

LinkedIn, GitHub, and Twitter metrics integrated.

Ads displayed for free-tier users, hidden for paid tiers.

Subscription and premium flows functional.

Positive user feedback on both default simplicity (free) and customization/AI support (paid).

First monetization revenue achieved from premium subscriptions.

✨ Features
- 🔐 **Google Account Sign-in** – secure login & account creation with Google OAuth.
- 📂 **Project Directory** – upload projects with images, tags, and links.
- 🔗 **Social Integrations** – pull likes, comments, stars, and retweets from:
  - LinkedIn
  - GitHub
  - Twitter (X)
- 📱 **Cross-Platform** – available on both web (Vercel) and mobile (React Native / Expo).
- 💰 **Monetization** – ads, subscriptions, and premium tiers for advanced analytics & visibility.
- ✅ **Best Practices** – unit + e2e tests, GitHub Actions CI/CD, error monitoring with Sentry.

---

## 🛠️ Tech Stack
**Frontend:** Next.js, React, Tailwind CSS v4, shadcn/ui  
**Backend:** Supabase (Auth, Database, Storage)  
**Infrastructure:** GitHub, Vercel, Sentry  
**Mobile:** React Native (Expo)  
**Payments:** Stripe  

