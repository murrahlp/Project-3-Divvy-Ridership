# Exploratory Data Analysis & Findings (2025 Divvy Ridership)

## 1. Key Seasonal & Weather Findings

### Strong Temperature-Driven Seasonality
- **Winter Lows:** Daily ridership regularly drops below 4,000 rides during freezing conditions. Extreme sub-zero cold snaps drove the lowest demand of the year, such as **January 21** (mean temp `-1.4°F`, **1,309 rides**) and **February 17–18** (mean temps `3.5°F` and `-0.1°F`, **2,511** and **2,761 rides**).
- **Summer Peaks:** Warm, dry summer days routinely exceed **25,000–32,000 daily rides**, peaking at **32,308 rides** on Saturday, **August 2** (mean temp `69.1°F`, `0.0"` precipitation).
- **Single-Year Caveat:** Because this dataset covers only one calendar year (2025), recurring seasonal trends cannot be statistically separated from one-off events (such as summer festivals, holidays, or anomalous weather spells).

### Sharp Suppression from Rain & Snow
- Precipitation and snowfall cause immediate drops in ridership even when temperatures are mild:
  - **June 4:** Heavy rain (`1.252"` at `60.7°F`) cut total rides by more than half to **10,603**, down from **21,671** the day before.
  - **June 18:** Heavy rainfall (`1.654"` at `70.8°F`) suppressed volume to **12,459 rides** compared to **25,641** on June 17.
  - **February 12:** Heavy snow (`2.48"` at `26.2°F`) reduced demand to **3,122 rides**, down from **5,964** the prior day.

### Rider Segment & Fleet Dynamics
- **Members vs. Casual Riders:** Annual members provide a consistent weekday commuting floor year-round (accounting for **65%–85%** of rides in winter). Casual ridership is highly elastic to warm, dry weather and weekends—frequently outnumbering members on peak summer Saturdays (e.g., **18,006 casual** vs. **13,907 member** rides on June 28, and **18,240 casual** vs. **11,205 member** rides on July 5).
- **Electric vs. Classic Bikes:** Electric bikes account for roughly **60%–65%** of daily trips across most of the year. The main exception occurs during extreme sub-zero cold snaps (e.g., January 21 at `-1.4°F`, when classic bikes made up **1,069 of 1,309** rides).

---

## 2. Operational Day-Type Regimes (Clustering Insights)
2025 days separate into five distinct operational demand regimes:

1. **Warm, Calm Workdays (134 days):** Avg `64.2°F`, `0.1"` precip, `8.6 mph` wind → **21,013 avg rides** (`13,747` member / `7,265` casual).
2. **Dry Weekends (96 days):** Avg `52.0°F`, `0.0"` precip, `10.8 mph` wind → **15,773 avg rides** (`8,282` member / `7,491` casual).
3. **Rainy, Windy Days (19 days):** Avg `54.9°F`, `0.8"` rain, `14.5 mph` wind → **11,709 avg rides** (`8,020` member / `3,689` casual).
4. **Cold, Dry Workdays (109 days):** Avg `35.0°F`, `0.0"` precip, `13.9 mph` wind → **8,934 avg rides** (`6,820` member / `2,115` casual).
5. **Freezing, Snowy Days (7 days):** Avg `28.5°F`, `2.2"` snow, `14.9 mph` wind → **3,815 avg rides** (`2,929` member / `886` casual).

