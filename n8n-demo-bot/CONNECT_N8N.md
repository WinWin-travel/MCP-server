# Install & Connect — WinWin Hotel Demo Bot (n8n)

Step-by-step setup for the **WinWin Hotel Demo Bot** — a Telegram AI travel agent built on the WinWin MCP, packaged as an n8n workflow. For an overview of what the bot does, see [README.md](./README.md).

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
1. Download [`winwin-hotel-demo-bot.json`](./winwin-hotel-demo-bot.json) from this repository.
2. In n8n, open the project where you want the bot.
3. Click the **⋮** menu → **Import from File…** (or **Import Workflow → From File**).
4. Select the downloaded `.json` file.
5. Continue to Step 3 below.

### 3. Create the four credentials
The template ships with placeholder credential IDs. Replace each one with your own:

| # | Credential | How to create it | Used by |
|---|-----------|-----------------|---------|
| 1 | **Telegram API** | Talk to [@BotFather](https://t.me/BotFather) in Telegram, run `/newbot`, copy the token. In n8n: *Credentials → New → Telegram API* and paste the token. | Every `Send …` and `Telegram Trigger` node |
| 2 | **Chat model provider** | The template ships wired to an OpenAI chat-model node. Pick the provider and model you want from the [Recommended AI Models](../../RECOMMENDED_MODELS.md) guide and use n8n's matching credential type (e.g. *OpenAI API*, *Anthropic API*, *Google Gemini(PaLM) API*, etc.). You can keep the shipped OpenAI node and just swap the model name, or replace the node entirely with your provider's equivalent. | **OpenAI Model (Agent)** (replaceable) |
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

## ❓ Troubleshooting

#### I see "Telegram account 4" / "OpenAI account 3" / etc. on each node — what does that mean?
Those are display names from the original creator's n8n instance. They don't affect anything; your own credential names will replace them once you select them in each node's dropdown.

#### The bot replies with "⏳ Request accepted. Searching..." but never returns results
Usually means one of three things:
1. **OpenAI model name mismatch** — the template ships with a placeholder model name that your account may not have access to. On the **OpenAI Model (Agent)** node, pick a model recommended in the [Recommended AI Models](../../RECOMMENDED_MODELS.md) guide.
2. **WinWin MCP bearer not set** — check the **WinWin MCP Tools** node: the credential dropdown must show your bearer credential, not be empty.
3. **Postgres unreachable** — check the **Postgres Chat Memory** node's credential and that the database accepts connections from n8n.

#### The bot searches but I get "I could not find any matching hotels"
Try a more specific query — destination, dates, and at least one constraint (pets / budget / stars). See the **Example Prompts** section in the main [README](../../README.md).

#### The Gallery button does nothing
The `OfferPhotos` Data Table is empty for that session. It only fills when the bot first runs a search — open a fresh search, wait for the offer cards, then tap **📸 View Gallery**.

#### Which AI clients are supported besides n8n?
Any MCP-compatible client works. See the main [README](../../README.md#-faq) for Cursor, Windsurf, and Claude Desktop setup.

---

← [Back to folder README](./README.md) · [↑ Main README](../../README.md)
