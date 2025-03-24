# Netlify Deployment Guide for Booking Webapp

This guide provides detailed instructions for deploying your booking webapp to Netlify.

## Prerequisites

- GitHub account
- Netlify account
- Firebase project (see FIREBASE_SETUP.md)

## Step 1: Push Your Code to GitHub

1. Create a new repository on GitHub
2. Initialize Git in your local project (if not already done):
   ```bash
   cd booking-webapp
   git init
   ```
3. Add your files to Git:
   ```bash
   git add .
   ```
4. Commit your changes:
   ```bash
   git commit -m "Initial commit"
   ```
5. Link your local repository to GitHub:
   ```bash
   git remote add origin https://github.com/yourusername/booking-webapp.git
   ```
6. Push your code:
   ```bash
   git push -u origin main
   ```

## Step 2: Deploy to Netlify

### Option 1: Deploy via Netlify UI

1. Log in to your Netlify account
2. Click "New site from Git"
3. Select GitHub as your Git provider
4. Authorize Netlify to access your GitHub account
5. Select your booking-webapp repository
6. Netlify will automatically detect the build settings from netlify.toml
7. Click "Deploy site"

### Option 2: Deploy via Netlify CLI

1. Install Netlify CLI:
   ```bash
   npm install netlify-cli -g
   ```
2. Log in to Netlify:
   ```bash
   netlify login
   ```
3. Initialize your site:
   ```bash
   netlify init
   ```
4. Follow the prompts to connect to your GitHub repository
5. Deploy your site:
   ```bash
   netlify deploy --prod
   ```

## Step 3: Configure Environment Variables

1. In your Netlify dashboard, go to your site
2. Navigate to Site settings > Build & deploy > Environment
3. Add your Firebase configuration variables:
   - NEXT_PUBLIC_FIREBASE_API_KEY
   - NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN
   - NEXT_PUBLIC_FIREBASE_PROJECT_ID
   - NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET
   - NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID
   - NEXT_PUBLIC_FIREBASE_APP_ID
   - NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID
4. Click "Save"
5. Trigger a new deployment

## Step 4: Set Up Custom Domain (Optional)

1. In your Netlify dashboard, go to your site
2. Navigate to Site settings > Domain management
3. Click "Add custom domain"
4. Enter your domain name
5. Follow the instructions to configure DNS settings

## Step 5: Enable HTTPS (Optional)

Netlify automatically provisions SSL certificates for custom domains. To ensure HTTPS:

1. In your Netlify dashboard, go to your site
2. Navigate to Site settings > Domain management > HTTPS
3. Ensure "Provision SSL certificate" is enabled

## Troubleshooting

- **Build Failures**: Check your build logs in Netlify for specific errors
- **Environment Variables**: Verify all Firebase configuration variables are correctly set
- **Deployment Issues**: Make sure your netlify.toml file is correctly configured
- **404 Errors**: Check your redirect rules in netlify.toml

## Updating Your Site

To update your deployed site:

1. Make changes to your local code
2. Commit your changes:
   ```bash
   git add .
   git commit -m "Update description"
   ```
3. Push to GitHub:
   ```bash
   git push
   ```
4. Netlify will automatically detect the changes and deploy your updated site

## Replicating for Multiple Clients

To deploy multiple instances for different clients:

1. Create a new GitHub repository for each client (or use branches)
2. Follow steps 1-5 for each new repository
3. Configure each deployment with client-specific Firebase credentials
4. Use custom domains to brand each instance for the specific client
