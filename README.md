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
| **`search`** | Searches for available hotel room offers by destination, stay dates, guest composition, and optional filters — such as meal plans, cancellation policies, amenities (pool, spa), accessibility features, pet policies, and 500+ other criteria. |
| **`get_filters`** | Returns the full list of available filter options — amenities, meal plans, cancellation types, accessibility features, pet policies, and more. Use this before searching to discover valid filter values. |
| **`create_reservation`** | Creates a reservation for a hotel room offer selected from search results and returns a payment link. The booking is confirmed automatically once payment is completed. |
| **`get_reservation_status`** | Checks the current status of a reservation using the identifier returned by `create_reservation`. |
| **`echo`** | Returns the provided test string without changes. Useful for diagnostics and connection checks. |
| **`test_mcp_authentication`** | Verifies that your MCP token is valid and authentication is working correctly. |

## 🔒 Security & Privacy

*   **Official Endpoint:** You are connecting directly to WinWin.travel's secure API gateway.
*   **Data Usage:** Your preferences are used to improve your booking experience. We do not sell your data to third parties.

---

## 💳 Booking Flow

1. **Search** — your AI calls `search` and returns a list of available offers.
2. **Select** — you tell the AI which option to book.
3. **Reserve** — the AI calls `create_reservation` and receives a **payment link**.
4. **Pay** — you complete payment in your browser. Card data is never passed through the AI.
5. **Confirm** — booking is confirmed automatically. Use `get_reservation_status` to check anytime.

---

## ❓ FAQ

#### Which AI clients are supported besides Claude Desktop?
Any MCP-compatible client works. Clients like **Cursor** and **Windsurf** support remote MCP servers natively — no `npx` or Node.js needed. Just point them directly at `https://mcp.winwin.travel/mcp/sse` with your `Authorization: Bearer YOUR_TOKEN` header in your client's MCP settings.

#### Where is `claude_desktop_config.json` located?
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

#### How do I know the connection is working?
Ask your AI: *"Use the `test_mcp_authentication` tool to check if WinWin.travel is connected."* A successful response confirms your token is valid. You should also see **WinWin.travel** listed in your client's active MCP tools.

#### How does the Affiliate Program work?
We pay affiliate commissions based on your monthly booking volume:

| Tier | Bookings/month | Commission |
|------|---------------|------------|
| Tier 1 | 5+ | 4% of total booking value |
| Tier 2 | 20+ | 7% of total booking value |
| Tier 3 | 50+ | 10% of total booking value |
| Tier 4 | 100+ | Special terms |

Tier is calculated per calendar month. Bookings are counted after a 14-day hold — from payment date for non-refundable bookings, or from guest checkout for refundable bookings.

An affiliate contract will be sent within 2 weeks after your first successful affiliate booking. Questions? [vadym.k@winwin.travel](mailto:vadym.k@winwin.travel)

#### The tool isn't showing up in Claude — what do I check?
1. Confirm the JSON in `claude_desktop_config.json` is valid (no trailing commas, no `//` comments inside the JSON).
2. Verify the file is saved in the correct OS path (see above).
3. Fully quit and relaunch Claude Desktop — closing the window is not enough.

#### My token isn't working — what should I check?
Run `test_mcp_authentication` — it returns an explicit error if the token is invalid or expired. Make sure you replaced `YOUR_ACCESS_TOKEN_HERE` with the full token string and there are no extra spaces.

#### Is this available globally?
Yes. The server covers worldwide hotel inventory. Availability and pricing depend on the destination and dates searched.

#### Is there a cost to use this MCP?
No. Connecting and searching is free. You only pay when completing a hotel booking.

---

<p align="center">
  Built with ❤️ by the WinWin.travel Team<br>
  <a href="mailto:vadym.k@winwin.travel">vadym.k@winwin.travel</a>
</p>
