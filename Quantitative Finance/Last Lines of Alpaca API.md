#alpaca #computer_science #data_science #api #quantitative_finance 

I'm usign this code:

# Fetch the historical data

bars = api.get_bars( symbol, timeframe, start, end, feed=data_source ).df

to get information about the market using the ALPACA API is there any way for me to retrieve the last 36 lines instead of the demanded dataset instead of specifing the lines using "start" and "end"?

### ChatGPT said:

Thought for 20 seconds

## Summary

You don’t need to specify explicit `start`/`end` timestamps if you only want the _most recent_ N bars. The Alpaca Python client’s `get_bars` method supports a `limit` parameter, which (when no `end` is provided) defaults to _now_ and returns that many bars counting backwards. Alternatively, if you’ve already fetched a larger dataset, you can simply take the last 36 rows with pandas.

---

## 1. Use the `limit` Parameter

The simplest approach is to pass `limit=36` to `get_bars` and omit both `start` and `end`. For example:

```python
python
CopyEdit
from alpaca_trade_api.rest import REST, TimeFrame

api = REST(API_KEY, API_SECRET, BASE_URL)

# Fetch the last 36 one-minute bars for AAPL
bars = api.get_bars(
    "AAPL",
    TimeFrame.Minute,
    limit=36,
    adjustment='raw'
).df

```

- **What happens internally?**
    
    When you provide `limit` but no `end`, Alpaca treats the end as “now” and returns that many bars counting backwards in time [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/pull/519?utm_source=chatgpt.com).
    
- **Why this works:**
    
    As shown in the Alpaca forum, you can specify `limit` alongside your other parameters; if `start` is omitted, it simply gives you the most recent bars up to the current moment [Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).
    

---

## 2. Slice an Existing DataFrame

If for some reason you’ve already pulled a larger dataset (e.g. an entire day’s worth of bars) and just want the tail, you can use pandas:

```python
python
CopyEdit
# Say `bars` contains hundreds of rows...
last_36 = bars.tail(36)

```

This is purely a pandas operation and works regardless of how you fetched the data.

---

### Notes on Behavior

- **Ascending order guarantee:** Alpaca always returns bars in **ascending** chronological order (oldest first). So `.tail(36)` truly gives you the last 36 [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v1/bars/?utm_source=chatgpt.com).
- **Tier and rate limits:** On the free tier you may hit limits if you request too many bars at once (e.g. `limit=2000`), but for 36 bars this will not be an issue [Alpaca Community Forum](https://forum.alpaca.markets/t/get-bars-per-minute-only-return-724-bars/10282?utm_source=chatgpt.com).

---

With either of these methods, you avoid manually computing `start` and `end` timestamps and get precisely the last 36 bars you need.