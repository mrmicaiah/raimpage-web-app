# Raimpage Tech Stack

## Overview

Raimpage is an AI music streaming platform built on Cloudflare's edge infrastructure. The stack is designed to start simple, scale automatically, and have clear escape hatches when growth demands it.

---

## Frontend

### Web App
- **React** - UI framework
- **Tailwind CSS** - Styling
- **Vite** - Build tool

### Mobile Apps
- **React Native** - Cross-platform iOS/Android
- **Expo** - Build and deployment

### Shared
- **TypeScript** - Type safety across all apps
- **Shared package** - Common types, utils, API client

---

## Backend

### API Layer
- **Cloudflare Workers** - Edge-deployed API
- **Hono** - Lightweight web framework for Workers
- **TypeScript** - Type safety

### Database
- **Cloudflare D1** - SQLite at the edge
- **Abstracted via db.ts** - Single file for all DB calls (escape hatch ready)

### File Storage
- **Cloudflare R2** - Audio files, cover art, avatars
- **Signed URLs** - Secure, time-limited access to files
- **No egress fees** - Critical for streaming economics

### Authentication
- **Custom auth** - Email/password with JWT tokens
- **Future:** Google/Apple OAuth

### Payments
- **Stripe Connect** - Creator payouts, platform fees
- **10-15% platform cut** on tips

---

## Infrastructure

### Cloudflare Services
| Service | Purpose |
|---------|--------|
| Workers | API and business logic |
| D1 | Primary database |
| R2 | Audio and image storage |
| Pages | Web app hosting (optional) |

### External Services
| Service | Purpose |
|---------|--------|
| Stripe Connect | Payment processing |
| GitHub | Code repository |

---

## Escape Hatches (When Scale Demands)

### D1 Write Limits
If D1 write limits become an issue:
- Move play logging to **Cloudflare Analytics Engine** or **Tinybird**
- Migrate hot tables to **Supabase** (Postgres) or **PlanetScale** (MySQL)

### Search
D1 doesn't have full-text search:
- Add **Meilisearch** or **Algolia** when needed
- Sync from D1 on song create/update

### Real-time Features
If we need live features later:
- **Durable Objects** for WebSocket connections
- Live listening parties, real-time play counts

---

## Key Architectural Principle

**Abstract the database layer.**

All database calls go through `workers/api/src/services/db.ts`. When we need to swap databases, we only change that one file — not every route.

```typescript
// All routes use this interface
const db = createDatabase(env.DB);
const user = await db.users.findById(id);

// Swap implementation without touching routes
export function createDatabase(d1: D1Database): Database { ... }
// Future: createDatabase(supabase: SupabaseClient): Database { ... }
```
