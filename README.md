# PM2.5 Air Quality Forecasting

This project is a time series forecasting study for hourly PM2.5 air pollution levels. It analyzes pollutant behavior, prepares leakage-safe time series features, compares statistical and machine learning forecasting models, and selects the strongest model for short-term PM2.5 forecasting.

The existing analysis code was not modified while preparing this GitHub-ready version.

## Problem Statement

PM2.5 is a major air quality indicator because fine particulate matter can affect public health and visibility. The forecasting problem is to predict future PM2.5 concentrations from historical pollutant measurements and time-based patterns so that pollution risk can be evaluated ahead of time.

## Dataset

The notebook downloads the main dataset from Kaggle using `kagglehub`:

- Dataset: `atifkhan12/global-air-pollution-dataset-2025-2026`
- Main modeling city: Lahore, Pakistan
- Frequency: hourly observations
- Target variable: `PM2.5`, cleaned as `PM2.5_Cleaned`
- Supporting pollutants used in modeling: `NO2`, `CO`, `SO2`
- Additional pollutants used in EDA where available: `PM10`, `Ozone`

The repository also includes [Actual_Series.csv](Actual_Series.csv), which contains 1,344 actual PM2.5 observations for eight cities from `2026-02-07 00:00:00` through `2026-02-13 23:00:00`.

## Main Objective

The objective is to compare SARIMAX, VARMA/VARMAX, and XGBoost forecasting approaches on a shared holdout period, identify the best-performing model, and use it for short-term PM2.5 forecasting.

## Methods

- Exploratory Data Analysis: pollutant trends, daily patterns, correlation heatmap, PM2.5 threshold counts, STL decomposition, ACF/PACF, and stationarity testing.
- SARIMAX: univariate PM2.5 forecasting with exogenous pollutant lag features and daily seasonality.
- VARMA/VARMAX: multivariate pollutant forecasting using PM2.5, NO2, CO, and SO2.
- XGBoost: supervised recursive forecasting using lag features, rolling features, time features, and pollutant regressors.

## Project Structure

```text
.
├── Air Quality Forecasting.ipynb        # Main notebook with EDA, preprocessing, modeling, and forecasts
├── Air Quality Forecasting.pdf          # Exported report/notebook PDF
├── Actual_Series.csv                    # Actual PM2.5 observations used for final forecast evaluation
├── Levon_Gevorgyan_TS_Project_Proposal.pdf
├── docs/
│   ├── report.md                        # Project report
│   ├── project_file_guide.md            # File and folder guide
│   └── github_publish_checklist.md      # Pre-publishing checklist
├── requirements_suggested.txt           # Suggested Python dependencies
├── LICENSE
└── .gitignore
```

## Installation

1. Clone the repository.
2. Create and activate a Python virtual environment.
3. Install the suggested dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements_suggested.txt
```

The notebook downloads the Kaggle dataset through `kagglehub`. Make sure Kaggle access is configured if the dataset requires authentication in your environment.

## How To Run

Open and run the notebook from top to bottom:

```bash
jupyter notebook "Air Quality Forecasting.ipynb"
```

Recommended run order:

1. Libraries and dataset loading
2. Preprocessing and feature engineering
3. Exploratory analysis
4. SARIMAX modeling and diagnostics
5. VARMAX modeling and diagnostics
6. XGBoost training and recursive forecasting
7. Final 7-day forecast and actual-observation evaluation

There are no standalone Python scripts in the current project structure.

## Main Results

On the shared 48-hour Lahore holdout window, the notebook reports the following model comparison:

| Model | MAE | RMSE | MAPE | sMAPE | R2 |
|---|---:|---:|---:|---:|---:|
| SARIMAX recursive | 9.991 | 16.096 | 6.108% | 5.871 | 0.903 |
| VARMAX | 11.263 | 18.515 | 6.419% | 6.303 | 0.872 |
| Naive baseline | 16.273 | 20.848 | 10.063% | 9.893 | 0.838 |
| SARIMAX dynamic | 17.876 | 23.448 | 10.499% | 10.594 | 0.795 |
| XGBoost | 20.076 | 24.082 | 13.184% | 12.170 | 0.783 |

The best model by MAPE is the recursive SARIMAX model.

For the final 7-day multi-city evaluation against actual observations, the recursive SARIMAX approach produced the strongest MAPE for Dhaka and London and the weakest for Beijing, where MAPE was sensitive to low and volatile observed values.

## Key Visualizations

The notebook and exported PDF contain the project visualizations:

- Hourly and daily pollutant concentration plots
- Multi-pollutant interaction view
- Correlation heatmap
- Hourly PM2.5 boxplot
- Healthy vs unhealthy PM2.5 count chart
- STL decomposition
- ACF/PACF plots
- SARIMAX and VARMAX diagnostic plots
- Holdout forecast comparison plots
- Final 7-day forecast plots

See [Air Quality Forecasting.ipynb](Air%20Quality%20Forecasting.ipynb) or [Air Quality Forecasting.pdf](Air%20Quality%20Forecasting.pdf).

## Limitations

- The main model comparison is focused on Lahore and a 48-hour holdout window.
- Recursive SARIMAX uses actual observations during holdout updates, making it easier than a fully open-loop forecast.
- Future exogenous pollutant values are approximated using recent recurring patterns.
- MAPE can be unstable when actual PM2.5 values are close to zero.
- Plot images are embedded in the notebook/PDF and are not currently exported as separate figure files.

## Future Improvements

- Add reusable Python scripts for data loading, training, evaluation, and forecasting.
- Export final figures to a dedicated `figures/` directory.
- Add a longer rolling-origin backtesting procedure.
- Compare against additional models such as Prophet, LightGBM, LSTM, or temporal CNNs.
- Add experiment tracking and saved model artifacts.
- Package the workflow so new cities can be forecast with a command-line interface.

## Documentation

- [Full project report](docs/report.md)
- [Project file guide](docs/project_file_guide.md)
- [GitHub publishing checklist](docs/github_publish_checklist.md)

## Author

Levon Gevorgyan
