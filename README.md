# FinAlly — AI Trading Workstation

A visually stunning AI-powered trading workstation that streams live market data, lets you trade a simulated portfolio, and integrates an LLM chat assistant that can analyze positions and execute trades on your behalf. Looks and feels like a Bloomberg terminal with an AI copilot.

## Features

- **Live price streaming** via SSE — prices flash green/red on every tick
- **Sparkline mini-charts** that accumulate from the live stream
- **Buy & sell** with instant market orders against $10,000 virtual cash
- **Portfolio heatmap** (treemap) sized by weight, colored by P&L
- **P&L chart** tracking total portfolio value over time
- **AI chat assistant** — ask questions, get analysis, and let the AI execute trades and manage your watchlist through natural language

## Quick Start

### Prerequisites

- Docker
- An OpenAI API key

### Setup

```bash
# 1. Clone the repo
git clone <repo-url>
cd finally

# 2. Create your .env file
cp .env.example .env
# Edit .env and add your OPENAI_API_KEY

# 3. Build and run
docker build -t finally .
docker run -v finally-data:/app/db -p 8000:8000 --env-file .env finally
```

Open `http://localhost:8000` in your browser.

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `OPENAI_API_KEY` | Yes | Powers the AI chat assistant |
| `MASSIVE_API_KEY` | No | Use real market data (omit to use the built-in simulator) |
| `LLM_MOCK` | No | Set `true` for deterministic mock responses (testing) |

## Architecture

Single Docker container on port 8000:

- **Frontend**: Next.js (TypeScript), built as a static export and served by FastAPI
- **Backend**: FastAPI (Python/uv) — REST API, SSE streaming, LLM integration
- **Database**: SQLite (volume-mounted, auto-initialized with seed data)
- **Market data**: Built-in GBM simulator by default; Massive REST API if key provided

## Development

```
finally/
├── frontend/     # Next.js TypeScript project
├── backend/      # FastAPI uv project (Python)
├── planning/     # Project documentation
├── scripts/      # Start/stop helper scripts
├── test/         # Playwright E2E tests
└── db/           # Volume mount target for SQLite
```

See `backend/README.md` for backend-specific setup and `planning/PLAN.md` for full project specification.
