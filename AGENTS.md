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

## Python Optimization Settings

### Commands
- **Install & lint**: `uv sync && uv run ruff check --fix . && uv run ruff format .`
- **Type check**: `uv run mypy src/`
- **Test**: `uv run pytest && uv run pytest --cov=src --cov-branch --cov-fail-under=90`
- **Security scan**: `uv run safety check && uv run bandit -r src/`

### Coverage Thresholds
- **≥95%**: Business logic (src/services/, src/core/)
- **≥90%**: APIs (src/api/, src/routes/)
- **≥85%**: Models (src/models/)

### Pre-commit Hooks
Install via: `uv run pre-commit install`
Update hooks: `uv run pre-commit autoupdate`

### Branch Protection (Manual)
- Go to: Settings → Branches → Add rule for `main`
- Enable: Require pull request reviews + Require status checks to pass
