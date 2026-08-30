# Forecasting Crude Oil Inventory Changes

Master's thesis (SKEMA Business School). The project studies the weekly change in US commercial crude oil stocks excluding the SPR (EIA series `WCESTUS1`) and asks whether geopolitical risk carries forecasting or pricing value beyond standard market fundamentals.

The work is organized as two hypotheses, developed sequentially in `thesis.ipynb`.

- **H1 — Inventory forecasting.** Does the geopolitical risk index (GPR, Caldara & Iacoviello, 2022) improve out-of-sample forecasts of the weekly stock change beyond a fundamentals block in the spirit of Kilian & Murphy (2014)? Models: ARIMA/SARIMA baseline, VAR, Random Forest, XGBoost, each estimated with and without the GPR block and evaluated by walk-forward, one-step-ahead, out-of-sample forecasts (RMSE, MAE, directional accuracy).
- **H2 — Price reaction to inventory surprises.** Does the surprise component of the weekly EIA release (actual minus analyst consensus) move the WTI price and the Brent/WTI spread on the announcement day, following Bu (2014)? Also tests whether geopolitical risk amplifies that reaction, and whether the H1 forecasts carry information beyond the analyst consensus.

## Repository structure

    thesis.ipynb              Main notebook: EDA, modelling, and results for H1 and H2
    data-cleaning.ipynb       Builds the aligned weekly dataset from raw sources
    Dataset/
      IEA/                    EIA weekly series (stocks, production, refinery, imports, WTI)
      GPR/                    Geopolitical Risk Index (Caldara & Iacoviello)
      FRED/                   Index of Global Real Economic Activity (IGREA)
      H1_dataset_weekly.csv   Aligned weekly dataset used for H1
      H2_dataset.csv          Aligned daily/event data used for H2 (public series only)

## Data availability

All EIA, FRED and GPR data used here are publicly available and included in this repository. **Bloomberg data (the EIA survey consensus and the WTI, Brent and OVX price series) are not redistributed, in accordance with Bloomberg's licensing terms.** The code that processes them is included, and the series can be reconstructed by any user with a Bloomberg terminal using the tickers documented in the notebook.

## Data and alignment conventions

- EIA weekly data is labelled by the Friday week-ending date but published the following Wednesday (or Thursday on holiday weeks). Every predictor is aligned to that publication date to avoid look-ahead bias.
- GPR (daily) is averaged over the week preceding each release, ending the day before publication.
- IGREA (monthly) is lagged and forward-filled onto the weekly calendar, reflecting real-time availability.
- The forecasting target is the first difference of crude stocks (`target_stock_change`), which is stationary, unlike the level.

## Methodology summary

All forecasting models follow the same evaluation protocol: a chronological train/test split, walk-forward one-step-ahead forecasts, and out-of-sample metrics only (RMSE, MAE, directional accuracy), benchmarked against a no-change forecast. Each model is run in a restricted specification (fundamentals only) and an augmented specification (fundamentals plus GPR) to isolate the incremental contribution of geopolitical risk.

## Requirements

Python 3.11, with `pandas`, `numpy`, `statsmodels`, `scikit-learn`, `xgboost`, `matplotlib`, and `seaborn`.

## Authors

Anthony Da Conceiçao and Axel Cohen — MSc Financial Markets and Investments, SKEMA Business School.
