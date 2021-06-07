---
title: API Reference

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - http
  - websocket
  - errors
  - enums
  - notes

search: true

code_clipboard: false
---

# Introduction

Welcome to the Cryptick API!

If you have any questions or feedback relating to the API, please [reach out](https://cryptick.net/contact)!

## Endpoints

Cryptick currently has 2 endpoints you can integrate with: Server and Edge.

### Server Endpoints

The server handles the following requests:

- `GET /v1/candleSets`
- `GET /v1/candles`

API Endpoint: `https://api.cryptick.net`

WebSocket Endpoint: `wss://api.cryptick.net/v1/realtime`

### Edge Endpoints

The edge handles the following requests:

- `GET /v1/markets`
- `GET /v1/trades`

API Endpoint: `https://edge.cryptick.net`

WebSocket Endpoint: `wss://edge.cryptick.net/v1/realtime`

As Cryptick grows, multiple edge endpoints will become available to reduce latency.

# Authentication

Both HTTP requests and WebSockets are authenticated using the header: `Authorization: <key>` (replacing `<key>` with your API key).

All HTTP requests must be authenticated.

All WebSocket requests must be authenticated EXCEPT for these subscriptions:

- Realtime Trades
- Order Books
- Candles for the public Candle Sets:
  - 1 Minute
  - 15 Minute
  - 1 Hour
  - 4 Hour
  - 1 Day
  - 100k Tick
  - 1M Volume (Quote)

WebSockets can only be authenticated as part of the initial connection. You will need to create a new connection if you need to authenticate with a different API key or didn't authenticate initially.

# Rate Limits

Current rate limit:  10 requests / 5 seconds.

Requests are limited per account, across all connections.

If you make 5 requests via HTTP and 6 requests via WebSocket in under 5 seconds, the last request will be rate limited.

## HTTP Rate Limits

> HTTP Rejected Response (Status: 429):

```json
{
  "error": "Too many requests"
}
```

All HTTP responses will have headers `X-RateLimit-Limit` and `X-RateLimit-Remaining`.

- `X-RateLimit-Limit` indicates the maximum number of requests in a timeframe.
- `X-RateLimit-Remaining` indicates the number of requests remaining in a timeframe. 

HTTP rejected responses will have a status code of 429 and header `X-RateLimit-Retry`. 

- `X-RateLimit-Retry` indicates the number of milliseconds to wait before at least 1 request will be successful.

## WebSocket Rate Limits

> WebSocket Rejected Response (with 500ms retry value):

```json
{
  "error": "Too many requests",
  "retry": 500
}
```

Unauthenticated WebSockets are subject to the same limits (20 requests / 10 seconds) per connection.

WebSocket rejected requests will have a `retry` key indicating the number of milliseconds to wait before sending another request.
