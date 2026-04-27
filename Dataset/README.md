# Dataset

The challenge dataset is provided by ZoomCharts and is **not included** in this repository.

## Download

Register and download from the official challenge page:
https://zoomcharts.com/en/microsoft-power-bi-custom-visuals/challenges/

## Structure

| Table | Rows | Purpose |
|---|---|---|
| FactKPI_Monthly | 440,000+ | Core fact table |
| DimKPI | 10 | KPI metadata (name, category, polarity, format) |
| DimDate | 24 | Monthly calendar + IsBoardSeason flag |
| DimOrg | 18 | Division > BusinessUnit > ProductLine |
| DimRegion | 34 | Region > Subregion > Country |
