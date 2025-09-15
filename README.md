# DexStreamer

A Next.js 14 web app that tracks Pump.fun tokens where developers are livestreaming and shows their token stats in real time.

## Features

- **Real-time Token Discovery**: Connects to PumpPortal WebSocket to discover new tokens
- **Livestream Detection**: Parses Pump.fun pages to detect livestreaming developers
- **Live Stats**: Fetches token statistics from Moralis API every 30-60 seconds
- **Global Viewer Tracking**: Tracks total current viewers and daily peak viewers
- **Futuristic UI**: Cyberpunk-themed interface with neon green accents

## Tech Stack

- Next.js 14 (App Router)
- TypeScript
- Tailwind CSS
- Prisma + PostgreSQL
- Framer Motion
- Recharts
- Moralis API
- PumpPortal WebSocket

## Environment Variables

Create a `.env.local` file with the following variables:

```env
# Database - Replace with your actual database URL
DATABASE_URL="postgresql://username:password@localhost:5432/dexstreamer"

# Moralis API - Get your API key from https://moralis.io/
MORALIS_KEY="your_moralis_api_key_here"

# Next.js - Generate a random secret
NEXTAUTH_SECRET="your_nextauth_secret_here"
```

## Setup

1. Install dependencies:
```bash
npm install
```

2. Set up your database:
```bash
# For local development
npm run db:generate
npm run db:push

# For production deployment
npm run db:migrate:deploy
```

3. Run the development server:
```bash
npm run dev
```

4. Start the background workers (optional for local development):
```bash
# WebSocket worker (in a separate terminal)
npm run worker

# Stats worker (in a separate terminal)
npm run stats-worker
```

## Deployment

This app is configured for deployment on Vercel:

1. **Connect Repository**: Connect your GitHub repository to Vercel
2. **Environment Variables**: Add these environment variables in Vercel dashboard:
   - `DATABASE_URL`: Your Vercel Postgres connection string
   - `MORALIS_KEY`: Your Moralis API key
   - `NEXTAUTH_SECRET`: A random secret string
3. **Deploy**: The app will automatically:
   - Run `prisma generate` on install
   - Execute database migrations
   - Set up CRON jobs for stats updates

### Vercel Postgres Setup

1. In your Vercel dashboard, go to Storage
2. Create a new Postgres database
3. Copy the connection string to your `DATABASE_URL` environment variable

### CRON Jobs

The app includes a CRON job that runs every 30 seconds to update token stats. This is configured in `vercel.json` and will automatically run on Vercel.

## API Routes

- `GET /api/tokens` - List all livestreaming tokens with stats
- `GET /api/token/[mint]` - Get detailed token information
- `GET /api/global` - Get global viewer statistics

## Database Schema

- **Token**: Basic token information (mint, name, symbol)
- **Livestream**: Livestream metadata (viewers, developer, stream status)
- **Stats**: Token statistics (price, liquidity, volume, market cap, holders)
- **GlobalStats**: Global viewer counts and peak tracking