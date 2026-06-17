# PIT data — the demand-test sample

The free asset for Phase 0 of the data venture: a one-click notebook that *proves* the survivorship + look-ahead gap a prospect's current data is hiding. This is what you hand a quant/PMS/fintech lead so the value sells itself.

## What's here
- **`PIT_survivorship_demo.ipynb`** — self-contained Colab/Jupyter notebook. Runs the *same* value strategy four ways and shows the gap. Outputs are pre-embedded, so it's compelling even before they run it; `Runtime → Run all` reproduces it in ~10s with zero setup.
- **`preview_equity_curves.png`** — the money chart (four equity curves). Drop it straight into a forum post, X thread, or DM.

## The punchline it lands
| Treatment | CAGR | Sharpe | Max DD |
|---|---|---|---|
| Honest (PIT + survivorship-free) | 12.0% | 0.79 | −35% |
| + Survivorship bias only | 17.1% | 1.09 | −27% |
| + Look-ahead bias only | 14.9% | 0.96 | −34% |
| **Naive (both — typical screener backtest)** | **20.4%** | **1.26** | **−29%** |

**+8.4 pp/yr CAGR and +0.47 Sharpe of pure data bias** — no strategy change. The survivorship-only effect (~+5pp) is calibrated to match published research (NIFTY Smallcap 250: survivor-only overstates returns ~4.9 pp/yr), so the demo is realistic, not theatrical.

## How to use it in the demand test
1. Upload the `.ipynb` to **Google Colab**, set link-sharing, and paste the link in your outreach (forum reply, X DM, LinkedIn). One click, runs in the browser — no install friction.
2. Or attach the `.ipynb` directly / screenshot the PNG for the hook.
3. The ask stays: *"Try it, then — how are you handling point-in-time + delisted names today, and would you pay for a feed that just gives you the honest line?"* (full script in `docs/gtm/DEMAND_TEST.md`).

## Honesty note (keep it in)
The dataset is **simulated and labelled as such** — calibrated to real published bias magnitudes. The real product is the same structure on real NSE/BSE names. The notebook has a **"plug in YOUR data"** cell with the exact schema your pipeline outputs, so the real-data version is a swap-in once a prospect wants a sample on actual tickers. Don't pass the simulation off as real data — the credibility *is* the honesty.
