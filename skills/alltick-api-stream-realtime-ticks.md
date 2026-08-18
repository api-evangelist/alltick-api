---
name: alltick-api-stream-realtime-ticks
description: >-
  Open and keep an AllTick WebSocket stream alive — subscribe to tick-by-tick trades or order-book
  depth, honour the 10-second heartbeat, respect the per-plan connection budget, and reconnect
  without being throttled.
api: AllTick WebSocket Streaming API
generated: '2026-08-18'
method: generated
source: >-
  https://en.apis.alltick.co/websocket-api/websocket-interface-api/ +
  asyncapi/alltick-api-event-surface.yml
operations: []
operations_note: >-
  The streaming surface is NOT in AllTick's OpenAPI — it is a numbered `cmd_id` protocol documented
  only in prose. The message numbers below are quoted from AllTick's own WebSocket reference; there
  are no operationIds to ground them in.
---

# Stream real-time ticks from AllTick

## Pick your endpoint and connect

Two streams, chosen by asset class, with the token appended to the handshake URL:

- Stocks (HK / US / A-share): `wss://quote.alltick.co/quote-stock-b-ws-api?token=yourToken`
- Forex / crypto / commodities: `wss://quote.alltick.co/quote-b-ws-api?token=yourToken`

Each counts as **one connection** against your plan budget:

| Plan | Concurrent connections |
|---|---|
| Free | 1 |
| Basic | 1 |
| Premium | 3 |
| Professional / All HK / All CN | 10 |

Connections are counted **per token, not per IP**. A second connection over budget is not queued —
it is dropped immediately.

## Message envelope

Every frame, both directions, is JSON:

```json
{"cmd_id": 22000, "seq_id": 123, "trace": "3baaa938-…", "data": {}}
```

Responses add `ret` and `msg`. `seq_id` and `trace` are caller-generated and echoed, so use them to
match a reply to its request.

## Step 1 — Start the heartbeat immediately

Send `cmd_id 22000` **every 10 seconds**; the server replies `22001`. If no heartbeat arrives within
30 seconds the server closes the connection. Start the heartbeat timer as soon as the socket opens —
before you subscribe — or a slow first subscription can time you out.

## Step 2 — Subscribe

Latest trade price (tick-by-tick) — request `cmd_id 22004`:

```json
{"cmd_id": 22004, "seq_id": 1, "trace": "<id>", "data": {"symbol_list": [{"code": "BTCUSDT"}, {"code": "ETHUSDT"}]}}
```

Acknowledged with `22005`. Ticks then arrive as pushes on **`cmd_id 22998`** carrying
`code, seq, tick_time, price, volume, turnover, trade_direction`.

Order book (market depth) — request `cmd_id 22002`, acknowledged `22003`, pushes on
**`cmd_id 22999`** carrying `code, seq, tick_time, bids[], asks[]`.

Each connection holds **one active subscription per stream type** — a new `symbol_list` replaces the
previous set, it does not add to it. Track the full desired set client-side and resend it whole.

Concurrent code limits per connection:

| Plan | Codes per subscription |
|---|---|
| Free | 5 |
| Basic | 100 |
| Premium | 200 |
| Professional / All HK / All CN | 3000 |

Over the limit, AllTick processes the **first N and ignores the rest** — silently. Verify the codes
you actually receive against the codes you asked for.

## Step 3 — Unsubscribe

`cmd_id 22006` with `cancel_type`: `0` cancel all, `1` cancel order book, `2` cancel trade,
`3` cancel exchange rate. Acknowledged with `22007`.

## Pacing rules that will bite you

- At least **1 second** between requests on the same connection.
- At least **3 seconds** between requests across different connections.
- Reconnect backoff: **10 seconds** minimum on Free, **3 seconds** minimum on paid plans.
- Violating these returns `ret 606 "too many requests and connection will be closed"` — the
  connection is dropped, so a naive retry loop turns one throttle into a connect/drop cycle.

## What the stream will not give you

- **No candlestick push.** AllTick states explicitly that K-lines are HTTP-only, both historical and
  real-time. Build bars locally from `22998` ticks, or poll `/kline`.
- **No historical replay.** Both subscriptions deliver live data only; there is no backfill on
  connect, so a gap during a disconnect is a permanent gap unless you fill it from the HTTP API.
- **No machine-readable contract.** There is no AsyncAPI document, so no client can be generated —
  hand-write the codec against the numbers above.

## Ordering and gaps

Each tick carries `seq`, a per-instrument sequence number. Use it to detect drops and to order
messages; do not rely on arrival order or on `tick_time` alone.
