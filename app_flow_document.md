# App Flow Document for Personal Project Directory

# App Flow for Personal Project Directory

---

## 1. Authentication
- Users can create an account using **Google, GitHub, or Twitter OAuth**.  
- On first login, profile (name, email, avatar, bio, links) stored in Supabase.  
- Profile can be edited anytime (bio, avatar, links).

---

## 2. Main Page (Visitor Access)
- Visitors (not logged in) can:
  - **Search users/projects** from main page.  
  - View only **public projects**.  
  - Access login/signup to create account.

---

## 3. Dashboard
- After login, users land on dashboard.  
- Key entry points:
  - **Project Directory** → browse/search projects.  
  - **Projects** → create/edit projects.  
  - **Settings** → profile + subscription management.  
  - **Subscription** → view plan, upgrade, cancel.  
  - **Social Integrations** → link LinkedIn, GitHub, Twitter.  
  - **Notifications** → system/email updates.  
  - **Mobile Access** → simplified mobile sync.

---

## 4. Project Management
- Users can create/edit projects:  
  - Title, description, tags, cover image(s), links.  
  - **Visibility** (public/private).  
- Projects are versioned → “Last updated” shown.  

---

## 5. Directory
- Public-facing gallery view.  
- Search/filter by user, tags, keywords.  
- Clicking a user shows their profile and projects (respecting visibility).  

---

## 6. Monetization
- **Free Tier**:
  - Default project directory design.  
  - Ads displayed.  
  - Limited edits only.  
- **Premium/Pro Tier**:
  - Ads removed.  
  - Full customization (directory + project pages).  
  - **AI Design Assistant (real-time)** for layouts/text.  
  - Access to **Design Catalog** (templates/block packs).  
  - **Analytics dashboard**:  
    - Profile views  
    - Project impressions  
    - Engagement trends  

---

## 7. Subscription Cancellation
- User can cancel Premium/Pro → reverts to Free.  
- Effects:
  - Ads reappear.  
  - Customization locked → projects shown in default template.  
  - AI Assistant disabled.  
  - Analytics dashboard hidden.  
  - User keeps all projects (data preserved).  

---

## 8. Social Integrations
- LinkedIn: fetch likes/comments.  
- GitHub: fetch stars/forks/watchers.  
- Twitter: fetch likes/retweets/replies.  
- One linked post/repo/tweet per project.  

---

## 9. Mobile App
- Simplified version:
  - OAuth login  
  - Create/view projects  
  - Apply templates (no full customization)  
  - View metrics  

---

## 10. Notifications
- Welcome + updates sent via SendGrid.  
- Subscription status changes (upgrade/cancel) trigger emails.  


## Onboarding and Sign-In/Sign-Up
When a new user visits the Personal Project Directory website or opens the mobile app, they land on a clean, minimal landing page that showcases sample project galleries and invites them to sign in. Prominent buttons labeled “Sign in with Google”, “Sign in with GitHub”, and “Sign in with Twitter” initiate their respective OAuth flows. Users are redirected to each provider’s secure consent screen where they approve permissions. Once they complete authentication, they are returned to the app and automatically see their own dashboard.

There is no manual password or email signup. If a user ever loses access, they recover their account by managing access through the provider’s account settings. To sign out, the user clicks the profile icon and selects “Sign Out,” which ends the session and returns them to the landing page. On mobile, the same steps occur inside the React Native/Expo app, with the device’s native OAuth login screen appearing for authentication.

## Main Dashboard or Home Page
After signing in, the user sees their main dashboard. At the top is a header with the app logo on the left, a global search bar in the center for finding other users/projects, and a profile menu on the right. Along the left side is a collapsible navigation panel that includes links to Projects, Social Integrations, Settings, and Upgrade.

The central area displays the user’s project gallery in a Notion-inspired grid layout. Free users see default styling with placeholder cards if no projects exist. Paid users see an additional toolbar above the gallery for customization actions. On both web and mobile, tapping on any project card navigates to the project edit or detail page. A floating “Add Project” button at the bottom right opens the project creation form.

Visitors (not logged in) can also use the global search bar on the main page to find users by name. Clicking a result navigates to the public profile at /username, where all published projects appear in gallery form with engagement metrics.

## Detailed Feature Flows and Page Transitions
When the user clicks “Add Project”, they arrive at the project form page where they enter the title, description, tags, cover image, additional media, technologies, live link, repository link, and an optional highlight section. Upon saving, the app returns them to the dashboard and displays the new project card instantly. Editing follows the same flow, with pre-populated fields and immediate updates.

