#quantitative_finance #trading_actions #math 

[[Slippage Backteste Default Values]]

Great question. The cleanest way to add slippage in a backtest is to (1) compute an **effective execution price** different from the mid/close you’re using, and (2) size trades against that effective price so your cash flows match reality.

Below is a **drop-in** approach using constant slippage in basis points (bps). It works for buys, sells, opening shorts, and covering shorts.

---

### 1) Config + helpers (put near the top)

```python
# --- Slippage config ---
SLIPPAGE_BPS_BUY  = 5    # e.g., 5 bps = 0.05% worse for any buy/cover
SLIPPAGE_BPS_SELL = 5    # e.g., 5 bps = 0.05% worse for any sell/short

def exec_price_with_slippage(side: str, mid_price: float,
                             bps_buy: float = SLIPPAGE_BPS_BUY,
                             bps_sell: float = SLIPPAGE_BPS_SELL) -> float:
    """
    Returns the executed price including slippage relative to mid/close.
      - 'buy'  or 'cover' => pay a higher price: mid * (1 + bps_buy/1e4)
      - 'sell' or 'short' => receive lower price: mid * (1 - bps_sell/1e4)
    """
    if side in ("buy", "cover"):
        return mid_price * (1.0 + bps_buy / 1e4)
    elif side in ("sell", "short"):
        return mid_price * (1.0 - bps_sell / 1e4)
    else:
        return mid_price  # fallback (shouldn't happen)
```

> Why price adjustment (vs a separate “cost”)?  
> Adjusting the **price** makes the math consistent: your _intended notional_ is spent/received at the _actual executed price_. You won’t overspend on buys, and your proceeds reflect slippage on sells.

---

### 2) Use it in each trade block

#### A) BUY (long open/add)

Replace your buy cost lines with:

```python
exec_px = exec_price_with_slippage("buy", current_price)
num_shares_to_buy = amount_to_invest / exec_px
cost = num_shares_to_buy * exec_px

fund -= cost
total_ops += cost
num_shares += num_shares_to_buy

print(f"[Período {i}] COMPRA LONG: {num_shares_to_buy:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}). Custo: ${cost:.2f}. Liquidez: ${fund:.2f}.")
```

#### B) SELL (reduce/close long)

Replace proceeds lines with:

```python
exec_px = exec_price_with_slippage("sell", current_price)
proceeds = amount_to_sell * exec_px

SEC_fee = calculate_fee("sell", proceeds)
Total_Fees += SEC_fee
fund += proceeds - SEC_fee
total_ops += proceeds
num_shares -= amount_to_sell

print(f"[Período {i}] VENDA LONG: {amount_to_sell:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}). Proventos: ${proceeds:.2f}, SEC ${SEC_fee:.2f}. Liquidez: ${fund:.2f}.")
```

#### C) OPEN SHORT

Two important tweaks: (1) use the **sell** execution price with slippage; (2) **credit** the sale proceeds to cash (recommended), so your NAV and mark-to-market are correct.

```python
exec_px = exec_price_with_slippage("short", current_price)
num_shares_to_short = amount_to_short / exec_px
proceeds = num_shares_to_short * exec_px

SEC_fee = calculate_fee("short", proceeds)
Total_Fees += SEC_fee
fund += proceeds - SEC_fee              # credit short sale proceeds to cash
time_at_short = last_subsets_dates_test[i]
num_shares_short = num_shares_to_short
short_sale_price = exec_px              # store executed price (incl. slippage)
total_ops += proceeds

print(f"[Período {i}] ABERTURA SHORT: {num_shares_to_short:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}). Proventos: ${proceeds:.2f}, SEC ${SEC_fee:.2f}. Caixa: ${fund:.2f}.")
```

> If you absolutely must keep your old “don’t credit proceeds” approach, at least assign `short_sale_price = exec_px`. But the accounting will be cleaner if you do credit proceeds.

#### D) COVER SHORT

Use a **buy** execution price with slippage for the cover:

```python
exec_px = exec_price_with_slippage("cover", current_price)
notional_cover = num_shares_short * exec_px

SEC_fee = calculate_fee("cover", num_shares_short * short_sale_price, time_at_short, last_subsets_dates_test[i])
Total_Borrowings += SEC_fee

profit = num_shares_short * (short_sale_price - exec_px) - SEC_fee
fund += profit
total_ops += notional_cover

print(f"[Período {i}] COBERTURA SHORT: {num_shares_short:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}). Notional: ${notional_cover:.2f}, Borrow ${SEC_fee:.2f}. "
      f"P&L: ${profit:.2f}. Caixa: ${fund:.2f}.")

num_shares_short = 0.0
short_sale_price = 0.0
```

