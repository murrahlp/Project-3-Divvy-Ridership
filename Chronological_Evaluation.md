# Chronological Evaluation 

## 1. Train/Test Split Design (70/30 Chronological Holdout)
- **Split Structure:** We partitioned the 365 daily observations from `daily_ridership_weather_2025.csv` in strict chronological order  
  - **Training Set (First 70% — ~255 days):** January 1, 2025 through mid-September 2025 (covering winter, spring, and peak summer conditions).
  - **Test Set (Final 30% — ~110 days):** Mid-September 2025 through December 31, 2025 (covering autumn transition and early winter).
- **Strict Leakage Prevention:** All model features (`temperature_2m_mean (°F)`, `precipitation_sum (inch)`, `snowfall_sum (inch)`, `wind_speed_10m_max (mp/h)`, and `is_weekend`) represent calendar or forecasted weather inputs known prior to the target day, and the baseline lag (`total_rides.shift(7)`) strictly uses past realized demand from seven days earlier.

---

## 2. Why We Use a Chronological Split Instead of Sampling Across Every Month
1. **Mirrors Real-World Operational Deployment:** In production, an operations manager forecasts upcoming days using only historical data from the past—never future observations. Evaluating on the final 30% of the year tests whether the model generalizes forward in time.
2. **Prevents Temporal Data Leakage & Autocorrelation Inflation:** Daily bike-share ridership is strongly autocorrelated across consecutive days (weather fronts, multi-day heatwaves, and local events span multiple days). Randomly sampling or pulling test days from every month places training days immediately before and after test days, allowing the model to "peek" at adjacent days' conditions and artificially inflating accuracy.
3. **Tests Out-of-Sample Seasonal Transition:** Holding out the final 30% of the calendar tests how well relationships learned during winter, spring, and summer hold up as temperatures cool down through fall and early winter a stress test before generating a live seven-day outlook.

---

## 3. Holdout Performance Results (`forecasting.ipynb`)
Evaluated on the unseen final 30% chronological test window (~110 days) using Mean Absolute Error (MAE):

| Model / Benchmark | Predictors / Method | Holdout MAE (Rides/Day) | Improvement vs. Baseline |
| :--- | :--- | :---: | :---: |
| **Naive Lag Baseline** | Same weekday from 7 days prior (`shift(7)`) | **3,141** | — |
| **Linear Regression** | Mean Temp, Precip, Snow, Max Wind, Weekend Flag | **2,600** | **-541 rides/day (-17.2%)** |

---

## 4. Key Trade-Offs & Limitations of a Single-Year Chronological Split
- **Seasonal Asymmetry:** Because 2025 provides only one calendar year, training on the first 70% (Jan–mid-Sep) and testing on the last 30% (mid-Sep–Dec) means the model is evaluated primarily on cooling fall/winter weather rather than peak summer heatwaves.
- **Unmodeled Calendar & Event Effects:** The current linear model captures weather and weekend effects well, reducing daily error by 541 rides compared to a 7-day lag, but residual errors remain around fall and winter.
