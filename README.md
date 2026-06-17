# Is your Indian equity backtest lying to you?

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Finance-broski/pit-data-sample/blob/main/PIT_survivorship_demo.ipynb)

A runnable, ~60-second demo. Most Indian equity backtests are quietly inflated by two bugs that live in the **data**, not the strategy:

- **Survivorship bias** — delisted / merged companies are dropped from almost every Indian data source, so your backtest never holds the value-traps that actually blew up.
- **Look-ahead bias** — retail screeners show *restated / latest* fundamentals, not what was knowable on the date, so the strategy silently "sees the future."

This notebook runs the **same value strategy four ways** on a controlled dataset and shows how much each bias adds.

## The result

| Treatment | CAGR | Sharpe | Max DD |
|---|---|---|---|
| Honest (point-in-time + survivorship-free) | 12.0% | 0.79 | −35% |
| + Survivorship bias only | 17.1% | 1.09 | −27% |
| + Look-ahead bias only | 14.9% | 0.96 | −34% |
| **Naive (both — a typical screener backtest)** | **20.4%** | **1.26** | **−29%** |

**+8.4 pp/yr CAGR and +0.47 Sharpe of pure data bias** — no change to the strategy. The survivorship-only effect (~+5 pp/yr) is calibrated to published research on the NIFTY Smallcap 250 (survivor-only overstates returns ~4.9 pp/yr), so the gap is realistic, not theatrical. The tell that a backtest has these bugs: a Sharpe that looks too good and a suspiciously shallow drawdown.

## Run it

Click **Open in Colab** above → `Runtime → Run all`. ~10 seconds, no setup (it only uses numpy / pandas / matplotlib). Or open `PIT_survivorship_demo.ipynb` in any Jupyter environment.

## A note on the data

The dataset is **simulated and labelled as such**, calibrated to real published bias magnitudes — the *mechanism* is honest even though the tickers aren't real. The notebook includes a "plug in your own data" cell with the schema for running it on real names.

It's built from ongoing work on real **point-in-time, survivorship-bias-free NSE/BSE fundamentals + corporate-action-adjusted prices**. If clean historical Indian data is a problem you run into, open an issue.
