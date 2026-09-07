<div align="center">

# ATLAS // Industrial Knowledge Intelligence

**One database — zero hallucinations.** Atlas unifies fragmented enterprise silos
(SAP PM, Maximo, SCADA, P&ID PDFs) into a single, deterministic knowledge graph:
conflict-aware entity resolution, recursive-SQL multi-hop root-cause analysis, and
100% grounded citations.

UCS503P — Software Engineering Project · Thapar Institute of Engineering & Technology

</div>

---

## Status — Sprint 1

This repository demonstrates **Sprint 1** of the phased build plan: the product's
public face and its design system, running on the platform's data path.

| Delivered in this sprint | Where |
|---|---|
| **Public landing page** — hero, telemetry, capabilities, modules, connectors, deployment tiers, benchmark dispatches | `frontend/src/pages/Landing.tsx` |
| **ATLAS // TACTICAL TELEMETRY design system** — dark substrate, hazard-red accent, mono/Archivo type, zero-radius geometry, CRT scanlines, reduced-motion support | `frontend/src/index.css`, `frontend/src/pages/landing.css` |
| **Operational app** (dashboard, knowledge graph, copilot, workflows, connectors, assets, review) restyled to the same language | `frontend/src/pages/`, `frontend/src/components/` |
| **Backend platform** — FastAPI + asyncpg + pgvector, entity resolution, recursive-CTE graph, Docling ingestion, LangGraph workflows (Ask / RCA / Compliance) | `backend/app/` |

## Stack

| Tier | Technology |
|---|---|
| Frontend | Vite + React 19 + TypeScript, Tailwind CSS v4, d3-force-3d (canvas) |
| Backend | Python 3.13 · FastAPI (async, SSE streaming) |
| Database | PostgreSQL + pgvector (Supabase) — one database: records, vectors, graph, knowledge docs |
| AI | NVIDIA NIM (Nemotron) via LangChain / LangGraph — shared rate budget |
| Documents | Docling (PDF layout + tables) with a low-memory text fallback |

## Run it locally

**Prerequisites:** Docker Desktop, Python 3.13, Node 18+, NVIDIA NIM API key.

```powershell
# 1. Database — self-hosted Supabase (Postgres on :5433, Studio on :8000)
docker compose -f supabase/docker/docker-compose.yml up -d
#    then run backend/app/db/schema.sql in the Supabase SQL editor (first time only)

# 2. Backend — FastAPI on :8010
cd backend
py -3.13 -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
copy .env.example .env        # fill in DATABASE_URL + NVIDIA_API_KEY
.venv\Scripts\python.exe -m uvicorn app.main:app --host 127.0.0.1 --port 8010

# 3. Frontend — Vite on :5173
cd frontend
npm install
copy .env.example .env        # VITE_API_URL=http://localhost:8010
npm run dev
```

Then open **http://localhost:5173** — the landing page; **/home** is the operational
dashboard. Demo data: `demo\reset.bat` (needs the venv; honors `ATLAS_API`,
default `http://127.0.0.1:8010`), or add the JSON files in `demo/connectors/` from
the Connectors page and press Sync.

## Repository layout

```
backend/     FastAPI service — ingestion, resolution, graph, workflows, tests, eval
frontend/    Vite + React + TS SPA — landing page + operational app
demo/        Two-industry demo corpus (JSON connectors) + reset/seed script
deploy/      docker-compose + Caddy (VPS option)  ·  Dockerfile(.slim) for PaaS
```

## Security

Secrets live only in git-ignored `.env` files. Committed `.env.example` templates
contain placeholders — never commit real keys.

## License

[MIT](LICENSE)
