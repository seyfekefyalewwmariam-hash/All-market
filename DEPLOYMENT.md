# All Market Ethiopia — Deployment Guide

This guide walks you from zero to a live Telegram Mini App.

---

## 1. Create Your Telegram Bot

1. Open Telegram and talk to [@BotFather](https://t.me/BotFather)
2. Send `/newbot`
3. Choose a name (e.g. `All Market Ethiopia`)
4. Choose a username (must end with `bot`, e.g. `AllMarketEthiopiaBot`)
5. Copy the **Bot Token** (looks like `7123456789:AAH...`)

### Create the Mini App

1. Still in BotFather, send `/newapp` (or `/myapps` → select your bot)
2. Follow the prompts:
   - Title: `All Market Ethiopia`
   - Description: short description
   - Photo: upload a logo (optional for now)
   - **Web App URL**: you will set this after deploying the frontend
3. Note the short name / link BotFather gives you

---

## 2. Prepare Environment Variables

### Backend

```env
DATABASE_URL=postgresql://...
JWT_SECRET=generate-a-long-random-string-at-least-32-chars
TELEGRAM_BOT_TOKEN=your-bot-token-from-botfather
PORT=3000
NODE_ENV=production
```

### Frontend

```env
VITE_API_URL=https://your-backend-domain.com/api
```

---

## 3. Recommended Stack (Easiest Path)

| Component     | Platform          | Why                          |
|---------------|-------------------|------------------------------|
| Frontend      | **Vercel**        | Free, fast, perfect for Vite |
| Backend       | **Railway**       | Easy NestJS + Postgres       |
| Database      | Railway Postgres  | Same platform, simple        |

Alternative good options: Render, Fly.io, DigitalOcean App Platform.

---

## 4. Deploy Backend on Railway

1. Go to [railway.app](https://railway.app) and sign in with GitHub
2. Click **New Project** → **Deploy from GitHub repo**
3. Select your repository (or upload the `backend` folder)
4. Add a **PostgreSQL** plugin to the same project
5. In the backend service → **Variables**, add:

   ```
   DATABASE_URL=${{Postgres.DATABASE_URL}}
   JWT_SECRET=your-long-random-secret
   TELEGRAM_BOT_TOKEN=your-bot-token
   PORT=3000
   NODE_ENV=production
   ```

6. Set the **Root Directory** to `backend` (if monorepo)
7. Build command: `npm install && npx prisma generate && npm run build`
8. Start command: `npx prisma migrate deploy && node dist/main`
9. Deploy

Railway will give you a public URL like `https://all-market-backend.up.railway.app`

---

## 5. Deploy Frontend on Vercel

1. Go to [vercel.com](https://vercel.com) and import your GitHub repo
2. Set **Root Directory** to `frontend`
3. Framework Preset: Vite
4. Add Environment Variable:

   ```
   VITE_API_URL=https://your-railway-backend-url.up.railway.app/api
   ```

5. Deploy

Vercel will give you a URL like `https://all-market-ethiopia.vercel.app`

---

## 6. Connect Mini App to Telegram

1. Go back to BotFather
2. Use `/myapps` → select your bot → select the app
3. Set / edit the **Web App URL** to your Vercel frontend URL
4. Optionally set a menu button:

   ```
   /setmenubutton
   ```
   Choose your bot → set button text to “Open Market” and point it to the Mini App

---

## 7. Local Development with Docker

```bash
# Start Postgres
docker compose up -d

# Backend
cd backend
cp .env.example .env
# DATABASE_URL="postgresql://allmarket:allmarket123@localhost:5432/all_market_ethiopia"
npm install
npx prisma migrate dev
npm run start:dev

# Frontend (new terminal)
cd frontend
cp .env.example .env
npm install
npm run dev
```

For testing the Mini App locally, use a tunnel:

```bash
npx localtunnel --port 5173
# or
ngrok http 5173
```

Then temporarily set that URL in BotFather.

---

## 8. Production Checklist

- [ ] `JWT_SECRET` is long and random
- [ ] `TELEGRAM_BOT_TOKEN` is correct
- [ ] Frontend `VITE_API_URL` points to the live backend
- [ ] CORS is enabled on the backend (already configured)
- [ ] Database migrations have been applied (`prisma migrate deploy`)
- [ ] Mini App URL in BotFather points to the live frontend
- [ ] Test: open the bot → open Mini App → complete profile → post a listing → search → message seller

---

## 9. Common Issues

**“Invalid Telegram authentication data”**  
→ Make sure `TELEGRAM_BOT_TOKEN` matches the bot that owns the Mini App.

**CORS errors**  
→ Backend already has `origin: true`. If still failing, restrict it to your frontend domain in production.

**Database connection failed**  
→ Check `DATABASE_URL` format and that the Postgres instance is running.

**Blank screen in Telegram**  
→ Open the Mini App URL directly in a browser first and check the console. Make sure `VITE_API_URL` is correct.

---

You’re ready to launch.

After the core is live, the next valuable additions are:
1. Real image upload (Cloudflare R2 or similar)
2. Credit system before posting
3. Admin moderation panel
