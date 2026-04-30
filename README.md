# 🚀 Opencode Proxy

[![LiteLLM](https://img.shields.io/badge/Powered%20by-LiteLLM-blueviolet?style=for-the-badge)](https://github.com/BerriAI/litellm)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A high-performance [LiteLLM](https://github.com/BerriAI/litellm) proxy that seamlessly maps OpenCode and Claude Code CLI to Google's Gemini models. This allows you to use the power and cost-efficiency of Gemini Gemma 4 models.

---

## ✨ Features

- 🐳 **Docker Native**: Ready-to-use Docker environment for instant deployment.
- ⚙️ **Customizable**: Easily modify model mappings and settings via `config.yaml`.
- 🔐 **Secure**: Environment-based API key management.
- ⚡ **Lightweight**: Built on Python 3.12-slim and LiteLLM for minimal overhead.

---

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [Docker Desktop](https://docs.docker.com/get-docker/) or Docker Engine
- [Docker Compose](https://docs.docker.com/compose/install/)
- A **Gemini API Key** from [Google AI Studio](https://aistudio.google.com/)

---

## 🚀 Quick Start

### 1. Clone & Enter

```bash
git clone <repository-url>
cd opencode-proxy
```

### 2. Configure Environment

```bash
cp .env.example .env
# Open .env and add your GEMINI_API_KEY
```

### 3. Deploy

```bash
docker compose up -d --build
```

The proxy is now running at `http://localhost:4000`.

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
    "ANTHROPIC_BASE_URL": "http://localhost:4000/",
    "ANTHROPIC_AUTH_TOKEN": "sk-xxx",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "gemma-4-31b-it",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "gemini-3.1-flash-lite-preview",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "gemma-4-26b-a4b-it"
  }
}
```

---

## 🔧 Docker Customization

If you need to change the default port (e.g., due to a conflict), you can modify the `ports` section in `docker-compose.yml`:

```yaml
services:
  gemini-proxy:
    ...
    ports:
      - "8080:4000" # Maps port 8080 on your host to 4000 in the container
    ...
```

After changing the port, restart the proxy:

```bash
docker compose down
docker compose up -d
```

---

| Action          | Command                  |
| :-------------- | :----------------------- |
| **Start Proxy** | `docker compose up -d`   |
| **Stop Proxy**  | `docker compose down`    |
| **View Logs**   | `docker compose logs -f` |
| **Restart**     | `docker compose restart` |

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details (or MIT if not provided).

---

<p align="center">Made with ❤️ for the AI community</p>
