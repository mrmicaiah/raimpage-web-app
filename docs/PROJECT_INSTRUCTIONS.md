# Raimpage - Claude Project Instructions

Copy and paste this into Claude project instructions.

---

## Project Overview

**Raimpage** is an AI music streaming platform. Creators upload AI-generated music, listeners stream and create playlists, fans tip artists. We're building this because Apple/Spotify gatekeep AI music — Raimpage welcomes it.

The name: **Rampage** with **AI** injected into it.

Domains: raimpage.com, raimpage.io, raimpage.app (all available)

---

## Business Model

### Subscription Tiers

| Tier | Price | Key Features |
|------|-------|-------------|
| **Free** | $0 | Stream, playlists, tip creators, 128kbps |
| **Premium Listener** | $4.99/mo | No ads, offline downloads, 320kbps, early access |
| **Creator Pro** | $9.99/mo | Unlimited uploads, 92% tip cut (vs 85%), deep analytics, verified badge, priority review |

### Revenue Streams
1. Platform cut on tips (15% free creators, 8% pro creators)
2. Premium Listener subscriptions
3. Creator Pro subscriptions

---

## Tech Stack

### Frontend
- **React** (web app)
- **React Native** with Expo (iOS/Android)
- **Tailwind CSS** for styling
- **TypeScript** everywhere

### Backend
- **Cloudflare Workers** (API)
- **Hono** (web framework for Workers)
- **Cloudflare D1** (SQLite database)
- **Cloudflare R2** (audio/image storage)
- **Stripe Connect** (tips/payouts)
- **Stripe Subscriptions** (premium tiers)

### Key Principle
All database calls go through `workers/api/src/services/db.ts`. This abstraction lets us swap D1 for Supabase/PlanetScale later without rewriting routes.

---

## Repository Structure

```
raimpage-web-app/
├── apps/
│   ├── web/                 # React web app
│   └── mobile/              # React Native app
├── packages/
│   └── shared/              # Shared types, utils, API client
├── workers/
│   └── api/                 # Cloudflare Worker API
│       ├── src/
│       │   ├── routes/      # auth.ts, songs.ts, playlists.ts, tips.ts, admin.ts, subscriptions.ts
│       │   ├── services/    # db.ts, storage.ts, stripe.ts
│       │   └── index.ts
│       └── schema.sql
└── docs/                    # Documentation
```

---

## Core Features (MVP)

### Listeners
- Browse/search songs
- Stream music (128kbps free, 320kbps premium)
- Create playlists
- Follow creators
- Tip creators
- Offline downloads (premium only)

### Creators
- Upload songs (10/month free, unlimited pro)
- Profile page
- Receive tips via Stripe Connect (85% free, 92% pro)
- Analytics (basic free, deep pro)

### Admin
- 3-step approval queue for uploads
- Platform stats dashboard
- Manage payouts

---

## Database Tables

- `users` - accounts with role (listener/creator/admin) and subscription_tier
- `songs` - uploaded tracks with status (pending/approved/rejected)
- `playlists` - user playlists
- `playlist_songs` - songs in playlists
- `tips` - payment records
- `follows` - creator follows
- `plays` - play logs

### Subscription Fields on Users
```sql
subscription_tier TEXT DEFAULT 'free'  -- 'free', 'premium', 'creator_pro'
subscription_stripe_id TEXT
subscription_expires_at INTEGER
monthly_uploads INTEGER DEFAULT 0
upload_reset_at INTEGER
```

---

## Build Order

1. **Phase 1** ✅ - Auth + storage (Workers, D1, R2)
2. **Phase 2** ✅ - Song upload, streaming, admin queue
3. **Phase 3** ✅ - Playlists, profiles, follows
4. **Phase 4** - Stripe Connect (tips) + Stripe Subscriptions
5. **Phase 5** - React Native mobile apps
6. **Phase 6** - Web UI (React)

---

## Escape Hatches (When We Scale)

- **D1 write limits** → Move plays to Analytics Engine or Tinybird
- **Need search** → Add Meilisearch or Algolia
- **D1 maxed out** → Migrate to Supabase or PlanetScale

---

## Legal Requirements

- DMCA Safe Harbor compliance (agent registered, takedown policy, repeat infringer policy)
- ToS with indemnification and content ownership clauses
- Privacy Policy (CCPA/GDPR compliant)
- Creator Agreement
- **Critical:** Strict policy against voice cloning of real artists

---

## Coding Preferences

- TypeScript strict mode
- Hono for Worker routing
- Zod for validation
- UUID v4 for IDs
- Unix timestamps (seconds) in database
- Signed URLs for R2 files (1 hour expiry)
- JWT tokens for auth (7-day expiry)
- Platform tip cut: 15% (free creators), 8% (pro creators)

---

## Current Status

- [x] Monorepo initialized
- [x] Worker deployed
- [x] D1 schema created
- [x] R2 bucket configured
- [x] Auth routes working
- [x] Song upload working
- [x] Streaming working
- [x] Admin queue working
- [x] Playlists working
- [x] Creator profiles working
- [x] Follow system working
- [ ] Stripe Connect (tips) integrated
- [ ] Stripe Subscriptions integrated
- [ ] Web app UI
- [ ] Mobile app started
- [ ] Launch ready
