# Notes

## Base / Quote Currency Definition

Base Currency is the first currency that appears in a crypto market pair.

Quote Currency is the second currency that appears in a crypto market pair.

**Example**: For the market BTC / USD, the base currency is BTC (Bitcoin) and the quote currency is USD (U.S. Dollar).

## Buyer Maker Definition

These examples define limit orders as orders that do NOT execute immediately and are placed on the order book.

These examples define market orders as orders that execute immediately and are NOT placed on the order book.

**Example**: When a market SELL order is executed and matched against a passive limit BUY order, Cryptick records this trade as `buyerMaker: true`.

**Example**: When a market BUY order is executed and matched against a passive limit SELL order, Cryptick records this trade as `buyerMaker: false`.
