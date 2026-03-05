# Workflow Architecture Guide

## Core Patterns

### 1. Trigger → Transform → Act

Every workflow follows this 3-stage pattern:

```
[Trigger]  →  [Transform / Logic]  →  [Action / Output]
Webhook        Code Node (Python)       HTTP Request / Email / Sheet
Schedule       Set / Merge Nodes        Database / Notification
```

---

### 2. Authentication Pattern

Never hardcode credentials in workflow JSON. Always use:

- **n8n Credentials** (preferred): Create in n8n UI under Settings → Credentials
- **n8n Variables** (for non-secret config): Referenced as `{{ $vars.VAR_NAME }}`

---

### 3. Python Code Node Template

All Code Nodes using Python must return a list of dicts:

```python
# Access input data
items = $input.all()

results = []
for item in items:
    data = item.json
    # ... your logic here ...
    results.append({"json": {"processed": data}})

return results
```

---

### 4. Error Handling Pattern

Add an **Error Trigger** node connected to a notification action (Slack/Email) for production flows:

```
[Error Trigger] → [Format Error Message] → [Send Slack / Email Alert]
```

---

### 5. State Logging (Heartbeat Pattern)

For long or critical flows, add checkpoint nodes:

```python
# Heartbeat payload
return [{"json": {
    "status": "running",
    "node": "ProcessOrders",
    "timestamp": $now.toISO(),
    "record_count": len(items)
}}]
```

Send this to a Google Sheet or monitoring endpoint at critical junctions.

---

## Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Workflow files | `snake_case.json` | `session_ics_emailer.json` |
| Webhook paths | `/kebab-case` | `/session-confirmed` |
| Credential names | `SCREAMING_SNAKE` | `GMAIL_OAUTH`, `API_ACCESS` |
| Node names | `Title Case` | `Format ICS File`, `Send Email` |
