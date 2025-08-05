#api #computer_science #quantitative_finance #algorithms #data_science #tensorflow #machine_learning 

[[Spliting Data & Dividens with Alpaca API]]
[[Last Lines of Alpaca API]]
[[Alpaca API get_bars() full guide]]

If I have a free Alpaca Subscription can I get Stock Market data from SIP? If yes, With how much delay?

### ChatGPT said:

Thought for 15 seconds

## Summary

With Alpaca’s Free (Basic) market data subscription, you receive **real‑time data from the IEX exchange** and can also query **SIP (CTA/UTP) data**, but **only for timestamps at least 15 minutes in the past**. Attempting to retrieve SIP data for the most recent 15‑minute window will result in a permission error. To get **real‑time SIP data**, you must upgrade to the Unlimited plan.

---

## Free Plan Data Sources

- **IEX (Investors Exchange LLC)** real‑time quotes, trades, bars, and snapshots.
    
- **SIP (CTA/UTP) feeds** are available, but **only for data older than 15 minutes**.
    
    > “The Free plan consists of data from IEX (Investors Exchange LLC). For the Unlimited plan, we receive direct feeds from the CTA (administered by NYSE) and UTP (administered by Nasdaq) SIPs.” Alpaca
    

---

## Accessing SIP with Free Subscription

- If you explicitly set `feed='sip'` (or rely on the default for SIP requests), your **end datetime** must be **at least 15 minutes earlier** than “now.” Otherwise, you’ll see:
    
    ```
    json
    CopyEdit
    {"message":"subscription does not permit querying recent SIP data"}
    
    ```
    
- Free subscribers **can** retrieve **historical SIP quotes, trades, and bars** once they exceed the 15‑minute delay.
    
    > “Subscribers to the Free plan can access full market SIP quotes, trades and bars if less recent than 15 minutes from the current time.” Alpaca Community Forum
    

---

## Delay Details

- The **exact delay** on SIP data for Free users is **15 minutes**.
    
- All Standard plans (including Basic/Free) share this characteristic: **real‑time IEX** but **15‑min delayed SIP**.
    
    > “Similar to the free plan, all the standard plans are real time IEX or 15 mins delayed SIP.” Alpaca API Docs
    
- Documentation tables confirm:
    
    - **Historical data delay** for Free plan: **15 minutes**
    - **Unlimited plan**: **no delay** on SIP [Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/market-data/alpaca-data-api-v2/?utm_source=chatgpt.com)

---

## Real‑Time SIP Access (Paid Upgrade)

- To remove the 15‑minute restriction and access **real‑time SIP** data, you must subscribe to Alpaca’s **Unlimited** market data plan (currently **$9/month**).
    
    > “With it [the Unlimited subscription], you can access the latest SIP data without delay.” Alpaca Community Forum
    

---

**In practice**, if you’re on the Free plan and want the freshest SIP data, you’ll need to wait **15 minutes** after the event. For true real‑time SIP feeds, consider upgrading to the Unlimited plan.


## Summary

The `api.get_clock()` endpoint returns the **official U.S. equity market clock**—i.e. whether the primary markets (NYSE/NASDAQ) are open and the timestamps for their next open and close. It is **not** tied to a specific data feed (IEX or SIP); instead, it reflects the general market session during which both IEX and SIP feeds distribute quotes and trades.

---

## What Does `get_clock()` Return?

- **`is_open`**: A boolean indicating whether the **U.S. equity markets** are currently in their regular trading session (9:30 AM–4:00 PM Eastern) [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/clock/?utm_source=chatgpt.com).
- **`timestamp`**: The current market timestamp, aligned with the market clock rather than your local machine’s clock [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/sdks/python/api_reference/trading/clock.html?utm_source=chatgpt.com).
- **`next_open` / `next_close`**: ISO 8601/RFC 3339 strings for the upcoming market open and close times, again reflecting the **primary exchange hours**, not feed-specific schedules [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/clock/?utm_source=chatgpt.com).

---

## Relationship to IEX and SIP Feeds

