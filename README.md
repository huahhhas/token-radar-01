# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/huahhhas/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/huahhhas/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar ranks every pre-token DeFi protocol by what its launch would be worth, and shows how to farm each one. It reads a sealed DefiLlama snapshot (758 protocols, 40 on the watchlist), scores the 198 rankable pre-token protocols with model radar-2, and writes a static dashboard: a sortable, filterable board, a detail view for every protocol with its model breakdown and farm plan, and a live-token view kept apart.

## The model (radar-2)

Implied value follows only from the snapshot's own figures:

```
implied = round(tvl_usd x category_multiple
  x (1 + clamp(change_7d, -50, 150) / 100 x 0.6)
  x (0.55 + min(age_days, 1095) / 1095 x 0.45)
  x (1 + min(chains - 1, 5) x 0.02))
```

The category multiple is the median market-cap-to-TVL ratio of the live-token protocols in that category, read from the same snapshot (a category with fewer than 3 live protocols uses the snapshot-wide median), clamped to [0.05, 1.0]. Exchanges, wrapped, staked and bridged products, and versioned pools are left out of the ranking entirely: they are a token that already trades, not one still to come. "No token" means the protocol records no token — never that it has none. Implied value is this entry's own claim, not a price.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <kit>/input/snapshot.json
```

Requires Node.js 22.18 or later, standard library only. The program writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and `out/dossier.json`. Run it twice: both runs write the same bytes.

## Layout

- `main.ts` — entrypoint: reads the snapshot, writes the board, data and dossier
- `lib/model.ts` — the radar-2 model, farm plans and snapshot parsing
- `vendor/makefun-board.ts` — make.fun's dossier library, byte for byte from the kit
- `dashboard/` — the static page (index.html, app.css, app.js), no network
- `screenshots/` — the board, a protocol's detail view and a filtered view

---

<sub>Entered on [make.fun](https://make.fun/?ref=4bs5ey91&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
