# Opening Range Strategy — NSXUSD (2020–2023)

> **Goal:** Present and rigorously test a simple Opening Range intraday strategy on NSXUSD (CFD/Index), with visuals that are understandable even for non-coders.

## TL;DR (to be filled after first pass)

- **Net performance (2020–2023):** _TBD_
- **Max drawdown:** _TBD_
- **Win rate:** _TBD_
- **Sample equity curve & drawdown:** _TBD (figure)_
- **Conclusion in one sentence:** _TBD_

---

## 1) Strategy Summary

**Session window (market time):**

- **Opening Range (OR) window:** 09:30–10:00 (first 30 minutes after regular open).
- **Entry time:** **10:22** (exact minute; see open questions below).
- **Hard exit (time):** 12:00 (close any open position at market).

**Position logic (single trade per day):**

- Compute **OR High** and **OR Low** from 09:30–10:00.
- At the **entry time**, check where the price is relative to the OR:

  - **Long** if price is in the **top 35%** of the OR range.
  - **Short** if price is in the **bottom 35%**.
  - **No trade** if price is in the middle 30%.

- **Stops/Targets:**

  - **Stop Loss:** fixed **25 points**.
  - **Take Profit:** fixed **75 points**.
  - **Value per point:** **\$80**.

- **Forced exit:** Close any open trade at **12:00**.

**Capital:**

- Start with **\$100,000**. Update daily P\&L into equity curve.

> **Note:** We will finalize exact execution assumptions (use of OHLC for fills, slippage, fees, partial fills, tick size, timezone) before running the baseline.

---

## 2) Data Requirements

**Expected columns (semicolon “;” delimited, no header):**

```
datetime;open;high;low;close;volume
```

**Assumptions to confirm:**

- **Timezone of `datetime`:** _TBD_ (likely **America/New_York** for RTH alignment; confirm source).
- **Resolution:** 1-minute bars.
- **Session coverage:** Are pre/post-market bars included? _TBD_
- **Point definition & tick size:** Confirm that **1 point = 1.0 price unit** on NSXUSD and **tick size** (e.g., 0.1, 0.25, 1.0).
- **Holidays / half days:** _TBD_ handling.

We will create a small **data contract** validator in the data-audit notebook (column types, monotonic timestamps, missing bars, duplicated bars, session boundaries).

---

## 3) Evaluation Metrics

- **Return:** Net P\&L, CAGR (if applicable), exposure/time-in-market.
- **Risk:** Max Drawdown, Ulcer Index, volatility, tail metrics.
- **Trade stats:** Win rate, Profit Factor, avg win/loss, expectancy.
- **Stability:** Performance by year/month/day-of-week/time-of-day.
- **Cost sensitivity:** Net vs. **fees + slippage** scenarios.
- **Robustness:** Parameter sensitivity (OR window, SL/TP), walk-forward, Monte Carlo re-ordering of trades.

---

## 4) Visuals (for non-coders)

- **Equity curve** and **drawdown** (same time axis).
- **Daily P\&L bars** with rolling stats.
- **Heatmaps** by weekday × time bucket (hit-rate / P\&L).
- **Distribution plots** (P\&L per trade, win/loss sizes).
- **Sensitivity lines** (e.g., SL/TP multiples, OR window alternatives).
- **Net vs. costs** curves.

All plots will include intuitive titles, units, and short captions.
