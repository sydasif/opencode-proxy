# 🚀 LiteLLM Proxy

[![LiteLLM](https://img.shields.io/badge/Powered%20by-LiteLLM-blueviolet?style=for-the-badge)](https://github.com/BerriAI/litellm)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A high-performance [LiteLLM](https://github.com/BerriAI/litellm) proxy that seamlessly maps OpenCode, Claude Code CLI and Zed editor to Google's Gemini models. This allows you to use the power and cost-efficiency of Gemini Gemma 4 models.

---

## ✨ Features

- 🐳 **Docker Native**: Official LiteLLM image, no build step — instant deployment.
- ⚙️ **Customizable**: Separate configs per service via isolated subdirectories.
- 🔐 **Secure**: Environment-based API key management with per-service isolation.
- 🔀 **Multi-Service**: Run OpenCode/Claude and Zed proxies side-by-side with different keys and models.

---

## 📂 Project Structure

```
litellm-proxy/
├── docker-compose.yml
├── .gitignore
├── AGENTS.md
├── README.md
├── opencode/
│   ├── config.yaml       # OpenCode model mappings
│   └── .env              # OpenCode Gemini API key
└── zed/
    ├── config.yaml       # Zed model mappings
    └── .env              # Zed Gemini API key
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
cp opencode/.env.example opencode/.env
cp zed/.env.example zed/.env
# Edit both .env files and add your GEMINI_API_KEY values
```

### 2. Deploy Both Proxies

```bash
docker compose up -d
```

- OpenCode/Claude proxy is now running at `http://localhost:4000`
- Zed proxy is now running at `http://localhost:4001`

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

## 🤖 Using with Zed editor

To redirect `zed` editor to your local proxy, update your global `settings.json` file:

```json
{
  "language_models": {
    "openai_compatible": {
      "LiteLLM": {
        "api_url": "http://localhost:4001/v1",
        "available_models": [
          {
            "name": "gemma-4-31b-it",
            "max_tokens": 262144,
            "max_output_tokens": 32768,
            "max_completion_tokens": 262144,
            "capabilities": {
              "tools": true,
              "images": true,
              "parallel_tool_calls": false,
              "prompt_cache_key": false,
              "chat_completions": true
            }
          },
          {
            "name": "gemma-4-26b-a4b-it",
            "max_tokens": 262144,
            "max_output_tokens": 32768,
            "max_completion_tokens": 262144,
            "capabilities": {
              "tools": true,
              "images": true,
              "parallel_tool_calls": false,
              "prompt_cache_key": false,
              "chat_completions": true
            }
          }
        ]
      }
    }
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
| **View Logs (Zed)**         | `docker compose logs -f zed-proxy`            |
| **Restart**                 | `docker compose restart`                      |
| **Update Image**            | `docker compose pull && docker compose up -d` |
| **Health Check (OpenCode)** | `curl http://localhost:4000/health`           |
| **Health Check (Zed)**      | `curl http://localhost:4001/health`           |

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details (or MIT if not provided).

---

<p align="center">Made with ❤️ for the AI community</p>
