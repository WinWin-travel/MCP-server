# n8n Demo Bots — WinWin.travel MCP

> **This is a demo, not a production template.**
> The workflows in this folder exist to **showcase** what the WinWin MCP can do in concrete, runnable n8n builds. They are **not** a recommended production architecture, **not** security-hardened (credentials, rate-limiting, abuse handling, prompt-injection defenses, etc. are out of scope), and **not** the only way to use the MCP. Treat them as a starting point you adapt to your own needs, not a turnkey product.

Example n8n workflows built on top of the **WinWin.travel MCP**.

* 🏨 **WinWin Hotel Demo Bot** — Telegram AI travel agent. See [CONNECT_N8N.md](./CONNECT_N8N.md) to install.
* 📦 **Importable workflow:** [`winwin-hotel-demo-bot.json`](./winwin-hotel-demo-bot.json)

---

## 🤖 What you get — WinWin Hotel Demo Bot

A fully working **Telegram travel agent** wired up to the WinWin MCP:

* 🏨 Free-text hotel search ("I want a hotel in Paris near the Louvre, Aug 5–7, under $500, travelling with a dog")
* 🎴 Rich offer cards with photos, prices, dates, AI summary and Google Maps link
* 📸 Per-offer photo galleries (hotel + room photos, validated for reachability)
* ➡️ "Show next 3 offers" pagination reusing the original search parameters
* 🧠 Multi-turn memory (template uses Postgres, but the memory node is swappable) — the bot remembers your previous searches and follow-up questions
* 💳 Direct "View details" link to the booking page (no payment goes through the bot)
* 🌍 "View results on WinWin.travel" link to the full results page

---

## 🔁 How the bot works (high level)

```
Telegram Trigger
       │
       ▼
  Route Update Type ── /start      → Send Welcome Message
       │            ── free text    → Send Searching Message
       │            ── gallery:*    → Send Gallery Type Menu (Hotel / Room)
       │            ── galleryOffer:* / galleryHotel:* → Parse Gallery Selection → Data Table → photo/media group
       │            ── everything else (pagination, "next") → Send Searching Message → re-use last search
       ▼
  Hotel Search Agent
   ├─ Chat Model (template: OpenAI)        — pick any provider/model from RECOMMENDED_MODELS.md
   ├─ WinWin MCP Tools                     — search, get_filters, create_reservation, get_reservation_status, echo, test_mcp_authentication
   ├─ Photo Validation Tool                — checks that photo URLs are reachable
   ├─ Hotel Results Parser                 — structured-output JSON schema
   └─ Chat Memory (template: Postgres)     — multi-turn conversation context; swap for any other memory node n8n supports
       │
       ▼
  Has Hotel Results?
       ├── yes → Normalize → split into batches of 3 → offer cards + photos + Data Table write
       └── no  → Send No Results Message
```

The agent is instructed (via its system prompt) to use `pagination.limit = 3, offset = 0` for every new search, to keep offers with duplicate hotel names separate (one entry per room type), to always reuse the search center point as each hotel's lat/lon, to validate all photo URLs in a single Photo Validation Tool call, and to detect the user's language for natural-language fields. The workflow, not the agent, is responsible for pagination — that's why "Show next 3 offers" reuses the previous search parameters via `callback_data: "next:<nextIndex>"`.

---

## ⚙️ Ready to install?

See **[CONNECT_N8N.md](./CONNECT_N8N.md)** for the step-by-step setup: install the template, create the four credentials, add the Data Table, and activate.

---

> See the [main README](../../README.md) for context and security notes.
