# Setup Guide: German Equity Research Agent

This guide walks you through setting up the autonomous research agent in your n8n environment.

## 1. Import the Workflow

1. Open your n8n workspace.
2. Go to **Workflows -> Add Workflow**.
3. In the top right menu, click **Import from File**.
4. Select `workflows/german_equity_research_agent.json` from this repository.

## 2. Configure Credentials

The imported workflow nodes contain placeholder credential mappings. You need to create the actual credentials in n8n and link them to the nodes:

### Google Sheets
* **Nodes Affected:** `Read Tickers from Sheets`, `Write to Sheets`
* **Action:** Open these nodes and select or create your **Google Sheets OAuth2 API** credential. Ensure your Google account has editor access to the spreadsheet you plan to use.

### AI Model (The "Brain")
* **Node Affected:** `LLM Brain`
* **Action:** By default, it uses a Google Gemini node placeholder. Select your desired LLM provider (e.g., Anthropic, OpenAI, or Gemini) and provide the relevant API Key via n8n's credential manager.

### Tavily Search
* **Node Affected:** `Tavily Search Tool`
* **Action:** Create an `Header Auth` credential named **Tavily API Key**:
  * Name: `Authorization`
  * Value: `Bearer YOUR_TAVILY_API_KEY`
  * *Note: Alternatively, ensure your n8n instance has `$vars.TAVILY_API_KEY` set.*

### Brave Search
* **Node Affected:** `Brave Search Tool`
* **Action:** Create an `Header Auth` credential named **Brave API Key**:
  * Name: `X-Subscription-Token`
  * Value: `YOUR_BRAVE_API_KEY`

*Note: Yahoo Finance and Jina Reader tools do not require API keys for basic usage.*

## 3. Prepare Your Data

Create a Google Sheet with at least two columns:
1. `ticker`: The stock ticker. (e.g., `SAP.DE` for SAP).
2. `focus`: The research directive (e.g., "Analyze recent dividend growth" or "Focus on Q3 ESG complaints").

Update the Google Sheets nodes in n8n to point to your specific Spreadsheet ID and the correct Sheet names.

## 4. Run and Monitor

* Click **Execute Workflow**.
* Monitor the execution. If an article read via Jina Reader truncates (due to 2000-token limits), the LLM agent will handle the partial text as best as it can.
* Review the final structured data written back to your spreadsheet.
