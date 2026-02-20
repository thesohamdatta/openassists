# OpenAssist Web Widget

A Next.js web widget for voice/chat interaction with LiveKit Agents.

This project includes:
- A full-page app demo
- An iframe embed flow
- A standalone popup script build (`public/embed-popup.js`)
- A token endpoint for LiveKit connection details

## Features

- Real-time voice interaction with LiveKit Agents
- Chat and transcription UI
- Camera and screen share support
- Theme support (light/dark)
- Configurable behavior via `app-config.ts`

## Tech Stack

- Next.js 15
- React 19
- TypeScript
- LiveKit Client + LiveKit Server SDK
- Tailwind CSS
- Webpack (for standalone popup bundle)

## Project Structure

```text
openassist/
  app/                    # App routes, iframe routes, API routes, test pages
  components/             # UI and widget components
  hooks/                  # Reusable React hooks
  lib/                    # Shared types and helpers
  public/                 # Static assets + generated embed-popup.js
  styles/                 # Global styles
  app-config.ts           # Widget feature defaults
  webpack.config.js       # Standalone popup bundle config
```

## Prerequisites

- Node.js 20+
- pnpm 9+
- A LiveKit project (Cloud or self-hosted)
- A running LiveKit Agent backend

## Getting Started

1. Install dependencies:

```bash
pnpm install
```

2. Create local environment file:

```bash
cp .env.example .env.local
```

3. Fill required values in `.env.local`:

```env
LIVEKIT_API_KEY=<your_api_key>
LIVEKIT_API_SECRET=<your_api_secret>
LIVEKIT_URL=wss://<project-subdomain>.livekit.cloud
NEXT_PUBLIC_CONN_DETAILS_ENDPOINT=http://localhost:3000/api/connection-details
```

4. Build popup bundle:

```bash
pnpm build-embed-popup-script
```

5. Start development server:

```bash
pnpm dev
```

Open `http://localhost:3000`.

## Available Scripts

- `pnpm dev` - Run Next.js in development mode
- `pnpm build-embed-popup-script` - Build `public/embed-popup.js`
- `pnpm build` - Production build + popup bundle build
- `pnpm start` - Run production server
- `pnpm lint` - Run ESLint
- `pnpm format` - Format files with Prettier
- `pnpm format:check` - Check formatting

## Configuration

Widget defaults are defined in `app-config.ts`:

```ts
export const APP_CONFIG_DEFAULTS = {
  supportsChatInput: true,
  supportsVideoInput: true,
  supportsScreenShare: true,
  isPreConnectBufferEnabled: true,
};
```

Update these flags to enable or disable supported input capabilities.

## API Endpoint

- `POST /api/connection-details`

This endpoint generates a short-lived participant token and returns:
- `serverUrl`
- `roomName`
- `participantName`
- `participantToken`

## Popup Script Notes

- Source entry: `components/embed-popup/standalone-bundle-root.tsx`
- Build output: `public/embed-popup.js`
- Local test page: `http://localhost:3000/test/popup`

If you change popup component code, rebuild the bundle:

```bash
pnpm build-embed-popup-script
```

## Troubleshooting

- `LIVEKIT_* is not defined` errors: Check `.env.local` values and restart the dev server.
- Popup changes not visible: Re-run `pnpm build-embed-popup-script`.
- Agent connection issues: Confirm your LiveKit Agent backend is running and reachable.
