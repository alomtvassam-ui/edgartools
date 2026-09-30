# Base44 Setup — EdgarTools

## What This Project Is

EdgarTools (`edgartools` v5.30.2) is a **Python library** for accessing SEC EDGAR filings — not a web application. There is no frontend or backend web server in the traditional sense.

## How It Runs In The Preview

Since this is a library (not a web app), the preview serves the **mkdocs documentation site** on port 3000.

- `docker-compose.base44.yml` runs a `python:3.12-slim` container that installs mkdocs + mkdocs-material on startup and runs `mkdocs serve --dev-addr 0.0.0.0:3000`.
- The repo is bind-mounted at `/app`, so edits to `docs/` or `mkdocs.yml` are picked up by mkdocs' live-reload watcher.
- No external credentials are needed for the docs site. The library itself uses `EDGAR_IDENTITY` for SEC API requests, but that is only needed when running the library or its tests — not for serving docs.

## Key Commands

- Start: `docker compose -f docker-compose.base44.yml up -d`
- Stop: `docker compose -f docker-compose.base44.yml down`
- Logs: `docker compose -f docker-compose.base44.yml logs docs`
- Health: `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/`

## Project Structure Notes

- `edgar/` — the Python package (entry point: `from edgar import Company, Filing, find`)
- `edgar/ai/mcp/server.py` — MCP server for AI agents (stdio-based, not a web server); console script `edgartools-mcp`
- `docs/` — mkdocs-material documentation source
- `mkdocs.yml` — mkdocs config (no mkdocstrings plugin; API ref pages are hand-written markdown)
- `pyproject.toml` — Hatch-based build; Python >=3.10
- Tests use pytest with markers (fast/slow/network/regression); network tests hit the live SEC EDGAR API
