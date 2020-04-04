# Tasks/Ideas
LW: Select 4? different weeks (months?). Needs to overlap with transit data and have no NaNs.
- Heaviest rain
- Heaviest snow
- Coldest
- Hottest
- Standard/Normal

MS: Weather Data:
- Remove unnecessary columns. (Choose relevant columns.)
- Remove rows outside of transit data range.
- Replace NaNs (nulls).
- Change Flags to boolean.
- Merge the two DataFrames.

BY: Correlation (boardings vs temp/precip).

JJ: 
- Aggregate hourly transit data. Need daily totals.
- Get mean monthly temperature 
- Get the delta between daily temperature adn mean monthly temperature
- Plot the delta in relation to the daily total

VN: Add station data
- Load, clean, merge into boardings.
