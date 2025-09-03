# Tech Stack Document for Personal Project Directory

This document explains, in everyday language, the technology choices behind the Personal Project Directory platform. It shows how each tool and service fits together to deliver a smooth, reliable, and engaging experience for both web and mobile users.

## 1. Frontend Technologies

We chose these tools to build the user‐facing parts of the app—the screens you interact with directly.

- **Next.js (React Framework)**
  - Provides fast page loads through server-side rendering (SSR) and static site generation (SSG).
  - Improves search engine visibility and performance by pre-rendering pages.
- **React**
  - The core library for building interactive user interfaces with reusable components.
- **Tailwind CSS v4**
  - A utility-first styling tool that lets us write CSS quickly and consistently.
  - Keeps the design minimal and cold-toned, matching our Notion-inspired gallery look.
- **shadcn/ui**
  - A set of prebuilt, accessible React components that speed up UI development.
  - Ensures consistent styling and behavior across buttons, cards, modals, etc.
- **React Native & Expo (Mobile)**
  - Shares much of the web codebase so we can maintain one platform for both iOS and Android.
  - Expo simplifies building and deploying the mobile app without deep native setup.

These frontend choices work together to give users a smooth, responsive interface, whether they’re on desktop or mobile.

## 2. Backend Technologies

Behind the scenes, these services store data, handle authentication, and power our APIs.

- **Supabase (Auth, Database, Storage)**
  - **Auth**: Manages user sign-up and login exclusively via Google OAuth. No passwords to remember.
  - **Database**: A Postgres database that holds user profiles, project details, tags, and social metrics.
  - **Storage**: Securely stores cover images, additional images, and optional video files.
- **Google OAuth**
  - Lets users sign in with their Google accounts in one click.
- **Design Catalog API**
  - Hosts prebuilt project templates and block packs that premium users can browse and apply.
- **OpenAI GPT-4**
  - Powers the AI design assistant that recommends layouts, rewrites text, and suggests styling based on user content and preferences.

These backend technologies work in harmony to simplify data management, scale seamlessly, and power intelligent features like the AI assistant.

## 3. Infrastructure and Deployment

This section covers where our code lives, how it’s tested, and how it goes live.

- **GitHub & GitHub Actions**
  - Hosts our code with clear, descriptive commits managed through the pnpm package manager.
  - Runs automated unit and end-to-end tests on each push to ensure core user journeys work as expected.
- **Vercel**
  - Automatically deploys our Next.js website with a global content delivery network (CDN).
  - Provides instant rollbacks and preview URLs for every pull request.
- **Sentry**
  - Monitors errors and tracks performance issues in both web and mobile apps.
  - Alerts the team in real time so we can fix problems quickly.
- **Expo (EAS) for Mobile Builds**
  - Handles building and publishing iOS and Android apps with minimal configuration.

These infrastructure choices guarantee reliable deployments, quick feedback loops, and easy scaling as our user base grows.

## 4. Third-Party Integrations

To enrich functionality without rebuilding everything from scratch, we integrate with established services:

- **Social Platform APIs**
  - **LinkedIn**: Fetches likes and comments on project posts.
  - **GitHub**: Retrieves stars, forks, and watchers from linked repositories.
  - **Twitter (X)**: Shows likes, retweets, and replies on shared project tweets.
  - Metrics refresh every 24 hours to balance freshness and API rate limits.
- **Stripe**
  - Powers subscription payments on the web (no in-app purchases).
  - Manages monthly and annual premium tiers, unlocks features, and removes ads.
- **Google AdSense**
  - Serves non-intrusive ads to free-tier users in sidebars and between project cards.
- **SendGrid (or Similar Email Service)**
  - Sends welcome messages, password resets, and subscription renewal reminders.
  - Provides branded, reliable email delivery beyond basic Supabase emails.
- **QR Code Library**
  - Generates QR codes so users can share their portfolio links in person.

These integrations enhance the platform by providing social proof, monetization, and communication tools.

## 5. Security and Performance Considerations

We’ve built security and speed into every layer of the stack:

Security Measures:
- Google OAuth with Supabase Auth ensures secure, token-based sign-in without storing passwords.
- All sensitive keys and credentials live in environment variables, not in code.
- HTTPS everywhere: data in transit is always encrypted.
- Sentry helps us catch and fix security-related errors early.

Performance Optimizations:
- Next.js pre-renders pages and caches them at the edge for lightning-fast loading.
- Tailwind CSS is tree-shaken (dead code removed) so users only download the styles they need.
- Images served via Supabase Storage can use on-the-fly resizing or Vercel’s built-in image optimization.
- API calls for social metrics run on a 24-hour schedule, reducing load and avoiding rate-limit issues.

Together, these measures keep user data safe and ensure a snappy experience.

## 6. Conclusion and Overall Tech Stack Summary

Our chosen technologies strike a balance between simplicity, performance, and extensibility:

- Frontend: Next.js, React, Tailwind CSS v4, shadcn/ui, React Native/Expo  
- Backend: Supabase Auth, Database, Storage + Google OAuth + Design Catalog API + OpenAI GPT-4  
- Infrastructure: GitHub & GitHub Actions, Vercel, Sentry, Expo EAS  
- Integrations: LinkedIn/GitHub/Twitter APIs, Stripe, Google AdSense, SendGrid, QR codes  

This stack allows us to:
- Deliver a clean, minimal portfolio platform that loads quickly and scales smoothly.
- Offer powerful customization and AI-driven design assistance without burdening users with complex tools.
- Monetize through ads and subscriptions while maintaining a premium experience for paying customers.
- Support both web and mobile with a shared backend and consistent user interface.

With these choices, the Personal Project Directory is positioned to meet users’ needs today and adapt as those needs grow.