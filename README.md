# Alpaca (alpaca-markets)

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

Alpaca is a developer-first, commission-free brokerage that exposes stock, ETF, options, and crypto trading and market data entirely through APIs. The **Trading API** places and manages orders against a free paper-trading sandbox (`paper-api.alpaca.markets`) or a live account (`api.alpaca.markets/v2`); the **Market Data API** serves historical and real-time stocks, crypto, options, news, and corporate actions over REST (`data.alpaca.markets`) plus WebSocket streams (`stream.data.alpaca.markets`); and the **Broker API** lets businesses open and fund end-user brokerage accounts (`broker-api.alpaca.markets/v1`).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/alpaca-markets/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/alpaca-markets/refs/heads/main/apis.yml)

## Access Model, Keys, and Authentication

- **Sign up is free.** Create an account and generate an **API key ID** and **secret key** from the Alpaca dashboard. Keys are scoped to either the paper or the live environment.
- **Trading and Market Data** authenticate with two headers on every request: `APCA-API-KEY-ID: <key id>` and `APCA-API-SECRET-KEY: <secret>`. Third-party apps can use **OAuth2** instead.
- **Paper trading is free forever** and identical to live: point the same code at `https://paper-api.alpaca.markets/v2` instead of `https://api.alpaca.markets/v2`.
- **Trading is commission-free** for US stocks and ETFs (options and crypto may carry per-contract or spread fees). **Market data** has a free **Basic** plan and a **$99/month Algo Trader Plus** plan (details below).
- The **Broker API** is a B2B product using **HTTP Basic auth** (key as username, secret as password), with a free sandbox at `https://broker-api.sandbox.alpaca.markets/v1`.

## Real-Time WebSocket

Alpaca publishes a **documented public WebSocket API** for real-time market data:

- **Stocks:** `wss://stream.data.alpaca.markets/v2/{feed}` where `{feed}` is `iex` (free Basic), `sip` (Algo Trader Plus), `delayed_sip`, or `test` (fake symbols, 24/7).
- **Crypto:** `wss://stream.data.alpaca.markets/v1beta3/crypto/us` (adds an `orderbooks` Level 2 channel; distributes Alpaca + Kraken data).
- **Options:** `wss://stream.data.alpaca.markets/v1beta1/{feed}`.
- **Sandbox mirror:** `wss://stream.data.sandbox.alpaca.markets`.

Connect, send `{"action":"auth","key":"...","secret":"..."}`, then a `{"action":"subscribe","trades":["AAPL"],"quotes":["*"],...}` message. Channels: `trades`, `quotes`, `bars`, `dailyBars`, `updatedBars`, `statuses`, `lulds`, `corrections`, `cancelErrors`, and (crypto) `orderbooks`. The Trading API also exposes a separate account/order WebSocket at `wss://{paper-}api.alpaca.markets/stream` (`trade_updates`). See [`asyncapi/alpaca-markets-asyncapi.yml`](asyncapi/alpaca-markets-asyncapi.yml).

## Tags

- Market Data
- Trading
- Brokerage
- Stocks
- Crypto
- Options
- FX Trading
- Financial Data
- Streaming
- WebSocket

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Alpaca Trading API

Commission-free trading of US stocks, ETFs, options, and crypto. Submit, replace, and cancel orders; read account balances, positions, portfolio history, and account activities; browse tradable assets; and manage watchlists, the market calendar, and clock. Runs identically against the free paper-trading sandbox and a funded live account.

- **Human URL:** [https://docs.alpaca.markets/reference/getaccount-1](https://docs.alpaca.markets/reference/getaccount-1)
- **Base URL:** `https://api.alpaca.markets/v2` (paper: `https://paper-api.alpaca.markets/v2`)

#### Tags

- Trading
- Orders
- Stocks
- Crypto
- Brokerage

#### Properties

- [Documentation](https://docs.alpaca.markets/docs/trading-api)
- [API Reference](https://docs.alpaca.markets/reference/getaccount-1)
- [OpenAPI](openapi/alpaca-markets-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/alpaca-markets.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/alpaca-markets.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Alpaca Market Data API

Historical and real-time pricing data for US stocks (SIP and IEX feeds), crypto, and options - bars, trades, quotes, snapshots, auctions, orderbooks, latest values, plus a news feed, corporate actions, and a most-actives / movers screener. REST over `data.alpaca.markets` with v2 stocks, v1beta3 crypto, and v1beta1 options / news / screener namespaces.

- **Human URL:** [https://docs.alpaca.markets/reference/stockbars](https://docs.alpaca.markets/reference/stockbars)
- **Base URL:** `https://data.alpaca.markets`

#### Tags

- Market Data
- Financial Data
- Stocks
- Crypto
- Options

#### Properties

- [Documentation](https://docs.alpaca.markets/docs/about-market-data-api)
- [API Reference](https://docs.alpaca.markets/reference/stockbars)
- [OpenAPI](openapi/alpaca-markets-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/alpaca-markets.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/alpaca-markets.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Alpaca Market Data Streaming API

Real-time market data over WebSocket. Stocks stream at `wss://stream.data.alpaca.markets/v2/{feed}` (iex, sip, delayed_sip, test) and crypto at `wss://stream.data.alpaca.markets/v1beta3/crypto/us`. Authenticate with an auth message, then subscribe to trades, quotes, bars, dailyBars, updatedBars, statuses, lulds, and (crypto) orderbooks per symbol or with a wildcard. This is a documented public `wss://` API.

- **Human URL:** [https://docs.alpaca.markets/docs/streaming-market-data](https://docs.alpaca.markets/docs/streaming-market-data)
- **Base URL:** `wss://stream.data.alpaca.markets`

#### Tags

- Streaming
- WebSocket
- Market Data
- Real Time
- Trading

#### Properties

- [Documentation](https://docs.alpaca.markets/docs/streaming-market-data)
- [API Reference](https://docs.alpaca.markets/docs/real-time-crypto-pricing-data)
- [AsyncAPI](asyncapi/alpaca-markets-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/v2.6.0)

### Alpaca Broker API

Build brokerage and trading products for end users. Open and manage customer brokerage accounts (KYC/onboarding), move money via ACH and wire transfers, journal cash and securities between accounts, place trades on behalf of customers, and access documents and events. Sandbox at `broker-api.sandbox.alpaca.markets` uses HTTP Basic auth.

- **Human URL:** [https://docs.alpaca.markets/docs/about-broker-api](https://docs.alpaca.markets/docs/about-broker-api)
- **Base URL:** `https://broker-api.alpaca.markets/v1`

#### Tags

- Brokerage
- Broker
- Accounts
- Embedded Finance
- Trading

#### Properties

- [Documentation](https://docs.alpaca.markets/docs/about-broker-api)
- [API Reference](https://docs.alpaca.markets/reference/createaccount)
- [OpenAPI](openapi/alpaca-markets-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/alpacahq)
- [Website](https://alpaca.markets/)
- [Documentation](https://docs.alpaca.markets)
- [GitHub Organization](https://github.com/alpacahq)
- [Plans](plans/alpaca-markets-plans-pricing.yml)
- [Rate Limits](rate-limits/alpaca-markets-rate-limits.yml)
- [Fin Ops](finops/alpaca-markets-finops.yml)
- [Blog](https://alpaca.markets/blog)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
