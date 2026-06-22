# Quick Connect — WinWin.travel MCP

> **This is the fastest way to connect WinWin.travel to Claude.**
> Two steps: get a token, paste a config snippet. You'll be searching 3M+ hotels in minutes.

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

---

← [Back to Main README](README.md)
