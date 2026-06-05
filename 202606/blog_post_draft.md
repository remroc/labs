# Is "Black Friday" 2026 the Start of Burry's Crash?

## A quantitative cross-asset stress diagnostic for June 5, 2026

**TL;DR.** On Friday, June 5, 2026, equities, precious metals, oil and crypto sold off in unison. The instinctive reading is that "everything correlated = systemic stress." A formal diagnostic across six independent quantitative measures (rolling pairwise correlation, PCA absorption ratio, cross-sectional dispersion, Forbes-Rigobon volatility-adjusted correlation, gold-equity decorrelation, and VIX dynamics) gives a **3-out-of-6 scorecard**: the flight-to-quality regime is *clearly* broken, tech has corrected meaningfully (avg ‑12.4% from 30-day peaks), and both metals have dropped together. However, the headline correlation jump is *largely a volatility artifact*: the Forbes-Rigobon adjustment puts the genuine correlation regime change at only +0.01 vs. the 180-day calm baseline. The picture is best described as **orange, not red** — a deteriorating regime, not yet a full systemic event.

The notebook, source data, and intermediate calculations are open and reproducible at [github.com/remroc/labs/tree/main/202606](https://github.com/remroc/labs/tree/main/202606) ([notebook](https://github.com/remroc/labs/blob/main/202606/crisis_2026_06_analysis.ipynb) · [synthesis PDF](https://github.com/remroc/labs/blob/main/202606/synthesis.pdf)).

---

## 1. The trigger

A YouTube video published on June 5, 2026 by **Thami Kabbaj** — French-Moroccan trader, *agrégé d'économie* (Paris-II Assas, Magistère Banque-Finance), former Eurex trader, FSA Derivatives certified, Series 7, graduate of the Society of Technical Analysts (London), founder of TKL Trading School (Dubai) and one of the most-watched financial educators in the French-speaking world (450,000+ YouTube subscribers, 80M+ views) — titled "*Vendredi noir : tout s'effondre en Bourse?*" flagged what looked like a coordinated decline in the Nasdaq 100, S&P 500, gold, silver, oil and large-cap stocks. The pattern Kabbaj highlighted is the kind that, in retrospect, has often preceded a deleveraging cascade. The credentials and audience of the source matter: this is not a random crash-call clip; Kabbaj's technical-analysis training (MST 1999 — rare among French-speaking finance professionals at the time) and behavioral-finance focus give the observation real weight.

Layered on top of this, Michael Burry (Scion Asset Management) has been publicly bearish since the end of 2025. The Q3 2025 13F-HR disclosed approximately **\$912M in notional Palantir puts** and **\$187M in notional Nvidia puts**. By mid-2026 commentary (Substack, May 10, 2026; Bloomberg, May 11, 2026), the shorts had been extended to **ORCL, SMH (semiconductor ETF), QQQ**, with January 2027 expiries — an unusually long-dated bet for someone whose previous Big Short was held for less than two years. The Shiller PE ratio crossed 40 in May, surpassing the 1999–2000 dot-com peak.

The question this note asks is not *whether Burry is right* (he himself notes that he has been wrong on previous crash calls). It is the narrower, more tractable question: **does the cross-asset behavior on June 5 fit the empirical signature of a systemic deleveraging event, or is it a more idiosyncratic move?**

## 2. Method

The diagnostic uses Yahoo Finance daily adjusted closes (via `yfinance`, `auto_adjust=True`) over a 400-day rolling window through June 5, 2026, on a universe of 21 instruments:

- **Equity indices**: NDX (Nasdaq 100), SPX (S&P 500), DJI (Dow Jones)
- **Mega-cap tech**: MSFT, AAPL, META, NFLX, NVDA, AVGO
- **Burry-disclosed shorts**: PLTR, NVDA, ORCL, SMH, QQQ
- **Safe haven / metals**: XAU (gold front-month), XAG (silver front-month), TLT (20+y Treasury ETF)
- **Energy**: WTI (CL=F), Brent (BZ=F)
- **Crypto**: BTC
- **Volatility / dollar**: VIX, DXY

Returns are computed as log returns. All cross-asset measures are restricted to U.S. trading days to avoid the BTC-weekend bias. Pairwise correlations use the maximum overlap available per pair, not a global `dropna` that would bias the panel.

The six diagnostic measures are:

1. **Average pairwise correlation index** (rolling 7d, 30d) with bootstrap 95% CIs and a 400-day P90 reference;
2. **PCA absorption ratio** AR(k=1) and AR(k=3) on a 60-day rolling window (Kritzman, Li, Page, Rigobon, 2010);
3. **Cross-sectional return dispersion** as the daily standard deviation of returns across the universe (Pukthuanthong & Roll, 2009);
4. **Forbes-Rigobon (2002) volatility-adjusted correlation** to control for the well-known fact that vanilla Pearson correlation inflates mechanically when one underlying becomes more volatile;
5. **Gold vs. equities pairwise correlation** as a flight-to-quality breakage test;
6. **VIX level and 7-day velocity** as a price-of-insurance gauge.

A composite scorecard aggregates these into a 0-6 heuristic. Each check is binary ("signal firing" or "not yet"); the score is *not* a probability of a crash but a summary of *how many* contemporaneous stress configurations are simultaneously present.

## 3. What the data says

### Performance distribution on the trigger day

Sorted from worst to best one-day return on June 5, 2026 (selection):

| Asset | 1-day | 7-day | 30-day |
|-------|-------|-------|--------|
| AVGO  | **-19.5%** | -13.7% | -9.3% |
| SMH (semis ETF) | **-10.7%** | -4.9% | +3.6% |
| ORCL | -7.2% | -5.4% | +10.1% |
| WTI | -6.0% | +3.3% | -5.1% |
| NDX | -5.3% | -4.5% | +1.3% |
| QQQ | -5.3% | -4.5% | +1.3% |
| BRENT | -5.1% | +0.9% | -8.3% |
| META | -4.8% | -6.3% | -3.2% |
| PLTR | -4.7% | -13.4% | +1.3% |
| NVDA | -4.4% | -2.8% | -1.2% |
| BTC | -3.5% | **-15.8%** | -24.1% |
| MSFT | -2.5% | -7.5% | +0.9% |
| SPX | -2.3% | -2.6% | +0.3% |
| XAU | -1.9% | -4.5% | -7.0% |
| **VIX** | **+33.9%** | **+40.4%** | +23.7% |
| DXY | +0.5% | +1.2% | +2.1% |

The dispersion of one-day returns is striking. AVGO and SMH lead the decline, consistent with a narrow semiconductor-led sell-off. Gold (XAU) is *down*, not up — the first hint that the move is *not* a classic equity-correction-with-flight-to-quality.

### Diagnostic 1: average pairwise correlation

- 7-day: +0.300 (bootstrap 95% CI: [-0.03, +0.54])
- 30-day: +0.170 (bootstrap 95% CI: [+0.04, +0.31])
- Historical 400-day P90: +0.314

The 7-day reading is *just below* the 90th percentile threshold. Elevated, but not yet in the deep tail.

### Diagnostic 2: PCA absorption ratio

The share of variance absorbed by the first principal component over a 60-day window is 0.426 — slightly above the historical median of 0.411 but **below** the P90 of 0.449. The z-score against the full sample distribution is +0.58 — a modest move, not a regime break.

### Diagnostic 3: Forbes-Rigobon adjustment — *this matters*

The raw 7-day average pairwise correlation rose from +0.134 (180-day calm baseline) to +0.180 (recent 7-day). The Forbes-Rigobon volatility-adjusted version, however, is +0.143 — **nearly identical to the calm baseline**.

The implication is sharp and important: the apparent correlation jump is **predominantly a mechanical consequence of higher within-asset volatility**, not a *structural* shift in the comovement relationships. This is the single most important caveat in the analysis. Eyeballing a heatmap during a vol spike can produce a misleading "everything is correlated now" impression.

### Diagnostic 4: gold-equities decoupling

This is the diagnostic that *did* fire, and dramatically:

- Current 7-day correlation between XAU and SPX: **+0.765**
- 30-day correlation: +0.771
- 180-day baseline: +0.265

A gold-equities correlation in the +0.7 to +0.8 range over a short window is *not* compatible with the textbook "gold as hedge" regime. It is, however, exactly the regime one observes during **margin-call cascades**, when leveraged funds liquidate every position — including gold — to meet calls. This is the deleveraging signature.

### Diagnostic 5: VIX dynamics

VIX closed at 21.5 — still in the calm-to-elevated zone, *below* the 30 stress threshold. But the 7-day acceleration is +36.7%. Velocity is high; level is not yet extreme. If the sell-off continues on Monday, VIX > 30 is plausible.

### Diagnostic 6: both metals decline together

XAU is down 3.2% on 7 days. XAG is down 10.1%. Both metals falling together — and silver leading the decline — is a hallmark of risk-asset liquidation. In risk-on regimes, silver tends to lead on the upside (more leverage to industrial demand); in distress, it leads on the downside (lower liquidity, larger discounts).

### Composite scorecard

| Diagnostic | Threshold | Reading | Fires? |
|------------|-----------|---------|--------|
| Avg pairwise corr 7d > P90 (30d hist) | >+0.314 | +0.300 | ☐ |
| XAU-SPX 7d corr > +0.5 (flight broken) | >+0.5 | **+0.765** | ☑ |
| Avg tech max-DD from 30d peak < -10% | <-10% | **-12.4%** | ☑ |
| VIX > 30 or VIX 7d return > +50% | breach | 21.5 / +37% | ☐ |
| Both metals 7d < -3% | both | **XAU -3.2%, XAG -10.1%** | ☑ |
| PCA AR(1) > P90 hist | >0.449 | 0.426 | ☐ |

**Score: 3 / 6** — orange, not red. Three diagnostics are firing simultaneously; three are *close to firing* but not yet through their historical 90th percentile thresholds. The deleveraging signature is present (flight-to-quality broken, metals down together), the tech correction is meaningful (avg -12.4% from peaks), but the systemic indicators (overall correlation, absorption ratio, VIX level) remain sub-threshold.

## 4. The Burry positions, on the day

A 13F filing discloses *notional* puts as of a snapshot date; it does not disclose realized P&L (puts have time decay and IV exposure that are not captured by the stock return). With that caveat:

| Ticker | Entry (Sep 30, 2025) | Latest (Jun 5, 2026) | Stock return | Stock-side put inference |
|--------|---------------------:|---------------------:|-------------:|--------------------------|
| **PLTR** | 182.42 | 135.53 | **-25.7%** | profitable |
| **ORCL** | 279.04 | 213.68 | **-23.4%** | profitable |
| NVDA | 186.34 | 205.10 | +10.1% | underwater |
| QQQ  | 598.84 | 705.06 | +17.7% | underwater |
| SMH  | 325.35 | 569.69 | +75.1% | underwater (but: -10.7% on June 5 alone) |

Two of five positions are clearly in the money on the stock side. SMH, the semiconductor ETF and the headline short, was up +75% from the snapshot — but lost **10.7% on the trigger day**. The convexity of put payoffs means that a continuation of the June 5 move would close the gap on SMH rapidly.

## 5. What this notebook is *not*

This is not a forecast. The Forbes-Rigobon result in particular says that one should be wary of inferring a regime change from raw correlation pictures alone during a volatility spike. The three diagnostics that *are* firing (flight broken, metals down, tech corrected) describe a real *configuration*; whether that configuration deepens into a self-reinforcing cascade depends on policy response, positioning, and macro flow data that this snapshot deliberately does not include.

Burry has been wrong before, and he says so. He has also been right before, with consequence. The right posture for risk management — independent of one's view on the AI bubble — is to treat the *combination* of (i) Shiller PE > 40, (ii) extended large-tech leverage, and (iii) a confirmed gold-equity decorrelation breakdown as **a higher base-rate environment for tail risk**, not a forecast of an imminent crash.

## 6. Reproducibility

The full notebook (Jupyter), source data (CSV per ticker, panels, intermediate calculations), and plots are released alongside this note.

- **Notebook**: [`crisis_2026_06_analysis.ipynb`](https://github.com/remroc/labs/blob/main/202606/crisis_2026_06_analysis.ipynb) — rendered by GitHub with all plots inline
- **Synthesis PDF**: [`synthesis.pdf`](https://github.com/remroc/labs/blob/main/202606/synthesis.pdf) — 4-page companion document
- **Raw data**: [`data/csv/`](https://github.com/remroc/labs/tree/main/202606/data/csv) — 21 per-ticker CSVs + `close_prices_panel.csv` + `log_returns_panel.csv`
- **Plots**: [`data/plots/`](https://github.com/remroc/labs/tree/main/202606/data/plots) — 9 PNGs
- **Scorecard JSON**: [`data/scorecard.json`](https://github.com/remroc/labs/blob/main/202606/data/scorecard.json)
- **Manifest**: [`data/manifest.json`](https://github.com/remroc/labs/blob/main/202606/data/manifest.json) with universe, date range, and source attribution.

To re-run end-to-end:

```bash
pip install yfinance pandas numpy matplotlib scipy jupyter
jupyter nbconvert --to notebook --execute crisis_2026_06_analysis.ipynb
```

## References

- Forbes, K. J. & Rigobon, R. (2002). *No Contagion, Only Interdependence: Measuring Stock Market Comovements*. **Journal of Finance** 57(5).
- Kritzman, M., Li, Y., Page, S., & Rigobon, R. (2010). *Principal Components as a Measure of Systemic Risk*. **Journal of Portfolio Management**.
- Pukthuanthong, K. & Roll, R. (2009). *Global Market Integration: An Alternative Measure and Its Application*. **Journal of Financial Economics**.
- Burry, M. (2026, May 10). *Short Thoughts*. michaeljburry.substack.com.
- Bloomberg News (2026, May 11). *Michael Burry Warns of Stock Crash as Tech Jump Echoes 2000 Peak*.
- Scion Asset Management 13F-HR filing as of September 30, 2025 (SEC EDGAR).
- Kabbaj, T. (2026, June 5). *Vendredi noir : tout s'effondre en Bourse?*. YouTube (TKL channel). https://www.youtube.com/watch?v=JqFg1Oup11k

---

*All data are end-of-day from Yahoo Finance. This note is for informational purposes only and is not investment advice.*
