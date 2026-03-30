![Nanobot](https://github.com/HKUDS/nanobot/raw/main/nanobot_logo.png)

## Nanobot AI on Railway (Template)

Template ini menjalankan **Nanobot gateway** + **admin dashboard** di Railway via Docker.

## Architecture
![Architecture](https://github.com/HKUDS/nanobot/raw/main/nanobot_arch.png)

## What this template provides
- **Admin dashboard** (Starlette + Jinja + Tailwind CDN)
- **Gateway process** (`nanobot gateway`) yang bisa start/stop/restart dari dashboard
- **Healthcheck** endpoint: `/health`
- **Persistent data** di `/data/.nanobot` (cocok untuk Railway Volume)

Catatan: **Nanobot bukan chat interface** — ini backend API gateway untuk routing ke berbagai LLM provider.

## Common Use Cases
**📅 Smart Daily Routine Manager**
![Architecture](https://github.com/HKUDS/nanobot/raw/main/case/scedule.gif)

**📈 24/7 Real-Time Market Analysis**
![Market Analyzer](https://github.com/HKUDS/nanobot/raw/main/case/search.gif)

## Deploy to Railway
- Create a new Railway project from this repository
- Build uses `Dockerfile` (configured in `railway.toml`)
- Railway will provide a public URL automatically

Recommended:
- Add a **Volume** mounted at `/data` so config & sessions persist across deploys

## Environment Variables

Required:
- **ADMIN_USERNAME**: Basic Auth username for dashboard
- **ADMIN_PASSWORD**: Basic Auth password for dashboard

Provided by Railway:
- **PORT**: app listens on this port (defaults to `8080` if not set)

Security note:
- If `ADMIN_PASSWORD` is empty, the server will auto-generate a password and print it to logs. For templates/public repos, always set `ADMIN_PASSWORD`.

See `.env.example` for a starting point.

## Endpoints
- **GET /**: Admin dashboard (Basic Auth)
- **GET /health**: Healthcheck
- **GET /api/config**: Read config (secrets are masked)
- **PUT /api/config**: Save config (optionally restart gateway)
- **GET /api/status**: Gateway status + provider/channel summary + cron jobs
- **GET /api/logs**: Gateway logs (for dashboard)
- **POST /api/gateway/start**: Start gateway
- **POST /api/gateway/stop**: Stop gateway
- **POST /api/gateway/restart**: Restart gateway

## FAQ

**Can I use Nanobot without Railway?**  
Yes. Nanobot is open source and runs anywhere Docker works. Railway just makes deployment faster with automatic container hosting and environment management.

**How much does it cost to host Nanobot on Railway?**  
Railway's free tier includes $5 credit per month. Basic Nanobot deployments typically cost $5-15/month depending on traffic. LLM API costs are separate and billed by your providers.

**Can I add multiple LLM providers to one Nanobot instance?**  
Yes, that's the main point. You configure multiple providers (OpenAI, Anthropic, Cohere, etc.) and route requests between them based on your rules.

**Do I need coding knowledge to deploy nanobot?**  
Not really. The Railway template handles deployment automatically. You just need to set environment variables for your API keys. Managing routing rules through the admin dashboard is point-and-click.

## Upstream project
- Nanobot: https://github.com/HKUDS/nanobot
