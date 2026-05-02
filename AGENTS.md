# AGENTS.md - Opencode Proxy

## Purpose

LiteLLM proxy that maps OpenCode CLI to Google Gemini models. Runs alongside a Zed proxy using the same official LiteLLM image with isolated configs and API keys.

## Operational Commands

- **Start All**: `docker compose up -d`
- **Stop All**: `docker compose down`
- **View Logs (All)**: `docker compose logs -f`
- **View Logs (OpenCode)**: `docker compose logs -f opencode-proxy`
- **View Logs (Zed)**: `docker compose logs -f zed-proxy`
- **Restart**: `docker compose restart`
- **Update Image**: `docker compose pull && docker compose up -d`

## Configuration

- **Model Mappings**: Defined in `opencode/config.yaml` and `zed/config.yaml`.
- **API Keys**: Managed via per-service `.env` files (`opencode/.env`, `zed/.env`), each requiring `GEMINI_API_KEY`.
- **Proxy Endpoints**: OpenCode → `http://localhost:4000/v1`, Zed → `http://localhost:4001/v1`

## Project Structure

- `opencode/` — OpenCode proxy config and environment
- `zed/` — Zed proxy config and environment
- `docker-compose.yml` — launches both services from the official LiteLLM image
