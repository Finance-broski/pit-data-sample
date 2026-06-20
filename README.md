# Is your Indian equity backtest lying to you?

Two bugs live in the **data**, not your strategy — and they quietly inflate almost every Indian equity backtest:

- **Survivorship bias** — delisted / merged companies are dropped from nearly every Indian data source, so your backtest never holds the names that actually blew up (DHFL, Jet Airways, Reliance Capital, IL&FS…).
- **Look-ahead bias** — retail screeners show *restated / latest* fundamentals, not what was knowable on the date. Your strategy silently "sees the future."

This is a **free, real-data sample** of a point-in-time, survivorship-bias-free NSE/BSE dataset built to kill both. Real tickers, real dates — poke at it yourself.

## What's in here (`/sample_data`, CC-BY 4.0)

| file | size | what it proves |
|---|---|---|
| `survivorship_universe.csv` | 3,838 names | every name that traded 2010–2026, including **1,209 dead/delisted** with first- and last-traded dates. The names other sources silently drop. |
| `prices_sample.csv` | 100 names, daily | corporate-action-adjusted close **+ total-return close** + delivery %, including 14 famous dead names (DHFL, Jet, RCOM, Reliance Capital, Future Retail, Gitanjali, the merged PSU banks…). |
| `fundamentals_sample.csv` | 17 names | as-reported quarterly revenue / PAT / EPS **stamped with `announce_date`** — the date the number became public, not a restated figure. Median 33-day lag from period-end. |

*(Money columns in ₹ crore; `eps_basic` in ₹/share; `announce_lag_days` = announce_date − period_end.)*

## Poke it yourself (30 seconds)

1. Open `survivorship_universe.csv`, filter `status == inactive` → DHFL, Jet Airways, RCOM, IL&FS, Andhra/Dena/Vijaya Bank. **Your screener doesn't have these** — that's survivorship bias, and it's why your backtest looks better than reality.
2. Open `fundamentals_sample.csv` and read DHFL down the page — the collapse is right there in the *as-reported* numbers: 2019-03 PAT **−₹2,223 cr**, 2020-03 **−₹7,507 cr**, 2020-12 **−₹13,095 cr** — each stamped with the date it was actually announced.
3. Every `announce_date` falls *after* its `period_end` (median 33 days). That gap is exactly what restated screeners erase — and exactly how look-ahead bias sneaks into a backtest.

## Why it matters — run the 60-second demo

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Finance-broski/pit-data-sample/blob/main/PIT_survivorship_demo.ipynb)

The notebook runs the **same value strategy four ways** and shows how much pure data bias adds — no change to the strategy. *(The demo uses a labelled, research-calibrated simulation for the result; the data files above are real.)* Published research puts survivor-only overstatement around **+4.9 pp/yr** on the NIFTY Smallcap 250 — the gap is real, not theatrical. The tell that a backtest has these bugs: a Sharpe that looks too good and a suspiciously shallow drawdown.

## The full dataset

This is a taste. The full set: ~3,800 names of CA-adjusted **total-return** prices (2010–present, delisted included), as-reported fundamentals across ~1,400 names with full **announce-date vintages**, and **point-in-time universe membership** — reproducible from source, maintained, and delivered as files into your own environment (not behind an API).

**Want the bias measured on _your_ tickers, or the full panel?** Open an issue or reach out — pick 15–20 names and a date range, and diff it against whatever you use today.

## Honest scope

Income-statement fundamentals today (full balance-sheet / cash-flow on the roadmap). EOD, not intraday. This sample is a bounded slice, not the maintained universe.

---
*Data is derived from public exchange filings (NSE/BSE bhavcopy, corporate-action announcements, and as-filed XBRL results) and provided for research and illustration.*

*Built by **Finance-broski**. Free under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, fork it, build on it; just keep the attribution.*
