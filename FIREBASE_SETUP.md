# Firebase Configuration Guide for Booking Webapp

This guide will help you set up Firebase for your booking webapp after deploying to Netlify.

## Step 1: Create a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Enter a project name (e.g., "booking-webapp")
4. Choose whether to enable Google Analytics (recommended)
5. Follow the prompts to complete project creation

## Step 2: Register Your Web App

1. In your Firebase project dashboard, click the web icon (</>) to add a web app
2. Enter a nickname for your app (e.g., "booking-webapp")
3. Check the box for "Also set up Firebase Hosting" if you plan to use it
4. Click "Register app"
5. You'll see your Firebase configuration - save this information for later

## Step 3: Set Up Authentication

1. In the Firebase console, go to "Authentication"
2. Click "Get started"
3. Enable the "Email/Password" sign-in method
4. Create an admin user:
   - Go to "Users" tab
   - Click "Add user"
   - Enter an email and password for your admin account

## Step 4: Set Up Firestore Database

1. In the Firebase console, go to "Firestore Database"
2. Click "Create database"
3. Choose "Start in production mode" (recommended)
4. Select a location closest to your users
5. Click "Enable"

## Step 5: Set Up Firestore Security Rules

1. In the Firestore Database section, go to the "Rules" tab
2. Replace the default rules with the following:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow admin access to authenticated users for all collections
    match /{document=**} {
      allow read, write: if request.auth != null && 
        exists(/databases/$(database)/documents/admins/$(request.auth.uid));
    }
    
    // Public read access for active artists and services
    match /artists/{artistId} {
      allow read: if resource.data.isActive == true;
    }
    
    match /services/{serviceId} {
      allow read: if resource.data.isActive == true;
    }
    
    // Public read access for blocked times
    match /blockedTimes/{timeId} {
      allow read: if true;
    }
    
    // Allow clients to create bookings and client profiles
    match /bookings/{bookingId} {
      allow create: if true;
    }
    
    match /clients/{clientId} {
      allow create: if true;
    }
  }
}
```

## Step 6: Add Firebase Configuration to Netlify

1. Go to your Netlify dashboard
2. Select your booking webapp site
3. Go to "Site settings" > "Build & deploy" > "Environment"
4. Add the following environment variables with your Firebase configuration:

```
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key_here
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=your_measurement_id
```

5. Click "Save"
6. Trigger a new deployment

## Step 7: Initialize Your Database (First-time Setup)

After deployment, you'll need to create your first admin user:

1. Sign in to your app using the admin email/password you created
2. Use the admin dashboard to:
   - Add artists
   - Add services
   - Set up blocked times if needed

## Troubleshooting

- **Authentication Issues**: Make sure your Firebase Authentication is properly enabled and the admin user is created
- **Database Access Issues**: Check your Firestore security rules
- **Environment Variables**: Verify all Firebase configuration variables are correctly set in Netlify

## Replicating for Multiple Clients

To replicate this booking system for multiple clients:

1. Create a new Firebase project for each client
2. Follow steps 1-7 for each new project
3. Deploy a new instance of the booking webapp to Netlify
4. Configure the new deployment with the client-specific Firebase credentials

This approach keeps each client's data completely separate and secure.
