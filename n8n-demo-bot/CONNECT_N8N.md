# n8n Demo Bot — WinWin.travel MCP

> **This is a demo, not a production template.**
> The workflow exists to **showcase** what the WinWin MCP can do in a concrete, runnable n8n build — a Telegram AI travel agent that searches hotels, returns offer cards, and supports photo galleries. It is **not** a recommended production architecture, **not** security-hardened (credentials, rate-limiting, abuse handling, prompt-injection defenses, etc. are out of scope), and **not** the only way to use the MCP. Treat it as a starting point you adapt to your own needs, not a turnkey product.

> **The fastest way to see WinWin MCP in action.**
> Install our official n8n template, plug in four credentials + one Data Table, and you've got a working Telegram AI travel agent searching 3M+ hotels — no glue code required.

## 🤖 What you get

A fully working **Telegram travel agent** wired up to the WinWin MCP:

* 🏨 Free-text hotel search ("I want a hotel in Paris near the Louvre, Aug 5–7, under $500, travelling with a dog")
* 🎴 Rich offer cards with photos, prices, dates, AI summary and Google Maps link
* 📸 Per-offer photo galleries (hotel + room photos, validated for reachability)
* ➡️ "Show next 3 offers" pagination reusing the original search parameters
* 🧠 Multi-turn memory (template uses Postgres, but the memory node is swappable) — the bot remembers your previous searches and follow-up questions
* 💳 Direct "View details" link to the booking page (no payment goes through the bot)
* 🌍 "View results on WinWin.travel" link to the full results page

---

## 🛠️ Setup

### 1. Get Your WinWin MCP Access Token
You need a secure token to connect.
👉 **[Generate Access Token](https://tally.so/r/GxdpeO)**

### 2. Install the Template
Pick whichever method fits your n8n instance — both use the same workflow:

#### Option A — One-click install from the n8n template gallery (recommended)
1. Open your **n8n** instance.
2. Go to **Templates** in the left sidebar.
3. Search for **"WinWin Hotel Demo Bot"**.
4. Click **Use workflow** → pick the project you want it in → confirm.
5. Continue to Step 3 below.

#### Option B — Manual import from the JSON file
1. Download [`winwin-hotel-demo-bot.json`](winwin-hotel-demo-bot.json) from this repository.
2. In n8n, open the project where you want the bot.
3. Click the **⋮** menu → **Import from File…** (or **Import Workflow → From File**).
4. Select the downloaded `.json` file.
5. Continue to Step 3 below.

### 3. Create the four credentials
The template ships with placeholder credential IDs. Replace each one with your own:

| # | Credential | How to create it | Used by |
|---|-----------|-----------------|---------|
| 1 | **Telegram API** | Talk to [@BotFather](https://t.me/BotFather) in Telegram, run `/newbot`, copy the token. In n8n: *Credentials → New → Telegram API* and paste the token. | Every `Send …` and `Telegram Trigger` node |
| 2 | **Chat model provider** | The template ships wired to an OpenAI chat-model node. Pick the provider and model you want from the [Recommended AI Models](RECOMMENDED_MODELS.md) guide and use n8n's matching credential type (e.g. *OpenAI API*, *Anthropic API*, *Google Gemini(PaLM) API*, etc.). You can keep the shipped OpenAI node and just swap the model name, or replace the node entirely with your provider's equivalent. | **OpenAI Model (Agent)** (replaceable) |
| 3 | **HTTP Bearer Auth** | Paste the WinWin MCP token from Step 1. In n8n: *Credentials → New → HTTP Bearer Auth*. | **WinWin MCP Tools** |
| 4 | **Chat-memory backend** | The template ships with Postgres as the chat-memory store. Any reachable Postgres database works (local Docker, Neon, Supabase, RDS, etc.). In n8n: *Credentials → New → Postgres*; the `public` schema and a writable database are enough. You can also swap the *Postgres Chat Memory* node for any other chat-memory node n8n supports (e.g. *Simple Memory*, *Redis Chat Memory*, *Window Buffer Memory*, …) and only need a credential for the one you pick. | **Postgres Chat Memory** (replaceable) |

> [!TIP]
> To open every node that needs a credential at once, search the canvas for `🔑` sticky notes — they mark each credential location.

### 4. Wire credentials to the right nodes
For each of the four credentials above, open every node listed in the table and select that credential in the node's **Credential to connect with** dropdown. There are many Telegram nodes — they all use the same credential.

### 5. Create the `OfferPhotos` Data Table
The bot stores every photo URL (with hotel/room tag and position) in an n8n Data Table so the per-offer Gallery button can look them up later.

1. In n8n go to **Data Tables → Create data table**.
2. Name it **`OfferPhotos`**.
3. Add these columns (types matter):

   | Column | Type |
   |--------|------|
   | `button_id` | string |
   | `chat_id` | string |
   | `photo_url` | string |
   | `position` | number |
   | `photo_type` | string |
   | `hotel_name` | string |
   | `room_type` | string |

4. Open the **Insert Offer Photos** and **Get Gallery Photos** nodes and select your new `OfferPhotos` table in each.

### 6. Activate and test
1. Click **Activate** (top right of the canvas).
2. Open your bot in Telegram and send `/start`.
3. You should see the welcome message with three example-search buttons.
4. Tap one — the bot searches, returns three offer cards with photos, and offers pagination + gallery buttons.

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

## ❓ Troubleshooting

#### I see "Telegram account 4" / "OpenAI account 3" / etc. on each node — what does that mean?
Those are display names from the original creator's n8n instance. They don't affect anything; your own credential names will replace them once you select them in each node's dropdown.

#### The bot replies with "⏳ Request accepted. Searching..." but never returns results
Usually means one of three things:
1. **OpenAI model name mismatch** — the template ships with a placeholder model name that your account may not have access to. On the **OpenAI Model (Agent)** node, pick a model recommended in the [Recommended AI Models](RECOMMENDED_MODELS.md) guide.
2. **WinWin MCP bearer not set** — check the **WinWin MCP Tools** node: the credential dropdown must show your bearer credential, not be empty.
3. **Postgres unreachable** — check the **Postgres Chat Memory** node's credential and that the database accepts connections from n8n.

#### The bot searches but I get "I could not find any matching hotels"
Try a more specific query — destination, dates, and at least one constraint (pets / budget / stars). See the **Example Prompts** section in the main [README](README.md).

#### The Gallery button does nothing
The `OfferPhotos` Data Table is empty for that session. It only fills when the bot first runs a search — open a fresh search, wait for the offer cards, then tap **📸 View Gallery**.

#### Which AI clients are supported besides n8n?
Any MCP-compatible client works. See the main [README](README.md#-faq) for Cursor, Windsurf, and Claude Desktop setup.

---

← [Back to Main README](README.md)
