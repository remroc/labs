# Cross-Asset Stress Diagnostic -- June 5, 2026

**Triggering event.** On Friday June 5, 2026, equities, precious metals, oil and crypto sold off in unison. The move was flagged the same day by Thami Kabbaj (TKL Trading School, *agrégé d'économie*, ex-Eurex trader) in his video *Vendredi noir : tout s'effondre en Bourse?*, and lands in the middle of Michael Burry's ongoing AI-bubble short campaign (Scion Asset Management, Q3 2025 13F: ~\$912M PLTR puts, ~\$187M NVDA puts; 2026 expansion to ORCL, SMH, QQQ with January 2027 expiries; Shiller PE > 40).

**Question.** Does the move carry the statistical signature of a forced-liquidation / margin-call regime, or is it an idiosyncratic asset-class move?

**Method.** Six independent diagnostics on 21 instruments over a 400-day window (Yahoo Finance daily adjusted closes):
1. Rolling pairwise correlation (Pearson + Spearman) with proper pairwise overlapping data
2. PCA absorption ratio AR(k=1), AR(k=3) (Kritzman, Li, Page, Rigobon 2010)
3. Average pairwise correlation index + bootstrap 95% CI
4. Cross-sectional return dispersion (Pukthuanthong-Roll 2009)
5. **Forbes-Rigobon (2002)** volatility-adjusted correlation
6. Gold-equity rolling correlation (flight-to-quality test)

**Verdict.** 3 out of 6 signals firing -- ORANGE regime, not red.
- ☑ XAU-SPX 7d corr = +0.77 (vs +0.27 baseline) -- flight-to-quality broken.
- ☑ Avg tech max-DD from 30d peak = -12.4%.
- ☑ Both metals 7d < -3% (XAU -3.2%, XAG -10.1%).
- ☐ Avg pairwise corr 7d = +0.300 (just below P90 = +0.314).
- ☐ VIX = 21.5, +37% velocity in 7d (level still sub-stress).
- ☐ PCA AR(1) = 0.426 (just below P90 = 0.449).

**The single most important finding.** The raw 7-day average pairwise correlation rose from +0.134 (180d calm baseline) to +0.180. The Forbes-Rigobon adjustment puts it at +0.143 -- **essentially identical to baseline**. The apparent "everything correlated now" picture is overwhelmingly a volatility artifact, not a structural regime change.

---

## Files in this folder

| File | Content |
|------|---------|
| `crisis_2026_06_analysis.ipynb` | Full notebook (11 sections, ~14 cells, all plots inlined) |
| `synthesis.pdf` | 4-page companion document (executive summary + methodology + key results + scorecard) |
| `blog_post_draft.md` | Long-form article draft (Medium / Substack format) |
| `linkedin_post.md` | LinkedIn post draft (~600 words) + design notes |
| `data/csv/` | 21 per-ticker daily CSVs + `close_prices_panel.csv` + `log_returns_panel.csv` + intermediate calculations |
| `data/plots/` | 9 PNG plots (correlation heatmaps, stress index, absorption ratio, drawdowns, flight-to-quality, Burry shorts, VIX/DXY, normalized prices, performance bars) |
| `data/scorecard.json` | Machine-readable verdict |
| `data/manifest.json` | Configuration used (tickers, date range, source attribution) |

## How to reproduce

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute crisis_2026_06_analysis.ipynb
```

End-to-end run downloads fresh data from Yahoo Finance (via `yfinance`) and regenerates all CSVs / plots / scorecards. The notebook caches downloaded data for 1 hour in `data/csv/<TICKER>.csv`.

## Sources

- [Thami Kabbaj -- *Vendredi noir : tout s'effondre en Bourse?* (YouTube, June 5, 2026)](https://www.youtube.com/watch?v=JqFg1Oup11k)
- [Michael Burry -- Short Thoughts (Substack, May 10, 2026)](https://michaeljburry.substack.com/p/short-thoughts-may-10-2026)
- [Bloomberg -- Michael Burry Warns of Stock Crash as Tech Jump Echoes 2000 Peak (May 11, 2026)](https://www.bloomberg.com/news/articles/2026-05-11/michael-burry-warns-of-stock-crash-as-tech-jump-echoes-2000-peak)
- Scion Asset Management 13F-HR filing, Q3 2025 (SEC EDGAR)

## References

- Forbes, K. J. & Rigobon, R. (2002). *No Contagion, Only Interdependence: Measuring Stock Market Comovements*. Journal of Finance 57(5).
- Kritzman, M., Li, Y., Page, S., & Rigobon, R. (2010). *Principal Components as a Measure of Systemic Risk*. Journal of Portfolio Management.
- Pukthuanthong, K. & Roll, R. (2009). *Global Market Integration: An Alternative Measure and Its Application*. Journal of Financial Economics.

---

*Disclaimer: this analysis is for informational purposes only and is not investment advice. All data are end-of-day from Yahoo Finance.*
