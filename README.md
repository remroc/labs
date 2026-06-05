# labs

Quantitative analyses on current financial-market events.

This repository collects standalone, reproducible Jupyter notebooks that apply quantitative-finance methods (correlation regimes, factor analysis, stress diagnostics, event studies) to news-driven episodes. Each analysis lives in a dated sub-folder and is self-contained: notebook, source data, intermediate calculations, plots, and any companion documents (PDF synthesis, blog draft) all sit together.

## Index of analyses

| Folder | Date | Title | One-line summary |
|--------|------|-------|------------------|
| [`202606/`](202606/) | 2026-06-05 | Cross-asset stress diagnostic | Six independent measures applied to the broad sell-off of June 5, 2026 (Burry AI-bubble thesis, Kabbaj "vendredi noir" video). Verdict: 3/6 ORANGE; Forbes-Rigobon shows the headline correlation jump is largely a volatility artifact. |

## Methodology principles

- **Reproducible**: each analysis ships with the exact data used (Yahoo Finance daily CSVs) so that re-running the notebook with `jupyter nbconvert --to notebook --execute` yields identical numbers.
- **Self-contained**: each sub-folder owns its data, plots, and outputs. No global config, no hidden dependencies between analyses.
- **Quantitative discipline**: when raw measures can mislead (e.g. correlation inflation under volatility spikes), we apply the appropriate correction (Forbes-Rigobon 2002, in the case of the June 2026 analysis) and report both versions.
- **Honest about limits**: each notebook includes a "limitations" section. Stress diagnostics describe configurations, not forecasts.

## How to run any analysis

```bash
cd <analysis_folder>
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute *.ipynb
```

Or open the notebook directly in JupyterLab / VSCode.

## Author

Rémi Roche -- [LinkedIn](https://www.linkedin.com/in/rremi/)

## License

[MIT](LICENSE). Fork, cite, adapt freely with attribution.