To connect social accounts, the user navigates to the Social Integrations page. They click each platform’s “Connect” button to go through LinkedIn, GitHub, or Twitter OAuth flows. After granting permissions, the app stores the API credentials in Supabase and begins fetching likes, comments, stars, forks, retweets, and replies. These metrics appear as small icons on each project card and refresh automatically every 24 hours. Errors display as banners with retry options.

Upgrading, Customization, and AI Assistance

Upgrading to a paid plan happens when the user selects “Upgrade” in the sidebar. They view pricing on a Stripe-powered checkout page. After payment, the app removes ads and unlocks customization, analytics, and the AI Design Assistant.

Customization Editor: Paid users can drag and drop blocks, adjust grid settings, swap fonts/colors, and edit text inline. A live preview shows changes in real time.

Design Catalog: Accessible inside the editor, where users browse and preview templates before applying them to their directory or individual project pages.

AI Design Assistant: Invoked from the customization toolbar. A real-time chat modal collects preferences (style, tone, layout). The assistant generates layout JSON, rewritten text suggestions, and styling tweaks, which users can apply instantly. pages.

Analytics Dashboard (Premium/Pro)

Premium and Pro users access an Analytics tab in the sidebar.
Metrics shown:

Profile Views (7d, 30d)

Project Impressions (per project, over time)

Engagement Trends (likes, stars, retweets growth)

Pro users can also export analytics and compare multiple projects side by side.

Visitors and Discovery

Anyone can use the global search bar on the landing page or dashboard to find users/projects. Results display matching usernames and lead to public profiles. Visitors can view public project galleries with metrics and open detailed project pages. Portfolio owners can share a QR code from their dashboard, which opens their profile in web or mobile.

## Settings and Account Management
In the Settings section, users manage their personal information such as display name, profile picture, and bio. They toggle notification preferences for project comments, social metric updates, and subscription reminders. Billing details and subscription status appear in a dedicated Billing tab, where users can view invoices, change plans, or cancel their subscription. Automated emails for welcome messages, subscription renewals, and other notifications are sent via Supabase’s email service or an integrated provider like SendGrid. From any settings page, users click “Back to Dashboard” to return to the main view.

The Settings section allows users to manage:

Profile info (name, avatar, bio, links)

Notification preferences (social metrics, subscription reminders)

Billing details and subscription status (via a Billing tab)

Subscription Cancellation

When a Premium/Pro user cancels in Billing:

Account downgrades to Free Plan.

Ads reappear.

Customization editor + AI Assistant disabled.

Analytics tab hidden.

Projects remain intact but displayed using default design.

A confirmation email is sent after cancellation.

## Error States and Alternate Paths
If Google OAuth fails or the user cancels consent, an error message appears on the landing page prompting them to retry. During project creation or editing, if required fields are missing or file uploads fail, inline validation highlights the issue with a brief message and a retry option. When social API calls hit rate limits or fail, the app shows a banner on the Social Integrations page explaining the error and offering a retry button. In the customization editor, if the AI assistant cannot generate recommendations, a fallback message suggests manual edits. Payment errors on Stripe display the error reason and invite the user to update their card. Network connectivity issues trigger a full-screen offline notice with a retry button. In all cases, once the issue is resolved, users return seamlessly to their previous page without losing unsaved work.

OAuth Failure → error on landing page with retry option.

Project Form Errors → inline validation + retry.

Social API Rate Limits → banner message with retry.

AI Assistant Failure → fallback suggests manual edits.

Stripe Payment Errors → show reason + option to update card.

Network Outages → full-screen offline message with retry.
All errors resolve smoothly without losing unsaved work.

Mobile Experience

The mobile app (React Native/Expo) offers a simplified experience:

OAuth login (Google, GitHub, Twitter).

Create/view projects.

Apply templates (no full customization editor).

View engagement metrics.

Advanced customization, AI assistant, and analytics remain web-only.

## Conclusion and Overall App Journey
The typical user journey begins with a quick Google sign-in and leads to an empty gallery that evolves into a personalized portfolio as they add projects. Connecting social accounts enriches their pages with real metrics, while free and paid tiers let them choose between simplicity and full customization. The AI Design Assistant and Design Catalog guide even non-designers toward polished results. Visitors discover directories via search or QR scan and engage with project galleries effortlessly. All of this happens across web and mobile, backed by Supabase, Next.js, React Native, and Stripe, ensuring a smooth end-to-end experience from sign-up to sharing a professional project showcase.
