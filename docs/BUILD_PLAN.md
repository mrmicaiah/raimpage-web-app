# Raimpage Build Plan

## Overview

Phased approach: Get core working first, then layer on features.

---

## Phase 1: Foundation (Week 1-2)

### Goals
- Monorepo structure
- Worker with D1 database
- R2 storage configured
- Basic auth working

### Tasks
- [ ] Initialize monorepo with pnpm workspaces
- [ ] Set up Cloudflare Worker with Hono
- [ ] Create D1 database and run schema
- [ ] Set up R2 bucket for audio/images
- [ ] Implement auth routes (register, login, me)
- [ ] Create db.ts abstraction layer
- [ ] Set up wrangler.toml with bindings
- [ ] Basic error handling and validation

### Deliverable
Worker deployed, can register/login users.

---

## Phase 2: Core Features (Week 3-4)

### Goals
- Song upload and streaming
- Admin approval queue
- Basic web UI

### Tasks
- [ ] Song upload route (multipart to R2)
- [ ] Signed URL generation for streaming
- [ ] Song CRUD operations
- [ ] Admin queue routes
- [ ] Initialize React web app
- [ ] Build audio player component
- [ ] Browse/search page
- [ ] Song detail page
- [ ] Admin approval interface

### Deliverable
Can upload songs, admin can approve, listeners can stream.

---

## Phase 3: Playlists & Profiles (Week 5)

### Goals
- Playlist functionality
- Creator profiles
- Follow system

### Tasks
- [ ] Playlist CRUD routes
- [ ] Add/remove songs from playlists
- [ ] Creator profile page
- [ ] Follow/unfollow functionality
- [ ] User profile/settings page
- [ ] Playlist UI in web app

### Deliverable
Full listener experience working.

---

## Phase 4: Payments (Week 6)

### Goals
- Stripe Connect integration
- Tipping system
- Creator payouts

### Tasks
- [ ] Set up Stripe Connect
- [ ] Creator onboarding flow (connect Stripe)
- [ ] Tip checkout flow
- [ ] Webhook handler for completed payments
- [ ] Tips received/sent views
- [ ] Platform fee calculation (10-15%)

### Deliverable
Creators can receive tips, platform takes cut.

---

## Phase 5: Mobile App (Week 7-9)

### Goals
- React Native app
- iOS and Android builds
- Feature parity with web

### Tasks
- [ ] Initialize React Native with Expo
- [ ] Shared API client from packages/shared
- [ ] Auth screens
- [ ] Browse/search screens
- [ ] Audio player with background playback
- [ ] Playlist screens
- [ ] Creator profile screens
- [ ] Tipping flow
- [ ] iOS TestFlight build
- [ ] Android APK build

### Deliverable
Mobile apps ready for beta testing.

---

## Phase 6: Polish & Launch (Week 10)

### Goals
- Bug fixes
- Performance optimization
- Launch prep

### Tasks
- [ ] Fix bugs from beta testing
- [ ] Add loading states and error handling
- [ ] Optimize audio streaming
- [ ] Add analytics (play counts, etc.)
- [ ] Legal pages (ToS, Privacy, DMCA)
- [ ] Domain setup (raimpage.com)
- [ ] App store submissions
- [ ] Marketing site / landing page

### Deliverable
Launch ready.

---

## Post-Launch Priorities

1. **Search** - Add Meilisearch/Algolia if needed
2. **Recommendations** - Simple "you might like" based on plays
3. **Social features** - Comments, shares
4. **Creator analytics** - Dashboard for creators
5. **Scale D1** - Move to Supabase/PlanetScale if write limits hit

---

## Key Milestones

| Week | Milestone |
|------|----------|
| 2 | Auth + storage working |
| 4 | Can upload, approve, stream songs |
| 5 | Playlists and profiles complete |
| 6 | Tipping live |
| 9 | Mobile apps in beta |
| 10 | Launch |
