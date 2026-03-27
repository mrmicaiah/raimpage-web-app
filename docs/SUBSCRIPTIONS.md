# Raimpage Subscription Tiers

## Overview

Raimpage uses a freemium model. Free to listen, premium for perks.

---

## Tiers

### Free (Listener)
**Price:** $0

| Feature | Included |
|---------|----------|
| Stream music | ✅ |
| Create playlists | ✅ |
| Follow creators | ✅ |
| Tip creators | ✅ |
| Audio quality | 128kbps |
| Ads | Yes (future) |
| Offline downloads | ❌ |
| Early access | ❌ |

---

### Premium Listener
**Price:** $4.99/month

| Feature | Included |
|---------|----------|
| Everything in Free | ✅ |
| Audio quality | 320kbps |
| Ads | ❌ None |
| Offline downloads | ✅ |
| Early access to new releases | ✅ |
| Profile badge | ✅ "Supporter" |

---

### Creator (Free)
**Price:** $0

Any user can become a creator by uploading their first song.

| Feature | Included |
|---------|----------|
| Upload songs | ✅ (10/month limit) |
| Receive tips | ✅ (85% - platform takes 15%) |
| Creator profile | ✅ |
| Basic analytics | ✅ (play counts) |
| Review queue | Standard |

---

### Creator Pro
**Price:** $9.99/month

| Feature | Included |
|---------|----------|
| Everything in Creator | ✅ |
| Upload limit | ✅ Unlimited |
| Tip cut | ✅ 92% (platform takes 8%) |
| Deep analytics | ✅ (listener locations, retention, trends) |
| Verified badge | ✅ |
| Priority review | ✅ (fast-track approval) |
| Profile customization | ✅ (banner, colors, featured tracks) |

---

## Database Schema Additions

```sql
-- Add to users table
ALTER TABLE users ADD COLUMN subscription_tier TEXT DEFAULT 'free';
-- Values: 'free', 'premium', 'creator_pro'

ALTER TABLE users ADD COLUMN subscription_stripe_id TEXT;
ALTER TABLE users ADD COLUMN subscription_expires_at INTEGER;

-- Track upload counts for free creators
ALTER TABLE users ADD COLUMN monthly_uploads INTEGER DEFAULT 0;
ALTER TABLE users ADD COLUMN upload_reset_at INTEGER;
```

---

## Feature Gating Logic

```typescript
// Check if user can upload
function canUpload(user: User): boolean {
  if (user.subscription_tier === 'creator_pro') return true;
  if (user.monthly_uploads >= 10) return false;
  return true;
}

// Get tip percentage for creator
function getCreatorCut(user: User): number {
  return user.subscription_tier === 'creator_pro' ? 0.92 : 0.85;
}

// Check if user gets high quality audio
function getAudioQuality(user: User): '128' | '320' {
  return user.subscription_tier === 'premium' ? '320' : '128';
}

// Check if user can download offline
function canDownload(user: User): boolean {
  return user.subscription_tier === 'premium';
}
```

---

## Stripe Products

| Product | Stripe Price ID | Amount |
|---------|-----------------|--------|
| Premium Listener Monthly | `price_premium_monthly` | $4.99 |
| Creator Pro Monthly | `price_creator_pro_monthly` | $9.99 |

---

## API Endpoints

```
POST /api/subscriptions/checkout
  - body: { tier: 'premium' | 'creator_pro' }
  - returns: { checkout_url }

POST /api/subscriptions/portal
  - returns: { portal_url }  (Stripe billing portal)

GET /api/subscriptions/status
  - returns: { tier, expires_at, will_renew }

POST /api/webhooks/stripe
  - handles: checkout.session.completed, customer.subscription.updated, customer.subscription.deleted
```
