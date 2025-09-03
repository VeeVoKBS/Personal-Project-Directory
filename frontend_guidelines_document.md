# Frontend Guideline Document

## 1. Frontend Architecture

# Frontend Guidelines – Personal Project Directory

---

## 1. Frameworks & Tools
- Next.js + React (Web)
- Tailwind CSS v4 + shadcn/ui (UI components)
- React Native + Expo (Mobile)
- ESLint 9 (linting)

---

## 2. Layout
- Minimal, clean, functional design.
- Cold-toned color palette.
- Notion-inspired project cards grid.
- Responsive across desktop, tablet, mobile.

---

## 3. Navigation
- **Header**:
  - App logo (left).
  - Global search bar (center).
  - Profile menu (right).
- **Sidebar (Web)**:
  - Projects
  - Social Integrations
  - Settings
  - Upgrade
  - Analytics (if Premium/Pro)
- **Mobile**:
  - Simplified navigation.
  - No full customization editor, no analytics tab.

---

## 4. Global Search
- Visible in header (web) and main landing page (visitors).  
- Visitors: can search users/projects, see only **public projects**.  
- Logged-in users: same, but results highlight connections to their own profile.  

---

## 5. Project Management
- Project form: title, description, tags, media, links, visibility (public/private).
- Card grid auto-updates after save.
- “Last updated” shown for versioned projects.

---

## 6. Customization & AI
- Free users: default styling only.
- Premium/Pro users:
  - Customization editor above gallery.
  - Access to Design Catalog templates.
  - AI Design Assistant (real-time chat modal).
- Cancelled users → customization toolbar hidden, default layout shown.

---

## 7. Analytics
- Premium/Pro:
  - Analytics tab in sidebar.
  - Charts for profile views, impressions, engagement trends.
- Downgraded users:
  - Analytics tab hidden.
  - Charts/data preserved for potential re-upgrade.

---

## 8. Notifications
- Inline banners for errors (API, upload, AI).  
- Toasts for success actions.  
- Settings → toggle email notifications.

### 1.1 Overview
We’re building a web and mobile platform using React-based frameworks. On the web, we use Next.js (latest version) with React, Tailwind CSS v4, and the shadcn/ui component library. For mobile, we leverage React Native via Expo. Supabase handles authentication, database, and storage. Vercel hosts the web app, GitHub manages source control, and Sentry tracks errors.

### 1.2 Scalability
- **Modular Structure**: Feature-based directories let us add new pages or components without affecting unrelated code.
- **Server-Side Rendering & Static Generation**: Next.js SSG/SSR reduces load on the client and scales under traffic spikes.
- **Edge Functions & Incremental Static Regeneration**: We can refresh engagement metrics and user pages every 24 hours without redeploying.

### 1.3 Maintainability
- **Component-First Approach**: Atomic components in shadcn/ui and Tailwind utility classes ensure consistency.
- **Clear Conventions**: File-based routing, naming conventions, and shared hooks keep the codebase easy to navigate.
- **Documentation & Comments**: Each module includes a README and JSDoc comments for public APIs.

### 1.4 Performance
- **Image Optimization**: Next.js Image component for lazy-loaded, responsive images.
- **Code Splitting & Dynamic Imports**: Load only what’s needed per page.
- **Caching & CDN**: Vercel’s edge network caches static assets globally.

## 2. Design Principles

### 2.1 Usability
- **Intuitive Layouts**: Clear navigation, prominent calls to action.
- **Consistent UI Patterns**: Buttons, forms, and cards follow the same style rules.

### 2.2 Accessibility
- **WCAG Compliance**: Semantic HTML, ARIA roles, keyboard navigability, and sufficient color contrast.
- **Accessible Forms**: Labels tied to inputs, focus outlines, error messages.

### 2.3 Responsiveness
- **Mobile-First**: Tailwind’s responsive utilities (`sm:`, `md:`, `lg:`) ensure layouts work on any screen.
- **Fluid Grids & Flexbox**: Components reshape naturally as the viewport changes.

## 3. Styling and Theming

### 3.1 Styling Approach
- **Tailwind CSS v4**: Utility-first CSS for rapid, consistent styling.
- **shadcn/ui**: Pre-built, accessible React components with Headless UI and Radix primitives.

### 3.2 Theming
- **CSS Variables**: Tailwind config extends `:root` variables enabling light/dark mode toggles.
- **Theme Switcher**: Persist user preference in local storage and sync across web & mobile.

### 3.3 Visual Style
- **Design Style**: Modern flat design with cold-toned palette inspired by Notion.
- **Glassmorphism** (optional accent areas): Semi-transparent cards with subtle blur for depth.

