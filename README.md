# Dhaka Air Quality Analysis

Analysis of PM2.5 pollution in Dhaka by month, season and hour of day, its relationship with weather, and next-day forecasting.

## Problem Statement
Which months and times of day have the worst air quality in Dhaka, how does weather relate to pollution, and how well can next-day PM2.5 be forecast?

## Data Sources
- Bangladesh Air Quality Index (AQI) Dataset (2000-2025), Mendeley Data: https://data.mendeley.com/datasets/9j447cynb9
- Historical weather data, Open-Meteo: https://open-meteo.com/en/docs/historical-weather-api

**Analysis period:** 2022-08-04 to 2025-11-23 (28,992 hours, 1,209 days). Timestamps converted from UTC to local time (UTC+6).

## Methodology
1. Data audit and cleaning (see Data Quality Notes)
2. Exploratory data analysis (monthly, seasonal, hourly patterns)
3. Correlation with temperature, humidity, rainfall and wind (Spearman)
4. Next-day PM2.5 forecasting (persistence baseline vs. gradient boosting)
5. Interactive dashboard (Power BI / Tableau)

## Data Quality Notes
- Records before 2022-08-04 show synthetic-looking patterns (constant monthly levels, linear yearly growth, unit change in CO) and were excluded.
- The "Azimpur" series is identical to "Dhaka" in the overlap period (correlation = 1.0) and was not used.
- The dataset's `aqi` column was not trusted; AQI was recomputed from PM2.5 using the US EPA (2024) breakpoints.
- Remaining data appear to be modeled estimates, not ground sensor readings.
- Timestamps were UTC; converted to local time (validated: ozone peaks at 14:00, PM2.5 is lowest at 15:00).

## Key Findings

**Seasonality**
- Monthly mean PM2.5 peaks in January (~98 µg/m³) and is lowest in July (~14 µg/m³): high in winter, low in the monsoon.

**Time of day**
- Hourly mean PM2.5 peaks at 8-9 PM (~61 µg/m³) and is lowest at 3 PM (~36 µg/m³), with a smaller bump near 7 AM.

**Health thresholds**
- 84.5% of days exceeded the WHO daily guideline (15 µg/m³).
- 38.0% of days fell in the "Unhealthy" category (55.5+ µg/m³).

**Weather relationships (Spearman, with PM2.5)**

| Variable | Correlation |
|---|---|
| Wind speed | -0.52 |
| Temperature | -0.48 |
| Precipitation | -0.45 |
| Humidity | -0.19 |

- Mean PM2.5 is ~65 µg/m³ at wind speeds of 0-5 km/h and ~15 µg/m³ above 20 km/h.
- Mean PM2.5 is 21.2 µg/m³ in rainy hours vs. 55.7 µg/m³ in dry hours (~62% lower).
- The temperature relationship is largely a seasonal effect. These are correlations, not causal effects.

**Seasonal correlations (Spearman, with PM2.5)**

| Season | Wind | Temperature | Precipitation | Humidity |
|---|---|---|---|---|
| Winter | -0.26 | -0.34 | -0.08 | 0.25 |
| Summer | -0.37 | -0.18 | -0.28 | -0.26 |
| Monsoon | -0.60 | 0.16 | -0.24 | 0.01 |
| Post-monsoon | -0.27 | -0.40 | -0.41 | 0.02 |

- Wind speed is negatively correlated with PM2.5 in every season, strongest in the monsoon (-0.60), making it the most consistent weather factor.
- Within-season temperature correlations are weaker than the overall -0.48 and change sign in the monsoon, confirming that much of the overall temperature effect is seasonal.
- Precipitation matters most in the post-monsoon season (-0.41) and little in winter (-0.08, the dry season).
- Humidity has no consistent direction (positive in winter, negative in summer).

**Forecasting (test: Dec 2024 - Nov 2025, 358 days)**

| Model | MAE | RMSE | R² |
|---|---|---|---|
| Persistence (yesterday) | 9.84 | 14.87 | 0.827 |
| GBM: lags + season | 10.41 | 15.18 | 0.820 |
| GBM: + weather | **9.74** | **13.53** | **0.857** |

- Adding weather features reduced RMSE by ~9% versus the persistence baseline; lags and seasonality alone did not beat it.
- Yesterday's PM2.5 (`lag1`) is the dominant predictor; wind speed is the most important weather feature.
- The model captures the winter peak, monsoon lows and the November rise, but under-predicts sudden sharp spikes.

## Recommendations
- Concentrate public health advisories and mask guidance in November-March, especially evening hours (7-10 PM).
- Schedule outdoor activity for afternoons and for windy or rainy days, when PM2.5 is lowest.
- Use low-wind, dry-weather forecasts as an early-warning signal for high-pollution days.
- Expand ground-sensor monitoring to replace modeled estimates.

## Limitations
- The data appear to be modeled estimates, not sensor measurements.
- Only ~3.3 years of usable data; year-over-year trends cannot be reliably assessed.
- Forecast weather features use observed historical weather. Real forecasts would use predicted weather, so R² = 0.857 is a best-case result.
- The test period is a single year; results were not validated with time-series cross-validation.
- Weather associations are correlational.

## Project Structure
```
dhaka-air-quality-analysis/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_data_loading_cleaning.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_weather_correlation.ipynb
│   └── 04_forecasting.ipynb
├── outputs/
│   ├── figures/
│   └── dashboard/
├── src/
│   └── utils.py
├── README.md
├── requirements.txt
└── .gitignore
```

## How to Run
```bash
git clone https://github.com/<your-username>/dhaka-air-quality-analysis.git
cd dhaka-air-quality-analysis
pip install -r requirements.txt
```
Download the datasets into `data/raw/` and run the notebooks in order (01 → 04).

## Tools
Python (pandas, NumPy, matplotlib, seaborn, scikit-learn), Jupyter, Power BI / Tableau

## Author
<Jaimul Haque> | <https://www.linkedin.com/in/jaimul-haque-317b07374/>