- **IEX feed** (real‑time on Free plan) and **SIP feeds** (CTA/UTP, delayed on Free plan) both follow the **same core market hours** defined by NYSE/NASDAQ. The clock API simply tells you if the markets are in session—not which feed is active [Alpaca](https://alpaca.markets/support/data-provider-alpaca?utm_source=chatgpt.com)[Alpaca API Docs](https://docs.alpaca.markets/docs/real-time-stock-pricing-data?utm_source=chatgpt.com).
- During regular hours (`is_open = True`), **IEX** is streaming **real‑time** data for Free subscribers, and **SIP** is streaming data subject to any subscription delay (15 min for Free) [Alpaca](https://alpaca.markets/support/data-provider-alpaca?utm_source=chatgpt.com)[Reddit](https://www.reddit.com/r/algotrading/comments/17ovh4u/question_on_limitation_with_iex_only_free_plan_on/?utm_source=chatgpt.com).
- Outside those hours (`is_open = False`), **neither** IEX nor SIP will produce new **regular‑session** data, though IEX may support extended‑hours for certain endpoints (not controlled by `get_clock()`) [Alpaca API Docs](https://docs.alpaca.markets/docs/real-time-stock-pricing-data?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/sdks/python/api_reference/trading/clock.html?utm_source=chatgpt.com).

---

## Interpreting Your Example

```python
python
CopyEdit
clock = api.get_clock()
print(clock)
# Clock({
#   'is_open': True,
#   'next_close': '2025-04-21T16:00:00-04:00',
#   'next_open': '2025-04-22T09:30:00-04:00',
#   'timestamp': '2025-04-21T13:47:19.43610036-04:00'
# })

```

- **`is_open: True`** means the **regular U.S. equity market** is currently trading (not specifically IEX or SIP) [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/clock/?utm_source=chatgpt.com).
- The **`timestamp`** is Alpaca’s view of the current market time, which you should use to ensure your 15‑minute SIP delay cutoff [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com).
- **`next_close`**/`next_open` reflect **NYSE/NASDAQ** hours (4 PM and 9:30 AM Eastern) and apply universally across feeds [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/clock/?utm_source=chatgpt.com).

---

## Key Takeaways

- **`get_clock()` is feed‑agnostic**: It reports the **primary market session**, not the status of IEX vs. SIP data feeds [Alpaca](https://alpaca.markets/support/data-provider-alpaca?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/sdks/python/api_reference/trading/clock.html?utm_source=chatgpt.com).
- Use **`timestamp`** from `get_clock()` to calculate your SIP‐delay cutoff accurately, ensuring your queries on the Free plan are ≥ 15 minutes behind market time [Alpaca API Docs](https://docs.alpaca.markets/reference/getclock-1?utm_source=chatgpt.com)[Reddit](https://www.reddit.com/r/algotrading/comments/17ovh4u/question_on_limitation_with_iex_only_free_plan_on/?utm_source=chatgpt.com).
- For **extended‑hours** trading or feed‐specific schedules, consult the individual feed documentation—`get_clock()` only covers **regular trading hours** [Alpaca](https://alpaca.markets/sdks/python/api_reference/trading/clock.html?utm_source=chatgpt.com)[Alpaca](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/clock/?utm_source=chatgpt.com).

## Summary

When you call `api.get_bars(...).df`, the returned DataFrame’s index (and its `timestamp` column) is **timezone‑aware** and always set to **UTC** [Alpaca Community Forum](https://forum.alpaca.markets/t/what-is-wrong-with-get-bars-between-start-and-end/11579?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/timezone-for-historicalbarsrequest/11537?utm_source=chatgpt.com).

---

## Default Timezone

All bar data returned by Alpaca’s market‑data API v2 are tagged with the UTC timezone, regardless of the timezone you used for the `start`/`end` parameters [Alpaca Community Forum](https://forum.alpaca.markets/t/timezone-for-historicalbarsrequest/11537?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/incorrect-historical-trades-info-using-sdk/12851?utm_source=chatgpt.com). Even if you specify a different offset on input, the output will still be in UTC [Alpaca Community Forum](https://forum.alpaca.markets/t/incorrect-historical-trades-info-using-sdk/12851?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/why-is-timezone-eastern-time-0500z/15637?utm_source=chatgpt.com).

---

## Converting to Local (Eastern) Time

If you need to view bars in U.S. Eastern Time (ET), convert the index after fetching:

```python
python
CopyEdit
df = api.get_bars(...).df
df = df.tz_convert('America/New_York')

```

This will shift the UTC timestamps into Eastern Time (including proper handling of DST) [GitHub](https://github.com/alpacahq/alpaca-trade-api-python/issues/457?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/is-timezone-configurable-in-the-ui/1504?utm_source=chatgpt.com).

---

## Verifying in Your Code

You can confirm the timezone in your DataFrame by inspecting:

```python
python
CopyEdit
print(df.index.tz)  # → UTC
print(df.index.dtype)  # → datetime64[ns, UTC]

```

This behavior is consistent across other market‑data endpoints (e.g., historical trades, quotes)—they too return UTC‑tagged timestamps [Alpaca Community ForumAlpaca Community Forum](https://forum.alpaca.markets/t/incorrect-historical-trades-info-using-sdk/12851?utm_source=chatgpt.com).

---

## API v2 Documentation

Alpaca’s v2 API serializes all date‑time inputs and outputs according to RFC 3339 (ISO 8601) with explicit offsets, but it uses UTC for all returned data. You’ll see timestamps like `2025-04-21T13:47:19.43610036Z` (the trailing `Z` denotes UTC) [alpaca.markets](https://alpaca.markets/deprecated/docs/api-documentation/api-v2/?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/why-is-timezone-eastern-time-0500z/15637?utm_source=chatgpt.com).

## Summary

The error

> Invalid format for parameter start: error parsing '2025-04-21T15:49:12.774699' as RFC3339 or 2006-01-02 time
> 
> occurs because Alpaca’s Market Data API **requires** either a **date-only** string (`YYYY-MM-DD`) **or** a **full RFC 3339 timestamp**, which must include:

1. A **`T`** separator between date and time,
    
2. A **time‐zone offset** (e.g. `Z` or `±HH:MM`) [Alpaca API Docs](https://docs.alpaca.markets/reference/stockbars?utm_source=chatgpt.com).
    
    Your string (`2025-04-21T15:49:12.774699`) has the `T` but **omits** the offset, so the API rejects it [Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).
    

---

## RFC 3339 vs. ISO 8601 Requirements

- **RFC 3339** is a **profile of ISO 8601** tailored for Internet protocols and **mandates**:
    - A **four‑digit year** and full date
    - The **`T`** delimiter (not a space) before the time component [IETF](https://www.ietf.org/rfc/rfc3339.txt?utm_source=chatgpt.com)
    - A **time‐zone designator** of either `Z` (Zulu, i.e. UTC) or `±HH:MM` [Stack Overflow](https://stackoverflow.com/questions/522251/whats-the-difference-between-iso-8601-and-rfc-3339-date-formats?utm_source=chatgpt.com)
- **ISO 8601** allows certain variations (including optional spaces), but RFC 3339 **strictly** enforces the above subset for interoperability [Wikipedia](https://en.wikipedia.org/wiki/ISO_8601?utm_source=chatgpt.com).

---

## Alpaca’s Date‑Parameter Enforcement

According to the **Historical Bars API Reference**:

> Format: RFC 3339 or YYYY-MM-DD. Default: current time if feed isn’t SIP or you have a paid SIP subscription; otherwise 15 minutes before current time. Alpaca API Docs
> 
> Any deviation from the RFC 3339 profile—such as omitting the `Z`/offset—triggers a **400 Bad Request** with the “Invalid format” error [Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).

---

## How to Fix It

### 1. Use Timezone‑Aware `datetime.isoformat()`

```python
python
CopyEdit
from datetime import datetime, timezone, timedelta

# Example: UTC timestamps with 'Z'
now_utc   = datetime.now(timezone.utc)
start_str = (now_utc - timedelta(hours=3)).isoformat()  # ⇒ '2025-04-21T12:49:12.774699+00:00'
end_str   = (now_utc - timedelta(minutes=15)).isoformat()  # ⇒ '2025-04-21T13:34:12.774699+00:00'
# Optionally replace '+00:00' with 'Z' for pure Zulu notation:
start_str = start_str.replace('+00:00', 'Z')  # ⇒ '2025-04-21T12:49:12.774699Z'
end_str   = end_str  .replace('+00:00', 'Z')  # ⇒ '2025-04-21T13:34:12.774699Z'

```

This approach guarantees a proper `T` separator and a valid offset or `Z` [Alpaca Community Forum](https://forum.alpaca.markets/t/need-help-with-start-time-in-specific-rfc-3339-time-format/8244?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).

### 2. Manually Format with `strftime` and Insert the Colon

If you need precise control over the format:

```python
python
CopyEdit
fmt       = '%Y-%m-%dT%H:%M:%S.%f%z'
start_dt  = datetime.now(timezone.utc) - timedelta(hours=3)
start_raw = start_dt.strftime(fmt)             # e.g. '2025-04-21T12:49:12.774699+0000'
start_str = start_raw[:-2] + ':' + start_raw[-2:]  # '2025-04-21T12:49:12.774699+00:00'

```

This ensures the offset adheres to `±HH:MM` [Gentoo Wiki](https://wiki.gentoo.org/wiki/Systemwide_RFC-3339_Dateformat?utm_source=chatgpt.com)[Wikipedia](https://en.wikipedia.org/wiki/Date_and_time_notation?utm_source=chatgpt.com).

---

## Putting It All Together

Here’s a complete snippet for SIP‐delayed bar retrieval in GMT‑4 converted to valid RFC 3339 UTC (`Z`) timestamps:

```python
python
CopyEdit
import time
from datetime import datetime, timedelta, timezone
from alpaca_trade_api import REST, TimeFrame

api = REST(API_KEY, SECRET_KEY, BASE_URL, api_version='v2')

# Calculate UTC times (Zulu), respecting a 15-min SIP delay
now_utc   = datetime.now(timezone.utc)
start_str = (now_utc - timedelta(days=1)).isoformat().replace('+00:00', 'Z')
end_str   = (now_utc - timedelta(minutes=15)).isoformat().replace('+00:00', 'Z')

symbol, tf, feed = "AAPL", TimeFrame(5, TimeFrame.Unit.Minute), "sip"
all_data = []

# Single request (example)
bars = api.get_bars(symbol, tf, start=start_str, end=end_str, feed=feed).df
all_data.append(bars)

```

- **`.isoformat()`** alone on a naive `datetime` yields no offset, causing your original error [Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).
- By making the `datetime` **timezone‑aware** (`timezone.utc`) and/or appending `Z`, you satisfy RFC 3339 and avoid parsing errors [Alpaca API Docs](https://docs.alpaca.markets/reference/stockbars?utm_source=chatgpt.com)[Alpaca Community Forum](https://forum.alpaca.markets/t/python-api-get-bars-limitations/9897?utm_source=chatgpt.com).