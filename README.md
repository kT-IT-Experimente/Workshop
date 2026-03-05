# Workshop-n8n

A curated collection of **n8n automation workflows** for the Antigravity environment.

## 📁 Repository Structure

```
Workshop-n8n/
├── workflows/        # n8n workflow JSON files (import directly into n8n)
├── docs/             # Architecture diagrams and setup guides
├── .env.example      # Template for required environment variables
└── README.md
```

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<YOUR_USERNAME>/Workshop-n8n.git
cd Workshop-n8n
```

### 2. Set up environment variables
```bash
cp .env.example .env
# Edit .env with your actual credentials (never commit this file!)
```

### 3. Import a workflow into n8n
1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Select any `.json` file from the `workflows/` folder

## 🔐 Authentication Pattern

All workflows use **n8n Credentials** — no secrets are hardcoded in JSON files.

| Variable Reference | Purpose |
|---|---|
| `{{ $vars.WEBHOOK_SECRET }}` | Webhook signature validation |
| `{{ $vars.API_BASE_URL }}`   | Target API base URL |

Credentials are referenced by name inside node properties (e.g., `"headerAuth": "API_ACCESS"`).

## 📬 Workflow Catalog

| File | Trigger | Description |
|---|---|---|
| `german_equity_research_agent.json` | Manual | Autonomous agent that researches German equities using Yahoo Finance, Jina Reader, and Web Search based on a Google Sheet input focus. |

## 🛠 Development Notes

- Python Code Nodes must return: `return [{"json": {"key": value}}]`
- Use `{{ $vars.SECRET_NAME }}` for all environment-specific values
- Test webhooks locally with [ngrok](https://ngrok.com) or n8n's built-in tunnel

## 📄 License

MIT
