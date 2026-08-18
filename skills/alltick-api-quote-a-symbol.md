---
name: alltick-api-quote-a-symbol
description: >-
  Get the current price, order book and recent candlesticks for one instrument from AllTick —
  choosing the right base path for the asset class, encoding the JSON query correctly, and reading
  the ret code instead of the HTTP status.
api: AllTick REST/HTTP Query API
generated: '2026-08-18'
method: generated
source: openapi/alltick-api-openapi.json + https://en.apis.alltick.co/
operations:
  - getStockTradeTick
  - getForexTradeTick
  - getStockDepthTick
  - getForexDepthTick
  - getStockKline
  - getForexKline
---

# Quote a symbol on AllTick

## Before you start

- You need a token from the AllTick dashboard (`API keys` section) after registering at
  <https://alltick.co/register>. It goes in the **query string** as `token`, on every call.
- Pick the base path by asset class. This is the single most common integration error:
  - Hong Kong / US / A-share equities → `https://quote.alltick.co/quote-stock-b-api`
  - Forex, crypto, precious metals, crude oil → `https://quote.alltick.co/quote-b-api`
  - Sending an equity code to `/quote-b-api` (or vice versa) returns `ret: 600 code invalid`,
    not a 404.

## Step 1 — Build the query document

GET operations do not take conventional parameters. The entire request is one JSON document,
URL-encoded, in a parameter named `query`:

```json
{"trace": "<your-unique-id>", "data": {"symbol_list": [{"code": "700.HK"}, {"code": "AAPL.US"}]}}
```

`trace` is caller-generated, max 64 characters, and is echoed back verbatim in the response — use
it to correlate your request with the reply. Generate a fresh one per request.

## Step 2 — Latest trade price

`getStockTradeTick` — `GET /quote-stock-b-api/trade-tick?token=…&query=…`
`getForexTradeTick` — `GET /quote-b-api/trade-tick?token=…&query=…`

Returns `data.tick_list[]` with `code`, `seq`, `tick_time` (ms), `price`, `volume`, `turnover` and
`trade_direction` (`0` neutral, `1` buy, `2` sell). **All numeric fields are strings** — parse them
as decimals, never as floats.

Keep the code list short: the Free plan caps at 5 codes, and paid plans are limited by GET URL
length to a recommended 50. Over-subscription is **silently truncated to the first N** — you get a
200 with fewer rows, not an error.

## Step 3 — Order book

`getStockDepthTick` — `GET /quote-stock-b-api/depth-tick`
`getForexDepthTick` — `GET /quote-b-api/depth-tick`

Same `symbol_list` query document. Returns `data.tick_list[]` with `bids[]` and `asks[]`, each an
ordered array of `{price, volume}` levels. Depth varies by market and plan.

Order book is **not available on the Free plan**.

## Step 4 — Candlesticks

`getStockKline` — `GET /quote-stock-b-api/kline`
`getForexKline` — `GET /quote-b-api/kline`

```json
{"trace": "<id>", "data": {"code": "700.HK", "kline_type": 1, "kline_timestamp_end": 0, "query_kline_num": 100, "adjust_type": 0}}
```

- `kline_type`: `1`=1min `2`=5min `3`=15min `4`=30min `5`=1h `6`=2h `7`=4h `8`=daily `9`=weekly `10`=monthly
- `kline_timestamp_end`: `0` means "latest"
- `adjust_type`: `0` ex-rights, `1` forward-adjusted
- **One code per request**, max **500** candlesticks returned. Asking for more returns the first 500
  silently. There is no cursor — you cannot page past the cap.

AllTick recommends caching retrieved history in your own database rather than re-querying it.

## Step 5 — Read the response correctly

Every response is the same envelope:

```json
{"ret": 200, "msg": "ok", "trace": "<your id>", "data": {}}
```

**Branch on `ret`, not on the HTTP status line.** The documented codes:

| `ret` | Meaning | What to do |
|---|---|---|
| 200 | ok | proceed |
| 400 | request header / data param invalid | fix the JSON structure or the `data` object |
| 401 | token invalid | wrong format, or the subscription expired |
| 402 | query invalid | the `query` parameter is not URL-encoded correctly |
| 429 / 605 | too many requests | back off; see the rate limits below |
| 600 | code invalid | wrong base path for the asset class, or an unknown code |
| 601 | body empty | you sent a POST with no JSON body |
| 603 | token level not enough | you asked for more codes/candles than the plan allows |
| 604 | code unauthorized | the symbol is outside your plan's basket |

## Rate limiting — you must self-govern

AllTick returns **no rate-limit headers at all**. There is no `RateLimit-Remaining`, no
`Retry-After`. You cannot discover your remaining budget at runtime — you must encode your plan's
limits in the client:

| Plan | Per endpoint | Account total | Daily cap |
|---|---|---|---|
| Free | 1 req / 10s | 10 req/min | 1,000 |
| Basic | 1 req/s | 60 req/min | 86,400 |
| Premium | 10 req/s | 600 req/min | 864,000 |
| Professional | 20 req/s | 1,200 req/min | 1,728,000 |

Limits are enforced **per token, not per IP**. Two processes sharing a token share one budget.
Frequency is counted per endpoint: a `/kline` call and a `/trade-tick` call in the same second both
succeed on Basic; two `/kline` calls do not.

## Do not

- Do not put the token in a header — it is not read there.
- Do not assume a 200 status means success; check `ret`.
- Do not retry a 603 or 604 — those are quota/entitlement, not transient.
- Do not poll `/kline` for real-time updates. Use the WebSocket stream (see the streaming skill).
