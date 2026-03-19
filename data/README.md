# Campaign Data

## File: campaign_data.csv

**Rows**: 2,685  
**Period**: September 2025 - January 2026 (136 days)

## Schema

### Dimensions
- `channel` - Acquisition channel (Google Ads, Facebook, Unity Ads, etc.)
- `campaign_network` - Specific campaign name
- `day` - Date of install cohort

### Metrics
- `network_cost` - Marketing spend ($)
- `installs` - User installs (count)
- `revenue_total_cal_d1` to `revenue_total_cal_d30` - Cumulative IAP revenue by day
- `ad_revenue_total_d1` to `ad_revenue_total_d30` - Cumulative ad revenue by day

## Data Quality Notes

**Issues Identified**:
- 144 rows with zero installs (exclude from analysis)
- 3 campaigns with attribution errors (ROAS > 10x)
- 120 Facebook campaigns with zero revenue
- 608 rows with <30 days maturity

**Filters Applied**:
```
Valid for Analysis = [installs] > 0 AND [days_since_install] >= 30
```

## Usage

Import into Tableau or Python for analysis. See `/analysis/01_data_audit_report.md` for detailed data quality assessment.
