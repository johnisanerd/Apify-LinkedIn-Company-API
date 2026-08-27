# 🏢 LinkedIn Company API: Firmographics to Structured JSON

> The most efficient, reliable, and developer-friendly way to use the LinkedIn Company API.

**Actor page:** [apify.com/johnvc/linkedin-company-api](https://apify.com/johnvc/linkedin-company-api?fpr=9n7kx3)
**Input schema:** [apify.com/johnvc/linkedin-company-api/input-schema](https://apify.com/johnvc/linkedin-company-api/input-schema?fpr=9n7kx3)

Send one or many public LinkedIn company page URLs and get back one clean JSON row per company: name, industry, employee size, headquarters, followers, specialties, website, and a Crunchbase link. It is built API-first and MCP-ready, so you can call it from Python or drive it as a tool from an AI agent.

## Video Walkthrough

[![Watch the walkthrough](https://img.youtube.com/vi/jREWahDGhJM/maxresdefault.jpg)](https://www.youtube.com/watch?v=jREWahDGhJM)

## Quick Start

### Prerequisites
- Python 3.11 or higher
- An Apify account and API key ([get a free key here](https://apify.com?fpr=9n7kx3))

1. **Clone the repository**
   ```bash
   git clone https://github.com/johnisanerd/Apify-LinkedIn-Company-API.git
   cd Apify-LinkedIn-Company-API
   ```

2. **Install dependencies with UV**
   ```bash
   # Install UV if you do not have it:
   curl -LsSf https://astral.sh/uv/install.sh | sh

   # Install project dependencies:
   uv sync
   ```

3. **Configure your API key**
   ```bash
   cp .env.example .env
   # Edit .env and add your Apify API key
   # Get your free API key at: https://apify.com?fpr=9n7kx3
   ```

4. **Run the example**
   ```bash
   uv run python linkedin-company-api-example.py
   ```

### Alternative: set the API key directly
```bash
export APIFY_API_TOKEN="your_api_key_here"
uv run python linkedin-company-api-example.py
```

## Why Use This LinkedIn Company API?

**A URL in, structured data out.** You never touch collection infrastructure. Pass a public company URL and get flat, predictable firmographic fields you can load straight into a sheet, a database, or a CRM.

**Batch friendly.** Send up to 1000 company URLs in one run. They are collected in parallel and returned one row each, so enriching an account list is one call.

**Pay per company.** Billing is per company returned, with no per-run setup fee, so you only pay for what is delivered.

**Reliable and predictable.** Every company comes back with the same field shape, and a URL that cannot be collected returns a clear error row instead of failing the whole run.

**MCP-ready.** Call it as a tool from Claude, Cursor, and other AI agents (see the install sections below).

## Features

### Core Capabilities
- Collect one or many public LinkedIn company pages by URL
- Name, industry, employee size band, headquarters, and specialties
- Follower count, website, organization type, and a Crunchbase link
- Office locations and country presence, plus the company logo URL

### Data Quality
- One consistent JSON row per company, every time
- A plain-language `summary` field on every row for quick scanning and AI use
- Clear per-URL error rows so a single bad link never sinks the batch

## Usage Examples

### Basic Example
```json
{
  "companyUrls": ["https://www.linkedin.com/company/microsoft"]
}
```

### Batch Example (collected in parallel)
```json
{
  "companyUrls": [
    "https://www.linkedin.com/company/microsoft",
    "https://www.linkedin.com/company/google"
  ]
}
```

## Input Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `companyUrls` | `list[str]` | YES | - | One or more public LinkedIn `/company/` page URLs. Up to 1000 per run; non-`/company/` URLs are skipped. |

## Output Format

Each company is returned as one JSON row:

```json
{
  "result_type": "company",
  "name": "Microsoft",
  "industries": "Software Development",
  "companySize": "10,001+ employees",
  "employeesInLinkedin": 233717,
  "followers": 28561289,
  "headquarters": "Redmond, Washington",
  "specialties": ["Business Software", "Developer Tools", "Cloud Computing"],
  "organizationType": "Public Company",
  "website": "https://news.microsoft.com/",
  "crunchbaseUrl": "https://www.crunchbase.com/organization/microsoft",
  "companyId": "1035",
  "publicUrl": "https://www.linkedin.com/company/microsoft",
  "summary": "Microsoft, Software Development, 10,001+ employees, Redmond, Washington, 28,561,289 followers"
}
```

The `crunchbaseUrl` and `website` fields are returned when the company lists them.

---

## Install in Claude Cowork Desktop

![Install in Claude Cowork Desktop](https://raw.githubusercontent.com/johnisanerd/ApifyPublicData/main/assets/guides/install_mcp_into_claude_desktop.png)

Cowork is the desktop app's automation mode. To give it the LinkedIn Company API as a tool, add the Apify MCP server as a connector.

1. Open the Claude desktop app and go to **Settings → Connectors** (or **Settings → Developer → Edit Config** to edit `claude_desktop_config.json` directly).
   - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
2. Add the Apify MCP server, preloaded with only this Actor:

```json
{
  "mcpServers": {
    "apify": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api"
      ]
    }
  }
}
```

3. Restart the app. When Cowork first calls the tool, complete the OAuth prompt in your browser, or add your Apify API token in the connector settings to skip OAuth.
4. In a Cowork chat, confirm the tool is available and ask it to run the LinkedIn Company API.

Download the desktop app and start a free trial: https://claude.ai/referral/uIlpa7nPLg
More help: https://docs.apify.com/platform/integrations/claude-desktop

---

## Install in Claude Code

![Install in Claude Code](https://raw.githubusercontent.com/johnisanerd/ApifyPublicData/main/assets/guides/install_mcp_into_claude_code.png)

Claude Code is the command-line tool. Add the Actor's MCP server with one command:

```bash
claude mcp add --transport http apify \
  "https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api"
```

To use a token instead of browser OAuth:

```bash
claude mcp add --transport http apify \
  "https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api" \
  --header "Authorization: Bearer YOUR_APIFY_TOKEN"
```

Then verify with `claude mcp list`, or run `/mcp` inside a session. Ask Claude Code to call the LinkedIn Company API.

Try Claude Code free: https://claude.ai/referral/uIlpa7nPLg
Claude Code MCP docs: https://code.claude.com/docs/en/mcp

---

## Install in Claude (website)

![Install in Claude (website)](https://raw.githubusercontent.com/johnisanerd/ApifyPublicData/main/assets/guides/install_mcp_into_claude_ai.png)

On claude.ai you add Apify as a connector, then enable just this Actor's tool.

1. Go to **Settings → Connectors → Browse connectors** and search for **Apify MCP server**. Install it (enable or update if prompted).
2. When connecting, authenticate with your Apify API token, and enable the tool `johnvc/linkedin-company-api`.
3. In any chat, open **+ → Connectors** and turn on **Apify**.
4. Alternatively, choose **Add custom connector** and paste the full MCP URL `https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api`, using OAuth when prompted.
5. Ask Claude to run the LinkedIn Company API.

Open Claude on the web: https://claude.ai/referral/uIlpa7nPLg

---

## Install in Cursor

![Install in Cursor](https://raw.githubusercontent.com/johnisanerd/ApifyPublicData/main/assets/guides/install_mcp_into_cursor.png)

Cursor reads MCP servers from a project file at `.cursor/mcp.json`.

1. In your project, create `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "apify": {
      "url": "https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api"
    }
  }
}
```

2. If you prefer token auth over browser OAuth, add a header:

```json
{
  "mcpServers": {
    "apify": {
      "url": "https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api",
      "headers": { "Authorization": "Bearer YOUR_APIFY_TOKEN" }
    }
  }
}
```

3. Open **Cursor → Settings → MCP** and confirm the **apify** server is connected (green dot).
4. In Composer or Chat, ask Cursor to call the LinkedIn Company API.

New to Cursor? Get it here: https://cursor.com/referral?code=XQP4VBLI3NNX

---

## Install in ChatGPT

![Install in ChatGPT](https://raw.githubusercontent.com/johnisanerd/ApifyPublicData/main/assets/guides/install_mcp_into_ChatGPT.png)

ChatGPT connects to the Apify MCP server through Developer mode (available on ChatGPT Pro, Plus, Business, Enterprise, and Education plans).

1. Click your profile icon, then go to **Settings > Apps**. If you do not see a **Create app** button, open **Advanced settings** and enable **Developer mode**.
2. Click **Create app** and fill out the form:
   - **Name:** Apify
   - **MCP Server URL:** `https://mcp.apify.com/?tools=actors,docs,johnvc/linkedin-company-api`
   - **Authentication:** OAuth
3. Click **Create** and authorize the connection with Apify.
4. To use the app in a conversation, click **+** in the chat, choose **Developer mode**, and select **Apify**.

More help: https://docs.apify.com/platform/integrations/mcp

---

[**Made with care**](https://apify.com/johnvc?fpr=9n7kx3)

*Use the LinkedIn Company API to power your sales intelligence, account enrichment, and CRM workflows with reliable, structured results.*

Last Updated: 2026.08.27
