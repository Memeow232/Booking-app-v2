# GitHub Workflow for Booking Webapp

This guide provides instructions for setting up GitHub Actions workflows to automate testing and deployment of your booking webapp.

## Prerequisites

- GitHub repository with your booking webapp code
- Netlify account connected to your GitHub repository
- Firebase project

## Setting Up GitHub Actions

### Step 1: Create Workflow Directory

First, create a directory for your GitHub Actions workflows:

```bash
mkdir -p .github/workflows
```

### Step 2: Create CI Workflow

Create a file named `.github/workflows/ci.yml` with the following content:

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '20'
        cache: 'pnpm'
    
    - name: Install pnpm
      run: npm install -g pnpm
    
    - name: Install dependencies
      run: pnpm install
    
    - name: Lint
      run: pnpm lint
    
    - name: Build
      run: pnpm build
      env:
        NEXT_PUBLIC_FIREBASE_API_KEY: ${{ secrets.NEXT_PUBLIC_FIREBASE_API_KEY }}
        NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN: ${{ secrets.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN }}
        NEXT_PUBLIC_FIREBASE_PROJECT_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_PROJECT_ID }}
        NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET: ${{ secrets.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET }}
        NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID }}
        NEXT_PUBLIC_FIREBASE_APP_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_APP_ID }}
        NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID }}
```

### Step 3: Create Deployment Workflow (Optional)

If you want to automate deployment to Netlify, create a file named `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Netlify

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '20'
        cache: 'pnpm'
    
    - name: Install pnpm
      run: npm install -g pnpm
    
    - name: Install dependencies
      run: pnpm install
    
    - name: Build
      run: pnpm build
      env:
        NEXT_PUBLIC_FIREBASE_API_KEY: ${{ secrets.NEXT_PUBLIC_FIREBASE_API_KEY }}
        NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN: ${{ secrets.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN }}
        NEXT_PUBLIC_FIREBASE_PROJECT_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_PROJECT_ID }}
        NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET: ${{ secrets.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET }}
        NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID }}
        NEXT_PUBLIC_FIREBASE_APP_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_APP_ID }}
        NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID: ${{ secrets.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID }}
    
    - name: Deploy to Netlify
      uses: netlify/actions/cli@master
      with:
        args: deploy --prod
      env:
        NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
        NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
```

## Setting Up GitHub Secrets

To use these workflows, you need to add your Firebase and Netlify credentials as GitHub secrets:

1. Go to your GitHub repository
2. Navigate to Settings > Secrets and variables > Actions
3. Click "New repository secret"
4. Add the following secrets:
   - NEXT_PUBLIC_FIREBASE_API_KEY
   - NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN
   - NEXT_PUBLIC_FIREBASE_PROJECT_ID
   - NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET
   - NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID
   - NEXT_PUBLIC_FIREBASE_APP_ID
   - NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID
   - NETLIFY_SITE_ID (if using automated deployment)
   - NETLIFY_AUTH_TOKEN (if using automated deployment)

## Getting Netlify Credentials

If you're using the automated deployment workflow:

1. Get your Netlify Site ID:
   - Go to your Netlify site dashboard
   - Navigate to Site settings > General > Site details
   - Copy the API ID

2. Get your Netlify Auth Token:
   - Go to your Netlify user settings (click your avatar)
   - Navigate to Applications
   - Under Personal access tokens, create a new token or use an existing one

## Benefits of GitHub Workflows

- Automated testing ensures your code works before deployment
- Continuous integration catches issues early
- Automated deployment reduces manual steps
- Consistent build environment for all deployments
- Easy to replicate for multiple client repositories

## Best Practices

- Keep sensitive information in GitHub secrets, never in code
- Test workflows on a development branch before using on main
- Use branch protection rules to require passing CI before merging
- Consider adding automated tests to your CI workflow
- Document any custom workflow requirements for your team
