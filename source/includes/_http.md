# HTTP

## Candle Sets (HTTP)

> Candle Sets Response Example:

```json
{
  "success": "true",
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

Path: `/v1/candleSets`

Full Endpoint: `https://api.cryptick.net/v1/candleSets`

### Candle Sets Request
| Key | Type | Details | Required
|-|-|-|-|
| marketId | number | Parent market id | true |

### Candle Sets Response

| Key | Type | Details |
|-|-|-|
| id | number | Candle set id |
| marketId | number | Candle set market id |
| type | number | [Candle set type](#candle-set-type) |
| size | number | Candle set size |
| timeType | number | [Candle set time type](#candle-set-time-type) |


## Candles (HTTP)

> Candles Response Example:

```json
{
  "success": "true",
  "data": [
    {
      "candleSetId": 16,
      "id": 601,
      "fromTradeId": 60000001,
      "toTradeId": 60100000,
      "open": 3666.6,
      "high": 3980,
      "low": 3662.4,
      "close": 3902.6,
      "volume": 397427000,
      "baseVolume": 103128,
      "buyerMakerVolume": 186263000,
      "buyerMakerBaseVolume": 48273.6,
      "vwap": 3854.7,
      "time": "2021-05-10T02:17:13.524Z"
    }
  ]
}
```

Get a subset of candles from a candle set.

Requests without a `fromId` are in descending order.

Requests with a `fromId` are in ascending order.


Path: `/v1/candles`

Full Endpoint: `https://api.cryptick.net/v1/candles`

### Candles HTTP Request

| Key | Type | Details | Required
|-|-|-|-|
| candleSetId | number | Parent candle set id | true |
| fromId | number | Inclusive id of the first candle returned | false |
| limit | number | Quantity of candles returned (Max: 1000) | false |

### Candles HTTP Response

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

## Markets (HTTP)

> Markets Response Example:

```json
{
  "success": "true",
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

Path: `/v1/markets`

Full Endpoint: `https://edge.cryptick.net/v1/markets`

### Markets HTTP Request

This request requires no parameters.

### Markets HTTP Response

| Key | Type | Details |
|-|-|-|
| id | number | Market id |
| exchange | number | Market exchange |
| symbol | number | Market symbol |
| type | number | [Market Type](#market-type) |

## Trades (HTTP)

> Trades Response Example:

```json
{
  "success": "true",
  "data": [
    {
      "marketId": 2,
      "id": 208226001,
      "amount": 80,
      "price": 3326.5,
      "buyerMaker": false,
      "externalId": "abb5694b-d4b5-ea3b-25fc-845d801bf0bc",
      "time": "2018-12-11T17:56:59.265Z"
    }
  ]
}
```

Get a subset of trades from a market. 

Requests without a `fromId` are in descending order.

Requests with a `fromId` are in ascending order.

Path: `/v1/trades`

Full Endpoint: `https://edge.cryptick.net/v1/trades`

### Trades HTTP Request

| Key | Type | Details | Required
|-|-|-|-|
| marketId | number | Parent market id | true |
| fromId | number | Inclusive id of the first trade returned | false |
| limit | number | Quantity of candles returned (Max: 1000) | false |

### Trades HTTP Response

| Key | Type | Details |
|-|-|-|
| marketId | number | Parent market id |
| id | number | Trade id |
| amount | number | Trade amount in [quote currency](#base-quote-currency-definition) |
| price | number | Trade price |
| buyerMaker | boolean | [Trade BuyerMaker](#buyer-maker-definition) |
| externalId | string | Exchange trade id |
| time | string | Trade time in format: YYYY-MM-DDTHH:mm:ss.sssZ (ISO 8601) |

