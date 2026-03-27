# Raimpage Database Schema

## Overview

Cloudflare D1 (SQLite at the edge). All tables use TEXT primary keys (UUIDs generated server-side).

---

## Tables

### users
```sql
CREATE TABLE users (
  id TEXT PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  name TEXT NOT NULL,
  role TEXT DEFAULT 'listener',  -- listener, creator, admin
  stripe_account_id TEXT,         -- for creators receiving tips
  avatar_url TEXT,
  bio TEXT,
  created_at INTEGER DEFAULT (unixepoch()),
  updated_at INTEGER DEFAULT (unixepoch())
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

### songs
```sql
CREATE TABLE songs (
  id TEXT PRIMARY KEY,
  creator_id TEXT NOT NULL REFERENCES users(id),
  title TEXT NOT NULL,
  audio_key TEXT NOT NULL,        -- R2 object key
  cover_key TEXT,                 -- R2 object key
  lyrics TEXT,
  duration_seconds INTEGER,
  status TEXT DEFAULT 'pending',  -- pending, step1, step2, approved, rejected
  rejection_reason TEXT,
  human_contribution TEXT,        -- lyrics, arrangement, both, minimal
  license_type TEXT DEFAULT 'all_rights_reserved',
  play_count INTEGER DEFAULT 0,
  created_at INTEGER DEFAULT (unixepoch()),
  approved_at INTEGER
);

CREATE INDEX idx_songs_creator ON songs(creator_id);
CREATE INDEX idx_songs_status ON songs(status);
CREATE INDEX idx_songs_approved ON songs(approved_at);
```

### playlists
```sql
CREATE TABLE playlists (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL REFERENCES users(id),
  name TEXT NOT NULL,
  description TEXT,
  is_public INTEGER DEFAULT 1,
  cover_key TEXT,
  created_at INTEGER DEFAULT (unixepoch()),
  updated_at INTEGER DEFAULT (unixepoch())
);

CREATE INDEX idx_playlists_user ON playlists(user_id);
```

### playlist_songs
```sql
CREATE TABLE playlist_songs (
  playlist_id TEXT NOT NULL REFERENCES playlists(id) ON DELETE CASCADE,
  song_id TEXT NOT NULL REFERENCES songs(id) ON DELETE CASCADE,
  position INTEGER NOT NULL,
  added_at INTEGER DEFAULT (unixepoch()),
  PRIMARY KEY (playlist_id, song_id)
);
```

### tips
```sql
CREATE TABLE tips (
  id TEXT PRIMARY KEY,
  from_user_id TEXT NOT NULL REFERENCES users(id),
  to_creator_id TEXT NOT NULL REFERENCES users(id),
  song_id TEXT REFERENCES songs(id),
  amount_cents INTEGER NOT NULL,
  platform_fee_cents INTEGER NOT NULL,
  creator_amount_cents INTEGER NOT NULL,
  stripe_payment_id TEXT,
  status TEXT DEFAULT 'pending',  -- pending, completed, failed
  created_at INTEGER DEFAULT (unixepoch())
);

CREATE INDEX idx_tips_from ON tips(from_user_id);
CREATE INDEX idx_tips_to ON tips(to_creator_id);
CREATE INDEX idx_tips_song ON tips(song_id);
```

### follows
```sql
CREATE TABLE follows (
  follower_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  creator_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  created_at INTEGER DEFAULT (unixepoch()),
  PRIMARY KEY (follower_id, creator_id)
);

CREATE INDEX idx_follows_creator ON follows(creator_id);
```

### plays
```sql
-- Note: May move to Analytics Engine at scale
CREATE TABLE plays (
  id TEXT PRIMARY KEY,
  song_id TEXT NOT NULL REFERENCES songs(id),
  user_id TEXT,  -- nullable for anonymous plays
  played_at INTEGER DEFAULT (unixepoch())
);

CREATE INDEX idx_plays_song ON plays(song_id);
CREATE INDEX idx_plays_user ON plays(user_id);
CREATE INDEX idx_plays_date ON plays(played_at);
```

---

## Role Values

| Role | Description |
|------|-------------|
| `listener` | Can browse, stream, create playlists, tip |
| `creator` | All listener abilities + upload songs, receive tips |
| `admin` | All abilities + approve/reject songs, view dashboard |

---

## Song Status Values

| Status | Description |
|--------|-------------|
| `pending` | Just uploaded, awaiting review |
| `step1` | Passed auto-checks, awaiting listen |
| `step2` | Passed listen, awaiting final review |
| `approved` | Live on platform |
| `rejected` | Rejected with reason |

---

## License Types

| Type | Description |
|------|-------------|
| `all_rights_reserved` | Standard copyright |
| `cc_by` | Creative Commons Attribution |
| `cc_by_sa` | CC Attribution-ShareAlike |
| `cc_by_nc` | CC Attribution-NonCommercial |
| `cc0` | Public domain |
