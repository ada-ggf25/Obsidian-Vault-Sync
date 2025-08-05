#alpaca #api #computer_science #data_science #quantitative_finance 

## Summary

In the Alpaca Python client’s `get_bars` method, the **`adjustment`** parameter controls how corporate actions—namely stock splits and cash dividends—are applied to the OHLC bar data returned by the API [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/). Valid values are:

- **`raw`** (the default),
- **`split`**,
- **`dividend`**,
- **`all`** [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/).

---

## Adjustment Values

### `raw`

- Returns **unadjusted** prices: all open, high, low, close (OHLC) values reflect the actual traded prices, including any gaps or discontinuities caused by splits or dividends [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/).
- This is also the **default** behavior in the Python SDK, as `adjustment='raw'` is the method’s default argument [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/blob/master/alpaca_trade_api/rest.py?ref=alpaca.markets).

### `split`

- **Adjusts for stock splits** by scaling historical prices prior to a split date according to the split ratio, so the series remains continuous across the split [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/).
- Ensures that backtests or analyses aren’t thrown off by sudden artificial gaps in the price caused by corporate splits [Alpaca Community Forum](https://forum.alpaca.markets/t/bars-api-returning-wrong-values/11662?utm_source=chatgpt.com).

### `dividend`

- **Adjusts for cash dividends** by applying a backward correction to pre-ex-dividend prices, effectively reflecting the price drop when a dividend is paid [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/).
- Provides a **total-return** view of the security by normalizing past prices for dividend payouts [GitHub](https://github.com/alpacahq/Alpaca-API/issues/175?utm_source=chatgpt.com).

### `all`

- Combines **both** split and dividend adjustments, delivering a fully adjusted price series for the most comprehensive historical perspective [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/historical/).

---

## How to Use

Here’s a quick example showing how to request split-adjusted daily bars for AAPL in Python:

```python
python
CopyEdit
from alpaca_trade_api.rest import REST, TimeFrame

api = REST(API_KEY, API_SECRET, BASE_URL)
bars = api.get_bars(
    "AAPL",
    TimeFrame.Day,
    limit=30,
    adjustment='split'
).df

```

You can replace `'split'` with `'raw'`, `'dividend'`, or `'all'` as needed [stackoverflow.comstackoverflow.com](https://stackoverflow.com/questions/64706924/how-to-obtain-split-and-dividend-adjusted-prices-using-alpaca-trade-api?utm_source=chatgpt.com).