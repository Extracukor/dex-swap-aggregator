# DEX Swap Aggregator

DEX swap aggregator on Base. Quotes and executes token swaps across Uniswap v3 and Aerodrome, selecting the best route with on-chain slippage protection and a 5 bps protocol fee.

**Spec repo**: https://github.com/Extracukor/dex-swap-aggregator-spec

---

## Jelenlegi állapot — 2026-04-03

### Implementáció (Feature 001 — Core Swap Aggregation)

| Fázis | Tartalom | Állapot |
|-------|----------|---------|
| Phase 1 — Setup | Monorepo struktúra, foundry.toml, pyproject.toml, railway.toml | ✅ Alap skeleton kész |
| Phase 2 — Contracts | `AggregatorRouter.sol`, adapterek, fork tesztek | ⏳ Következő |
| Phase 3 — Engine (US1 Quote) | FastAPI, quoters, `/v1/quote` endpoint | ❌ |
| Phase 4 — Frontend (US2 Execute) | React swap UI, wagmi hooks | ❌ |
| Phase 5 — Route Comparison (US3) | RouteComparison komponens | ❌ |
| Phase 6 — Polish | Revenue monitoring, Railway/Cloudflare config, sign-off | ❌ |

### CI/CD állapot

| Komponens | CI | Deploy |
|-----------|-----|--------|
| Contracts | GitHub Actions (forge build + fork tests) | Manuális (`forge script`) |
| Engine | GitHub Actions (pytest) | Railway (git push → auto-deploy) |
| Frontend | GitHub Actions (pnpm build + tsc) | Cloudflare Pages (git push → auto-deploy) |

> **Folytatáshoz**: Olvasd el a `specs/001-core-swap-aggregation/tasks.md`-t a spec repo-ban,
> és folytatsd a Phase 2 taskoknál (T010 — `AggregatorRouter.sol`).

---

## Repository Structure

```
contracts/   — Solidity smart contracts (Foundry)
engine/      — Python routing engine (FastAPI)
frontend/    — React swap UI (Vite + wagmi)
```

## Prerequisites

| Tool | Version | Install |
|------|---------|---------|
| Foundry | latest | https://getfoundry.sh |
| Python | 3.12+ | https://python.org |
| Node.js | 20+ | https://nodejs.org |
| pnpm | 9+ | `npm i -g pnpm` |

## Quick Start

### Contracts

```bash
cd contracts
forge install
forge build
cp engine/.env.example engine/.env  # fill in RPC URLs
forge test --fork-url $BASE_RPC_URL
```

### Engine

```bash
cd engine
python -m venv .venv
.venv\Scripts\activate      # Windows
pip install -r requirements.txt
cp .env.example .env        # fill in values
uvicorn app.main:app --reload
```

### Frontend

```bash
cd frontend
pnpm install
pnpm dev
```

## Deployment

- **Engine**: Railway — connect this repo, set env vars in Railway dashboard, push to deploy.
- **Frontend**: Cloudflare Pages — connect this repo, build command: `pnpm build`, output: `dist/`.
- **Contracts**: `forge script script/Deploy.s.sol --rpc-url $BASE_SEPOLIA_RPC_URL --broadcast`

## Spec & Architecture

All feature specs, data models, API contracts, and implementation plans are in the
[spec repo](https://github.com/Extracukor/dex-swap-aggregator-spec).
Start with `specs/001-core-swap-aggregation/spec.md`.
