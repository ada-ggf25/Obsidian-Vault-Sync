#quantitative_finance #trading_actions #math 

Slippage in **basis points (bps)** just means measuring the execution “worse-than-benchmark” in **hundredths of a percent**.

- **1 bp = 0.01% = 0.0001** in decimal.
    
- Slippage (bps) compares your **executed price** to a **benchmark** (mid/last/open/VWAP).
    

### Formulas

Let:

- `side = +1` for a **buy**, `side = -1` for a **sell**
    
- `exec` = executed price
    
- `bench` = benchmark price
    

**Signed cost in bps (positive = worse):**

slippage_bps=$$10,000×side×exec−benchbench\text{slippage\_bps} = 10{,}000 \times \text{side} \times \frac{\text{exec} - \text{bench}}{\text{bench}}$$

**Unsigned (magnitude only):**
$$
∣slippage_bps∣=10,000×∣exec−benchbench∣|\text{slippage\_bps}| = 10{,}000 \times \left| \frac{\text{exec} - \text{bench}}{\text{bench}} \right|$$

### Quick examples

- Buy benchmark $100, executed $100.05 →  
    $10,000×100.05−100100=5 $$ bps10{,}000 \times \frac{100.05-100}{100} = 5\ \text{bps}$$$ (you paid **0.05%** more).
    
- Sell benchmark $100, executed $99.97 →  
    10,000×(−1)×99.97−100100=3 bps$$10{,}000 \times (-1)\times \frac{99.97-100}{100} = 3\ \text{bps}$$ cost (you got **0.03%** less).
    

### Using bps to adjust price in a backtest

- For buys/covers: $$`exec = mid * (1 + bps/1e4)`$$
    
- For sells/shorts:$$ `exec = mid * (1 - bps/1e4)`$$
    

So “5 bps slippage” means you assume executions are 0.05% worse than the benchmark price on each trade.