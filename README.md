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

## 4. Goals & Objectives
- Enable login via **Google, GitHub, and Twitter OAuth**.
- Allow users to **create/edit projects** with visibility (public/private) and versioning.
- Provide **global search** so visitors can discover users and their public projects.
- Integrate with LinkedIn, GitHub, and Twitter APIs to fetch project engagement metrics.
- Deliver a **minimal, cold-toned UI** inspired by Notion.
- Build monetization features:
  - **Free plan** → default template, ads, limited edits.
  - **Premium/Pro plans** → ads removed, customization editor, AI design assistant, analytics.
- Handle **subscription cancellation** gracefully:
  - Downgrade to Free automatically.
  - Ads re-enabled, customization/AI/analytics disabled.
  - Projects remain visible in default design.
- Release a **simplified mobile app** (CRUD, templates, metrics).
- Maintain strong **security and reliability** via Supabase (RLS), Stripe, and Sentry.

---

## 5. Target Users
- **Students/Graduates**: building professional portfolios.
- **Developers/Designers**: showcasing projects with metrics.
- **Recruiters/Employers**: browsing public profiles for talent.
- **Visitors/Guests**: discovering projects via search or shared links.

---

## 6. Deliverables
- Web application deployed on Vercel.
- Mobile app (Expo) with simplified features.
- Supabase backend with RLS and subscriptions.
- Design Catalog with seed templates.
- Global search API & UI.
- Stripe integration for payments & cancellations.
- Analytics dashboard for premium tiers.
- Full documentation set (setup, schema, API, testing, deployment, security).

---

## 7. Success Criteria
- Visitors can search and view public projects without logging in.
- Users can log in with Google/GitHub/Twitter and manage their profiles/projects.
- Projects support visibility (public/private) and versioning.
- Social metrics display on project cards.
- Ads show for free users, hidden for premium.
- Paid users can customize and use AI design assistant.
- Cancelled subscriptions revert to Free plan correctly.
- Analytics show profile views, impressions, and engagement trends.
- Mobile app supports CRUD and viewing projects with metrics.
- First monetization milestone achieved.
