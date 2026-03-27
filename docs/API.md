# Raimpage API Routes

## Base URL

```
Production: https://api.raimpage.com
Development: http://localhost:8787
```

---

## Authentication

All authenticated routes require:
```
Authorization: Bearer <token>
```

### POST /api/auth/register
Create a new account.

**Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "name": "Display Name"
}
```

**Response:** `201 Created`
```json
{
  "user": { "id": "...", "email": "...", "name": "...", "role": "listener" },
  "token": "jwt_token_here"
}
```

### POST /api/auth/login
Sign in to existing account.

**Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:** `200 OK`
```json
{
  "user": { ... },
  "token": "jwt_token_here"
}
```

### GET /api/auth/me
Get current user. **Requires auth.**

**Response:** `200 OK`
```json
{
  "user": { "id": "...", "email": "...", "name": "...", "role": "..." }
}
```

---

## Songs

### GET /api/songs
Browse/search songs.

**Query params:**
- `q` - Search query (optional)
- `creator` - Filter by creator ID (optional)
- `limit` - Results per page (default: 20)
- `offset` - Pagination offset

**Response:** `200 OK`
```json
{
  "songs": [
    {
      "id": "...",
      "title": "...",
      "creator": { "id": "...", "name": "..." },
      "cover_url": "...",
      "duration_seconds": 180,
      "play_count": 1234
    }
  ],
  "total": 100,
  "limit": 20,
  "offset": 0
}
```

### GET /api/songs/:id
Get single song with streaming URL.

**Response:** `200 OK`
```json
{
  "song": {
    "id": "...",
    "title": "...",
    "creator": { ... },
    "lyrics": "...",
    "duration_seconds": 180,
    "play_count": 1234,
    "audio_url": "https://r2-signed-url...",
    "cover_url": "https://r2-signed-url..."
  }
}
```

### POST /api/songs
Upload a new song. **Requires auth (creator role).**

**Body:** `multipart/form-data`
- `audio` - Audio file (mp3, wav, flac)
- `cover` - Cover image (jpg, png)
- `title` - Song title
- `lyrics` - Song lyrics (optional)
- `human_contribution` - lyrics, arrangement, both, minimal
- `license_type` - License selection

**Response:** `201 Created`
```json
{
  "song": { "id": "...", "title": "...", "status": "pending" }
}
```

### DELETE /api/songs/:id
Delete own song. **Requires auth (owner only).**

**Response:** `204 No Content`

---

## Playlists

### GET /api/playlists
Get current user's playlists. **Requires auth.**

**Response:** `200 OK`
```json
{
  "playlists": [
    { "id": "...", "name": "...", "song_count": 12 }
  ]
}
```

### POST /api/playlists
Create a playlist. **Requires auth.**

**Body:**
```json
{
  "name": "My Playlist",
  "description": "Optional description",
  "is_public": true
}
```

### GET /api/playlists/:id
Get playlist with songs.

### PUT /api/playlists/:id
Update playlist. **Requires auth (owner only).**

### DELETE /api/playlists/:id
Delete playlist. **Requires auth (owner only).**

### POST /api/playlists/:id/songs
Add song to playlist. **Requires auth (owner only).**

**Body:**
```json
{
  "song_id": "..."
}
```

### DELETE /api/playlists/:id/songs/:songId
Remove song from playlist. **Requires auth (owner only).**

---

## Creators

### GET /api/creators/:id
Get creator profile with their songs.

**Response:** `200 OK`
```json
{
  "creator": {
    "id": "...",
    "name": "...",
    "bio": "...",
    "avatar_url": "...",
    "follower_count": 1234,
    "song_count": 15
  },
  "songs": [ ... ]
}
```

### POST /api/creators/:id/follow
Follow a creator. **Requires auth.**

### DELETE /api/creators/:id/follow
Unfollow a creator. **Requires auth.**

---

## Tips

### POST /api/tips
Send a tip to a creator. **Requires auth.**

**Body:**
```json
{
  "creator_id": "...",
  "song_id": "...",
  "amount_cents": 500
}
```

**Response:** `200 OK`
```json
{
  "checkout_url": "https://checkout.stripe.com/..."
}
```

### GET /api/tips/received
Get tips received (for creators). **Requires auth (creator role).**

### GET /api/tips/sent
Get tips sent. **Requires auth.**

---

## Admin

### GET /api/admin/queue
Get pending songs for review. **Requires auth (admin role).**

**Query params:**
- `status` - Filter by status (pending, step1, step2)

### PUT /api/admin/songs/:id
Approve or reject a song. **Requires auth (admin role).**

**Body:**
```json
{
  "action": "approve",  // or "reject"
  "reason": "Optional rejection reason"
}
```

### GET /api/admin/stats
Get platform statistics. **Requires auth (admin role).**

**Response:** `200 OK`
```json
{
  "total_users": 1234,
  "total_creators": 56,
  "total_songs": 789,
  "pending_songs": 12,
  "total_tips_cents": 123456,
  "platform_revenue_cents": 12345
}
```
