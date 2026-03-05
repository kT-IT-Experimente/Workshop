# SYSTEM

**Project**: Workshop-n8n (Agentic AI Workflows)
**Purpose**: This repository is the definitive artifact output collection for Antigravity-assisted n8n pipeline orchestration. It serves as a central hub for complex integrations, specifically focusing on agentic patterns over rigid deterministic ones.

## 1. System Tenets

- **Agentic Over Linear**: Wherever possible, we use an LLM "Brain" to parse inputs and decide which tools (HTTP endpoints, data transformers) to call dynamically.
- **Secure by Default**: Credentials (`TAVILY_API_KEY`, `BRAVE_API_KEY`, `GOOGLE_SHEETS_OAUTH`) are *never* stored directly in json node exports. They must map to native n8n variables (`{{ $vars.* }}`) or n8n internal credential objects.
- **Read-Process-Write Loop**: We favor idempotency. Workflows should fetch data (e.g., from Sheets), process it robustly handling rate limits, and write the augmented data back out safely.

## 2. Directory Map

- `/workflows/`: Contains raw `.json` maps exported from n8n. Import these directly into any n8n instance.
- `/docs/`: Implementation guides, setup parameters, and troubleshooting for the complex agent workflows.

## 3. Deployment

Refer to the specific Setup Guides in `/docs` for how to attach the necessary API keys into n8n and bind them to the node credentials before execution.
