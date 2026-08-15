# All Market Ethiopia

A free, fast, trust-first classifieds marketplace as a **Telegram Mini App**.

Buyers and sellers connect directly via Telegram DM or phone call.  
No payments, no cart, no in-app chat.

---

## What's Included (Phases 1 + 2)

### Backend (NestJS + Prisma + PostgreSQL)
- Telegram `initData` cryptographic verification (HMAC-SHA256)
- JWT authentication
- Full Prisma schema (Users, Listings, Categories, Favorites, Reviews, Reports, Credits, Ads...)
- Profile completion flow
- Categories (auto-seeded)
- Listings CRUD + search + filters
- Favorites
- Seller public profiles
- Proper empty-state friendly API responses

### Frontend (React + Vite + Tailwind + Telegram WebApp SDK)
- Mobile-first UI optimized for Telegram WebView
- Light / Dark mode (follows Telegram theme)
- Amharic + English (i18n)
- Home feed with categories
- Search
- Listing detail with Message / Call Seller
- Create listing flow
- Favorites
- Seller profiles
- Profile + language settings
- Complete profile modal
- Designed empty states everywhere
- Skeleton loaders

---

## Quick Start

### 1. Prerequisites
- Node.js 18+
- Docker (recommended) or local PostgreSQL
- A Telegram Bot (create via [@BotFather](https://t.me/BotFather))

### 2. Start Database

```bash
docker compose up -d
```

### 3. Backend Setup

```bash
cd backend
cp .env.example .env
# Defaults already match docker-compose. Add your TELEGRAM_BOT_TOKEN + a strong JWT_SECRET

npm install
npx prisma generate
npx prisma migrate dev --name init
npm run start:dev
```

API runs at `http://localhost:3000`

### 4. Frontend Setup

```bash
cd frontend
cp .env.example .env
npm install
npm run dev
```

Frontend runs at `http://localhost:5173`

### 5. Connect to Telegram & Deploy

See the full step-by-step guide in **[DEPLOYMENT.md](./DEPLOYMENT.md)**  
(covers BotFather, Railway, Vercel, localtunnel, and common issues).

---

## Project Structure

```
all-market-ethiopia/
├── backend/
│   ├── prisma/schema.prisma     # Full database schema
│   └── src/
│       ├── auth/                # Telegram auth + JWT
│       ├── users/
│       ├── listings/
│       ├── categories/
│       ├── favorites/
│       └── prisma/
├── frontend/
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── stores/              # Zustand auth store
│       ├── i18n/                # Amharic + English
│       └── lib/                 # API client + Telegram helpers
└── README.md
```

---

## Environment Variables

### Backend (`backend/.env`)
```
DATABASE_URL=postgresql://user:pass@localhost:5432/all_market_ethiopia
JWT_SECRET=your-super-secret-jwt-key
TELEGRAM_BOT_TOKEN=123456:ABC-DEF...
PORT=3000
```

### Frontend (`frontend/.env`)
```
VITE_API_URL=http://localhost:3000/api
```

---

## Deployment Recommendations

| Layer       | Recommended Platform      |
|-------------|---------------------------|
| Frontend    | Vercel / Cloudflare Pages |
| Backend     | Railway / Render / Fly.io |
| Database    | Railway Postgres / Neon / Supabase |
| Files (later) | Cloudflare R2 / AWS S3  |

---

## Next Phases (scaffolded in schema)

- **Phase 3**: Ratings, Reviews, Reporting, Moderation
- **Phase 4**: Credit system, Rewarded ads, Bot notifications
- **Phase 5**: Banner / Featured / Sponsored ads + analytics
- **Phase 6**: Full Admin dashboard

The database schema already contains all forward-compatible fields.

---

## Important Notes

- **No demo data** is seeded in production. Categories are the only auto-created records.
- Contact between users happens **only** via Telegram DM (`t.me/username`) or `tel:` links.
- There is **no payment, cart, or in-app messaging** code.

---

Built according to the All Market Ethiopia Master Implementation Prompt.
