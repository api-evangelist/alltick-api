---
name: alltick-api-check-trading-halts
description: >-
  List current and recent trading halts and resumptions on SSE, NYSE and NASDAQ from AllTick, and
  handle the fact that these three endpoints use a different response envelope and the API's only
  pagination.
api: AllTick REST/HTTP Query API
generated: '2026-08-18'
method: generated
source: openapi/alltick-api-openapi.json + https://en.apis.alltick.co/
operations:
  - getSseSuspension
  - getNyseSuspension
  - getNasdaqSuspension
  - getStockStaticInfo
---

# Check trading halts on AllTick

## The three endpoints

- `getSseSuspension` — `GET https://quote.alltick.co/api/suspension/sse`
- `getNyseSuspension` — `GET https://quote.alltick.co/api/suspension/nyse`
- `getNasdaqSuspension` — `GET https://quote.alltick.co/api/suspension/nasdaq`

Note the base path: `/api/suspension/…`, **not** `/quote-stock-b-api`. These are a third service
alongside the two market-data base paths.

Parameters are conventional here — no encoded `query` document:

- `token` (required)
- `page` (default `1`)
- `size` (default `10`)

## This endpoint family is the odd one out

Two things differ from every other AllTick operation, and both will break a client written against
the rest of the API:

**1. Different envelope.** There is no `ret`/`msg`/`trace`. You get:

```json
{"success": true, "timestamp": "…", "totalCount": 0, "totalPages": 0, "currentPage": 1, "currentSize": 10, "data": []}
```

Check `success` (boolean), not `ret` (integer). There is no `trace` echo, so you cannot correlate a
request with its reply the way you can elsewhere.

**2. It paginates.** This is the only paginated collection in the API. Walk it with
`page`/`size` until `currentPage == totalPages`. Every other AllTick endpoint truncates silently
instead of paging.

## Reading a record

Each `data[]` entry carries `symbol`, `symbolName`, `haltReason`, `haltDate`, `haltTime`,
`resumeDate`, `resumeTime` and `publishDate`. The schema sets `additionalProperties: true`, so treat
unknown fields as expected, not as an error.

An empty `resumeDate`/`resumeTime` means the halt has not been scheduled to lift — do not read it as
"resumed today".

## Rate limit

**1 request per minute, on every plan including Professional.** This is far tighter than the
market-data endpoints (up to 20 req/s) and it does not scale with the price you pay. Poll on a
minute cadence at most; cache the last page and diff.

Exceeding it returns HTTP 429 / `Too Many Requests`.

## Enriching a halt

`getStockStaticInfo` — `GET /quote-stock-b-api/static_info` — resolves a `symbol` to instrument
reference data (`name_en`, `name_cn`, `name_hk`, `exchange`, `board`, `currency`, `lot_size`,
`total_shares`, `eps`, `bps`, `dividend_yield`). It takes the encoded `query` document with a
`symbol_list`, like the other market-data endpoints, and covers **US, HK and A-share equities only**.

Beware the field-name mismatch: the suspension endpoints call the instrument key `symbol`, and so
does static info, but the K-line/tick/depth endpoints call it `code`. Normalise to one name in your
own model.

## What this does not cover

Hong Kong halts are not exposed by these three endpoints — only SSE, NYSE and NASDAQ. There is no
webhook or push notification for a halt; you must poll.
