# DEX Swap Aggregator

DEX swap aggregator on Base. Quotes and executes token swaps across Uniswap v3 and Aerodrome, selecting the best route with on-chain slippage protection and a 5 bps protocol fee.

**Spec repo**: https://github.com/Extracukor/dex-swap-aggregator-spec

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
