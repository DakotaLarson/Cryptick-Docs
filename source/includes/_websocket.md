# WebSocket

## WebSocket Introduction

WebSocket connections support data requests (like HTTP) and subscriptions.

All requests should be in JSON format.

### WebSocket Request Topics

> Request Topic Key Example:

```json
{
  "topic": "trades"
}
```

A request topic is required for every request.

Valid Request Topics:

- `trades`
- `orderBook`
- `markets`
- `candles`
- `candleSets`

### WebSocket Request Ids

> Request Id Key Example:

```json
{
  "id": 1
}
```

An id can be included with any request and will be present on the related response.

This is especially useful for [data requests](#websocket-data-requests).

## WebSocket Subscriptions

> Subscribe Request Example:

```json
{
  "topic": "trades",
  "subscribe": "trades"
}
```

> Unsubscribe Request Example:

```json
{
  "topic": "candles",
  "unsubscribe": "candlesRealtime"
}
```

Subscribe (or unsubscribe) to topics by passing a subscription topic.

Valid Subscription Topics:

- `trades` (topic: `trades`)
- `orderBook` (topic: `orderBook`)
- `candlesRealtime` (topic: `candles`)
- `candlesComplete` (topic: `candles`)

## WebSocket Data Requests

Cryptick supports historical data requests via WebSockets and HTTP requests.

### WebSocket Intelligent Subscriptions (Historical Data Merge)

Cryptick provides an easy solution to merge historical data with realtime updates.

This can greatly simplify an automated trading algorithm that needs to use recent historical data to seed indicators, etc.

This supports candles & trades.

## Candle Sets (WebSocket)

> Candle Sets Request Example:

```json
{
  "topic": "candleSets",
  "id": 420,
  "marketId": 3
}
```

> Candle Sets Response Example:

```json
{
  "topic": "candleSets",
  "id": 420,
  "data": [
    {
      "id": 239,
      "marketId": 3,
      "type": 1,
      "size": 15,
      "timeType": 2
    }
  ]
}
```

Get a list of accessible candle sets for a given market.

### Candle Sets WebSocket Request
| Key | Type | Details | Required
|-|-|-|-|
| marketId | number | Parent market id | true |

### Candle Sets WebSocket Response

| Key | Type | Details |
|-|-|-|
| id | number | Candle set id |
| marketId | number | Candle set market id |
| type | number | [Candle set type](#candle-set-type) |
| size | number | Candle set size |
| timeType | number | [Candle set time type](#candle-set-time-type) |

## Candles (WebSocket)

> Candles Request Example:

```json
{
  "topic": "candles",
  "id": 420,
  "candleSetId": 239,
  "fromId": 5,
  "limit": 1
}
```

> Candles Response Example:

```json
{
  "topic": "candles",
  "id": 420,
  "data": [
    {
      "candleSetId": 239,
      "id": 5,
      "fromTradeId": 5359,
      "toTradeId": 6549,
      "open": 5922,
      "high": 5930,
      "low": 5908,
      "close": 5911,
      "volume": 3071910,
      "baseVolume": 519018,
      "buyerMakerVolume": 771444,
      "buyerMakerBaseVolume": 130384,
      "vwap": 5918.69,
      "time": "2020-03-23T07:11:00.000Z"
    }
  ]
}
```

Get a subset of candles from a candle set.

Requests without a `fromId` are in descending order.

Requests with a `fromId` are in ascending order.

### Candles WebSocket Request

| Key | Type | Details | Required
|-|-|-|-|
| candleSetId | number | Parent candle set id | true |
| fromId | number | Inclusive id of the first candle returned | false |
| limit | number | Quantity of candles returned (Max: 1000) | false |

### Candles WebSocket Response

| Key | Type | Details |
|-|-|-|
| candleSetId | number | Candle set id |
| id | number | Candle id |
| fromTradeId | number | Id of the earliest included trade |
| toTradeId | number | Id of the latest included trade |
| open | number | Candle open price |
| high | number | Candle high price |
| low | number | Candle low price |
| close | number | Candle close price |
| volume | number | Candle volume in [quote currency](#base-quote-currency-definition) |
| baseVolume | number | Candle volume in [base currency](#base-quote-currency-definition) |
| buyerMakerVolume | number | Candle market sell / limit buy volume in [quote currency](#base-quote-currency-definition) |
| buyerMakerBaseVolume | number | Candle market sell / limit buy volume in [base currency](#base-quote-currency-definition) |
| vwap | number | Volume weighted average price |
| time | string | Candle close time in format: YYYY-MM-DDTHH:mm:ss.sssZ (ISO 8601) |

See the [BuyerMaker](#buyer-maker-definition) definition.

## Markets (WebSocket)

> Markets Request Example:

```json
{
  "topic": "markets",
  "id": 420
}
```

> Markets Response Example:

```json
{
  "topic": "markets",
  "id": 420,
  "data": [
    {
      "id": 1,
      "exchange": "Binance",
      "symbol": "BTCUSDT",
      "type": 1
    }
  ]
}
```

Get a list of all supported markets.

### Markets WebSocket Request

This request requires no additional parameters.

### Markets WebSocket Response

| Key | Type | Details |
|-|-|-|
| id | number | Market id |
| exchange | number | Market exchange |
| symbol | number | Market symbol |
| type | number | [Market Type](#market-type) |

## Trades (WebSocket)

> Trades Request Example

```json
{
  "topic": "trades",
  "id": 420,
  "marketId": 1,
  "fromId": 543444001,
  "limit": 1
}
```

> Trades Response Example:

```json
{
  "topic": "trades",
  "id": 420,
  "data": [
    {
      "marketId": 1,
      "id": 543444001,
      "amount": 3676.75,
      "price": 39114.4,
      "buyerMaker": true,
      "externalId": "544023881",
      "time": "2021-05-27T18:53:24.674Z"
    }
  ]
}
```

Get a subset of trades from a market.

Requests without a `fromId` are in descending order.

Requests with a `fromId` are in ascending order.

### Trades WebSocket Request

| Key | Type | Details | Required
|-|-|-|-|
| marketId | number | Parent market id | true |
| fromId | number | Inclusive id of the first trade returned | false |
| limit | number | Quantity of candles returned (Max: 1000) | false |

### Trades WebSocket Response

| Key | Type | Details |
|-|-|-|
| marketId | number | Parent market id |
| id | number | Trade id |
| amount | number | Trade amount in [quote currency](#base-quote-currency-definition) |
| price | number | Trade price |
| buyerMaker | boolean | [Trade BuyerMaker](#buyer-maker-definition) |
| externalId | string | Exchange trade id |
| time | string | Trade time in format: YYYY-MM-DDTHH:mm:ss.sssZ (ISO 8601) |
