---
name: Fetch Origin post-trade and termsheet data
description: Retrieve bond trade, termsheet, and post-trade data from the Origin Trades API (Airbrush v3), structured against the ISO 20022-aligned Airbrush data language.
api: openapi/origin-airbrush-openapi-original.yml
operations:
  - "GET /trades/"
  - "GET /trades/{TradeId}/termsheet-data/"
  - "GET /trades/{TradeId}/post-trade-data/"
---

# Fetch Origin post-trade and termsheet data

The Origin Trades API (Airbrush v3) is a read-only API for retrieving bond issuance data
for the trades your API key is entitled to see. All responses are structured against the
Airbrush "universal data language" (ISO 20022-aligned).

## Authentication

- Send your Origin-issued key in the `API-Key` request header on every call.
- Omitting the key returns the interactive documentation page, not data.
- An invalid key returns HTTP 403 with body `{"detail": "Invalid API Key"}`. Copy the key
  exactly as provided; contact Origin support if it still fails.
- Base URL: `https://airbrush.originmarkets.com/v3`. All responses are `application/json`.

## Steps

1. **List accessible trades** — `GET /trades/` returns a list of trade objects your key can
   access. Read each object's identifier to drive the follow-up calls.
2. **Get termsheet data** — `GET /trades/{TradeId}/termsheet-data/` returns the termsheet-stage
   data for one trade (issuer(s), programme, coupon/interest, denominations, listings, etc.).
3. **Get post-trade data** — `GET /trades/{TradeId}/post-trade-data/` returns the post-trade
   data (clearing, agents, identifiers) for one trade.

## Conventions and error handling

- The API is read-only (GET only); there are no write operations and no idempotency key.
- No pagination: `GET /trades/` returns the full accessible list.
- Errors use a plain `{"detail": "<message>"}` envelope (not RFC 9457). Treat any 403 as an
  authentication failure, not a per-trade permission signal.
- Entity shapes follow the Airbrush data model: a Trade has one Redemption/Clearing/Programme/
  Interest/Identifiers and many Issuers/Dealers/Guarantors/Ratings/Listings.