### 3.4 Color Palette
- Gray Scale: 
  - --gray-50: #f9fafb
  - --gray-100: #f3f4f6
  - --gray-200: #e5e7eb
  - --gray-500: #6b7280
  - --gray-800: #1f2937
- Accent Blue: 
  - --blue-500: #3b82f6
  - --blue-700: #1d4ed8
- Success Green: #10b981  
- Warning Amber: #f59e0b  
- Danger Red: #ef4444

### 3.5 Typography
- **Primary Font**: Inter (system font stack fallback).
- **Hierarchy**: 
  - H1: 2.25rem, font-semibold  
  - H2: 1.875rem, font-medium  
  - Body: 1rem, font-normal  
- **Line Height**: 1.5 for body text, 1.25 for headings.

## 4. Component Structure

### 4.1 Folder Organization
```
/src
  /components    # Reusable UI atoms/molecules
  /features      # Domain-specific components and hooks
  /pages         # Next.js page routes
  /styles        # Tailwind config and globals
  /utils         # Helpers, constants
  /hooks         # Custom React hooks
  /lib           # Supabase client, API wrappers
```

### 4.2 Reusability
- **Atomic Components**: Buttons, inputs, modals, cards, etc., live in `/components/ui`.
- **Feature Components**: Project galleries, profile editors, metric displays in `/features/project` or `/features/user`.

### 4.3 Benefits
- **Loose Coupling**: Changing one component doesn’t ripple through the app.
- **Easy Testing**: Small, focused components are straightforward to unit-test.

## 5. State Management

### 5.1 Tools & Patterns
- **React Context + useReducer**: Global UI state (theme, user session).
- **SWR (Stale-While-Revalidate)**: Data fetching from Supabase and social APIs with caching and revalidation.
- **Local State**: useState for ephemeral component state.

### 5.2 Data Flow
1. Request data via SWR hooks (`useUserProjects`, `useEngagementMetrics`).
2. Cache results and re-fetch every 24 hours or on focus.
3. Sync global auth state via Supabase’s onAuthStateChange.
4. UI state (modal open/close) managed in Context.

## 6. Routing and Navigation

### 6.1 Web Routing
- **Next.js File-Based Routing**: `/pages/index.js`, `/pages/[username]/index.js`, `/pages/dashboard.js`, etc.
- **Dynamic Routes**: `[username]` for public profiles, `[projectId]` for project detail.
- **Linking**: `next/link` for client-side transitions.

### 6.2 Mobile Navigation
- **React Navigation**: Stack and Tab navigators in Expo.
- **Deep Linking**: Support `myapp://username` to open mobile profile.

### 6.3 Navigation Structure
- Public: Home ➔ Browse ➔ Profile
- Authenticated: Dashboard ➔ Projects ➔ Settings ➔ Custom Editor

## 7. Performance Optimization

### 7.1 Lazy Loading & Code Splitting
- **Dynamic Imports**: import heavy components (e.g., drag-and-drop editor) only when needed.
- **Route-Based Splitting**: Next.js separates code per page automatically.

### 7.2 Asset Optimization
- **Image CDN**: Next/Image with Vercel’s default loader.
- **SVG Icons**: Inline or as React components for minimal overhead.

### 7.3 Caching & Throttling
- **SWR Cache**: In-memory and localStorage for offline/resume.
- **API Rate Handling**: Exponential backoff for social engagement endpoints.

## 8. Testing and Quality Assurance

### 8.1 Unit Testing
- **Jest + React Testing Library**: Test components, hooks, and utility functions.
- **Coverage Thresholds**: 80% overall, 90% in critical modules.

### 8.2 Integration Testing
- **Testing Library**: Combine components with mocked Supabase and SWR.

### 8.3 End-to-End Testing
- **Cypress**: Test signup, project creation, profile navigation, and payment flows.

### 8.4 Linting & Formatting
- **ESLint** (with `eslint-config-next`), **Prettier** for code consistency.
- **Tailwind Linting**: `tailwindcss-classnames` plugin to prevent unused classes.

## 9. Conclusion and Overall Frontend Summary
This frontend setup leverages Next.js and React Native for unified code practices across web and mobile. Tailwind CSS with shadcn/ui ensures a consistent, accessible, and responsive design. Data handling via SWR and React Context provides smooth UX with real-time feel and robust caching. Automated testing and performance optimizations guarantee reliability and speed. Together, these guidelines ensure our Personal Project Directory remains scalable, maintainable, and delightful for students, job seekers, and tech enthusiasts.