---

### 3) Net worth (NAV) each step

If you credited short proceeds to cash (recommended), keep the standard mark-to-market:

```python
current_networth = fund + num_shares * current_price - num_shares_short * current_price
```

This prices positions at **mid**; slippage only affects **execution** prices and cash flows, which is what you want.

---

### 4) Optional: make slippage dynamic (later)

If you want realism beyond constant bps, replace `exec_price_with_slippage` internals with a function of order size vs liquidity (ADV) and maybe volatility, e.g.:

```python
# Example: base bps + sqrt impact on participation rate
# participation ~ (order_notional / (ADV * mid))
impact = k * math.sqrt(max(0.0, participation))
bps = base_bps + impact_bps
```

…but the constant-bps version above is already a solid, deterministic upgrade.

---

That’s it. With these four insertion points, your backtest will (a) size using the _actual_ executed price and (b) capture realistic P&L drag from slippage on **all** sides (buy/sell/short/cover).

---

Absolutely—here’s a clean, **drop-in** implementation that adds your per-ticker bps slippage and applies it consistently to buys, sells, shorts, and covers. It also keeps P&L/NAV accounting correct.

> We model slippage as a **price adjustment vs. the midpoint/close** (buys/cover pay up; sells/short receive less). This matches the “effective spread” framing in SEC Rule 605 execution-quality reporting, and it’s necessary because Alpaca paper fills are simulated rather than exchange-routed. ([Legal Information Institute](https://www.law.cornell.edu/cfr/text/17/242.605?utm_source=chatgpt.com "17 CFR § 242.605 - Disclosure of order execution information."))

---

### 1) Config + helpers (add near the top of your script)

```python
# ====== Slippage configuration (per side, symmetric) ======
SLIPPAGE_BPS = {
    "VOO": 2, "DIA": 2, "IWM": 4,
    "AAPL": 3, "AMZN": 3, "GOOG": 3, "MSFT": 3, "NVDA": 3,
}
DEFAULT_SLIPPAGE_BPS = 5  # fallback if symbol not in dict

def _bps_for(side: str, ticker: str) -> float:
    # Same bps per side; you can split BUY/SELL later if you want asymmetry.
    return SLIPPAGE_BPS.get(ticker, DEFAULT_SLIPPAGE_BPS)

def exec_price_with_slippage(side: str, mid_price: float, ticker: str) -> float:
    """
    Adjust execution price by per-ticker slippage in basis points (1 bp = 0.01%).
      - 'buy'  or 'cover' => pay higher:   mid * (1 + bps/1e4)
      - 'sell' or 'short' => receive less: mid * (1 - bps/1e4)
    """
    bps = _bps_for(side, ticker)
    if side in ("buy", "cover"):
        return mid_price * (1.0 + bps / 1e4)
    elif side in ("sell", "short"):
        return mid_price * (1.0 - bps / 1e4)
    return mid_price
```

> Assumes you have a `symbol` string for the instrument being backtested (e.g., `"AAPL"`). If you batch across symbols, pass the current one.

---

### 2) Use it inside your loop (replace price used for fills)

**BUY (open/add long)**

```python
exec_px = exec_price_with_slippage("buy", current_price, symbol)
num_shares_to_buy = amount_to_invest / exec_px
cost = num_shares_to_buy * exec_px

fund -= cost
total_ops += cost
num_shares += num_shares_to_buy

print(f"[Período {i}] COMPRA LONG: {num_shares_to_buy:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}, { _bps_for('buy', symbol) } bps). "
      f"Custo: ${cost:.2f}. Liquidez: ${fund:.2f}.")
```

**SELL (reduce/close long)**

```python
exec_px = exec_price_with_slippage("sell", current_price, symbol)
proceeds = amount_to_sell * exec_px

SEC_fee = calculate_fee("sell", proceeds)
Total_Fees += SEC_fee
fund += proceeds - SEC_fee
total_ops += proceeds
num_shares -= amount_to_sell

print(f"[Período {i}] VENDA LONG: {amount_to_sell:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}, { _bps_for('sell', symbol) } bps). "
      f"Proventos: ${proceeds:.2f}, SEC ${SEC_fee:.2f}. Liquidez: ${fund:.2f}.")
```

**OPEN SHORT** _(credits short sale proceeds to cash so NAV is correct)_

```python
exec_px = exec_price_with_slippage("short", current_price, symbol)
num_shares_to_short = amount_to_short / exec_px
proceeds = num_shares_to_short * exec_px

SEC_fee = calculate_fee("short", proceeds)
Total_Fees += SEC_fee
fund += proceeds - SEC_fee          # credit the short sale to cash
time_at_short = last_subsets_dates_test[i]
num_shares_short = num_shares_to_short
short_sale_price = exec_px          # store executed open price (incl. slippage)
total_ops += proceeds

print(f"[Período {i}] ABERTURA SHORT: {num_shares_to_short:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}, { _bps_for('short', symbol) } bps). "
      f"Proventos: ${proceeds:.2f}, SEC ${SEC_fee:.2f}. Caixa: ${fund:.2f}.")
```

**COVER SHORT**

```python
exec_px = exec_price_with_slippage("cover", current_price, symbol)
notional_cover = num_shares_short * exec_px

SEC_fee = calculate_fee("cover", num_shares_short * short_sale_price, time_at_short, last_subsets_dates_test[i])
Total_Borrowings += SEC_fee

profit = num_shares_short * (short_sale_price - exec_px) - SEC_fee
fund += profit
total_ops += notional_cover

print(f"[Período {i}] COBERTURA SHORT: {num_shares_short:.6f} ações a "
      f"{exec_px:.4f} (mid {current_price:.4f}, { _bps_for('cover', symbol) } bps). "
      f"Notional: ${notional_cover:.2f}, Borrow ${SEC_fee:.2f}. P&L: ${profit:.2f}. "
      f"Caixa: ${fund:.2f}.")

num_shares_short = 0.0
short_sale_price = 0.0
```

---

### 3) NAV (mark-to-market) each step

If you credit the short sale proceeds to cash (as above), mark to **mid** on positions:

```python
current_networth = fund + num_shares * current_price - num_shares_short * current_price
```

This prices the **inventory at mid** while slippage only affects the **execution cashflows**—standard practice consistent with execution-quality metrics (effective spread). ([Legal Information Institute](https://www.law.cornell.edu/cfr/text/17/242.605?utm_source=chatgpt.com "17 CFR § 242.605 - Disclosure of order execution information."))

---

### 4) Apply slippage at the final liquidations too

```python
final_price = float(tensor_test_np[-1][0])

# Close long at final with slippage
if num_shares > 0:
    exec_px = exec_price_with_slippage("sell", final_price, symbol)
    proceeds = num_shares * exec_px
    SEC_fee = calculate_fee("sell", proceeds)
    Total_Fees += SEC_fee
    fund += proceeds - SEC_fee
    total_ops += proceeds
    print(f"[Final] FECHAMENTO LONG: {num_shares:.6f} ações a {exec_px:.4f} "
          f"(mid {final_price:.4f}, { _bps_for('sell', symbol) } bps). SEC ${SEC_fee:.2f}.")
    num_shares = 0.0

# Cover short at final with slippage
if num_shares_short > 0:
    exec_px = exec_price_with_slippage("cover", final_price, symbol)
    SEC_fee = calculate_fee("cover", num_shares_short * short_sale_price, time_at_short, last_subsets_dates_test[-1])
    Total_Borrowings += SEC_fee
    profit = num_shares_short * (short_sale_price - exec_px) - SEC_fee
    fund += profit
    total_ops += num_shares_short * exec_px
    print(f"[Final] FECHAMENTO SHORT: {num_shares_short:.6f} ações a {exec_px:.4f} "
          f"(mid {final_price:.4f}, { _bps_for('cover', symbol) } bps). Borrow ${SEC_fee:.2f}. "
          f"P&L: ${profit:.2f}.")
    num_shares_short = 0.0
    short_sale_price = 0.0
```

---

### Notes you can cite in your paper (1–2 lines)

- **Definition & modeling**: Slippage is the deviation between execution and benchmark (commonly the midpoint); using per-side bps is aligned with Rule 605’s effective-spread framework. ([Investopedia](https://www.investopedia.com/terms/s/slippage.asp?utm_source=chatgpt.com "Slippage: What It Means in Finance, With Examples"))
    
- **Why add it in backtests**: Alpaca paper fills simulate quotes and don’t route to venues; live trading can realize slippage/price improvement—so backtests must inject realistic bps. ([Alpaca](https://alpaca.markets/support/difference-paper-live-trading?utm_source=chatgpt.com "What's the difference between paper trading and live trading?"))
    

If you want, I can also add an optional **open/close multiplier** (e.g., ×2 bps in first/last 15 minutes or outside RTH) and a tiny **ADV-based impact bump**—both are easy to plug into `_bps_for`.