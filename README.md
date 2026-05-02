# 🚀 LiteLLM Proxy

[![LiteLLM](https://img.shields.io/badge/Powered%20by-LiteLLM-blueviolet?style=for-the-badge)](https://github.com/BerriAI/litellm)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-D97757?style=for-the-badge&logo=anthropic&logoColor=white)](https://claude.com/product/claude-code)
[![OpenCode](https://img.shields.io/badge/OpenCode-007ACC?style=for-the-badge)](https://opencode.ai)
[![Google Gemini](https://img.shields.io/badge/Backend-Google%20Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini/)
[![OpenAI Compatible](https://img.shields.io/badge/API-OpenAI%20Compatible-412991?style=for-the-badge&logo=openai&logoColor=white)](https://docs.litellm.ai/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A high-performance [LiteLLM](https://github.com/BerriAI/litellm) proxy that seamlessly maps `opencode` and `claude` code CLI to Google's `Gemma 4` models, allows you to use the power and cost-efficiency of `Gemma 4` models.

---

## ✨ Features

- 🐳 **Docker Native**: Official LiteLLM image, no build step — instant deployment.
- ⚙️ **Customizable**: Separate configs per service via isolated subdirectories.
- 🔐 **Secure**: Environment-based API key management with per-service isolation.
- 🔀 **Multi-Service**: Run OpenCode/Claude proxies side-by-side with different keys and models.

---

## 📂 Project Structure

```
litellm-proxy/
├── docker-compose.yml
├── .env              # API keys
├── .gitignore
├── AGENTS.md
├── README.md
├── opencode/
│   └── config.yaml       # OpenCode model mappings
└── claude/
    └── config.yaml       # Claude model mappings
```

---

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [Docker Desktop](https://docs.docker.com/get-docker/) or Docker Engine
- [Docker Compose](https://docs.docker.com/compose/install/)
- A **Gemini API Key** from [Google AI Studio](https://aistudio.google.com/)

---

## 🚀 Quick Start

### 1. Configure Environment

```bash
cp .env.example .env
# Edit .env and add your OPENCODE_GEMINI_API_KEY and CLAUDE_GEMINI_API_KEY values
```

### 2. Deploy Proxy

```bash
docker compose up -d
```

- OpenCode proxy is now running at `http://localhost:4000`
- Claude proxy is now running at `http://localhost:4001`

---

## 🤖 Using with OpenCode

To redirect `opencode` to your local proxy, update your global `opencode.json` file:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "litellm": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LiteLLM",
      "options": {
        "baseURL": "http://localhost:4000/v1"
      },
      "models": {
        "gemini/gemma-4-31b-it": {
          "name": "Gemma-4-31b"
        },
        "gemini/gemma-4-26b-a4b-it": {
          "name": "Gemma-4-26b"
        },
        "gemini/gemini-3.1-flash-lite-preview": {
          "name": "Gemini-3.1-flash-lite"
        }
      }
    }
  }
}
```

---

## 🤖 Using with Claude Code

To redirect `claude` to your local proxy, update your global `settings.json` file:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:4001/",
    "ANTHROPIC_AUTH_TOKEN": "sk-xxx",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "gemma-4-31b-it",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "gemini-3.1-flash-lite-preview",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "gemma-4-26b-a4b-it"
  }
}
```

---

## 🔧 Docker Customization

If you need to change the default port (e.g., due to a conflict), modify the `ports` section in `docker-compose.yml`:

```yaml
services:
  opencode-proxy:
    ports:
      - "8080:4000" # Maps port 8080 on your host to 4000 in the container
```

After changing the port, restart the proxy:

```bash
docker compose down
docker compose up -d
```

---

## 📋 Operational Commands

| Action                      | Command                                       |
| :-------------------------- | :-------------------------------------------- |
| **Start All**               | `docker compose up -d`                        |
| **Stop All**                | `docker compose down`                         |
| **View Logs (All)**         | `docker compose logs -f`                      |
| **View Logs (OpenCode)**    | `docker compose logs -f opencode-proxy`       |
| **View Logs (Claude)**      | `docker compose logs -f claude-proxy`         |
| **Restart**                 | `docker compose restart`                      |
| **Update Image**            | `docker compose pull && docker compose up -d` |
| **Health Check (OpenCode)** | `curl http://localhost:4000/health`           |
| **Health Check (Claude)**   | `curl http://localhost:4001/health`           |

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details (or MIT if not provided).

---

<p align="center">Made with ❤️ for the AI community</p>
