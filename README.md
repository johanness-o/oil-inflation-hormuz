# Oil, Inflation, and the Strait of Hormuz

An exploratory data analysis of the relationship between U.S. inflation, oil/gas prices, and tanker traffic through the Strait of Hormuz (2019–2025), with attention to how that relationship shifts during periods of geopolitical stress.

**Authors:** Joan Ojukwu, Duckwoo Kwon, Michael Brown

---

## Overview

This project investigates how U.S. inflation and energy prices move together over time, and whether that co-movement changes during major geopolitical events. It treats tanker traffic through the Strait of Hormuz — a critical chokepoint for global oil trade — as a third variable that may help explain price volatility beyond domestic factors.

The analysis is **correlational, not causal**. The goal is to identify and describe observable relationships across three major shocks:

- **COVID-19 (2020)** — historic collapse in global oil demand
- **Russia–Ukraine war (2022)** — WTI spiked to a record high near $114
- **Houthi attacks on Red Sea / Gulf of Aden shipping (late 2023–present)** — reduced Hormuz tanker traffic while oil prices stayed elevated

A key finding: the historically positive tanker–oil relationship broke after 2023, suggesting Hormuz throughput matters most as a *signal during geopolitical stress* rather than a steady predictor.

---

## Key Methodology Notes

Two decisions shape every result and are important for replication:

1. **Five-year window (2020–2025), not ten.** Shortened after reviewing the datasets' actual coverage.
2. **Month-over-month (MoM) inflation, not year-over-year (YoY).** YoY requires 12 months of prior data, which shrank the usable three-dataset merge to only 7 rows. MoM preserves all 31 months of the full three-dataset overlap.

The full three-dataset merge (CPI + commodities + Hormuz) spans **Jan 2019 – Jul 2021** (limited by the CPI dataset ending July 2021). A longer two-dataset window (commodities + Hormuz) extends to **Aug 2025**.

---

## Repository Structure

```
.
├── data/
│   ├── raw/                  # Original, unmodified source datasets
│   └── processed/            # Cleaned, monthly-frequency, merged outputs
├── notebooks/
│   ├── notebook1_commodity_prices.ipynb       # Clean commodity (WTI/Brent/gas) data
│   ├── notebook2_cpi_inflation.ipynb          # Clean CPI / inflation data
│   ├── notebook3_hormuz_arrivals.ipynb        # Clean Hormuz tanker traffic data
│   ├── INST447_ComparativeAnalysisNotebook.ipynb  # Merge + final analysis & figures
│   └── notebook5_ml.ipynb                     # Extra: predictive modeling
├── figures/                  # Exported visualizations
├── README.md
└── requirements.txt
```

---

## Data Sources

| Dataset | Source | Notes |
|---|---|---|
| U.S. inflation (CPI) | [Kaggle — varpit94](https://www.kaggle.com/datasets/varpit94/us-inflation-data-updated-till-may-2021) | Ends July 2021; limits the full merge window |
| Major natural resource prices (WTI, Brent, natural gas) | [Kaggle — Bircoci](https://www.kaggle.com/datasets/albertobircoci/historical-prices-of-major-natural-resource) | Daily; converted to monthly |
| Strait of Hormuz ship traffic | [IMF PortWatch](https://portwatch.imf.org/) | Daily; converted to monthly |
| U.S. crude oil production (context) | [U.S. EIA](https://www.eia.gov/dnav/pet/hist/LeafHandler.ashx?n=pet&s=f000000__3&f=m) | Reference |

---

## Replication Steps

### 1. Setup

```bash
git clone https://github.com/<your-username>/oil-inflation-hormuz.git
cd oil-inflation-hormuz
pip install -r requirements.txt
```

### 2. Obtain the raw data

The Kaggle and IMF datasets are not redistributed here if licensing requires direct download. Place each raw file in `data/raw/`. (If you *are* including them, note that here instead.)

### 3. Run the notebooks in order

| Step | Notebook | What it does |
|---|---|---|
| 1 | `notebook1_commodity_prices.ipynb` | Cleans the commodity price data (WTI, Brent, natural gas): converts daily data to monthly frequency and standardizes dates to a month-end key. Outputs to `data/processed/`. |
| 2 | `notebook2_cpi_inflation.ipynb` | Cleans the CPI/inflation data and computes month-over-month (MoM) inflation. Outputs to `data/processed/`. |
| 3 | `notebook3_hormuz_arrivals.ipynb` | Cleans the Strait of Hormuz tanker traffic data: converts daily arrivals to monthly frequency aligned to the same month-end join key. Outputs to `data/processed/`. |
| 4 | `INST447_ComparativeAnalysisNotebook.ipynb` | Merges all three cleaned datasets, runs the comparative analysis, and produces the final figures: inflation vs. WTI over time, the Pearson correlation heatmap, the rolling 6-month tanker–WTI correlation, and the event-marked WTI + tanker chart. |
| 5 | `notebook5_ml.ipynb` | *(Optional / extra)* Predictive modeling beyond the core correlational analysis. |

**Run order matters:** the three cleaning notebooks (1–3) must run before the comparative analysis (4), which depends on their cleaned outputs in `data/processed/`.

---

## Key Results (Full Window, 2019–2021)

Correlation with MoM inflation:

| Variable | Pearson r | Strength |
|---|---|---|
| WTI crude | 0.85 | Strong |
| Brent crude | 0.83 | Strong |
| Natural gas | 0.47 | Moderate |
| Tanker arrivals | 0.44 | Moderate |
| Total arrivals | 0.42 | Weak–moderate |

The COVID period (April 2020) produced the most extreme values, with inflation and WTI both bottoming out together. The energy–inflation correlation is real but **not stable across all periods**.

---

## Citation

If referencing this work:

> Ojukwu, J., Kwon, D., & Brown, M. (2026). *Oil, Inflation, and the Strait of Hormuz: A correlational analysis of U.S. energy prices and global supply disruptions.*

See `References` in the project report for full source citations.
