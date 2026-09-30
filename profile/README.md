# matchwire.win

We map sports prediction markets. **One row per game**, matched across **Kalshi, Polymarket US, Polymarket International and Predict.fun**, with each venue's own market id left intact — so you can compare a Kalshi API market with its Polymarket API twin without owning a reconciliation job.

Winners, spreads and handicaps, totals and props are matched too, and the line is part of the match: `over 45.5` is never confused with `over 48.5`. Every listing carries a tier (`exact`, `normalized`, `fuzzy`), a confidence, and the exact request that reads that market from the venue.

**Mapping only — no prices, no order books.** Prices come from each venue; we tell you which market is the same game.

```js
import { Matchwire } from 'matchwire.win';

const mw = new Matchwire({ apiKey: process.env.MATCHWIRE_API_KEY });
const { rows, seq } = await mw.rows({ sport: 'nfl', venue: ['kalshi', 'poly'] });

for (const game of rows.filter((r) => r.listings.length > 1)) {
  console.log(game.name, game.listings.map((l) => `${l.venue}:${l.marketId}`));
}
```

## Repositories

| Repository | What it is |
| --- | --- |
| **[matchwire-sdk](https://github.com/MatchWire-Win/matchwire-sdk)** | The client for the mapped API — Node.js (`npm i matchwire.win`) and Python (`pip install matchwire.win`), zero dependencies, MIT |

## The API

| | |
| --- | --- |
| Base URL | `https://api.matchwire.win` |
| Auth | `x-api-key` header |
| Rows | `GET /api/v1/rows` — one row per game, `?since=<seq>` for deltas, `?sport=` `?venue=` `?type=` `?league=` `?status=` |
| Sports | `GET /api/v1/sports` — sports, counts and group ids (`esports`, `american-football`, …) |
| Stream | `GET /api/v1/push` — the same updates as Server-Sent Events |
| Health | `GET /api/v1/status` — service and per-venue health, current `seq` |
| Agents | `POST https://api.matchwire.win/mcp` — MCP tools: `list_sports`, `find_rows`, `get_row`, `get_venue_market`, `report_issue`, `service_status` |

## Documentation

- Docs — <https://matchwire.win/docs/>
- Quickstart — <https://matchwire.win/docs/quickstart/>
- MCP for agents — <https://matchwire.win/docs/mcp/>
- How event mapping works — <https://matchwire.win/how-to-map-prediction-markets/>
- Kalshi API, mapped — <https://matchwire.win/kalshi-api/>
- Polymarket API, mapped — <https://matchwire.win/polymarket-api/>
- Prediction Market API — <https://matchwire.win/prediction-market-api/>
- `llms.txt` — <https://matchwire.win/llms.txt>
- Plans and pricing — <https://matchwire.win/#pricing>

Contact: support@matchwire.win · Privacy: <https://matchwire.win/privacy/> · Terms: <https://matchwire.win/terms/>

---

matchwire.win is an independent mapping layer for sports prediction markets. Not affiliated with, endorsed by, or sponsored by Kalshi, Polymarket or Predict.fun; venue names describe what is mapped.
