# Indian Equity Index-Tier Constituents & Weights — survivorship-free, point-in-time

**File:** `index_membership.csv` · **Coverage:** 2019-01-31 → 2026-01-31 (15 semi-annual reconstitutions) · **Reconstructed from public exchange filings**

A point-in-time, survivorship-free, **free-float-weighted** reconstruction of Indian equity index tiers — approximate NIFTY 50 / Next 50 / Midcap 150 / Smallcap 250 — with per-name **weights**. Built to fill the gap no vendor (incl. LSEG) supplies cleanly: *historical* Indian index constituents and weights that include the names which later delisted, with no look-ahead.

This is a **documented proxy, not official NSE/NIFTY membership.** Read §4 (Limitations) before using — the honesty is the point.

---

## 1. Schema

| column | type | meaning |
|---|---|---|
| `reconstitution_date` | date | Semi-annual reconstitution effective date (Jan-31 / Jul-31). The membership and weights are as-of this date. |
| `tier` | string | `NIFTY50_proxy` (rank 1–50) · `NEXT50_proxy` (51–100) · `MIDCAP150_proxy` (101–250) · `SMALLCAP250_proxy` (251–500). |
| `symbol` | string | NSE trading symbol. |
| `mcap_rank` | int | Rank by **free-float** market cap within the eligible universe at that reconstitution (1 = largest). |
| `avg_mcap_cr` | float | Trailing **6-month average free-float market cap**, ₹ crore. |
| `weight_pct` | float | Free-float market-cap weight **within the tier** (sums to 100% per tier per reconstitution). Uncapped. |
| `in_liquid_top500` | bool | Whether the name was also in the liquidity-ranked (turnover) top-500 universe at that reconstitution — a secondary investability flag. |

**Shape:** 7,245 constituent-rows · 904 distinct symbols · 15 reconstitutions · 50/50/150/250 names per tier.

---

## 2. How to use it (point-in-time)

- **Join key:** `(reconstitution_date, symbol)`.
- **As-of any date D:** take the most recent `reconstitution_date ≤ D`. Never use a later reconstitution — that would inject look-ahead.
- **Backtest:** on each `reconstitution_date`, rebalance to that tier's constituents at `weight_pct`. Between reconstitutions, hold.
- **Survivorship-free:** names that later delisted/merged appear in the reconstitutions where they were alive, then drop out. Your backtest holds the names that actually existed at the time, not today's survivors.

*Worked example — latest `NIFTY50_proxy` (2026-01-31), top 5 by weight:* HDFCBANK 12.56%, RELIANCE 8.94%, ICICIBANK 8.68%, BHARTIARTL 4.85%, INFY 4.58%. (Validation: this matches the live NIFTY 50, where HDFCBANK is the top weight ~13% — a market-cap-only screen never reproduces that ordering; free-float does.)

---

## 3. Methodology

**Shares outstanding** — derived, point-in-time. Base = `PAT ÷ EPS` from announce-dated quarterly XBRL, as a trailing rolling median per company (robust to corrupted/erratic filings). Splits and bonuses are then applied **at their ex-date** from the exchange corporate-action record, and **each action is validated against the actual EPS-share change**: a real equity event (split, or bonus e.g. HDFCBANK 1:1 on 2025-08-26, BSE 2:1 on 2025-05-23) is carried even before the next filing reports it, while a non-equity event (e.g. an NCRPS preference-share "bonus") is rejected. Validated within a few percent against ground truth across RELIANCE, HDFCBANK, TCS, INFY, ICICI, ITC, LT, KOTAK, AXIS, SBIN, and others.

**Market cap** — raw (unadjusted) close × derived shares. Using the *raw* close (not the split-adjusted series) keeps market cap corporate-action-invariant.

**Free float** — full market cap × `(1 − promoter & promoter-group %)`, where promoter % comes from the NSE quarterly **shareholding-pattern filings** (SEBI LODR Reg 31), point-in-time. For PSUs the promoter is the Government of India, so their small float is captured correctly (e.g. IRFC factor ≈ 0.15, HAL ≈ 0.28, COALINDIA ≈ 0.37).

**Tiers & weights** — at each semi-annual reconstitution, names are ranked by trailing-6-month-average free-float market cap; rank bands 1–50 / 51–100 / 101–250 / 251–500 map to the four tier proxies. `weight_pct` is each name's free-float-mcap share of its tier.

**Eligibility** — a name qualifies at a reconstitution if it was trading at the cutoff (survivorship-free, includes soon-to-delist names) with ≥90 days of history and a derivable share count.

---

## 4. Limitations — read before relying on this

1. **Proxy, not the rulebook.** Tiers are by free-float market-cap *rank*. The official NSE indices add eligibility rules this does not model — impact-cost/liquidity screens, F&O availability, listing history, and reconstitution buffers. Consequence: some names appear here that the official index excludes (e.g. PAYTM, POLICYBZR, TVSMOTOR by pure mcap), and the 50-vs-Next-50 split won't match official exactly. Use it as an **approximate tier proxy**, not as "NIFTY constituents."
2. **Free float is promoter-based.** `1 − promoter%` is a strong approximation of NSE's exact Investible Weight Factor, which additionally nets out locked-in/strategic holdings and rounds up to 0.05. It captures the dominant driver (promoter/government holding); it is not the exact official factor.
3. **Shares are derived, not official.** PAT/EPS plus corporate-action carry; accurate to a few percent on large caps. Known residual: a name with a very recent bonus can read ~10–15% high from PAT/EPS-ratio noise (e.g. RELIANCE), slightly inflating its weight. Does not change tier assignment.
4. **Universe coverage.** Limited to names with capturable announce-dated fundamentals and a derivable share count (~900 distinct names over the window). Names with no fundamentals, or near-zero/erratic EPS, are absent. Fine for large-caps; some mid/small names are missing. This is **not** absolute index coverage.
5. **Window is 2019–2026**, set by the announce-dated fundamentals. Prices reach 2010, but derived shares (hence market cap, hence membership) require the fundamentals window. Do not describe it as "since 2010."
6. **Weights are uncapped.** Real NSE applies single-stock / F&O caps to some indices; not modelled here.

---

## 5. Provenance & disclaimer

Reconstructed entirely from public sources: NSE/BSE bhavcopy (prices), exchange corporate-action filings, announce-dated XBRL quarterly results (shares, via PAT/EPS), and NSE shareholding-pattern filings (free float). No proprietary or licensed index data is used.

This is a **technical data reconstruction, not investment advice and not official index data.** It is an approximate, survivorship-free, point-in-time index-tier proxy. All figures are point-in-time and reproducible from the named public sources; the methodology and its limitations are stated above in full.
