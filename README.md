# NYSED Data Integrity & Risk Monitoring Platform

A synthetic-data demonstration of the statistical methods used to evaluate **data quality,
validation, and risk assessment** for a state education agency's data collection operation —
built entirely in Python (`.ipynb` notebooks) with a static dashboard site for GitHub Pages.

⚠️ **Demonstration purpose only.** All district names, enrollment figures, submission records,
and risk determinations are entirely simulated and do not represent any real school district,
student, or New York State agency data.

**Live site:** `https://<your-username>.github.io/NYSED-Data-Integrity/`
(after enabling GitHub Pages — see below)

## What this is

Modeled after [NYSED's actual data-governance function](https://www.nysed.gov/information-reporting-services)
(Special Education / IDEA / SIRS reporting oversight), this project builds a synthetic dataset of
60 districts across 5 NYS regions and walks through five families of statistical technique:

| # | Notebook | Statistical techniques |
|---|----------|------------------------|
| 01 | `data_generation.ipynb` | Synthetic data generation (districts, submissions, IDEA indicators, validation rules, student roster) |
| 02 | `data_quality_assessment.ipynb` | Missingness pattern analysis (MCAR/MAR diagnostic), Pareto analysis, Cohen's kappa, SPC p-chart |
| 03 | `validation_engine.ipynb` | Rule-based validation engine, Jaro-Winkler + Fellegi-Sunter-style record linkage, precision/recall/F1 |
| 04 | `risk_assessment.ipynb` | CUSUM/EWMA drift detection, Z-score/IQR/Isolation Forest outliers, Benford's Law, logistic regression & random forest risk model, empirical Bayes shrinkage |
| 05 | `executive_dashboard.ipynb` | KPI scorecard, Folium district map, threshold alerts, static site assembly |

Each notebook reads/writes CSVs in `data/` and (for 02–05) exports a static HTML dashboard page to
`docs/dashboard/`.

## Repository structure

```
├── notebooks/                     # all analysis code — run in order 01 → 05
│   ├── 01_data_generation.ipynb
│   ├── 02_data_quality_assessment.ipynb
│   ├── 03_validation_engine.ipynb
│   ├── 04_risk_assessment.ipynb
│   └── 05_executive_dashboard.ipynb
├── data/                          # synthetic CSVs written by notebook 01 (+ derived files)
├── docs/                          # GitHub Pages site root
│   ├── index.html                 # landing page
│   ├── assets/style.css           # shared design system
│   └── dashboard/                 # 7 dashboard pages, written by notebooks 02–05
├── requirements.txt
└── README.md
```

## Quick start

```bash
git clone https://github.com/zia207/NYSED-Data-Integrity.git
cd NYSED-Data-Integrity
pip install -r requirements.txt

# run notebooks in order — each depends on the previous one's output
jupyter nbconvert --to notebook --execute --inplace notebooks/01_data_generation.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/02_data_quality_assessment.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/03_validation_engine.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/04_risk_assessment.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/05_executive_dashboard.ipynb

# then open docs/index.html locally, or deploy docs/ via GitHub Pages
```

Or open each notebook in Jupyter Lab / VS Code and run all cells interactively — every notebook
is fully self-contained and reproducible (fixed random seed).

## Deploying to GitHub Pages

1. Push this repository to GitHub.
2. In **Settings → Pages**, set **Source** to "Deploy from a branch," branch `main`, folder `/docs`.
3. GitHub Pages will publish `docs/index.html` as the site root within a few minutes.

The dashboard pages use Plotly.js embedded inline (no external CDN dependency), so the site works
fully offline once cloned, and isn't affected by CDN availability once deployed.

## Notes on the synthetic data

- District names, BEDS-style codes, and student records are procedurally generated — none
  correspond to real NYSED entities.
- Regions/counties reuse real NY geography for realism, but district-to-county assignment is
  random.
- A "quality latent" parameter drives most downstream metrics via a two-component mixture
  (most districts perform well; a minority are chronic low performers) — this is what gives the
  risk-tier distribution its realistic, skewed shape.
- The student roster includes 150 **known** injected near-duplicate records, used as ground truth
  to evaluate the record-linkage algorithm in Notebook 03.

## License

Demonstration code, freely reusable. Not affiliated with or endorsed by NYSED.
