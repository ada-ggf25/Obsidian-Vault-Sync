#computer_science #alpaca #data_science #quantitative_finance #guide 

## Summary

The Python `get_bars()` method in the Alpaca client wraps the Market Data V2 **Historical Bars** endpoint (`GET /v2/stocks/bars`) and exposes nine main parameters you can use to customize your request:

- **`symbol`** or **`symbols`** – one ticker or a list of tickers
- **`timeframe`** – granularity of each bar (e.g. 1-Minute, 5-Minute, 1-Hour, 1-Day)
- **`start`** and **`end`** – ISO-8601 timestamps defining the inclusive time window
- **`adjustment`** – how corporate actions (splits, dividends) are applied
- **`limit`** – maximum total bars returned
- **`feed`** – data feed source (`iex`, `sip`, or `otc`)
- **`asof`** – “as-of” date for symbol mapping (e.g. before/after name changes)
- **`sort`** – order of the returned bars (`asc` or `desc`)

All parameters are optional except `symbol` and `timeframe`, and sensible defaults are provided (e.g. `adjustment='raw'`, `sort='asc'`, free users default to the `iex` feed). [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

---

## Available Parameters

### `symbol: Union[str, List[str]]`

One ticker symbol (e.g. `"AAPL"`) or a list of symbols (e.g. `["AAPL","TSLA"]`). When you pass multiple symbols, the API returns bars grouped by symbol. [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)

### `timeframe: TimeFrame`

Defines the bar interval. Use the `TimeFrame` enum:

- `TimeFrame.Minute` / `"1Min"` (1–59)
- `TimeFrame.Hour` / `"1Hour"` (1–23)
- `TimeFrame.Day` / `"1Day"`
- `TimeFrame.Week` / `"1Week"`
- `TimeFrame.Month` / `"1Month"` [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?utm_source=chatgpt.com)

### `start: Optional[str]`

ISO-8601 timestamp (e.g. `"2025-04-01T09:30:00-04:00"`) specifying the **inclusive** start of the interval. Defaults to the beginning of the current day if omitted. [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

### `end: Optional[str]`

ISO-8601 timestamp specifying the **inclusive** end of the interval. Defaults to “now” if omitted. [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

### `adjustment: str = 'raw'`

Controls corporate action adjustments on OHLC prices. Options:

- **`'raw'`** (default): no adjustment [Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)
- **`'split'`**: adjust for stock splits
- **`'dividend'`**: adjust for cash dividends
- **`'all'`**: both split and dividend adjustments

### `limit: Optional[int]`

Maximum number of bars **total** to return. The service may return fewer, and you can page through using next-page tokens (not exposed in `get_bars` directly). [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

### `feed: Optional[str]`

Choose the data feed source:

- **`'iex'`** – free IEX feed (default for non-subscribers)
- **`'sip'`** – consolidated SIP feed (premium subscription)
- **`'otc'`** – OTC feed (requires special permission) [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

### `asof: Optional[str]`

A date (`"YYYY-MM-DD"`) used to map symbols according to their historical identities (e.g. before/after a ticker rename). Defaults to today. Use `"-"` to skip mapping. [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[Postman API Platform](https://www.postman.com/alpacamarkets/alpaca-public-workspace/documentation/4bx4njh/market-data-v2-api)

### `sort: Optional[Sort]`

Order of the returned bars by timestamp. Values:

- **`Sort.Asc`** (`"asc"`, default) – oldest first
- **`Sort.Desc`** (`"desc"`) – newest first [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets)[GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?utm_source=chatgpt.com)

---

## Example Usage

```python
python
CopyEdit
from alpaca_trade_api.rest import REST, TimeFrame, Sort

api = REST(API_KEY, API_SECRET, BASE_URL)

# Fetch the last 50 5-minute bars for AAPL,
# adjusted for splits only, descending order:
bars = api.get_bars(
    "AAPL",
    TimeFrame(5, TimeFrameUnit.Minute),  # 5-minute bars
    limit=50,
    adjustment='split',
    feed='sip',
    sort=Sort.Desc
).df

```

This call returns a DataFrame of the 50 most recent 5-minute bars, split-adjusted, from the SIP feed, with the newest bar first.