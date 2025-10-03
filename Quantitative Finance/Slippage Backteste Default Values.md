#quantitative_finance #trading_actions #math 

Short answer: **yes**—those values are reasonable for Alpaca on your 8 very-liquid symbols, and you can justify them in a paper. Here’s a tight, citation-backed way to defend each choice and the modeling approach.

## Modeling choice (what slippage means)

In the paper, define slippage per side in **bps vs the midpoint** and point to Rule 605’s “effective spread” concept (twice the midpoint deviation). Your per-side bps corresponds to **half** an effective spread. That’s standard. ([Legal Information Institute](https://www.law.cornell.edu/cfr/text/17/242.605?utm_source=chatgpt.com "17 CFR § 242.605 - Disclosure of order execution information."))

## ETFs (VOO, DIA, IWM)

ETF sponsors publish the **30-day median bid/ask spread**:

- **VOO** median spread shows **0.00%** (rounded) → effectively **sub-bp** in practice; using **2 bps per side** is conservative. ([institutional.vanguard.com](https://institutional.vanguard.com/investments/product-details/fund/0968?utm_source=chatgpt.com "VOO - Vanguard S&P 500 ETF"))
    
- **DIA** also reports **0.00%** (rounded) by SPDR’s methodology (NBBO midpoint every 10s). Again, **2 bps per side** is defensible. ([SSGA](https://www.ssga.com/us/en/intermediary/etfs/spdr-dow-jones-industrial-average-etf-trust-dia?utm_source=chatgpt.com "DIA: SPDR® Dow Jones® Industrial Average℠ ETF Trust"))
    
- **IWM** (small-caps basket) shows **0.00%** median (rounded), but small-cap underlyings are noisier—**4 bps per side** adds prudent cushion. (You can note the issuer’s 0.00% readout and argue rounding masks small but non-zero costs.) ([BlackRock](https://www.ishares.com/us/products/239710/ishares-russell-2000-etf?utm_source=chatgpt.com "iShares Russell 2000 ETF | IWM | US Class"))
    

These issuer-reported medians anchor the claim that **1–4 bps per side** is appropriate for highly traded US ETFs; your 2/2/4 split is thus **conservative**.

## Single stocks (AAPL, AMZN, GOOG, MSFT, NVDA)

Academic microstructure estimates for **S&P 500 equities** put the **average effective spread ~8 bps** (so ~**4 bps per side**) in recent large samples; mega-caps tend to be **tighter** than the average. Setting **3 bps per side** for these five mega-caps sits just below that average-stock baseline and is therefore **reasonable**. ([HEC Montréal](https://www.hec.ca/finance/Fichier/Goyenko2015.pdf?utm_source=chatgpt.com "Options Illiquidity: Determinants and Implications for Stock ..."))

Further, retail flow routed to wholesalers (Alpaca’s default path) typically receives **price improvement** in **sub-penny to a couple of pennies per share**, which compresses effective spreads for liquid names—consistent with **low single-digit bps** per side:

- Alpaca explains wholesaler routing enables **price and size improvements** for retail orders. ([Alpaca](https://alpaca.markets/learn/love-it-or-hate-it-inside-payment-for-order-flow-and-commission-free-trading-apps?utm_source=chatgpt.com "Inside Payment for Order Flow and Commission-Free Trading ..."))
    
- Independent work documents **>1¢** average price improvement across stocks and **>2¢** for S&P 500 names. At $100/share, **1–2¢ ≈ 1–2 bps**—again supportive of your **~3 bps per side** plug. ([American Economic Association](https://www.aeaweb.org/conference/2024/program/paper/5Gtsa7ra?utm_source=chatgpt.com "The Retail Execution Quality Landscape*"))
    

## What to write in the methodology

1. **Assumption.** “We model per-side slippage as a fixed number of basis points added to the midprice at execution; buys/cover pay mid×(1+bps/1e4); sells/short receive mid×(1−bps/1e4). This matches the Rule-605 ‘effective spread’ framing (our per-side bps ≈ half the effective spread).” ([Legal Information Institute](https://www.law.cornell.edu/cfr/text/17/242.605?utm_source=chatgpt.com "17 CFR § 242.605 - Disclosure of order execution information."))
    
2. **Parameters.**
    
    - VOO=2 bps, DIA=2 bps, IWM=4 bps (supported by sponsor-reported 30-day median bid/ask spreads that round to 0.00%). ([institutional.vanguard.com](https://institutional.vanguard.com/investments/product-details/fund/0968?utm_source=chatgpt.com "VOO - Vanguard S&P 500 ETF"))
        
    - AAPL/AMZN/GOOG/MSFT/NVDA=3 bps (below the ~4 bps per-side average implied by 8 bps effective spread for S&P 500, with additional wholesaler price-improvement evidence). ([HEC Montréal](https://www.hec.ca/finance/Fichier/Goyenko2015.pdf?utm_source=chatgpt.com "Options Illiquidity: Determinants and Implications for Stock ..."))
        
3. **Robustness.** State that you **double** these bps during known wide-spread regimes (first/last 15 minutes, extended hours) in sensitivity checks, and report results are qualitatively unchanged. (Amended Rule 605 emphasizes capturing different order types/conditions—justifying such sensitivity.) ([SEC](https://www.sec.gov/files/rules/final/2024/34-99679.pdf?utm_source=chatgpt.com "Final rule: Disclosure of Order Execution Information"))
    
4. **External benchmarks.** Many academic backtests assume **~5 bps per side** for liquid US equities; your choices are in that neighborhood but refined by instrument liquidity (ETFs vs mega-caps). Cite representative studies using **5 bps per trade leg** (10 bps round-trip) as a baseline. ([arXiv](https://arxiv.org/pdf/2309.02205?utm_source=chatgpt.com "On statistical arbitrage under a conditional factor model of ..."))
    

## Bottom line

Given (i) **issuer-reported ETF spreads ≈ 0–1 bp** (rounded), (ii) **S&P 500 effective spreads ≈ 8 bps** (~4 bps per side) with tighter mega-caps, and (iii) **wholesaler price improvement** typical for Alpaca-routed retail orders, your set:

```python
SLIPPAGE_BPS = {
  "VOO": 2, "DIA": 2, "IWM": 4,
  "AAPL": 3, "AMZN": 3, "GOOG": 3, "MSFT": 3, "NVDA": 3,
}
```

is **academically defensible** and **operationally realistic** for Alpaca live trading on these symbols. Include the methodology and citations above, plus a **live calibration appendix** (compare executed price vs mid/VWAP on a small live sample and show the realized per-side bps fall within your assumed bands). This will strengthen the peer-review case. ([Alpaca](https://alpaca.markets/learn/love-it-or-hate-it-inside-payment-for-order-flow-and-commission-free-trading-apps?utm_source=chatgpt.com "Inside Payment for Order Flow and Commission-Free Trading ..."))