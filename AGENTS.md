# AGENTS.md - Opencode Proxy

## Purpose

LiteLLM proxy that maps OpenCode CLI to Google Gemini models.

## Operational Commands

- **Build & Start**: `docker compose up -d --build`
- **Stop**: `docker compose down`
- **View Logs**: `docker compose logs -f`
- **Restart**: `docker compose restart`

## Configuration

- **Model Mappings**: Defined in `config.yaml`.
- **API Keys**: Managed via `.env` file (requires `GEMINI_API_KEY`).
- **Proxy Endpoint**: `http://localhost:4000/v1`
