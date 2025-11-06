# 🧮 NYC Mayor Analysis — FastAPI Backend

This directory contains the **Python 3.11 FastAPI** service responsible for analytics, data processing, and GeoJSON output.

---

## ⚙️ Tech Stack

- **Framework:** FastAPI
- **Language:** Python 3.11
- **Dependency Manager:** Poetry
- **Server:** Uvicorn (auto-reload)
- **Container:** Python:3.11-slim (Dockerfile)

---

## 🏗️ Structure

```
apps/py-analytics/
├─ py_analytics/
│  ├─ main.py                # FastAPI entrypoint
│  ├─ routers/
│  │  └─ geo.py              # Example GeoJSON endpoint
│  ├─ services/              # Place data/analysis logic here
│  └─ domain/                # Pydantic models & schemas
├─ pyproject.toml            # Poetry config
├─ Dockerfile
└─ .dockerignore
```

---

## 🔌 API Example

**`/geo/sample`**
```python
@router.get("/sample")
def sample():
    return {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {"name": "Example Point"},
          "geometry": {"type": "Point", "coordinates": [-122.33, 47.61]},
        }
      ],
    }
```

### Test in browser
```
http://localhost:8001/geo/sample
```

### OpenAPI Docs
```
http://localhost:8001/docs
```

---

## 🐳 Run with Docker

Builds automatically via `docker-compose.yml`.

If you want to run locally:
```bash
cd apps/py-analytics
poetry install
poetry run uvicorn py_analytics.main:app --reload --port 8001
```

---

## 🧩 Integration with Next.js

- Next.js API routes proxy to this backend using the internal service URL (`http://fastapi:8001`).
- You can add new routes under `py_analytics/routers/`.
- For larger datasets, serve pre-generated PMTiles or GeoJSON files via S3 or local volume.

---

## 🧱 Add Dependencies

Edit `pyproject.toml`, then:
```bash
poetry add <package>
poetry lock
```

Rebuild the Docker image:
```bash
docker compose build fastapi
```

---

## 💡 Future Extensions

- Implement `/stats/fit` for model fitting
- Add `/tiles/build` for PMTiles generation
- Integrate Celery or Dramatiq for background jobs
- Add Pandera data validation schemas

