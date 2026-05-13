# 🌍 WinWin.travel MCP Server

![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-blue)
![Hosted](https://img.shields.io/badge/Deployed-Hosted%20%E2%9C%94-green)
![Price](https://img.shields.io/badge/Price-Free-brightgreen)
![Cashback](https://img.shields.io/badge/Cashback-Up%20to%2010%25-orange)

> The official **WinWin.travel Model Context Protocol (MCP) server**.  
> Connect AI agents directly to a massive inventory of **3M+ hotels** with real-time availability and booking capabilities.

---

## 📌 Overview

WinWin.travel MCP is an enterprise-grade, fully hosted MCP server that allows AI agents (Claude, custom agents, etc.) to:

- Search structured hotel inventory
- Retrieve filter metadata
- Create reservations
- Track reservation status
- Validate authentication
- Run diagnostics

Unlike scraper-based travel integrations, this MCP connects directly to WinWin.travel’s official commercial inventory.

---

## 🚀 Why Use WinWin.travel MCP?

- **🏢 3M+ Hotel Inventory** — Direct access to one of the world’s largest hotel databases.
- **⚡ Fully Hosted** — No Docker, no local setup, no tunneling required.
- **💰 Cashback Model** — Earn up to 10% cashback on bookings made through your integration.
- **🧠 Structured Search** — Use official filtering parameters and validated technical criteria.
- **🔐 Secure Access** — Token-based MCP authentication.

---

## 🛠 Quick Start

### 1️⃣ Get Your Access Token

You need a secure MCP token to connect:

👉 https://tally.so/r/GxdpeO

---

### 2️⃣ Configure Claude Desktop

Add the following to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "winwin-travel": {
      "url": "https://sandbox.api.winwin.travel/mcp/sse",
      "headers": {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN_HERE"
      }
    }
  }
}
```

Restart Claude after saving the configuration.

---

## 🧰 Available Tools

| Tool | Description |
|------|------------|
| **`search`** | Searches for available hotels and room offers by destination, stay dates, guest composition, and optional technical filters. |
| **`get_filters`** | Returns a complete directory of available parameters and criteria for filtering commercial offers. Useful for building structured and validated search requests. |
| **`create_reservation`** | Executes a hotel booking transaction based on the selected offer and generates a payment link. ⚠️ **Planned for refactoring** — this tool may not function reliably in its current version. |
| **`get_reservation_status`** | Returns the current status of an existing reservation by its unique identifier. The reservation unique identifier is returned after successful reservation creation. |
| **`echo`** | Returns the provided test string without changes. Useful for diagnostics and connection checks. |
| **`test_mcp_authentication`** | Tests MCP server authentication by calling a protected backend endpoint requiring the `TOURIST` role. This tool verifies that user authentication via MCP token is processed successfully. |

---

## 🧪 Example Prompts

Once connected, your AI agent can handle structured requests such as:

- “Search hotels in Tokyo for 2 adults from June 10 to June 15 under $200 per night.”
- “Show available hotel offers in New York with free cancellation.”
- “Book the second offer and generate a payment link.”
- “Check the status of my reservation using its ID.”
- “Test whether my MCP authentication token works.”

---

## 🔐 Authentication & Security

- Access is secured via **Bearer MCP token authentication**
- All calls are made to WinWin.travel’s official API gateway
- `test_mcp_authentication` can be used to validate proper token handling
- Reservation status checks require a valid reservation identifier returned at booking time

---

## ⚠️ Known Limitations

- `create_reservation` is currently scheduled for refactoring and may experience instability.
- Sandbox environment behavior may differ from production deployment.

---

## 📧 Support

For technical questions or integration support:

mcp@winwin.travel

---

## 📄 License

Proprietary — WinWin.travel MCP Server  
All rights reserved.

---

<p align="center">
Built with ❤️ by the WinWin.travel Team
</p>
