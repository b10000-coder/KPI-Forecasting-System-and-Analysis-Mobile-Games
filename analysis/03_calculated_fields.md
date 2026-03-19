# TABLEAU CALCULATED FIELDS - QUICK REFERENCE
## Mobile Game Company Marketing Analyst Case Study

Copy and paste these directly into Tableau:

---

## 🔢 CORE METRICS

### Total Revenue D30
```
[revenue_total_cal_d30] + [ad_revenue_total_d30]
```

### CPI (Cost Per Install)
```
IF [Installs] > 0 THEN 
    [network_cost] / [Installs] 
ELSE 
    NULL 
END
```

### ROAS D30 (Return on Ad Spend)
```
IF [network_cost] > 0 THEN 
    ([revenue_total_cal_d30] + [ad_revenue_total_d30]) / [network_cost]
ELSE 
    NULL 
END
```

### LTV D30 (Lifetime Value per User)
```
IF [Installs] > 0 THEN 
    ([revenue_total_cal_d30] + [ad_revenue_total_d30]) / [Installs]
ELSE 
    NULL 
END
```

### ROI Percentage
```
IF [network_cost] > 0 THEN
    (([revenue_total_cal_d30] + [ad_revenue_total_d30]) - [network_cost]) / [network_cost] * 100
ELSE
    NULL
END
```

---

## 📅 DATA MATURITY FIELDS

### Days Since Install
```
DATEDIFF('day', [Day], TODAY())
```

### Is Mature Data (Boolean)
```
DATEDIFF('day', [Day], TODAY()) >= 30
```

### Data Maturity Label
```
IF DATEDIFF('day', [Day], TODAY()) >= 30 THEN 
    "Mature (30+ days)"
ELSEIF DATEDIFF('day', [Day], TODAY()) >= 14 THEN
    "Partial (14-29 days)"
ELSE 
    "Incomplete (<14 days)"
END
```

---

## 🏷️ DATA CLEANING FIELDS

### Channel Clean (Rename ASO)
```
IF [Channel] = "Google Organic Search" THEN 
    "ASO Keywords"
ELSEIF [Channel] = "Unknown Devices" THEN 
    "Unattributed Ad Revenue"
ELSE 
    [Channel]
END
```

### Valid for Campaign Analysis
```
[Installs] > 0 AND [Channel] != "Unknown Devices"
```

### Has Cost (Paid Campaign)
```
[network_cost] > 0
```

---

## 🎯 PERFORMANCE CLASSIFICATIONS

### Profitability Status
```
IF [network_cost] = 0 THEN 
    "Organic"
ELSEIF [ROAS D30] >= 1.5 THEN 
    "🟢 Highly Profitable"
ELSEIF [ROAS D30] >= 1.0 THEN 
    "🟡 Profitable"
ELSEIF [ROAS D30] >= 0.7 THEN 
    "🟠 Near Break-even"
ELSE 
    "🔴 Losing Money"
END
```

### Scale Recommendation
```
IF [network_cost] = 0 THEN 
    "N/A - Organic"
ELSEIF [ROAS D30] >= 1.2 AND [Installs] >= 500 THEN 
    "✅ Scale Up"
ELSEIF [ROAS D30] >= 1.2 AND [Installs] < 500 THEN 
    "🔍 Test & Scale"
ELSEIF [ROAS D30] >= 0.8 THEN 
    "⚠️ Optimize"
ELSE 
    "🛑 Pause/Reduce"
END
```

### Campaign Health Score (1-10)
```
IF [network_cost] = 0 THEN 
    NULL
ELSE
    CASE
        WHEN [ROAS D30] >= 2.0 THEN 10
        WHEN [ROAS D30] >= 1.5 THEN 8
        WHEN [ROAS D30] >= 1.2 THEN 7
        WHEN [ROAS D30] >= 1.0 THEN 5
        WHEN [ROAS D30] >= 0.7 THEN 3
        ELSE 1
    END
END
```

---

## 📊 COHORT ANALYSIS FIELDS

### D7 Revenue per Install
```
IF [Installs] > 0 THEN 
    ([revenue_total_cal_d7] + [ad_revenue_total_d7]) / [Installs]
ELSE 
    NULL 
END
```

### D14 Revenue per Install
```
IF [Installs] > 0 THEN 
    ([revenue_total_cal_d14] + [ad_revenue_total_d14]) / [Installs]
ELSE 
    NULL 
END
```

### D30 Revenue per Install
```
IF [Installs] > 0 THEN 
    ([revenue_total_cal_d30] + [ad_revenue_total_d30]) / [Installs]
ELSE 
    NULL 
END
```

### D7 to D30 Growth Rate
```
IF [revenue_total_cal_d7] + [ad_revenue_total_d7] > 0 THEN
    (([revenue_total_cal_d30] + [ad_revenue_total_d30]) - ([revenue_total_cal_d7] + [ad_revenue_total_d7])) / 
    ([revenue_total_cal_d7] + [ad_revenue_total_d7]) * 100
ELSE
    NULL
END
```

---

## 💰 EFFICIENCY METRICS

### Revenue per Dollar Spent
```
IF [network_cost] > 0 THEN 
    ([revenue_total_cal_d30] + [ad_revenue_total_d30]) / [network_cost]
ELSE 
    NULL 
END
```

