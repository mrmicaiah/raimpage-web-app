# Raimpage - Claude Project Instructions

Copy and paste this into Claude project instructions.

---

## Project Overview

**Raimpage** is an AI music streaming platform. Creators upload AI-generated music, listeners stream and create playlists, fans tip artists. We're building this because Apple/Spotify gatekeep AI music — Raimpage welcomes it.

The name: **Rampage** with **AI** injected into it.

Domains: raimpage.com, raimpage.io, raimpage.app (all available)

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
- **Stripe Connect** (payments/tips)

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
│       │   ├── routes/      # auth.ts, songs.ts, playlists.ts, tips.ts, admin.ts
│       │   ├── services/    # db.ts, storage.ts, stripe.ts
│       │   └── index.ts
│       └── schema.sql
└── docs/                    # Documentation
```

---

## Core Features (MVP)

### Listeners
- Browse/search songs
- Stream music
- Create playlists
- Tip creators

### Creators
- Upload songs (audio + lyrics + cover art)
- Profile page
- Receive tips via Stripe Connect

### Admin
- 3-step approval queue for uploads
- Platform stats dashboard
- Manage payouts

---

## Database Tables

- `users` - accounts with role (listener/creator/admin)
- `songs` - uploaded tracks with status (pending/approved/rejected)
- `playlists` - user playlists
- `playlist_songs` - songs in playlists
- `tips` - payment records
- `follows` - creator follows
- `plays` - play logs (may move to Analytics Engine at scale)

---

## Build Order

1. **Phase 1** - Auth + storage (Workers, D1, R2)
2. **Phase 2** - Song upload, streaming, admin queue
3. **Phase 3** - Playlists, profiles, follows
4. **Phase 4** - Stripe Connect, tipping
5. **Phase 5** - React Native mobile apps
6. **Phase 6** - Polish, legal pages, launch

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
- JWT tokens for auth
- 10-15% platform cut on tips

---

## Current Status

[Update this as you build]

- [ ] Monorepo initialized
- [ ] Worker deployed
- [ ] D1 schema created
- [ ] R2 bucket configured
- [ ] Auth routes working
- [ ] Song upload working
- [ ] Streaming working
- [ ] Admin queue working
- [ ] Web app basic UI
- [ ] Playlists working
- [ ] Stripe Connect integrated
- [ ] Mobile app started
- [ ] Mobile app beta
- [ ] Launch ready
