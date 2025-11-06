# 🧭 NYC Mayor Analysis — Web (Next.js)

This directory contains the **Next.js 14** application that powers the newsroom frontend and lightweight backend API routes.  
It was originally bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

---

## ⚙️ Tech Stack

- **Framework:** [Next.js 14 (App Router)](https://nextjs.org)
- **Language:** TypeScript
- **Styling:** TailwindCSS
- **Mapping:** Mapbox GL JS + Deck.gl + PMTiles
- **Data Source:** FastAPI backend (`http://fastapi:8001`)
- **Package Manager:** pnpm

---

## 🏗️ Project Structure

```
apps/web/
├─ app/
│  ├─ layout.tsx              # Root layout (with suppressHydrationWarning)
│  ├─ page.tsx                # Home page
│  ├─ dashboard/
│  │  ├─ map/
│  │  │  ├─ page.tsx          # Map visualization page
│  │  │  └─ MapView.tsx       # Client component with Mapbox map
│  │  └─ test-api/page.tsx    # Example API fetch page
│  └─ api/
│     └─ geo/sample/route.ts  # Next.js backend route proxying to FastAPI
├─ lib/
│  ├─ map/deckLayers.ts       # Deck.gl layer definitions
│  └─ map/pmtiles.ts          # PMTiles protocol registration
├─ Dockerfile
├─ .dockerignore
├─ .env.local                 # Local Mapbox token
└─ package.json
```

---

## 🚀 Getting Started

### 1️⃣ Run locally (no Docker)
```bash
cd apps/web
pnpm install
pnpm dev
```

Then open [http://localhost:3000](http://localhost:3000) with your browser.  
Make sure the FastAPI backend is running at [http://localhost:8001](http://localhost:8001).

You can start editing the main page in `app/page.tsx`. The app will auto-reload as you edit files.

### 2️⃣ Run inside Docker
Your `docker-compose.yml` handles all environment variables and builds automatically:

```yaml
web:
  build:
    context: ./apps/web
  environment:
    NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN: ${NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN}
    ANALYTICS_URL: http://fastapi:8001
```

Start both frontend and backend containers:
```bash
docker compose up --build
```

Visit:
- Frontend → http://localhost:3000  
- Backend (FastAPI) → http://localhost:8001/docs

---

## 🔁 API Proxy Example

The Next.js backend includes API routes that proxy requests to FastAPI.

**File:** `app/api/geo/sample/route.ts`
```ts
export async function GET() {
  const backendUrl = process.env.ANALYTICS_URL ?? "http://localhost:8001";
  const r = await fetch(`${backendUrl}/geo/sample`, { cache: "no-store" });
  const data = await r.json();
  return new Response(JSON.stringify(data), {
    headers: { "content-type": "application/json" }
  });
}
```

Example client-side usage:
```ts
fetch('/api/geo/sample')
  .then(r => r.json())
  .then(console.log);
```

---

## 🗺️ Map Integration

- Mapbox basemap: `mapbox://styles/mapbox/light-v11`
- Custom Deck.gl layers: `GeoJsonLayer`, `MVTLayer`, `ScatterplotLayer`
- PMTiles protocol registered via `lib/map/pmtiles.ts`
- Example dataset endpoint: `/api/geo/sample`

---

## 💡 Development Tips

- Keep Map components as **Client Components** (`'use client'`) to avoid SSR hydration mismatches.
- Set `suppressHydrationWarning` on `<html>` and `<body>` inside `layout.tsx` to silence Grammarly-related hydration warnings.
- Store your Mapbox token in `.env.local` or pass it through Docker environment variables.

---

## 📚 Learn More

To learn more about Ne