### Profit/Loss D30
```
([revenue_total_cal_d30] + [ad_revenue_total_d30]) - [network_cost]
```

### Break-even Status
```
IF [network_cost] = 0 THEN 
    "N/A - Organic"
ELSEIF ([revenue_total_cal_d30] + [ad_revenue_total_d30]) >= [network_cost] THEN 
    "✅ Profitable"
ELSE 
    "❌ Not Profitable"
END
```

### Days to Break-even (Approximate)
This requires pivoted data with daily revenue. Formula concept:
```
// Note: This is a conceptual formula - requires restructured data
// Find first day where cumulative revenue >= network_cost
LOOKUP(MIN(IF [Cumulative Revenue] >= [network_cost] THEN [Day Number] END))
```

---

## 🎨 COLOR CODING FIELDS

### ROAS Color
```
IF [ROAS D30] >= 1.5 THEN "#2ECC71"  // Green
ELSEIF [ROAS D30] >= 1.0 THEN "#F39C12"  // Orange
ELSEIF [ROAS D30] >= 0.7 THEN "#E67E22"  // Dark Orange
ELSE "#E74C3C"  // Red
END
```

### Performance Indicator (Symbol)
```
IF [network_cost] = 0 THEN "⚪"
ELSEIF [ROAS D30] >= 1.2 THEN "🟢"
ELSEIF [ROAS D30] >= 0.8 THEN "🟡"
ELSE "🔴"
END
```

---

## 🔍 FILTER FIELDS

### Exclude Bad Data
```
[Installs] > 0 AND [Channel] != "Unknown Devices"
```

### Mature Cohorts Only
```
DATEDIFF('day', [Day], TODAY()) >= 30
```

### Paid Campaigns Only
```
[network_cost] > 0 AND [Installs] > 0
```

### Minimum Volume Filter
```
[Installs] >= 100
```

### Valid ROAS Calculation
```
[network_cost] > 0 AND [Installs] > 0 AND [Days Since Install] >= 30
```

---

## 📈 BENCHMARK REFERENCE LINES

Add these as reference lines in your visualizations:

### ROAS Break-even
- **Value:** 1.0
- **Label:** "Break-even (1.0x ROAS)"
- **Color:** Gray dashed line

### Good ROAS Target
- **Value:** 1.5
- **Label:** "Target ROAS (1.5x)"
- **Color:** Green dashed line

### CPI Benchmark
- **Value:** 1.50
- **Label:** "Target CPI ($1.50)"
- **Color:** Blue dashed line

### LTV Benchmark
- **Value:** 2.00
- **Label:** "Healthy LTV ($2.00)"
- **Color:** Green dashed line

---

## 🎯 PRO TIPS FOR TABLEAU

1. **Create a "Clean Data" Data Source Filter:**
   - Add condition: `[Valid for Campaign Analysis] = True`
   - This automatically excludes bad rows from all worksheets

2. **Use Parameters for Dynamic Analysis:**
   ```
   Parameter: Analysis Window
   Values: D7, D14, D30
   
   Calculated Field: Selected Revenue
   CASE [Analysis Window]
       WHEN "D7" THEN [D7 Revenue per Install]
       WHEN "D14" THEN [D14 Revenue per Install]
       WHEN "D30" THEN [D30 Revenue per Install]
   END
   ```

3. **Create a Performance Dashboard Action:**
   - Click on channel → filter all sheets
   - Hover on campaign → show detailed tooltip
   - Click on campaign → open detailed drill-down sheet

4. **Use LOD Expressions for Aggregations:**
   ```
   Channel Average ROAS:
   {FIXED [Channel] : AVG([ROAS D30])}
   
   Above/Below Average:
   IF [ROAS D30] > {FIXED [Channel] : AVG([ROAS D30])} 
   THEN "Above Average" 
   ELSE "Below Average" 
   END
   ```

---

## 📋 WORKSHEET SUGGESTIONS

### Sheet 1: Campaign Efficiency Matrix (Scatter)
- **X-axis:** CPI
- **Y-axis:** LTV D30
- **Size:** Installs
- **Color:** ROAS Color
- **Labels:** Campaign Network
- **Reference Lines:** Break-even diagonal (where LTV = CPI)

### Sheet 2: Channel Performance (Bar)
- **Rows:** Channel Clean (sorted by ROAS)
- **Columns:** ROAS D30
- **Color:** Profitability Status
- **Label:** ROAS value + Scale Recommendation

### Sheet 3: Daily Performance Trend (Line)
- **Columns:** Day (continuous)
- **Rows:** 
  - Line 1: Installs
  - Line 2: Revenue D30 (dual axis)
- **Color:** Channel Clean
- **Filter:** Data Maturity = "Mature"

### Sheet 4: Campaign Deep Dive (Table)
- **Rows:** Channel, Campaign Network
- **Columns:** Installs, Cost, CPI, LTV D30, ROAS D30, Profit/Loss
- **Color:** Conditional formatting on ROAS
- **Sort:** By ROAS descending

---

**Remember:** Always validate your calculations by spot-checking a few rows manually before presenting!
