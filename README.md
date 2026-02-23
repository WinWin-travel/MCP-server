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

### 1. 🔑 Get Your Access Token
You need a secure token to connect.
👉 **[Generate Access Token](https://tally.so/r/GxdpeO)**

### 2. 🔌 Configure Claude Desktop
Add this to your `claude_desktop_config.json`:

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

*That's it. Restart Claude and start booking.*

## 🧰 Capabilities

| Tool | Description |
| :--- | :--- |
| **`search_hotels`** | **Deep Search:** Filter hotel/rooms/hotel deals using over 500 specific filters to find the perfect match. |
| **`get_hotel_details`** | **Rich Intelligence:** Retrieve full details on hotels, rooms, amenities, facilities, and reviews. |
| **`book_reservation`** | **Full Logic Booking:** Manage reservations end-to-end. Generate secure payment links or process transactions via Stripe MCP integration. |
| **`rate_hotel`** | **Preference Learning:** Give hotels "likes" and "dislikes" to train our AI-assisted selection engine for better future recommendations. |
| **`get_market_trends`** | **Market Insights:** Access real-time hotel statistics, track price changes, and set up alerts/triggers. |

## 🔒 Security & Privacy

*   **Official Endpoint:** You are connecting directly to WinWin.travel's secure API gateway.
*   **Data Usage:** Your preferences are used to improve your booking experience. We do not sell your data to third parties.

---

<p align="center">
  Built with ❤️ by the WinWin.travel Team<br>
  <a href="mailto:mcp@winwin.travel">mcp@winwin.travel</a>
</p>
