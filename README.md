# AllTick API

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AllTick (AllTick PTE. LTD., Singapore) sells real-time and historical financial market tick data through a token-authenticated HTTPS query API and two WebSocket streams, covering forex, Hong Kong / US / A-share equities, indices, precious metals, crude oil and cryptocurrencies across roughly 100,000 symbols. The HTTP surface returns candlesticks (1-minute through monthly, with ex-rights and forward-adjusted variants), latest trade ticks, order-book depth, equity reference data and SSE/NYSE/NASDAQ trading-halt listings; the WebSocket surface pushes tick-by-tick trades and order-book updates. Sold self-serve in USDT on a per-symbol-basket or whole-market basis, with a permanent free tier limited to ten demo symbols. Built for exchanges, brokers, quantitative teams, trading platforms and fintech developers.

**Areas:** Financial market data · Real-Time Data · stock-market-data · forex-data · Cryptocurrency Data · commodities-data · Tick Data · WebSocket streaming · Fintech · Quantitative Trading · market-data-api · trading-halts

## AllTick API on APIs.io

Everything profiled for AllTick API is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/alltick-api/](https://apis.io/providers/alltick-api/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [AllTick WebSocket Streaming API](https://apis.io/apis/alltick-api/alltick-websocket-streaming-api/) — Token-authenticated WebSocket streaming API for real-time market data. Two endpoints — wss://quote.alltick.co/quote-stock-b-ws-api for equities and…
- [AllTick API Quote B API](https://apis.io/apis/alltick-api/alltick-api-quote-b-api-api/) — The Quote B Api API from AllTick API — 4 operation(s) for quote b api.
- [AllTick API Quote Stock B API](https://apis.io/apis/alltick-api/alltick-api-quote-stock-b-api-api/) — The Quote Stock B Api API from AllTick API — 5 operation(s) for quote stock b api.
- [AllTick API Suspension API](https://apis.io/apis/alltick-api/alltick-api-suspension-api/) — The Suspension API from AllTick API — 3 operation(s) for suspension.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Alltick Api Event Surface](https://apis.io/asyncapis/alltick-api/alltick-api-event-surface/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [alltick-api-mcp.yml](https://apis.io/servers/alltick-api/alltick-api-mcp-yml/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Alltick Api Openapi Overlay](https://apis.io/overlays/alltick-api/alltick-api-openapi-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Alltick Api Authentication](https://apis.io/security/alltick-api/alltick-api-authentication/)
- [Alltick Api Domain Security](https://apis.io/security/alltick-api/alltick-api-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Alltick Api Plans Pricing](https://apis.io/plans/alltick-api/alltick-api-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Alltick Api Rate Limits](https://apis.io/rate-limits/alltick-api/alltick-api-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.alltick.co/)
- [Documentation](https://en.apis.alltick.co/)
- [Getting started](https://en.apis.alltick.co/integration-process/process-description)
- [API reference](https://apis.alltick.co/en/api-reference/stock/kline)
- [Pricing](https://alltick.co/pricing)
- [Sign up](https://alltick.co/register)
- [Support](https://alltick.co/#contact)
- [Blog](https://blog.alltick.co/)
- [Terms of service](https://alltick.co/terms)
- [Privacy policy](https://alltick.co/policy)
- [GitHub](https://github.com/alltick)
- [Status](https://status.alltick.co/)

## API Evangelist rating

**Kin Score 56.9** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 53.4 |
| Contract governance | 4.5 |
| Operational transparency | 50.0 |
| Developer ergonomics | 66.1 |
| Access clarity | 76.3 |

**Agent readiness 32.6** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/alltick-api/](https://apis.io/providers/alltick-api/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for AllTick API: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/alltick-api/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
