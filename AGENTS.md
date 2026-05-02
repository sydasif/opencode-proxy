# AGENTS.md

## What This Repo Is

- **LiteLLM proxy** - Docker containers that expose OpenCode and Claude code CLI APIs to Google's Gemma models
- **Not a Python codebase** - No source code to develop; config-driven Docker deployment

## Key Commands

```bash
# Start proxies
docker compose up -d

# View logs
docker compose logs -f opencode-proxy   # or: -f claude-proxy
docker compose logs -f              # all

# Restart/update
docker compose restart
docker compose pull && docker compose up -d

# Health checks
curl http://localhost:4000/health   # OpenCode proxy
curl http://localhost:4001/health    # Claude proxy
```

## Architecture

- `opencode-proxy` → runs on port 4000, config in `opencode/config.yaml`
- `claude-proxy` → runs on port 4001, config in `claude/config.yaml`
- Both use official `docker.litellm.ai/berriai/litellm:main-stable` image
- Environment variables loaded from `.env` (never commit this)

## Config Structure

Each `config.yaml` defines:
- `model_list` - available models and their LiteLLM mappings
- `litellm_params` - API key via `os.environ/VAR_NAME`, RPM limits
- `litellm_settings.drop_params` - drop unsupported params

Model naming: internal name (e.g., `gemma-4-31b-it`) maps to LiteLLM model (e.g., `gemini/gemma-4-31b-it`).

## Setup

```bash
cp .env.example .env
# Add required API keys:
# OPENCODE_GEMINI_API_KEY
# CLAUDE_GEMINI_API_KEY
```

## Changing Ports or Models

1. Edit `docker-compose.yml` for ports
2. Edit `opencode/config.yaml` or `claude/config.yaml` for models
3. Restart: `docker compose down && docker compose up -d`