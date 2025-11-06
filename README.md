# 🗽 NYC Mayor Analysis Platform

A data-driven journalism platform built with **Next.js**, **FastAPI**, and **Docker**.  
It integrates interactive Mapbox maps, GeoJSON visualizations, and statistical models.

---

## 🚀 Architecture Overview

```
nyc-mayor-analysis/
├─ apps/
│  ├─ web/             → Next.js frontend + lightweight backend (Node)
│  └─ py-analytics/    → FastAPI backend for analytics & geo tasks (Python)
├─ docker-compose.yml  → orchestrates both services
├─ packages/           → optional workspace for shared libraries (currently empty)
└─ package.json        → root manifest for pnpm workspace
```

---

## 🧩 Stack Summary

| Layer | Tech | Role |
|-------|------|------|
| **Frontend + SSR + API routes** | Next.js (React, TypeScript) | UI, API proxy to FastAPI, Mapbox rendering |
| **Backend analytics service** | FastAPI (Python 3.11) | Geo/statistical analysis, heavy jobs |
| **Mapping** | Mapbox GL JS + Deck.gl + PMTiles | Visualization of OSM + custom data |
| **Container orchestration** | Docker Compose | Runs both services together |

---

## 🐳 Running with Docker

### 1️⃣ Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Mapbox account + access token](https://account.mapbox.com/)

### 2️⃣ Set up environment
Create a `.env` file at the repo root:
```bash
NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN=your_mapbox_token_here
ANALYTICS_URL=http://fastapi:8001
```

### 3️⃣ Build & start
```bash
docker compose up --build
```

- **Next.js app:** http://localhost:3000  
- **FastAPI docs:** http://localhost:8001/docs

---

## 🔁 Request Flow

```
Browser (React)
   ↓
Next.js API route (apps/web/app/api/geo/sample)
   ↓
FastAPI service (apps/py-analytics/geo/sample)
   ↓
JSON/GeoJSON response → rendered on Mapbox
```

---

## 🧱 Folder Breakdown

| Folder | Description |
|---------|--------------|
| `apps/web` | Next.js frontend & API routes |
| `apps/py-analytics` | FastAPI backend service |
| `packages/` | Reserved for shared TS/ESLint/config packages |
| `docker-compose.yml` | Builds both services & links them |
| `.env` | Holds environment variables (Mapbox token, backend URL) |

---

## 🧭 Development Notes

- Hot reload works for both containers (Next.js + FastAPI).
- Each service is isolated — Node deps are in the `web` image, Python deps in the `py-analytics` image.
- Use `suppressHydrationWarning` in `layout.tsx` to silence Grammarly/extension hydration mismatches.

---

## 📄 License
MIT License © 2025
