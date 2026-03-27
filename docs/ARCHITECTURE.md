# Raimpage Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENTS                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │   Web App    │  │  iOS App     │  │ Android App  │           │
│  │   (React)    │  │(React Native)│  │(React Native)│           │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘           │
│         └──────────────────┼──────────────────┘                  │
│                            ▼                                     │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │              Cloudflare Workers (API)                        ││
│  │   /api/auth    /api/songs    /api/playlists   /api/tips     ││
│  └─────────────────────────┬───────────────────────────────────┘│
│                            │                                     │
│         ┌──────────────────┼──────────────────┐                  │
│         ▼                  ▼                  ▼                  │
│  ┌────────────┐    ┌────────────┐    ┌────────────┐             │
│  │     D1     │    │     R2     │    │   Stripe   │             │
│  │ (Database) │    │  (Audio/   │    │  Connect   │             │
│  │            │    │   Images)  │    │            │             │
│  └────────────┘    └────────────┘    └────────────┘             │
└─────────────────────────────────────────────────────────────────┘
```

---

## Data Flow

### User Uploads a Song
```
1. Creator selects audio file + cover art
2. Web/mobile app uploads to Worker
3. Worker streams files to R2, gets keys
4. Worker creates song record in D1 (status: pending)
5. Song enters admin approval queue
```

### User Streams a Song
```
1. User clicks play
2. App requests song from Worker
3. Worker generates signed R2 URL (expires 1hr)
4. App streams audio directly from R2
5. Worker logs play to D1 (or Analytics Engine at scale)
```

### User Tips a Creator
```
1. User clicks tip button
2. App sends tip request to Worker
3. Worker creates Stripe Checkout session
4. User completes payment on Stripe
5. Stripe webhook notifies Worker
6. Worker records tip in D1
7. Stripe Connect transfers funds to creator (minus platform %)
```

---

## Audio Streaming Strategy

### MVP (Simple)
- Direct MP3 streaming from R2
- Signed URLs prevent unauthorized access
- Works great for most use cases

### Scale (If Needed)
- Convert to HLS for adaptive streaming
- Multiple quality levels (128kbps, 256kbps, 320kbps)
- Better experience on poor connections

---

## Admin Approval Flow (3-Step Funnel)

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   PENDING   │────▶│   STEP 1    │────▶│   STEP 2    │────▶│  APPROVED   │
│             │     │  Auto-Check │     │   Listen    │     │             │
│  Uploaded   │     │  Format OK? │     │  Quality?   │     │   Live!     │
│             │     │  Duration?  │     │  Voice      │     │             │
│             │     │  Metadata?  │     │  cloning?   │     │             │
└─────────────┘     └──────┬──────┘     └──────┬──────┘     └─────────────┘
                           │                   │
                           ▼                   ▼
                    ┌─────────────┐     ┌─────────────┐
                    │  REJECTED   │     │  REJECTED   │
                    │  (reason)   │     │  (reason)   │
                    └─────────────┘     └─────────────┘
```

---

## Monorepo Structure

```
raimpage/
├── apps/
│   ├── web/                 # React web app
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   ├── hooks/
│   │   │   └── lib/
│   │   └── package.json
│   │
│   └── mobile/              # React Native app
│       ├── src/
│       └── package.json
│
├── packages/
│   └── shared/              # Shared types, utils
│       ├── types/
│       └── utils/
│
├── workers/
│   └── api/                 # Cloudflare Worker
│       ├── src/
│       │   ├── routes/
│       │   ├── services/
│       │   └── index.ts
│       ├── schema.sql
│       └── wrangler.toml
│
├── docs/                    # Documentation
│
└── package.json             # Monorepo root
```
