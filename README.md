# Booking Webapp

A complete booking system with client-side booking flow and admin dashboard, built with Next.js and Firebase.

## Features

### Client Side
- Artist selection
- Service selection
- Date and time booking
- Client information collection with age verification (18+)
- Real-time availability checking

### Admin Side
- Artist management (CRUD operations)
- Service management (CRUD operations)
- Time blocking functionality
- Admin authentication

## Tech Stack
- Next.js
- Firebase (Firestore, Authentication)
- Tailwind CSS

## Getting Started

### Prerequisites
- Node.js 16+
- npm or pnpm
- Firebase account

### Installation

1. Clone the repository
```bash
git clone https://github.com/yourusername/booking-webapp.git
cd booking-webapp
```

2. Install dependencies
```bash
pnpm install
```

3. Create a `.env.local` file based on `.env.example` and add your Firebase configuration

4. Run the development server
```bash
pnpm dev
```

5. Open [http://localhost:3000](http://localhost:3000) in your browser

## Deployment

### Netlify Deployment

1. Push your code to GitHub
2. Connect your GitHub repository to Netlify
3. Configure the build settings:
   - Build command: `pnpm build`
   - Publish directory: `.next`
4. Add your environment variables in Netlify dashboard
5. Deploy!

## Firebase Setup

1. Create a new Firebase project
2. Enable Firestore Database
3. Set up Authentication with Email/Password
4. Add your Firebase configuration to environment variables
5. Set up Firestore security rules

## Project Structure

- `src/app/` - Next.js pages
- `src/components/` - React components
- `src/lib/` - Utility functions and Firebase configuration
- `public/` - Static assets

## License

This project is licensed under the MIT License - see the LICENSE file for details.
