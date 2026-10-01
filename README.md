# Dhaka Air Quality Analysis

Analyzing air pollution (PM2.5 / AQI) trends in Dhaka by month, season and hour of day, and exploring how weather affects pollution levels.

## Problem Statement
Which months and times of day have the worst air quality in Dhaka, and is pollution increasing or decreasing over the years?

## Data Sources
- Bangladesh Air Quality Index (AQI) Dataset (2000-2025), Mendeley Data: https://data.mendeley.com/datasets/9j447cynb9
- Historical weather data, Open-Meteo: https://open-meteo.com/en/docs/historical-weather-api

## Methodology
1. Data cleaning (missing values, duplicates, outliers)
2. Exploratory data analysis (monthly, seasonal, hourly patterns)
3. Correlation with temperature, humidity, rainfall and wind
4. Time series forecasting
5. Interactive dashboard (Power BI / Tableau)

## Key Findings
_To be added after analysis._

## Recommendations
_To be added after analysis._

## Project Structure
```
dhaka-air-quality/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── outputs/
├── src/
├── README.md
└── requirements.txt
```

## How to Run
```bash
git clone https://github.com/<your-username>/dhaka-air-quality-analysis.git
cd dhaka-air-quality-analysis
pip install -r requirements.txt
```
Download the datasets into `data/raw/` and run the notebooks in order.

## Tools
Python (pandas, matplotlib, seaborn, statsmodels), SQL, Power BI / Tableau

## Author
<Your Name> | <LinkedIn link>
