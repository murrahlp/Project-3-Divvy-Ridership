# Data Quality & Cleaning Summary

## Overview
This document summarizes the checks and cleaning steps used to combine the 12 monthly 2025 Divvy trip files and the 2025 Chicago weather spreadsheet into a single master dataset.

* **Final Dataset:** `daily_ridership_weather_2025.csv`
* **Final Dimensions:** 365 rows (one per day of 2025) and 13 columns
* **Missing Values:** 0 across all columns

---

## 1. Checks We Performed
* **File Count Check:** Verified that all 12 monthly Divvy .zip files (`202501` through `202512`) were found and loaded in chronological order.
* **Row Count & Date Range Check:** Checked the number of rows before and after cleaning to confirm both datasets covered the exact same 365 calendar days (`2025-01-01` to `2025-12-31`) with no duplicate dates.
* **Column & Header Inspection:** Printed column names and sample rows from both sources to verify formatting and check for corrupted text.
* **Missing Value Check:** Ran `.isna().sum()` across every column after merging to confirm 0 missing values.

---

## 2. What We Had to Clean
* **Removed 2024 Spillover Rides:** Grouping the Divvy trip data by start date initially produced 366 rows because 53 trips started on `2024-12-31` right before midnight and ended in January 2025. We filtered the dataset to `date >= 2025-01-01` so only 2025 dates remained.
* **Skipped Weather Metadata Rows:** `Chicago-Weather-2025.xlsx` contained 3 metadata rows at the top (latitude, longitude, elevation, timezone, and a blank spacer row), which shifted the real column headers down and created `Unnamed` columns. We used `skiprows=3` so the actual headers loaded cleanly on row 4.
* **Standardized Date Columns:** Converted Divvy timestamps (`started_at`) to daily dates (`date`) and renamed the weather date column (`time`) to `date` so both tables joined cleanly on a single key.

---

## 3. Typos, Formatting & Missing Values
* **Character Encoding Typo (`Â°F`):** The weather export contained a text encoding artifact in the temperature headers (`temperature_2m_max (Â°F)`). We replaced `Â°` with `°` so all temperature columns read cleanly as `(°F)`.
* **Missing Values:** Neither the aggregated daily ridership counts nor the daily weather metrics contained any missing values.
* **Selected Only Needed Columns:** Raw Divvy trip files include station names and GPS coordinates that often have missing values. When grouping the rides by day, our code only aggregated the columns needed for our analysis (`ride_id`, `member_casual`, and `rideable_type`), leaving out those incomplete station and location fields.