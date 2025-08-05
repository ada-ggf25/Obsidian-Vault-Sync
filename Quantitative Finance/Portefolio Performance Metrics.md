#quantitative_finance #economics #quant_metrics 

## A. Period-Frequency Estimation

### `estimate_periods_per_year(returns)`

- **What:** Estimates the _effective_ number of return observations (“periods”) you get in a calendar year.
- **How:**
    1. Groups your `returns` Series by year (`returns.index.year`).
    2. Counts the number of bars in each year.
    3. Takes the arithmetic mean of those annual counts.
- **Why:** Your data only covers market-open 3-hour bars (gaps overnight/weekends/holidays). Annualizing formulas (volatility scaling, Sharpe √P, RF conversion) need to know “P” (periods/year) to be accurate.

---

## B. Risk-Free Conversion

### `_rf_per_period(rf_annual)`

- **What:** Converts an _annualized_ risk-free rate (e.g. 1% p.a.) into the equivalent _per-period_ rate, given P periods/year.
    
- **Formula:**rf_period=(1+rf_annual)1/P−1
    
    rf_period=(1+rf_annual)1/P−1 \text{rf\_period} = (1 + \text{rf\_annual})^{1/P} - 1
    
- **Why:** When you compute excess returns (`returns – rf_period`), you must subtract the correct per-bar risk-free amount, not the headline annual figure.
    

---

## C. Return Metrics

### `total_return(returns)`

- **What:** Cumulative return over the entire backtest.
    
- **Formula:**i=1∏n(1+ri)−1
    
    ∏i=1n(1+ri)  −  1 \prod_{i=1}^n (1 + r_i)\;-\;1
    
    where rir_iri are your period returns.
    
- **Why:** The “bottom-line” growth factor of your strategy.
    

### `annualized_return(returns)`

- **What:** Compound Annual Growth Rate (CAGR), expressed per year.
- **Formula:**
    1. Compute Rtot=total_return(returns)R_{\text{tot}} = \text{total\_return}(returns)Rtot=total_return(returns).
    2. Let nnn = number of bars.
    3. CAGR=(1+Rtot)Pn−1 \text{CAGR} = (1 + R_{\text{tot}})^{\tfrac{P}{n}} - 1CAGR=(1+Rtot)nP−1
- **Why:** Allows apples-to-apples comparison of strategies of different lengths or frequencies.

---

## D. Risk Metrics

### `annualized_volatility(returns)`

- **What:** Standard deviation of returns, scaled to an annual basis.
    
- **Formula:**σann=σperiod×P
    
    σann=σperiod×P \sigma_{\text{ann}} = \sigma_{\text{period}} \times \sqrt{P}
    
    where σperiod\sigma_{\text{period}}σperiod is the sample standard deviation (`ddof=1`).
    
- **Why:** A fundamental measure of how “bumpy” your returns are.
    

### `max_drawdown(returns)`

- **What:** The single-largest peak-to-trough drop in your cumulative‐return curve.
- **Algorithm:**
    1. Build cumulative series: Ct=∏i≤t(1+ri)C_t = \prod_{i\le t}(1+r_i)Ct=∏i≤t(1+ri).
    2. Compute running max: Ht=max⁡i≤tCiH_t = \max_{i\le t}C_iHt=maxi≤tCi.
    3. Drawdown series: (Ct/Ht)−1(C_t / H_t) - 1(Ct/Ht)−1.
    4. Return the minimum (most negative) value.
- **Why:** Worst loss you’d have experienced from peak to bottom.

### `drawdown_duration(returns)`

- **What:** Longest stretch (in bars) between a new peak and its full recovery.
- **Algorithm:**
    1. Identify periods where Ct<HtC_t < H_tCt<Ht.
    2. Group consecutive “in‐drawdown” bars and count lengths.
    3. Return the maximum count.
- **Why:** Shows how long you stay underwater.

### `ulcer_index(returns)`

- **What:** A combined depth-and-duration drawdown measure.
- **Formula:**
    1. Compute drawdowns in %: Dt=(Ct/Ht−1)×100D_t = (C_t / H_t - 1)\times100Dt=(Ct/Ht−1)×100.
    2. Ulcer Index = 1n∑Dt2\sqrt{\frac{1}{n}\sum D_t^2}n1∑Dt2.
- **Why:** Penalizes both deep and long drawdowns; more intuitive than bare volatility for “stress.”

---

## E. Risk-Adjusted Ratios

### `sharpe_ratio(returns, rf)`

- **What:** Excess return per unit _total_ risk.
- **Formula:**
    1. Subtract per-period RF: ei=ri−rf_per_periode_i = r_i - \text{rf\_per\_period}ei=ri−rf_per_period.
    2. Sharpe=eˉσ(e)×P \text{Sharpe} = \frac{\bar{e}}{\sigma(e)}\times\sqrt{P}Sharpe=σ(e)eˉ×P
- **Why:** Industry standard for risk‐adjusted performance.

### `sortino_ratio(returns, rf)`

