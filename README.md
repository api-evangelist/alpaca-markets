# Alpaca (alpaca-markets)

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
