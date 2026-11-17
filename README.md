# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/evan913/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/evan913/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar: it reads a frozen DeFiLlama snapshot of 758 protocols, finds the ones that have not launched a token yet — and are not just a wrapped or staked version of somebody else's already-trading token — ranks them by the implied value a launch would carry, and writes a board, a static dashboard, and a dossier the judge can read. On the sealed snapshot it scores 198 protocols, all 40 watchlist names included.

How it ranks: implied value = TVL × a category-comparable multiple × growth × maturity × chains. The multiple is the median market-cap / TVL ratio of live tokens in the same category (at least 3 samples, clamped 0.05–1.00; too few samples fall back to the global median 0.241). Growth is clamped to −60…120 and weighted 0.5; maturity rises with protocol age; chains add a small logarithmic term. The watchlist's own exclusion rule — Bridge, Liquid Staking, Liquid Restaking, Restaked BTC and CEX categories, wrapped / staked / staking products, and exchange-prefixed names — is applied to every row, so a wrapped Bitcoin or a staked-ETH product can never outrank a protocol still waiting for its own token. Each ranked row also carries a 3–12 step farming plan, tailored by category, chains, TVL and age. Nothing in the board is invented: every figure on the dashboard comes from the snapshot, and the dashboard recomputes each row from those same figures.

## Run it

```sh
node main.ts
```

The grader mounts the sealed snapshot at `/input/snapshot.json` and collects the output from `/output`. To run it locally against a snapshot file:

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <path-to-snapshot.json>
```

That writes `board.json`, `data.js`, `dashboard/`, `screenshots/` and `dossier.json` into the output directory. Node.js 22.18 or later, standard library only, no network, no model keys. Running it twice produces byte-identical output.

Files: `main.ts` is the entrypoint; `lib/model.ts` holds the radar-2 model and the farm-plan writer; `dashboard/` is the static page (board, per-protocol detail views, filters, search); `screenshots/` holds the rendered board, detail and filtered views; `vendor/makefun-board.ts` is make.fun's board library, vendored unchanged.

---

Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards
