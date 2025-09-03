# CI/CD Pipeline (GitHub Actions)

Our CI/CD automates linting, testing, and deployment.

---

## Workflow Overview
- Trigger: push or PR to `main` branch
- Jobs:
  1. **Install & Lint**
  2. **Run Unit Tests**
  3. **Run e2e Tests**
  4. **Build & Deploy Web**
  5. **Build Mobile App**

---

## Workflow File: `.github/workflows/ci.yml`
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  install:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
        with:
          version: 8
      - run: pnpm install
      - run: pnpm lint

  test:
    runs-on: ubuntu-latest
    needs: install
    steps:
      - uses: actions/checkout@v3
      - run: pnpm test

  e2e:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v3
      - run: pnpm e2e

  deploy-web:
    runs-on: ubuntu-latest
    needs: e2e
    steps:
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}

  build-mobile:
    runs-on: ubuntu-latest
    needs: e2e
    steps:
      - uses: actions/checkout@v3
      - uses: expo/expo-github-action@v8
        with:
          eas-access-token: ${{ secrets.EAS_TOKEN }}
      - run: eas build --platform all --non-interactive
