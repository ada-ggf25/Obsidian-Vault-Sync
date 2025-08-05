#api #computer_science #data_science #quantitative_finance #polygon #alpaca

[[Polygon Hours API]]

# Free APIs for Real-Time US Market Session Information

## Overview

Developers seeking **real-time U.S. stock market session info** (using SIP consolidated data) have a few free API options. These APIs can indicate if the market is currently open or closed, including whether we are in regular hours or extended pre-market/after-hours sessions. They can also provide recent closing times and upcoming opening times. Below we outline several free (API-key accessible) services that meet these requirements, and compare their features.

## Free API Options

- **Finnhub** – A free financial data API with generous limits (60 calls/minute​[medium.com](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends)). It offers a _Market Status_ endpoint to check if major exchanges (e.g. US market) are open or closed​[finnhub.io](https://finnhub.io/docs/api#:~:text=API%20Documentation%20%7C%20Finnhub%20,). This returns a simple status (open vs. closed) for the current trading session​[stackoverflow.com](https://stackoverflow.com/questions/9428989/how-to-test-if-the-stock-market-nyse-is-currently-open-closed#:~:text=Finnhub,the%20function%20would%20look%20like). _Note:_ It treats the market as **closed during extended hours**, since it only reports whether the exchange is open or not (no separate flag for pre/post-market). There are no direct fields for next opening/closing timestamps in this endpoint. Separate endpoints (e.g. trading calendars or holiday schedules) would be needed to infer upcoming open/close times if required.
    
- **Alpaca** – A broker API that provides a free paper-trading account and data access. Its **Clock API** gives the current market timestamp, a boolean for whether the market is open, plus the time of the next market open and next close​[docs.alpaca.markets](https://docs.alpaca.markets/reference/getclock-1#:~:text=get%20https%3A%2F%2Fpaper). This means you get real-time open/closed status and **explicit timestamps** for the upcoming opening and closing of the regular session. For example, the clock might return `is_open: false` with `next_open` at 09:30 ET and `next_close` at 16:00 ET​[forum.alpaca.markets](https://forum.alpaca.markets/t/clock-api-and-extended-hours/118#:~:text=I%20am%20also%20in%20need,05%3A00%E2%80%99%2C%20is_open%3A%20false). However, Alpaca’s clock **does not indicate extended-hours trading** – it considers the market closed outside 9:30-16:00 regular hours (extended sessions are not distinguished in the response). The free tier allows about **200 API calls/minute** for data​[forum.alpaca.markets](https://forum.alpaca.markets/t/rate-limit-clarity/7202#:~:text=The%20standard%20rate%20limit%20is,increased%20to%201000%20API%20Calls%2FMin), which is quite generous. No payment is needed for the basic data (uses IEX data for free plan), and an API key is obtained by signing up for a free account.
    
- **Polygon.io** – A real-time market data API that offers a free tier (requires signup). The free plan allows **5 REST calls/minute**​[polygon.io](https://polygon.io/knowledge-base/article/what-is-the-request-limit-for-polygons-restful-apis#:~:text=REST). Polygon’s `/v1/marketstatus/now` endpoint provides a consolidated U.S. market status with rich detail. It returns whether the overall market is _open_, _closed_, or in **pre-market/after-hours**​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods). In the JSON response, it includes flags like `earlyHours` and `afterHours` to indicate if we are in extended trading, and it even breaks down status by exchange (NYSE, NASDAQ, etc.)​[postman.com](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%7B%20,). This makes it easy to know if we’re within an extended session. However, the **current status endpoint does not directly return the next open or next close timestamp**. You can obtain schedule info via a separate “market holidays/upcoming” endpoint, which lists upcoming market days or holidays with their planned open and close times​[postman.com](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%5B%20%7B%20,). In summary, Polygon excels at real-time status (including extended-hours)​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods), but you would use its calendar/holidays API to get the next opening/closing timings if needed. The free tier’s 5/min limit is sufficient for moderate use (and Polygon offers higher limits or websocket streaming for real-time needs if you upgrade).
    
- **Financial Modeling Prep (FMP)** – A free API (free API key with **250 calls/day** limit​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/pricing#:~:text=Ideal%20for%20testing%20our%20endpoints,and%20exploring%20our%20data)) that includes an _Is the Market Open_ endpoint. This returns a simple true/false on whether a given exchange (e.g. US market, Euronext, etc.) is currently open​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/is-the-market-open-api#:~:text=The%20FMP%20Market%20Open%20endpoint,avoid%20trading%20during%20market%20closures). It covers regular trading hours – if the query is for “US”, it will reflect the combined NYSE/Nasdaq schedule. It **does not explicitly account for pre-market or after-hours** in the response (those would be reported as “market closed”). The endpoint is handy for a quick binary status, but it **does not provide timestamps** for next open or last close in its free usage. (Developers might consult FMP’s trading calendar endpoints or hardcode market hours for that info.) The free tier is somewhat limited in call volume (roughly 250/day), but sufficient if you only need periodic status checks​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/pricing#:~:text=Ideal%20for%20testing%20our%20endpoints,and%20exploring%20our%20data).
    
- **Tradier** – A brokerage API that offers market data via REST. Tradier’s **Clock endpoint** provides intraday market status including extended hours. The response contains fields for the current state (e.g. _open_, _closed_, _premarket_, _postmarket_) and a human-readable description of the session hours​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,). It also gives a `next_change` timestamp with a `next_state`, indicating when the next session state will begin (for example, if currently open, `next_change` might be 16:00 with `next_state: postmarket`​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)). This effectively yields the upcoming close or next open time as appropriate. Tradier’s free access requires registering for an API key; **real-time data is available for brokerage account holders**​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/overview/market-data#:~:text=Market%20Data%20,based%20stocks%20and%20options) (you can use a sandbox with delayed data without an account). The free rate limit is generous (up to **120 calls/minute** for market data in production use​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/overview/rate-limiting#:~:text=,Sandbox%3A%2060%20request%20per%20minute)). The main limitation is that for true real-time status you need a funded Tradier brokerage account (opening one is free, but it’s an extra step). For pure free use (without a brokerage account), you’d be limited to delayed data in sandbox mode.
    

## Comparison Table of APIs

Below is a comparison of the key features of these APIs:

|**API**|**Free Tier Limits**|**Real-Time Open/Close Status**|**Extended Hours Indicator**|**Next Open/Close Timestamps**|**Notable Limitations**|
|---|---|---|---|---|---|
|**Finnhub**|60 calls/min (free)​[medium.com](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends)  <br>_(~86,400/day)_|Yes – provides `market_status` for US (open vs. closed)​[stackoverflow.com](https://stackoverflow.com/questions/9428989/how-to-test-if-the-stock-market-nyse-is-currently-open-closed#:~:text=Finnhub,the%20function%20would%20look%20like)|**No** – Treats extended hours as “closed” (no separate flag)​[finnhub.io](https://finnhub.io/docs/api#:~:text=API%20Documentation%20%7C%20Finnhub%20,)|**No** – Does _not_ return next open/close times in response|Free plan covers U.S. market status. No explicit extended session info. Generous rate limits.|
|**Alpaca**|~200 calls/min (free)​[forum.alpaca.markets](https://forum.alpaca.markets/t/rate-limit-clarity/7202#:~:text=The%20standard%20rate%20limit%20is,increased%20to%201000%20API%20Calls%2FMin)|Yes – `is_open` field in Clock API​[docs.alpaca.markets](https://docs.alpaca.markets/reference/getclock-1#:~:text=get%20https%3A%2F%2Fpaper)|**No** – Does not denote pre/post-market (only regular hours)​[forum.alpaca.markets](https://forum.alpaca.markets/t/clock-api-and-extended-hours/118#:~:text=I%20am%20also%20in%20need,05%3A00%E2%80%99%2C%20is_open%3A%20false)|**Yes** – Returns `next_open` and `next_close` timestamps​[forum.alpaca.markets](https://forum.alpaca.markets/t/clock-api-and-extended-hours/118#:~:text=I%20am%20also%20in%20need,05%3A00%E2%80%99%2C%20is_open%3A%20false)|Free data uses IEX feed (8am–5pm ET); extended hours not reflected. Requires free account signup.|
|**Polygon.io**|5 calls/min (free)​[polygon.io](https://polygon.io/knowledge-base/article/what-is-the-request-limit-for-polygons-restful-apis#:~:text=REST)  <br>_(higher limits require paid plan)_|Yes – `market` status shows open/closed and extended state​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods)|**Yes** – Flags for pre-market (`earlyHours`) and after-hours (`afterHours`)​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods)​[postman.com](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%7B%20,)|**Partial** – Not in `/marketstatus/now`; use calendar API for schedule​[postman.com](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=View%20More)​[postman.com](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%5B%20%7B%20,)|Free tier is limited to 5 requests/min. Full SIP real-time data on paid plans. No account needed for free key.|
|**F.M. Prep**|250 calls/day (free)​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/pricing#:~:text=Ideal%20for%20testing%20our%20endpoints,and%20exploring%20our%20data)|Yes – simple true/false if market is open​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/is-the-market-open-api#:~:text=The%20FMP%20Market%20Open%20endpoint,avoid%20trading%20during%20market%20closures)|**No** – No indication of extended hours (closed during off-hours)|**No** – Does not return timing info for next session|Free plan mainly for light use. Lacks detail (no extended hours or timing).|
|**Tradier**|120 calls/min (live acct)​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/overview/rate-limiting#:~:text=,Sandbox%3A%2060%20request%20per%20minute)  <br>60/min sandbox|Yes – `state` field (open, closed, premarket, postmarket)​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)|**Yes** – Distinguishes pre-market and post-market in state/next_state​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)|**Yes** – Provides `next_change` time for next state (open/close)​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)|Real-time data requires a Tradier brokerage account​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/overview/market-data#:~:text=Market%20Data%20,based%20stocks%20and%20options) (free signup). Sandbox (no account) is 15-min delayed.|

## Recommendation

For a **completely free solution** (no paid upgrades) that covers all sessions, **Polygon.io’s Market Status API** is an excellent choice. It indicates normal vs. extended trading hours in real time​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods) and consolidates NYSE/NASDAQ data (SIP). Despite the 5 calls/min limit, it’s usually sufficient for checking market status (and you can always cache or only poll around session transitions). If you only need a simple open/closed flag and high allowance, Finnhub is very developer-friendly with its generous free tier​[medium.com](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends) – though it won’t explicitly flag pre/post-market, it will show “closed” during those periods.

 

**Overall**, for a free-only use case requiring granular session info, **Polygon.io** best balances functionality and cost. It requires just a free API key and provides real-time status including pre-market and after-hours, which fits the needs for comprehensive market session information. Finnhub or Alpaca are strong alternatives if extended-hour detail is less critical (Finnhub for simplicity, Alpaca if you want built-in next open/close times). Tradier is powerful but might be overkill unless you already have (or don’t mind creating) a brokerage account.

 

**Sources:** The comparison above is based on official documentation and plans of each API​[medium.com](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends)​[docs.alpaca.markets](https://docs.alpaca.markets/reference/getclock-1#:~:text=get%20https%3A%2F%2Fpaper)​[polygon.io](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods)​[site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/is-the-market-open-api#:~:text=The%20FMP%20Market%20Open%20endpoint,avoid%20trading%20during%20market%20closures)​[documentation.tradier.com](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,), ensuring the information is up-to-date and accurate for 2025.

Citations

[

![Favicon](https://www.google.com/s2/favicons?domain=https://medium.com&sz=32)

Plot Recommendation Trends from Finnhub using Plotly Library for Python | by Sugath Mudali | Medium

https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec

](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends)[

![Favicon](https://www.google.com/s2/favicons?domain=https://finnhub.io&sz=32)

API Documentation | Finnhub - Free APIs for realtime stock, forex ...

https://finnhub.io/docs/api

](https://finnhub.io/docs/api#:~:text=API%20Documentation%20%7C%20Finnhub%20,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://stackoverflow.com&sz=32)

ios - How to test if the stock market (NYSE) is currently open/closed? - Stack Overflow

https://stackoverflow.com/questions/9428989/how-to-test-if-the-stock-market-nyse-is-currently-open-closed

](https://stackoverflow.com/questions/9428989/how-to-test-if-the-stock-market-nyse-is-currently-open-closed#:~:text=Finnhub,the%20function%20would%20look%20like)[

![Favicon](https://www.google.com/s2/favicons?domain=https://docs.alpaca.markets&sz=32)

Get Market Clock info

https://docs.alpaca.markets/reference/getclock-1

](https://docs.alpaca.markets/reference/getclock-1#:~:text=get%20https%3A%2F%2Fpaper)[

![Favicon](https://www.google.com/s2/favicons?domain=https://forum.alpaca.markets&sz=32)

Clock API and Extended Hours - Feature Requests - Alpaca Community Forum

https://forum.alpaca.markets/t/clock-api-and-extended-hours/118

](https://forum.alpaca.markets/t/clock-api-and-extended-hours/118#:~:text=I%20am%20also%20in%20need,05%3A00%E2%80%99%2C%20is_open%3A%20false)[

![Favicon](https://www.google.com/s2/favicons?domain=https://forum.alpaca.markets&sz=32)

Rate Limit Clarity - Alpaca Market Data - Alpaca Community Forum

https://forum.alpaca.markets/t/rate-limit-clarity/7202

](https://forum.alpaca.markets/t/rate-limit-clarity/7202#:~:text=The%20standard%20rate%20limit%20is,increased%20to%201000%20API%20Calls%2FMin)[

![Favicon](https://www.google.com/s2/favicons?domain=https://polygon.io&sz=32)

What is the request limit for Polygon’s RESTful APIs? | Polygon

https://polygon.io/knowledge-base/article/what-is-the-request-limit-for-polygons-restful-apis

](https://polygon.io/knowledge-base/article/what-is-the-request-limit-for-polygons-restful-apis#:~:text=REST)[

![Favicon](https://www.google.com/s2/favicons?domain=https://polygon.io&sz=32)

Market Status | Stocks API - Polygon

https://polygon.io/docs/rest/stocks/market-operations/market-status

](https://polygon.io/docs/rest/stocks/market-operations/market-status#:~:text=Retrieve%20the%20current%20trading%20status,current%20or%20upcoming%20trading%20periods)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.postman.com&sz=32)

Polygon API | Documentation | Postman API Network

https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api

](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%7B%20,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.postman.com&sz=32)

Polygon API | Documentation | Postman API Network

https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api

](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%5B%20%7B%20,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://site.financialmodelingprep.com&sz=32)

Pricing | Financial Modeling Prep | FMP

https://site.financialmodelingprep.com/developer/docs/pricing

](https://site.financialmodelingprep.com/developer/docs/pricing#:~:text=Ideal%20for%20testing%20our%20endpoints,and%20exploring%20our%20data)[

![Favicon](https://www.google.com/s2/favicons?domain=https://site.financialmodelingprep.com&sz=32)

Holidays and Trading Hours API | Financial Modeling Prep

https://site.financialmodelingprep.com/developer/docs/is-the-market-open-api

](https://site.financialmodelingprep.com/developer/docs/is-the-market-open-api#:~:text=The%20FMP%20Market%20Open%20endpoint,avoid%20trading%20during%20market%20closures)[

![Favicon](https://www.google.com/s2/favicons?domain=https://documentation.tradier.com&sz=32)

How to get the current market status | Brokerage API Documentation | Tradier

https://documentation.tradier.com/brokerage-api/markets/get-clock

](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://documentation.tradier.com&sz=32)

Market Data | Brokerage API Documentation | Tradier

https://documentation.tradier.com/brokerage-api/overview/market-data

](https://documentation.tradier.com/brokerage-api/overview/market-data#:~:text=Market%20Data%20,based%20stocks%20and%20options)[

![Favicon](https://www.google.com/s2/favicons?domain=https://documentation.tradier.com&sz=32)

Rate Limiting | Brokerage API Documentation | Tradier

https://documentation.tradier.com/brokerage-api/overview/rate-limiting

](https://documentation.tradier.com/brokerage-api/overview/rate-limiting#:~:text=,Sandbox%3A%2060%20request%20per%20minute)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.postman.com&sz=32)

Polygon API | Documentation | Postman API Network

https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api

](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=View%20More)

All Sources

[

![Favicon](https://www.google.com/s2/favicons?domain=https://medium.com&sz=32)medium

](https://medium.com/@sugath.mudali/plot-recommendation-trends-from-finnhub-using-plotly-library-for-python-6487a9c9e4ec#:~:text=Finnhub%20offers%20real,the%20API%20is%20Recommendation%20Trends)[

![Favicon](https://www.google.com/s2/favicons?domain=https://finnhub.io&sz=32)finnhub

](https://finnhub.io/docs/api#:~:text=API%20Documentation%20%7C%20Finnhub%20,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://stackoverflow.com&sz=32)stackoverflow

](https://stackoverflow.com/questions/9428989/how-to-test-if-the-stock-market-nyse-is-currently-open-closed#:~:text=Finnhub,the%20function%20would%20look%20like)[

![Favicon](https://www.google.com/s2/favicons?domain=https://docs.alpaca.markets&sz=32)docs.alpaca

](https://docs.alpaca.markets/reference/getclock-1#:~:text=get%20https%3A%2F%2Fpaper)[

![Favicon](https://www.google.com/s2/favicons?domain=https://forum.alpaca.markets&sz=32)forum.alpaca

](https://forum.alpaca.markets/t/clock-api-and-extended-hours/118#:~:text=I%20am%20also%20in%20need,05%3A00%E2%80%99%2C%20is_open%3A%20false)[

![Favicon](https://www.google.com/s2/favicons?domain=https://polygon.io&sz=32)polygon

](https://polygon.io/knowledge-base/article/what-is-the-request-limit-for-polygons-restful-apis#:~:text=REST)[

![Favicon](https://www.google.com/s2/favicons?domain=https://www.postman.com&sz=32)postman

](https://www.postman.com/polygonio-api/polygon-io-workspace/documentation/8y0nlue/polygon-api#:~:text=%7B%20,)[

![Favicon](https://www.google.com/s2/favicons?domain=https://site.financialmodelingprep.com&sz=32)site.fin...elingprep

](https://site.financialmodelingprep.com/developer/docs/pricing#:~:text=Ideal%20for%20testing%20our%20endpoints,and%20exploring%20our%20data)[

![Favicon](https://www.google.com/s2/favicons?domain=https://documentation.tradier.com&sz=32)document...n.tradier

](https://documentation.tradier.com/brokerage-api/markets/get-clock#:~:text=%7B%20%22clock%22%3A%20%7B%20%22date%22%3A%20%222019,)