- **What:** Excess return per unit _downside_ risk.
- **Formula:**
    1. Excess series eie_iei as above.
    2. Downside deviation: 1n∑ei<0ei2\sqrt{\frac{1}{n}\sum_{e_i<0}e_i^2}n1∑ei<0ei2.
    3. Sortino=eˉdownside_dev×P \text{Sortino} = \frac{\bar{e}}{\text{downside\_dev}}\times\sqrt{P}Sortino=downside_deveˉ×P
- **Why:** Doesn’t penalize upside volatility—focuses on harmful moves only.

### `calmar_ratio(returns)`

- **What:** Return per unit drawdown risk.
    
- **Formula:**Calmar=∣Max Drawdown∣CAGR
    
    Calmar=CAGR∣Max Drawdown∣ \text{Calmar} = \frac{\text{CAGR}}{|\text{Max Drawdown}|}
    
- **Why:** Practical if you care primarily about drawdown severity relative to growth.
    

### `information_ratio(returns, benchmark_returns)`

- **What:** Active return (vs. benchmark) per unit _tracking error_.
    
- **Formula:**σ(r−rb)r−rb×P
    
    r−rb‾σ(r−rb)×P \frac{\overline{r - r_b}}{\sigma(r - r_b)}\times\sqrt{P}
    
- **Why:** Measures how consistently you beat your benchmark.
    

### `treynor_ratio(returns, benchmark_returns, rf)`

- **What:** Excess return per unit _systematic_ (market) risk.
    
- **Formula:**Treynor=βCAGR−rf
    
    Treynor=CAGR−rfβ \text{Treynor} = \frac{\text{CAGR} - rf}{\beta}
    
- **Why:** Focuses on compensation for market exposure rather than total volatility.
    

---

## F. Tail‐Risk Metrics

### `value_at_risk(returns, confidence)`

- **What:** Worst loss not exceeded at the given confidence level (e.g. 95%).
    
- **Formula:**VaRα=−Q1−α(r)
    
    VaRα=−Q1−α(r) \text{VaR}_\alpha = -Q_{1-\alpha}(r)
    
    where QqQ_{q}Qq = the qqqth quantile of returns.
    
- **Why:** A quick “how bad can it get” threshold for a single bar.
    

### `expected_shortfall(returns, confidence)`

- **What:** Average loss _beyond_ the VaR cutoff.
    
- **Formula:**ESα=−∣{ri≤Q1−α}∣1ri≤Q1−α∑ri
    
    ESα=−1∣{ri≤Q1−α}∣∑ri≤Q1−αri \text{ES}_\alpha = -\frac{1}{|\{r_i\le Q_{1-\alpha}\}|}\sum_{r_i\le Q_{1-\alpha}} r_i
    
- **Why:** Captures tail severity, not just the cutoff.
    

---

## G. Distribution Moments

### `skewness(returns)`

- **What:** Third standardized moment; measures asymmetry.
- **Why:** Positive skew means more frequent small losses with occasional big gains (or vice versa).

### `kurtosis(returns)`

- **What:** Fourth moment; measures tail “fatness.” Fisher’s definition (kurtosis of normal = 0).
- **Why:** High kurtosis ⇒ more frequent extreme outliers (good or bad).

### `omega_ratio(returns, threshold)`

- **What:** Ratio of cumulative gains above a threshold to cumulative losses below it.
    
- **Formula:**∑ri<θ(θ−ri)∑ri>θ(ri−θ)
    
    ∑ri>θ(ri−θ)∑ri<θ(θ−ri) \frac{\sum_{r_i>\theta}(r_i - \theta)}{\sum_{r_i<\theta}(\theta - r_i)}
    
- **Why:** A flexible generalization of risk-return trade-off around any floor θ\thetaθ (often zero).
    

---

## H. CAPM-Based Metrics

### `_capm_regression(returns, benchmark_returns, rf)`

- **What:** Runs an OLS regression of excess strategy returns (yyy) on excess benchmark returns (xxx), i.e.yi=α+βxi+εi
    
    yi=α+β xi+εi y_i = \alpha + \beta\,x_i + \varepsilon_i
    
- **Why:** Underpins α, β, and R2R^2R2.
    

### `beta(…)`

- **What:** Slope β\betaβ from the CAPM OLS; sensitivity to benchmark.
- **Why:** Quantifies your systematic market exposure.

### `alpha(…)`

- **What:** Jensen’s alpha; the intercept annualized.
- **Calculation:**
    1. Extract daily (period) intercept aaa.
    2. Annualize: (1+a)P−1(1 + a)^P - 1(1+a)P−1.
- **Why:** Measures performance _beyond_ what CAPM predicts.

### `r_squared(…)`

- **What:** R2R^2R2 of the CAPM regression; percent of strategy-return variance explained by market moves.
- **Why:** Tells you how “market-driven” your strategy is.

---

### Putting It All Together

- **Frequency‐aware**: all annualized metrics use your data’s true “P periods/year.”
- **Risk‐adjustment**: Sharpe/Sortino/Calmar/Treynor/IR give different lenses on reward per unit of risk (total, downside, drawdown, market).
- **Drawdowns & stress**: Max DD, duration, ulcer index highlight worst-case drops.
- **Tail behavior**: VaR, ES, skew, kurtosis, Omega capture the shape and extremity of your return distribution.
- **Benchmark context**: CAPM metrics and Information Ratio calibrate your strategy against a chosen index.
