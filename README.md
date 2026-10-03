# USDJPY 30-Minute ORB — Systematic Strategy Research

## Overview

This repository contains my personal quantitative research on a rule-based **USDJPY 30-minute Opening Range Breakout (ORB)** strategy.

The objective was to test whether a simple, fully specified breakout rule could produce a repeatable return profile across historical USDJPY data, while evaluating **performance, drawdown, trade behaviour and robustness**.

The research was designed around a **predefined strategy specification** rather than discretionary trading decisions.

> **Important:** This is a personal research project, not investment advice. The backtest excludes spread, commission and slippage, so the results should not be interpreted as live-trading performance.

---

## Research Question

**Can a fixed 30-minute USDJPY opening-range breakout rule generate a persistent and repeatable positive return profile across historical data?**

The research process was:

**Hypothesis → rule definition → coded backtest → trade-level analysis → monthly/yearly analysis → robustness testing → frozen candidate → manual audit**

---

## Strategy Specification

| Parameter | Rule |
|---|---|
| Instrument | USDJPY |
| Strategy clock | UTC−4 |
| ORB window | 06:00–06:30 UTC−4 |
| IST equivalent | 15:30–16:00 IST |
| Entry | Stop order at ORB boundary |
| Long | Buy Stop at ORB High |
| Short | Sell Stop at ORB Low |
| Trigger rule | First side triggered wins; cancel opposite order |
| Stop loss | Opposite ORB boundary |
| 1R | ORB High − ORB Low |
| Take profit | 2.5R |
| Maximum holding time | 45 minutes |
| Trade expiry | 16:45 UTC−4 |
| Trades per day | Maximum 1 |
| Re-entry | None |
| Buffer | 0 pips |
| Confirmation | None; no candle-close confirmation |
| Overnight | No |
| News filter | None in the frozen backtest |

### Execution rules

- Before 06:00: no trade.
- 06:00–06:30: build the ORB high/low only.
- At 06:30: place both Buy Stop and Sell Stop orders.
- When one side triggers, cancel the opposite order immediately.
- After the first trigger, ignore a second breakout.
- If neither side triggers, no trade is taken.
- Same-minute SL/TP situations are handled conservatively using SL-first treatment.
- If both ORB sides are touched in the same minute before entry, the day is treated as an ambiguous breakout and no trade is taken.

---

## Data

- **Instrument:** USDJPY
- **Frequency:** 1-minute OHLC
- **Research period:** 2021–2026 YTD
- **Trade observations:** 1,301
- **Execution model:** coded from historical 1-minute data
- **Costs:** no spread, commission or slippage included

The detailed workbook contains:

- Frozen strategy rules
- Backtest summary
- Monthly analysis
- Individual trade log
- Daily state log
- Research notes

---

## Backtest Results

### Frozen candidate

**30-minute ORB / 06:00 UTC−4 / stop entry / 2.5R TP / 45-minute expiry**

| Metric | Result |
|---|---:|
| Trades | 1,301 |
| Net R | **+99.14R** |
| Win Rate | **50.96%** |
| Profit Factor | **1.28** |
| Average R / Trade | **+0.076R** |
| Maximum Drawdown | **−15.60R** |

The result is expressed in **R-multiples**, so it is independent of a particular account size or position size.

### Yearly results

| Year | Trades | Net R | Win Rate | Profit Factor |
|---|---:|---:|---:|---:|
| 2021 | 228 | +35.86R | 52.63% | 1.60 |
| 2022 | 240 | +15.20R | 47.50% | 1.23 |
| 2023 | 177 | +15.30R | 54.80% | 1.33 |
| 2024 | 244 | +12.00R | 49.18% | 1.17 |
| 2025 | 250 | +5.85R | 47.60% | 1.08 |
| 2026 YTD | 162 | +14.93R | 57.41% | 1.38 |

All six listed years are positive in the workbook's backtest output.

---

## Robustness & Research Tests

The research did not stop at the first profitable configuration.

Tests included:

- Entry timing variations
- Different opening-range windows
- Different take-profit levels
- Different maximum holding periods
- Stop-entry vs. confirmation/market-entry approaches
- Walk-forward research
- Trade-level manual audit fields
- Strategy combinations and execution assumptions

The frozen candidate was selected before the manual audit rather than continuing to optimise after seeing the full dataset.

The workbook also records a research backup configuration:

> 30M ORB / 07:00 UTC−4 / 3R TP / 60-minute hold → +119.60R, PF approximately 1.238.

It was retained as a research backup because its timing was considered less practical.

---

## Important Research Caveats

This backtest has several limitations:

1. **No spread, commission or slippage** are included.
2. Historical fills may differ from live execution.
3. The research uses historical data and therefore does not guarantee future performance.
4. Parameter selection can introduce overfitting risk.
5. Manual audit is required to verify that the frozen rules can be followed candle-by-candle without hindsight.
6. The research intentionally avoids claiming that the observed return is guaranteed or that the strategy will continue to work.

These limitations are part of the research process rather than being hidden.

---

## Implementation Note

I designed the **research question, strategy rules, testing methodology and performance evaluation**.

The Python implementation was developed with **AI assistance for parts of the coding/implementation**. I reviewed the workflow, checked the calculations and independently analysed the resulting strategy behaviour.

My strongest contribution to this project is the **research logic and methodology** rather than writing every line of code from scratch.

---

## Repository Structure

```text
USDJPY-ORB-Systematic-Research/
│
├── README.md
├── USDJPY_ORB_Backtest.xlsx
├── cumulative_r.png
└── monthly_net_r.png
```

### Workbook structure

- `Rules` — frozen strategy specification
- `Backtest Summary` — aggregate and yearly results
- `Monthly Analysis` — monthly performance
- `Trade Log` — individual trade records
- `Daily State Log` — day-by-day strategy state
- `Research Notes` — methodology, caveats and research decisions

---

## Key Takeaway

The main purpose of this project was not simply to find a profitable backtest.

It was to demonstrate a repeatable quantitative research workflow:

**Define a hypothesis → formalize rules → test on historical data → analyse failures and drawdowns → test robustness → freeze the specification → audit execution assumptions.**

