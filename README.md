![Nanobot](https://github.com/HKUDS/nanobot/raw/main/nanobot_logo.png)

## Nanobot on Railway — usage guide

This template runs the **Nanobot gateway** and **admin dashboard** via Docker. Nanobot is a backend for routing requests across multiple LLM providers and messaging channels (e.g. Telegram), not a full chat UI.

![Architecture](https://github.com/HKUDS/nanobot/raw/main/nanobot_arch.png)

### Step 1 — Deploy on Railway

1. Create a new Railway project from this repository (or use a Railway template if available).
2. The build uses the `Dockerfile` (configured in `railway.toml`).
3. Railway will assign a public URL automatically.
4. **Strongly recommended:** add a **Volume** mounted at `/data` so configuration and data persist across redeploys.

### Step 2 — Admin username and password

Set these in Railway environment variables:

- **`ADMIN_USERNAME`** — username for dashboard login.
- **`ADMIN_PASSWORD`** — password for dashboard login.

`PORT` is usually provided by Railway. If **`ADMIN_PASSWORD`** is empty, the server may generate a random password and print it to the logs — for production, always set it explicitly.

See `.env.example` for a starting point.

### Step 3 — LLM API keys and models

1. Open the Nanobot dashboard (your Railway URL) and sign in with Basic Auth using the credentials above.
2. Open the **AI Providers** tab.
3. For each provider you use (e.g. OpenAI, Groq, Gemini), enter the **API Key**.
4. Choose the **model** you want (curated lists; **Others** for a custom model id).
5. Turn on one provider as the default (toggle) as needed.
6. Save configuration from the dashboard (data is stored on the `/data` volume).

### Step 4 — Messaging channels (example: Telegram bot)

1. Open the **Channels** tab.
2. For **Telegram**, enable the channel and paste the **token** from BotFather.
3. Set **Allowed User IDs** — use `*` alone to allow everyone, or comma-separated numeric IDs.
4. Configure other channels (WhatsApp bridge, Feishu, etc.) here per their documentation.
5. Save your changes in the dashboard.

### Step 5 — Run the gateway and test

1. Under **Settings** or the **Save & Start** flow on Overview, apply configuration and start or **Restart** the **gateway** from the dashboard when available.
2. Check **Logs** to confirm the gateway is running without errors.
3. Send a test message to your Telegram bot (or another channel you configured).

### Key endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /` | Admin dashboard (Basic Auth) |
| `GET /health` | Healthcheck (for Railway) |
| `GET /api/config` | Read configuration (secrets masked) |
| `PUT /api/config` | Save configuration |
| `GET /api/status` | Gateway status; provider/channel summary |
| `GET /api/logs` | Gateway logs |
| `POST /api/gateway/start` | Start gateway |
| `POST /api/gateway/stop` | Stop gateway |
| `POST /api/gateway/restart` | Restart gateway |

### Upstream project

- Upstream Nanobot: https://github.com/HKUDS/nanobot
