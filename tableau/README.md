# Tableau Dashboard Guide

## Overview

This folder contains resources for building the Tableau dashboard.

## Files

- `calculated_fields_library.md` - All formulas ready to copy-paste

## Quick Setup

1. **Import Data**
   - Open Tableau Desktop
   - Connect to `../data/campaign_data.csv`
   - Add data source filter: `[Installs] > 0`

2. **Create Calculated Fields**
   - Copy formulas from `calculated_fields_library.md`
   - Create each field (Right-click Data pane → Create Calculated Field)

3. **Build Dashboard**
   - Follow `../analysis/02_tableau_guide.md` for step-by-step instructions
   - Reference `../analysis/04_dashboard_mockup.md` for visual layout

## Core Metrics

```
CPI = SUM([network_cost]) / SUM([installs])
LTV = (SUM([revenue_total_cal_d30]) + SUM([ad_revenue_total_d30])) / SUM([installs])
ROAS = (SUM([revenue_total_cal_d30]) + SUM([ad_revenue_total_d30])) / SUM([network_cost])
```

## Dashboard Components

1. **Executive Summary** - KPIs + channel ROAS bar chart
2. **Channel Performance** - Scatter plot + campaign table
3. **Data Quality** - Outliers and zero-revenue campaigns

## Support

For detailed build instructions, see `/analysis/02_tableau_guide.md`
