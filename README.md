# 🌍 WinWin.travel MCP Server

![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-blue) ![Hosted](https://img.shields.io/badge/Deployed-Hosted%20%E2%9C%94-green) ![Cost](https://img.shields.io/badge/Price-Free-brightgreen) ![Cashback](https://img.shields.io/badge/Cashback-Up%20to%2010%25-orange)

> **The official WinWin.travel Model Context Protocol server.**
> Connect your AI agents directly to a massive inventory of **3 Million+ Hotels** with real-time booking capabilities.

---

## 🚀 Why use this MCP?

Most travel MCPs are just scrapers. **WinWin.travel** is an official enterprise gateway.

*   **🏢 3M+ Inventory:** Access one of the world's largest hotel databases directly.
*   **⚡ Zero Setup:** No Docker, no `npm install`, no localhost tunneling. It's fully hosted.
*   **💰 Earn While You Build:** We pay *you* for usage. Get up to **10% cashback** on every booking made through your agent.
*   **🧠 Smarter Search:** Filter by 500+ attributes (amenities, vibes, policies) not just price/location.

## 🗣️ Example Prompts

Once connected, your AI (Claude, etc.) can handle complex travel requests:

*   *"Find me a boutique hotel in Tokyo, Shibuya district, under $200/night that allows dogs (check **pet fees**) and has **blackout curtains** for jet lag."*
*   *"Check availability for the Marriott Marquis in NY. I need a room with a **rain shower** and a **city view**."*
*   *"What are the trending hotels in Bali right now? Filter for ones with a **pillow menu** and **disability-friendly bed height**."*
*   *"Book the second option for 2 adults. Ensure there's a dedicated **pet station** request attached, and send the payment link to my email."*

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
> **Restart Claude after saving the configuration.**

---

## 🧰 Capabilities

| Tool | Description |
|------|------------|
| **`search`** | Searches for available hotels and room offers by destination, stay dates, guest composition, and optional technical filters. |
| **`get_filters`** | Returns a complete directory of available parameters and criteria for filtering commercial offers. Useful for building structured and validated search requests. |
| **`create_reservation`** | Executes a hotel booking transaction based on the selected offer and generates a payment link. ⚠️ **Planned for refactoring** — this tool may not function reliably in its current version. |
| **`get_reservation_status`** | Returns the current status of an existing reservation by its unique identifier. The reservation unique identifier is returned after successful reservation creation. |
| **`echo`** | Returns the provided test string without changes. Useful for diagnostics and connection checks. |
| **`test_mcp_authentication`** | Tests MCP server authentication by calling a protected backend endpoint requiring the **authentication**. This tool verifies that user authentication via MCP token is processed successfully. |

## 🔒 Security & Privacy

*   **Official Endpoint:** You are connecting directly to WinWin.travel's secure API gateway.
*   **Data Usage:** Your preferences are used to improve your booking experience. We do not sell your data to third parties.

---

<p align="center">
  Built with ❤️ by the WinWin.travel Team<br>
  <a href="mailto:mcp@winwin.travel">mcp@winwin.travel</a>
</p>
