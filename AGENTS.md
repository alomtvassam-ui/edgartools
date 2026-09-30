# Base44 Dev Environment

## Project Overview
EdgarTools — a Python library for accessing SEC EDGAR filings. Not a web app; the preview serves the mkdocs documentation site on port 3000.

## Running the Project
```bash
docker compose -f docker-compose.base44.yml up -d
```
- Serves mkdocs-material docs at http://localhost:3000
- Live reload enabled via `mkdocs serve --watch`
- Dependencies (mkdocs, mkdocs-material, pymdown-extensions) install on container startup

## Architecture
- **Base image:** `python:3.12-slim` (plain runtime, not a prebuilt app image)
- **Source:** bind-mounted at `/app`
- **Docs config:** `mkdocs.yml` at repo root; doc files in `docs/`
- **No database or external services required** for the docs preview

## Library (not used by preview, but available)
- Install: `pip install -e ".[ai]"` (includes MCP server, starlette, uvicorn)
- MCP server: `edgartools-mcp --transport streamable-http --port 8000`
- Tests: `hatch run test-fast` (fast, no network) / `hatch run test-network` (SEC API)
- Requires `EDGAR_IDENTITY` env var for SEC API access (set in compose as a default)

## Verification
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → 200
- `docker compose -f docker-compose.base44.yml ps` → healthy
- Preview screenshot shows the EdgarTools docs landing page

## Known Warnings (non-blocking)
mkdocs build emits warnings about broken intra-doc links (missing target .md files). These are pre-existing in the repo and do not prevent the site from serving.
