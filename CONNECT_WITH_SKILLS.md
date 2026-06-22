# Connect with Skills — WinWin.travel MCP

> **This guide walks you through connecting WinWin.travel MCP with a Claude Skill installed.**
> Skills extend Claude with reusable, domain-specific instructions — meaning Claude will automatically apply hotel search best practices every time you plan a trip.

## What Are Skills?

Skills extend Claude with reusable domain-specific instructions and best practices. Once installed, Claude automatically applies the appropriate skill when it is relevant to the user's request — no extra prompting needed.

---

## 🛠️ Quick Start

### 1. Get Your Access Token
You need a secure token to connect.
👉 **[Generate Access Token](https://tally.so/r/GxdpeO)**

### 2. Configure Claude Desktop
Add following code snippet to your existing json `claude_desktop_config.json`:

```json
{
  // ... your existing settings

  "mcpServers": {
    "WinWin.travel": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.winwin.travel/mcp/sse",
        "--header",
        "Authorization:${AUTH_HEADER}"
      ],
      "env": {
        "AUTH_HEADER": "Bearer YOUR_ACCESS_TOKEN_HERE"
      }
    }
  }

  // ... your existing settings continue
}
```

> [!IMPORTANT]
> Replace `YOUR_ACCESS_TOKEN_HERE` with the full token you received in Step 1, then **fully quit and relaunch Claude Desktop** — closing the window is not enough.
> 
---

## Install the Skill

### Step 1 — Download the Skill

1. Navigate to the **Releases** section of this repository.
2. Click the name of the skill — you will be redirected to the skill's page.
3. In the **Assets** section, download `search-offers-skill.zip` by clicking on its name.

### Step 2 — Add the Skill to Claude

1. Open **Claude**.
2. Navigate to **Customize → Skills**.
3. On the **Skills** list, click the **+** sign, then **Create skill → Upload a skill**.
4. Upload the `.zip` archive downloaded from this repository containing a `SKILL.md` file.
5. After the upload completes, the skill will appear in your list of available skills.

---

← [Back to Main README](README.md)
